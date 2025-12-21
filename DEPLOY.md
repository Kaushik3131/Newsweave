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

## Local Docker Testing

You can build and run this container locally to verify everything works before deploying.

1.  **Build the image**:
    You need to pass your Clerk Publishable Key during the build.
    ```powershell
    docker build -t newsletter-app --build-arg NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_... .
    ```

2.  **Run the container**:
    Since your app needs environment variables, use the `--env-file` flag to pass your local `.env` file.
    ```powershell
    docker run -p 3000:3000 --env-file .env newsletter-app
    ```

3.  **Access**: Open http://localhost:3000

---


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

## Step 3: Build and Deploy

Since your app needs build arguments, we use `cloudbuild.yaml`.

```powershell
# Submit the build using Cloud Build config
# Replace keys and project ID with your values
gcloud builds submit --config cloudbuild.yaml `
  --substitutions=_NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY="pk_test_...",_IMAGE_NAME="us-central1-docker.pkg.dev/[YOUR_PROJECT_ID]/my-repo/newsletter-app:latest"
```

Note: This single command handles both building the image and deploying it to Cloud Run (defined in the yaml file). Or you can remove the deploy step from yaml and do it manually.


## Step 5: Verify


## Testing with Clerk (Development Mode)

Yes, you can deploy with Clerk in **Development Mode** for testing.

1.  **Environment Variables**: Use your `pk_test_...` and `sk_test_...` keys in the Cloud Run environment variables.
2.  **Allowed Domains**:
    *   After deploying, copy your Cloud Run URL (e.g., `https://newsletter-app-xyz.a.run.app`).
    *   Go to **Clerk Dashboard > Configure > Domains**.
    *   Add your Cloud Run URL to the allowed/satellite domains list to prevent CORS/Origin errors.
3.  **Note**: Your app will display a "Development Mode" banner.
