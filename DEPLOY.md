# Deploying NewsWeave to Google Cloud Platform

This guide provides comprehensive instructions for deploying the NewsWeave platform to Google Cloud Run using automated CI/CD with GitHub Actions.

## 📋 Table of Contents

- [Prerequisites](#prerequisites)
- [Cost Expectations](#cost-expectations)
- [Local Docker Testing](#local-docker-testing)
- [Google Cloud Setup](#google-cloud-setup)
- [Service Account Configuration](#service-account-configuration)
- [GitHub Actions Setup](#github-actions-setup)
- [Manual Deployment](#manual-deployment)
- [Clerk Configuration](#clerk-configuration)
- [Troubleshooting](#troubleshooting)

## Prerequisites

### Required Tools

1. **Google Cloud SDK**: Install and authenticate `gcloud` CLI
   ```powershell
   # Install from: https://cloud.google.com/sdk/docs/install
   
   # Authenticate
   gcloud auth login
   
   # Set your project
   gcloud config set project [YOUR_PROJECT_ID]
   ```

2. **Docker**: For local testing (optional but recommended)
   - Install from: https://www.docker.com/products/docker-desktop

3. **Git**: For version control and GitHub Actions

### Required Accounts

- Google Cloud Platform account with billing enabled
- GitHub account with repository access
- Clerk account for authentication
- MongoDB database (Atlas recommended)

## 💰 Cost Expectations

### Free Tier Limits

- **Cloud Run**: 
  - 2 million requests/month free
  - 360,000 GB-seconds of memory free
  - 180,000 vCPU-seconds free
  
- **Cloud Build**: 
  - 120 build-minutes/day free
  - First 10 builds/day are free

- **Artifact Registry**: 
  - 0.5 GB storage free per month

### Estimated Monthly Costs (Low Traffic)

For a small application with minimal traffic, you'll likely stay within the free tier. Estimated costs if exceeding free tier:
- Cloud Run: $0-5/month
- Cloud Build: $0-2/month
- Artifact Registry: $0-1/month

**Total: ~$0-8/month for low traffic**

## 🐳 Local Docker Testing

Test your Docker container locally before deploying to ensure everything works correctly.

### 1. Build the Docker Image

You need to pass the Clerk publishable key as a build argument:

```powershell
docker build -t newsweave-app `
  --build-arg NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_your_key_here .
```

### 2. Run the Container

Run the container with your environment variables:

```powershell
docker run -p 3000:3000 --env-file .env newsweave-app
```

### 3. Test the Application

Open http://localhost:3000 in your browser and verify:
- ✅ Application loads correctly
- ✅ Clerk authentication works
- ✅ Database connection is successful
- ✅ RSS feeds can be added
- ✅ Newsletter generation works

## ☁️ Google Cloud Setup

### Step 1: Enable Required APIs

Enable the necessary Google Cloud APIs:

```powershell
# Enable Cloud Run
gcloud services enable run.googleapis.com

# Enable Cloud Build
gcloud services enable cloudbuild.googleapis.com

# Enable Artifact Registry
gcloud services enable artifactregistry.googleapis.com

# Enable IAM (if not already enabled)
gcloud services enable iam.googleapis.com
```

### Step 2: Create Artifact Registry Repository

Create a Docker repository to store your container images:

```powershell
# Replace 'us-central1' with your preferred region
gcloud artifacts repositories create newsletter-repo `
  --repository-format=docker `
  --location=us-central1 `
  --description="NewsWeave Docker repository"
```

Verify the repository was created:

```powershell
gcloud artifacts repositories list
```

### Step 3: Configure Docker Authentication

Authenticate Docker to push images to Artifact Registry:

```powershell
gcloud auth configure-docker us-central1-docker.pkg.dev
```

## 🔐 Service Account Configuration

### Create a Service Account

Create a dedicated service account for deployment:

```powershell
gcloud iam service-accounts create newsweave-deployer `
  --display-name="NewsWeave Deployment Service Account" `
  --description="Service account for automated NewsWeave deployments"
```

### Assign Required Permissions

Grant the minimum necessary permissions (principle of least privilege):

```powershell
# Set variables
$PROJECT_ID = "your-project-id"
$SA_EMAIL = "newsweave-deployer@${PROJECT_ID}.iam.gserviceaccount.com"

# Artifact Registry Writer (push Docker images)
gcloud projects add-iam-policy-binding $PROJECT_ID `
  --member="serviceAccount:${SA_EMAIL}" `
  --role="roles/artifactregistry.writer"

# Cloud Build Editor (submit and manage builds)
gcloud projects add-iam-policy-binding $PROJECT_ID `
  --member="serviceAccount:${SA_EMAIL}" `
  --role="roles/cloudbuild.builds.editor"

# Service Account User (act as service account)
gcloud projects add-iam-policy-binding $PROJECT_ID `
  --member="serviceAccount:${SA_EMAIL}" `
  --role="roles/iam.serviceAccountUser"

# Logging Writer (write logs)
gcloud projects add-iam-policy-binding $PROJECT_ID `
  --member="serviceAccount:${SA_EMAIL}" `
  --role="roles/logging.logWriter"

# Cloud Run Admin (deploy to Cloud Run)
gcloud projects add-iam-policy-binding $PROJECT_ID `
  --member="serviceAccount:${SA_EMAIL}" `
  --role="roles/run.admin"

# Service Usage Consumer (use GCP services)
gcloud projects add-iam-policy-binding $PROJECT_ID `
  --member="serviceAccount:${SA_EMAIL}" `
  --role="roles/serviceusage.serviceUsageConsumer"
```

### Generate Service Account Key

Create a JSON key file for GitHub Actions:

```powershell
gcloud iam service-accounts keys create key.json `
  --iam-account=newsweave-deployer@${PROJECT_ID}.iam.gserviceaccount.com
```

⚠️ **Important**: Keep this key file secure! Add it to `.gitignore` and never commit it to version control.

## 🔧 GitHub Actions Setup

### Configure GitHub Secrets

Add the following secrets to your GitHub repository (Settings → Secrets and variables → Actions):

| Secret Name | Description | Example |
|-------------|-------------|---------|
| `GCP_PROJECT_ID` | Your Google Cloud project ID | `newsweave-prod-123456` |
| `GCP_REGION` | Deployment region | `us-central1` |
| `GCP_SA_KEY` | Service account JSON key (entire file content) | `{ "type": "service_account", ... }` |
| `DATABASE_URL` | MongoDB connection string | `mongodb+srv://user:pass@cluster.mongodb.net/db` |
| `CLERK_SECRET_KEY` | Clerk secret key | `sk_test_...` or `sk_live_...` |
| `NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY` | Clerk publishable key | `pk_test_...` or `pk_live_...` |
| `GOOGLE_GENERATIVE_AI_API_KEY` | Google Gemini AI API key | `AIza...` |

### Deployment Workflow

The GitHub Actions workflow (`.github/workflows/deploy.yml`) automatically:

1. **Triggers** on push to `main` branch when relevant files change
2. **Authenticates** to Google Cloud using the service account
3. **Builds** the Docker image using Cloud Build
4. **Pushes** the image to Artifact Registry
5. **Deploys** to Cloud Run with environment variables

### Workflow Configuration

The workflow is configured with:

```yaml
env:
  PROJECT_ID: ${{ secrets.GCP_PROJECT_ID }}
  REGION: ${{ secrets.GCP_REGION }}
  SERVICE_NAME: newsweave
  REPOSITORY: newsletter-repo
  IMAGE: newsweave-app
```

You can modify these values in `.github/workflows/deploy.yml` if needed.

## 🚀 Manual Deployment

If you need to deploy manually without GitHub Actions:

### Build and Push with Cloud Build

```powershell
# Set your variables
$REGION = "us-central1"
$PROJECT_ID = "your-project-id"
$REPOSITORY = "newsletter-repo"
$IMAGE = "newsweave-app"
$CLERK_KEY = "pk_test_your_key_here"

# Submit build to Cloud Build
gcloud builds submit . `
  --config=cloudbuild.yaml `
  --substitutions="_REGION=${REGION},_REPOSITORY=${REPOSITORY},_IMAGE=${IMAGE},_NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=${CLERK_KEY}" `
  --timeout=900s
```

### Deploy to Cloud Run

```powershell
# Set environment variables
$DATABASE_URL = "your-mongodb-connection-string"
$CLERK_SECRET = "sk_test_your_secret_key"
$GEMINI_KEY = "your-gemini-api-key"
$CLERK_PUBLIC = "pk_test_your_public_key"

# Deploy to Cloud Run
gcloud run deploy newsweave `
  --image ${REGION}-docker.pkg.dev/${PROJECT_ID}/${REPOSITORY}/${IMAGE}:latest `
  --platform managed `
  --region ${REGION} `
  --allow-unauthenticated `
  --port 3000 `
  --memory 512Mi `
  --cpu 1 `
  --min-instances 0 `
  --max-instances 1 `
  --set-env-vars "DATABASE_URL=${DATABASE_URL},CLERK_SECRET_KEY=${CLERK_SECRET},GOOGLE_GENERATIVE_AI_API_KEY=${GEMINI_KEY},NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=${CLERK_PUBLIC}"
```

### Deployment Options Explained

- `--platform managed`: Use fully managed Cloud Run
- `--allow-unauthenticated`: Allow public access (required for web app)
- `--port 3000`: Container listens on port 3000
- `--memory 512Mi`: Allocate 512MB RAM (adjust based on needs)
- `--cpu 1`: Allocate 1 vCPU
- `--min-instances 0`: Scale to zero when not in use (saves costs)
- `--max-instances 1`: Limit to 1 instance (increase for production)

## 🔑 Clerk Configuration

### Development Mode

For testing with Clerk in development mode:

1. Use your `pk_test_...` and `sk_test_...` keys
2. After deployment, get your Cloud Run URL
3. In Clerk Dashboard → Configure → Domains:
   - Add your Cloud Run URL (e.g., `https://newsweave-xyz.a.run.app`)
   - This prevents CORS/Origin errors
4. Your app will display a "Development Mode" banner

### Production Mode

For production deployment:

1. Switch to production keys (`pk_live_...` and `sk_live_...`)
2. Configure your custom domain in Clerk
3. Update GitHub secrets with production keys
4. Redeploy the application

## 🔍 Troubleshooting

### Build Failures

**Issue**: Cloud Build fails with "permission denied"
```
Solution: Verify service account has 'roles/cloudbuild.builds.editor'
```

**Issue**: Docker build fails with "NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY not set"
```
Solution: Ensure the key is passed in --substitutions parameter
```

### Deployment Failures

**Issue**: Cloud Run deployment fails with "permission denied"
```
Solution: Verify service account has 'roles/run.admin'
```

**Issue**: Application crashes on startup
```
Solution: Check Cloud Run logs:
gcloud run services logs read newsweave --region=us-central1
```

### Runtime Errors

**Issue**: Database connection fails
```
Solution: Verify DATABASE_URL is correctly set in Cloud Run environment variables
Check MongoDB Atlas allows connections from 0.0.0.0/0 or Cloud Run IPs
```

**Issue**: Clerk authentication fails
```
Solution: Verify Cloud Run URL is added to Clerk allowed domains
Check CLERK_SECRET_KEY and NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY are correct
```

### Viewing Logs

```powershell
# View Cloud Run logs
gcloud run services logs read newsweave --region=us-central1 --limit=50

# View Cloud Build logs
gcloud builds list --limit=5
gcloud builds log [BUILD_ID]
```

## 📊 Monitoring and Management

### View Service Status

```powershell
# List Cloud Run services
gcloud run services list

# Describe service details
gcloud run services describe newsweave --region=us-central1
```

### Update Environment Variables

```powershell
gcloud run services update newsweave `
  --region=us-central1 `
  --update-env-vars "NEW_VAR=value"
```

### Scale Configuration

```powershell
# Update scaling settings
gcloud run services update newsweave `
  --region=us-central1 `
  --min-instances=0 `
  --max-instances=5
```

## 🎯 Best Practices

1. **Use Secrets Manager**: For production, consider using Google Secret Manager instead of environment variables
2. **Enable Cloud Armor**: Add DDoS protection for production deployments
3. **Set up Monitoring**: Configure Cloud Monitoring alerts for errors and performance
4. **Use Custom Domain**: Configure a custom domain for production
5. **Implement CI/CD**: Use the GitHub Actions workflow for consistent deployments
6. **Regular Backups**: Ensure MongoDB has automated backups enabled
7. **Review Logs**: Regularly check Cloud Run logs for errors

## 📚 Additional Resources

- [Cloud Run Documentation](https://cloud.google.com/run/docs)
- [Cloud Build Documentation](https://cloud.google.com/build/docs)
- [Artifact Registry Documentation](https://cloud.google.com/artifact-registry/docs)
- [Clerk Documentation](https://clerk.com/docs)
- [Next.js Deployment](https://nextjs.org/docs/deployment)

## 🆘 Support

If you encounter issues:

1. Check the troubleshooting section above
2. Review Cloud Run and Cloud Build logs
3. Verify all environment variables are set correctly
4. Ensure all required APIs are enabled
5. Check service account permissions

---

**Last Updated**: December 2025  
**Maintained by**: NewsWeave Team
