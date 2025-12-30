# NewsWeave Platform 📰

An AI-powered newsletter generation platform that curates content from RSS feeds and creates professional newsletters using Google's Gemini AI.

## 🚀 Live Demo

[Hosted on google cloud run]
(https://newsletter-ai-app-831884341768.asia-south1.run.app)

## 🧩 Problem Statement
Staying updated with high-quality content across multiple sources is time-consuming.
NewsWeave solves this by aggregating RSS feeds, summarizing articles using AI,
and generating professional newsletters tailored to user preferences.

## 🌟 Features

- **RSS Feed Management**: Add and manage multiple RSS feeds from your favorite sources
- **AI-Powered Content Generation**: Leverage Google's Gemini AI to generate engaging newsletter content
- **Smart Article Curation**: Automatically fetch and organize articles from your RSS feeds
- **Newsletter Customization**: Personalize newsletters with custom branding, tone, and style
- **User Settings**: Configure newsletter preferences including:
  - Newsletter name and description
  - Target audience
  - Brand voice and company information
  - Custom footer and sender details
- **Authentication**: Secure user authentication powered by Clerk
- **Modern UI**: Beautiful, responsive interface built with Next.js 16 and Tailwind CSS
- **Database**: MongoDB with Prisma ORM for efficient data management

## 🚀 Tech Stack

- **Framework**: [Next.js 16](https://nextjs.org/) with React 19
- **Language**: TypeScript
- **Styling**: Tailwind CSS v4
- **Database**: MongoDB with [Prisma](https://www.prisma.io/)
- **Authentication**: [Clerk](https://clerk.com/)
- **AI**: Google Gemini AI via Vercel AI SDK
- **UI Components**: [shadcn/ui](https://ui.shadcn.com/) components
- **Package Manager**: pnpm
- **Code Quality**: Biome (linting & formatting)
- **Deployment**: Google Cloud Run with Docker

## 🏗 Architecture Overview
- Next.js application running serverlessly on Google Cloud Run
- Authentication handled via Clerk
- RSS feeds ingested and normalized on the backend
- Prisma ORM manages database access
- MongoDB Atlas stores users, feeds, and articles
- Gemini AI generates newsletter content
- CI/CD automated using GitHub Actions

## 📋 Prerequisites

- Node.js 20 or higher
- pnpm (recommended) or npm
- MongoDB database (local or cloud)
- Clerk account for authentication
- Google AI API key for Gemini

## 🛠️ Getting Started

### 1. Clone the repository

```bash
git clone <repository-url>
cd Newletter-AI-App
```

### 2. Install dependencies

```bash
pnpm install
```

### 3. Set up environment variables

Create a `.env` file in the root directory with the following variables:

```env
# Database
DATABASE_URL="your-mongodb-connection-string"

# Clerk Authentication
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY="pk_test_..."
CLERK_SECRET_KEY="sk_test_..."

# Google AI
GOOGLE_GENERATIVE_AI_API_KEY="your-gemini-api-key"
```

### 4. Set up the database

```bash
# Generate Prisma Client
pnpm prisma:generate

# Push schema to database
pnpm prisma:push
```

### 5. Run the development server

```bash
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000) to see the application.

## 📦 Available Scripts

- `pnpm dev` - Start development server
- `pnpm build` - Build for production
- `pnpm start` - Start production server
- `pnpm lint` - Run Biome linter
- `pnpm format` - Format code with Biome
- `pnpm prisma:generate` - Generate Prisma Client
- `pnpm prisma:push` - Push schema changes to database
- `pnpm prisma:studio` - Open Prisma Studio

## 🗄️ Database Schema

The application uses the following main models:

- **User**: Stores user information (linked to Clerk)
- **UserSettings**: User preferences for newsletter generation
- **RssFeed**: RSS feed sources
- **RssArticle**: Individual articles from RSS feeds (deduplicated)
- **Newsletter**: Generated newsletters with AI content

## 🎨 Project Structure

```
├── app/                    # Next.js app directory
│   ├── api/               # API routes
│   ├── dashboard/         # Dashboard pages
│   └── page.tsx           # Landing page
├── actions/               # Server actions
│   ├── newsletter.ts      # Newsletter operations
│   ├── rss-feed.ts        # RSS feed management
│   ├── user-settings.ts   # User settings
│   └── ...
├── components/            # React components
│   ├── dashboard/         # Dashboard components
│   ├── landing/           # Landing page components
│   └── ui/                # Reusable UI components
├── lib/                   # Utility functions
├── prisma/                # Prisma schema
├── public/                # Static assets
├── .github/workflows/     # GitHub Actions CI/CD
├── Dockerfile             # Docker configuration
└── cloudbuild.yaml        # Google Cloud Build config
```

## 🐳 Docker

### Build locally

```bash
docker build -t newsletter-app \
  --build-arg NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_... .
```

### Run locally

```bash
docker run -p 3000:3000 --env-file .env newsletter-app
```

## ☁️ Deployment

The application is configured for deployment to Google Cloud Run with automated CI/CD via GitHub Actions.

### Prerequisites

1. Google Cloud Project with billing enabled
2. Required APIs enabled:
   - Cloud Run
   - Cloud Build
   - Artifact Registry

### Service Account Permissions

The deployment service account needs:
- `roles/artifactregistry.writer`
- `roles/cloudbuild.builds.editor`
- `roles/iam.serviceAccountUser`
- `roles/logging.logWriter`
- `roles/run.admin`
- `roles/serviceusage.serviceUsageConsumer`

### GitHub Secrets

Configure the following secrets in your GitHub repository:

- `GCP_PROJECT_ID` - Your GCP project ID
- `GCP_REGION` - Deployment region (e.g., us-central1)
- `GCP_SA_KEY` - Service account JSON key
- `DATABASE_URL` - MongoDB connection string
- `CLERK_SECRET_KEY` - Clerk secret key
- `NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY` - Clerk publishable key
- `GOOGLE_GENERATIVE_AI_API_KEY` - Google AI API key

### Deployment Workflow

The application automatically deploys to Cloud Run when changes are pushed to the `main` branch. See [DEPLOY.md](./DEPLOY.md) for detailed deployment instructions.

## 🧠 Production Challenges Solved
- Debugged Cloud Run runtime failures caused by missing environment variables
- Resolved Prisma P2010 errors due to MongoDB Atlas network access restrictions
- Configured IAM roles and service accounts for Cloud Run deployments
- Implemented secure secret injection using GitHub Actions
- Used Cloud Run logs to debug production-only issues

## 🔒 Security

- Authentication handled by Clerk
- Environment variables for sensitive data
- Service account with minimal required permissions
- MongoDB connection with proper access controls

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 Code Style

This project uses Biome for linting and formatting. Run `pnpm lint` before committing.

## 📄 License

This project is intended for demonstration and learning purposes.

## 🎯 What This Project Demonstrates
- End-to-end cloud deployment experience
- Secure authentication and secret management
- Serverless backend design
- Production debugging and issue resolution
- Real-world CI/CD workflows

## 🙏 Acknowledgments

- [Next.js](https://nextjs.org/)
- [Clerk](https://clerk.com/)
- [Prisma](https://www.prisma.io/)
- [Google Gemini AI](https://ai.google.dev/)
- [shadcn/ui](https://ui.shadcn.com/)
- [Tailwind CSS](https://tailwindcss.com/)

## 📧 Support

For support, please open an issue in the repository.

---

Built with ❤️ using Next.js and Google Gemini AI
