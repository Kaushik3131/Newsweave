# Deploying to Google Cloud Platform (Cloud Run)

This guide helps you deploy your Next.js application to Google Cloud Run, a serverless platform for running containerized applications.

## Prerequisites

1.  **Google Cloud SDK**: Make sure you have `gcloud` installed and authenticated.
    ```powershell
    gcloud auth login
    gcloud config set project [YOUR_PROJECT_ID]
    ```
2.  **Billing Enabled**: Ensure billing is enabled for your Google Cloud Project.

## Cost expectations (Free Tier)
- **Cloud Run**: 2 million requests/month free.
- **Cloud Build**: 120 build-minutes/day free.
- **Artifact Registry**: Storage costs apply (approx. $0.10/GB/month).

## Step 1: Enable Required Services

Run these commands to enable the necessary APIs:

```powershell
gcloud services enable cloudbuild.googleapis.com
gcloud services enable run.googleapis.com
gcloud services enable artifactregistry.googleapis.com
```

## Step 2: Create an Artifact Registry Repository

This is where your Docker images will be stored.

```powershell
gcloud artifacts repositories create my-repo --repository-format=docker --location=us-central1 --description="Docker repository"
```

## Step 3: Build and Push the Image

Replace `[YOUR_PROJECT_ID]` with your actual project ID.

```powershell
# Submit the build to Cloud Build
gcloud builds submit --tag us-central1-docker.pkg.dev/[YOUR_PROJECT_ID]/my-repo/newsletter-app:latest .
```

## Step 4: Deploy to Cloud Run

You need to pass your environment variables. Since you have a `.env` file, the easiest way to manage secrets securely is using Secret Manager, but for a quick start, you can set them directly (be careful with sensitive keys in your history).

**Option A: Deploy using a .env file (Preview feature or manual mapping)**
Cloud Run doesn't natively read `.env` files directly from the CLI in a single flag easily without beta features, so it's often better to reference them.

**Option B: Direct command (Replace values with your actual secrets)**

```powershell
gcloud run deploy newsletter-app `
  --image us-central1-docker.pkg.dev/[YOUR_PROJECT_ID]/my-repo/newsletter-app:latest `
  --platform managed `
  --region us-central1 `
  --allow-unauthenticated `
  --set-env-vars DATABASE_URL="your_mongodb_url" `
  --set-env-vars NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY="your_clerk_key" `
  --set-env-vars CLERK_SECRET_KEY="your_clerk_secret"
  # Add other env vars from your .env file here
```

## Step 5: Verify


## Testing with Clerk (Development Mode)

Yes, you can deploy with Clerk in **Development Mode** for testing.

1.  **Environment Variables**: Use your `pk_test_...` and `sk_test_...` keys in the Cloud Run environment variables.
2.  **Allowed Domains**:
    *   After deploying, copy your Cloud Run URL (e.g., `https://newsletter-app-xyz.a.run.app`).
    *   Go to **Clerk Dashboard > Configure > Domains**.
    *   Add your Cloud Run URL to the allowed/satellite domains list to prevent CORS/Origin errors.
3.  **Note**: Your app will display a "Development Mode" banner.
