# 📰 NewsWeave - AI-Powered Newsletter Platform

<div align="center">

![Next.js](https://img.shields.io/badge/Next.js-16-black?style=for-the-badge&logo=next.js)
![React](https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?style=for-the-badge&logo=typescript)
![Tailwind CSS](https://img.shields.io/badge/Tailwind-4-38B2AC?style=for-the-badge&logo=tailwind-css)
![Google AI](https://img.shields.io/badge/Google_AI-Gemini-4285F4?style=for-the-badge&logo=google)
![Cloud Run](https://img.shields.io/badge/Cloud_Run-Deployed-4285F4?style=for-the-badge&logo=google-cloud)

An intelligent newsletter generation platform that transforms RSS feeds into professional, AI-curated newsletters using Google Gemini

<p align="center">
  <a href="https://newsletter-ai-app-831884341768.asia-south1.run.app" target="_blank" rel="noopener noreferrer">
    🚀 <strong>Live Demo on Cloud Run</strong>
  </a>
</p>

[Features](#-features) • [Tech Stack](#️-tech-stack) • [Getting Started](#-getting-started) • [Architecture](#️-architecture) • [Deployment](#-deployment)

</div>

---

## 🎯 Overview

An enterprise-grade newsletter platform that revolutionizes content curation with AI-powered automation. Content creators can aggregate RSS feeds, customize newsletter settings, and generate professional newsletters using natural language prompts, while Pro users access unlimited history and advanced customization features.

### Why This Project?

Staying updated with high-quality content across multiple sources is time-consuming. NewsWeave solves this by:
- 📡 **Aggregating** RSS feeds from your favorite sources
- 🤖 **Summarizing** articles using Google's Gemini AI
- ✍️ **Generating** professional newsletters tailored to your preferences
- 💾 **Saving** newsletter history for future reference (Pro feature)

---

## ✨ Features

### 🤖 AI-Powered Generation
- **Natural Language Instructions** - Guide the AI with custom prompts for newsletter generation
- **Smart Content Curation** - Automatically fetch and organize up to 100 articles from RSS feeds
- **Intelligent Summarization** - AI-powered article summaries with context preservation
- **Multi-Source Aggregation** - Combine content from multiple RSS feeds seamlessly
- **Customizable Output** - Generate 500-1000 word newsletters with 10+ main headings
- **Real-time Streaming** - Watch your newsletter being generated in real-time

### 📰 Newsletter Management
- **RSS Feed Manager** - Add, edit, and organize multiple RSS feeds
- **Date Range Selection** - Generate newsletters for specific time periods
- **Article Preview** - Review articles before generating newsletters
- **Newsletter History** - Access past newsletters (Pro feature)
- **Export Options** - Copy markdown or download generated content
- **Template Suggestions** - 5 title and 5 subject line variations per newsletter

### ⚙️ Customization & Settings
- **Brand Voice Configuration** - Define your unique brand voice and tone
- **Target Audience Settings** - Tailor content to specific audiences
- **Custom Disclaimers** - Add required legal or informational text
- **Footer Customization** - Include company information and contact details
- **Sender Information** - Configure sender name and email
- **Newsletter Metadata** - Set newsletter name, description, and tags

### 🔐 Security & Authentication
- **User Authentication** - Clerk-powered auth with social login
- **Pro Plan Management** - Subscription-based feature access
- **Secure API Keys** - Environment-based secret management
- **Role-Based Access** - Free vs Pro user permissions
- **Data Privacy** - User-specific feed and newsletter isolation

---

## 🛠️ Tech Stack

### Frontend
- **[Next.js 16](https://nextjs.org/)** - React framework with App Router
- **[React 19](https://react.dev/)** - UI library with latest features
- **[TypeScript](https://www.typescriptlang.org/)** - Type-safe development
- **[Tailwind CSS v4](https://tailwindcss.com/)** - Utility-first styling
- **[Radix UI](https://www.radix-ui.com/)** - Accessible component primitives
- **[Lucide Icons](https://lucide.dev/)** - Beautiful icon library

### AI & Intelligence
- **[Vercel AI SDK](https://sdk.vercel.ai/)** - AI framework for streaming responses
- **[Google Gemini 2.5 Flash](https://ai.google.dev/)** - Large language model
- **Streaming Generation** - Real-time newsletter creation with progress updates
- **Custom Prompt Engineering** - Optimized prompts for newsletter generation

### Backend & Database
- **[MongoDB Atlas](https://www.mongodb.com/atlas)** - Cloud database
- **[Prisma ORM](https://www.prisma.io/)** - Type-safe database client
- **Server Actions** - Type-safe server mutations
- **RSS Parser** - Custom RSS feed ingestion and normalization

### Payments & Auth
- **[Clerk](https://clerk.com/)** - User authentication and management
- **Pro Plan Integration** - Feature gating based on subscription

### DevOps & Tools
- **[Biome](https://biomejs.dev/)** - Fast linter and formatter
- **[Docker](https://www.docker.com/)** - Containerization
- **[Google Cloud Run](https://cloud.google.com/run)** - Serverless deployment
- **[GitHub Actions](https://github.com/features/actions)** - CI/CD pipeline

---

## 🚀 Getting Started

### Prerequisites
- Node.js 20 or higher
- pnpm (recommended) or npm
- MongoDB database (local or cloud)
- Clerk account for authentication
- Google AI API key for Gemini

### Installation

1. **Clone the repository**
```bash
git clone <repository-url>
cd Newletter-AI-App
```

2. **Install dependencies**
```bash
pnpm install
```

3. **Set up environment variables**

Create a `.env` file in the root directory:

```env
# Database
DATABASE_URL="your-mongodb-connection-string"

# Clerk Authentication
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY="pk_test_..."
CLERK_SECRET_KEY="sk_test_..."

# Google AI
GOOGLE_GENERATIVE_AI_API_KEY="your-gemini-api-key"
```

4. **Set up the database**
```bash
# Generate Prisma Client
pnpm prisma:generate

# Push schema to database
pnpm prisma:push
```

5. **Run the development server**
```bash
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000) to see the application.

### Available Scripts

- `pnpm dev` - Start development server
- `pnpm build` - Build for production
- `pnpm start` - Start production server
- `pnpm lint` - Run Biome linter
- `pnpm format` - Format code with Biome
- `pnpm prisma:generate` - Generate Prisma Client
- `pnpm prisma:push` - Push schema changes to database
- `pnpm prisma:studio` - Open Prisma Studio

---

## 🏗️ Architecture

### System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Client (Browser)                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │ Landing Page │  │  Dashboard   │  │   Settings   │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                   Next.js App Router                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │ Server Pages │  │ API Routes   │  │Server Actions│      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└─────────────────────────────────────────────────────────────┘
         │                    │                    │
         ▼                    ▼                    ▼
┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│   Clerk      │    │  Google AI   │    │   MongoDB    │
│     Auth     │    │    Gemini    │    │   + Prisma   │
└──────────────┘    └──────────────┘    └──────────────┘
```

### Newsletter Generation Flow

1. **Feed Selection** - User selects RSS feeds and date range
2. **Article Fetching** - System retrieves up to 100 articles from feeds
3. **AI Processing** - Gemini AI analyzes articles and user settings
4. **Content Generation** - AI generates newsletter with streaming updates
5. **Preview & Edit** - User reviews generated content
6. **Save & Export** - Newsletter saved to history (Pro users)

### Data Flow

```
RSS Feeds → Parser → Database → AI Prompt Builder → Gemini AI → Newsletter
                         ↓
                   User Settings
                         ↓
                   Custom Instructions
```

---

## 📁 Project Structure

```
├── app/                    # Next.js app directory
│   ├── api/               # API routes
│   │   └── newsletter/    # Newsletter generation endpoints
│   ├── dashboard/         # Dashboard pages
│   │   ├── generate/      # Newsletter generation page
│   │   ├── history/       # Newsletter history
│   │   ├── settings/      # User settings
│   │   ├── pricing/       # Subscription management
│   │   └── account/       # Account settings
│   └── page.tsx           # Landing page
├── actions/               # Server actions
│   ├── newsletter.ts      # Newsletter CRUD operations
│   ├── rss-feed.ts        # RSS feed management
│   ├── rss-article.ts     # Article operations
│   ├── rss-fetch.ts       # RSS feed fetching
│   ├── user-settings.ts   # User settings management
│   └── user.ts            # User operations
├── components/            # React components
│   ├── dashboard/         # Dashboard components
│   │   ├── newsletter-generator.tsx
│   │   ├── rss-feed-manager.tsx
│   │   ├── settings-form.tsx
│   │   └── ...
│   ├── landing/           # Landing page components
│   └── ui/                # Reusable UI components (shadcn/ui)
├── lib/                   # Utility functions
│   ├── newsletter/        # Newsletter utilities
│   │   ├── prompt-builder.ts  # AI prompt construction
│   │   └── types.ts       # Newsletter types
│   ├── rss/              # RSS utilities
│   │   ├── parser.ts      # RSS feed parser
│   │   ├── feed-refresh.ts # Feed caching logic
│   │   └── types.ts       # RSS types
│   ├── auth/             # Authentication helpers
│   └── prisma.ts         # Prisma client
├── prisma/               # Prisma schema
│   └── schema.prisma     # Database models
├── .github/workflows/    # GitHub Actions CI/CD
│   └── deploy.yml        # Cloud Run deployment
├── Dockerfile            # Docker configuration
└── cloudbuild.yaml       # Google Cloud Build config
```

---

## 🤖 AI Newsletter Generation

### How It Works

1. **Article Collection** - Fetches up to 100 articles from selected RSS feeds
2. **Context Building** - Combines articles with user settings and custom instructions
3. **Prompt Engineering** - Constructs optimized prompt for Gemini AI
4. **Streaming Generation** - AI generates newsletter in real-time
5. **Structured Output** - Returns JSON with titles, body, announcements, and metadata

### AI Prompt Structure

```typescript
{
  "context": {
    "dateRange": "Dec 1 - Dec 30, 2025",
    "articleCount": 100,
    "userSettings": {
      "newsletterName": "Tech Weekly",
      "targetAudience": "Software Developers",
      "tone": "Professional yet friendly",
      "brandVoice": "Informative and concise"
    }
  },
  "instructions": "Generate a 500-1000 word newsletter...",
  "articles": [/* 100 articles with summaries */]
}
```

### Generated Output

- **5 Newsletter Titles** - Creative title options
- **5 Subject Lines** - Email-optimized subject lines
- **Newsletter Body** - 500-1000 words with 10+ headings
- **5 Top Announcements** - Brief, punchy highlights
- **Additional Info** - Supplementary notes and trends

---

## 🎨 UI Components

### Design System
- **Color Palette** - Carefully curated color scheme with dark mode support
- **Typography** - Geist Sans and Geist Mono fonts
- **Spacing** - Consistent spacing scale
- **Animations** - Smooth transitions and micro-interactions

### Key Components
- **Newsletter Generator** - Real-time streaming generation interface
- **RSS Feed Manager** - CRUD operations for feeds
- **Settings Form** - Comprehensive settings configuration
- **Newsletter History** - Searchable, filterable history view
- **Pricing Cards** - Subscription plan comparison
- **Page Headers** - Consistent page headers with icons

---

## 🔐 Security Features

### Authentication
- Clerk-powered authentication with social login
- Session management and token refresh
- Protected routes and API endpoints

### Data Protection
- Environment-based secret management
- User-specific data isolation
- Secure MongoDB connections with access controls

### API Security
- Rate limiting on API routes
- Input validation and sanitization
- CORS configuration

---

## 📊 Data Models

### User Schema
```prisma
model User {
  id              String          @id @default(auto()) @map("_id") @db.ObjectId
  clerkId         String          @unique
  email           String
  name            String?
  imageUrl        String?
  createdAt       DateTime        @default(now())
  updatedAt       DateTime        @updatedAt
  settings        UserSettings?
  rssFeeds        RssFeed[]
  newsletters     Newsletter[]
}
```

### RSS Feed Schema
```prisma
model RssFeed {
  id              String          @id @default(auto()) @map("_id") @db.ObjectId
  url             String
  title           String
  description     String?
  lastFetched     DateTime?
  userId          String          @db.ObjectId
  user            User            @relation(fields: [userId], references: [id])
  articles        RssArticle[]
  createdAt       DateTime        @default(now())
  updatedAt       DateTime        @updatedAt
}
```

### Newsletter Schema
```prisma
model Newsletter {
  id                    String    @id @default(auto()) @map("_id") @db.ObjectId
  userId                String    @db.ObjectId
  user                  User      @relation(fields: [userId], references: [id])
  suggestedTitles       String[]
  suggestedSubjectLines String[]
  body                  String
  topAnnouncements      String[]
  additionalInfo        String?
  startDate             DateTime
  endDate               DateTime
  userInput             String?
  feedsUsed             String[]  @db.ObjectId
  createdAt             DateTime  @default(now())
  updatedAt             DateTime  @updatedAt
}
```

---

## 🎬 Demo

### Live Application

**🌐 Cloud Run Deployment:** <a href="https://newsletter-ai-app-831884341768.asia-south1.run.app" target="_blank">https://newsletter-ai-app-831884341768.asia-south1.run.app</a>

**Features to Try:**
- 📰 **RSS Feed Management** - Add your favorite RSS feeds
- 🤖 **AI Newsletter Generation** - Generate newsletters with custom instructions
- ⚙️ **Settings Configuration** - Customize your newsletter preferences
- 📚 **Newsletter History** - View past newsletters (requires Pro plan)
- 🎨 **Dark Mode** - Toggle between light and dark themes

**Test Credentials:**
- Use any email to sign up via Clerk authentication
- Pro features require subscription upgrade

---

## 🚢 Deployment

### Docker Deployment

**Build locally:**
```bash
docker build -t newsletter-app \
  --build-arg NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_... .
```

**Run locally:**
```bash
docker run -p 3000:3000 --env-file .env newsletter-app
```

### Google Cloud Run

**Prerequisites:**
1. Google Cloud Project with billing enabled
2. Required APIs enabled:
   - Cloud Run API
   - Cloud Build API
   - Artifact Registry API

**Service Account Permissions:**
- `roles/run.admin`
- `roles/iam.serviceAccountUser`
- `roles/artifactregistry.writer`
- `roles/cloudbuild.builds.builder`

**Environment Variables Setup:**

Configure the following secrets in GitHub:
- `GCP_PROJECT_ID` - Your GCP project ID
- `GCP_REGION` - Deployment region (e.g., asia-south1)
- `GCP_SA_KEY` - Service account JSON key
- `DATABASE_URL` - MongoDB connection string
- `CLERK_SECRET_KEY` - Clerk secret key
- `NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY` - Clerk publishable key
- `GOOGLE_GENERATIVE_AI_API_KEY` - Google AI API key

**Automated Deployment:**

The application automatically deploys to Cloud Run when changes are pushed to the `main` branch via GitHub Actions.

---

## 🧪 Testing

### Manual Testing Checklist

**Authentication:**
- [ ] User can sign up with email
- [ ] User can sign in with social providers
- [ ] Protected routes redirect to sign-in
- [ ] User can sign out successfully

**RSS Feed Management:**
- [ ] User can add new RSS feeds
- [ ] User can edit feed details
- [ ] User can delete feeds
- [ ] Feeds are validated before saving

**Newsletter Generation:**
- [ ] User can select feeds and date range
- [ ] Articles are fetched correctly
- [ ] AI generates newsletter with streaming
- [ ] Generated content matches user settings
- [ ] Custom instructions are applied

**Settings:**
- [ ] User can update newsletter settings
- [ ] Settings persist across sessions
- [ ] Settings are applied to generation

**History (Pro):**
- [ ] Pro users can view newsletter history
- [ ] Newsletters can be searched/filtered
- [ ] Individual newsletters can be viewed
- [ ] Free users see upgrade prompt

---

## 🎯 Roadmap

### Phase 1: Core Features ✅ (Completed)
- [x] RSS feed management
- [x] AI-powered newsletter generation
- [x] User authentication
- [x] Basic settings configuration
- [x] Cloud Run deployment

### Phase 2: Enhancements 🚧 (In Progress)
- [ ] Email sending integration
- [ ] Newsletter templates
- [ ] Advanced analytics
- [ ] Scheduled generation
- [ ] Multi-language support

### Phase 3: Advanced Features 📋 (Planned)
- [ ] Team collaboration
- [ ] Custom AI models
- [ ] A/B testing for subject lines
- [ ] Subscriber management
- [ ] Newsletter performance tracking

### Phase 4: Scaling 🔮 (Future)
- [ ] Multi-tenant architecture
- [ ] White-label solution
- [ ] API for third-party integrations
- [ ] Mobile app
- [ ] Enterprise features

---

## 🤝 Contributing

We welcome contributions! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Coding Standards

- Use TypeScript for all new code
- Follow the existing code style (enforced by Biome)
- Write meaningful commit messages
- Add comments for complex logic
- Update documentation as needed

---

## 📝 License

This project is intended for demonstration and learning purposes.

---

## 👨‍💻 Author

Built with ❤️ by [Your Name]

---

## 🙏 Acknowledgments

- [Next.js](https://nextjs.org/) - The React Framework
- [Clerk](https://clerk.com/) - Authentication and User Management
- [Prisma](https://www.prisma.io/) - Next-generation ORM
- [Google Gemini AI](https://ai.google.dev/) - AI-powered content generation
- [shadcn/ui](https://ui.shadcn.com/) - Beautiful UI components
- [Tailwind CSS](https://tailwindcss.com/) - Utility-first CSS framework
- [Vercel AI SDK](https://sdk.vercel.ai/) - AI framework for streaming

---

## 📧 Support

For support, please open an issue in the repository or contact [your-email@example.com].

---

<div align="center">

**⭐ Star this repo if you find it helpful!**

Made with ❤️ using Next.js and Google Gemini AI

</div>
