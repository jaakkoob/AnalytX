
# 📊 X ANALYTICS PLUS

```
██╗  ██╗     █████╗ ███╗   ██╗ █████╗ ██╗  ██╗   ██╗████████╗██╗ ██████╗███████╗ 
╚██╗██╔╝    ██╔══██╗████╗  ██║██╔══██╗██║  ╚██╗ ██╔╝╚══██╔══╝██║██╔════╝██╔════╝ 
 ╚███╔╝     ███████║██╔██╗ ██║███████║██║   ╚████╔╝    ██║   ██║██║     ███████╗ 
 ██╔██╗     ██╔══██║██║╚██╗██║██╔══██║██║    ╚██╔╝     ██║   ██║██║     ╚════██║ 
██╔╝ ██╗    ██║  ██║██║ ╚████║██║  ██║███████╗██║      ██║   ██║╚██████╗███████║ 
╚═╝  ╚═╝    ╚═╝  ╚═╝╚═╝  ╚═══╝╚═╝  ╚═╝╚══════╝╚═╝      ╚═╝   ╚═╝ ╚═════╝╚══════╝ 
                                                                                                                             
```

## 🌟 Overview

X Analytics Plus is a privacy-first analytics platform that extracts CSV data from X/Twitter Analytics to provides insightful tips to help you improve your posts. 

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                        Chrome Extension                             │
│  ┌─────────────┐    ┌──────────────┐    ┌────────────────────┐      │
│  │  Popup UI   │───▶│Service Worker│───▶│  Content Scripts   │      │
│  │             │    │              │    │                    │      │
│  │ • Auth      │    │ • Orchestrate│    │ • React Fiber      │      │
│  │ • Progress  │    │ • JWT Token  │    │ • CSV Extraction   │      │
│  │ • Status    │    │ • Parallel   │    │ • Direct Upload    │      │
│  └─────────────┘    └──────────────┘    └────────────────────┘      │
└─────────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────┐
│                          Convex Backend                             │
│  ┌─────────────┐    ┌──────────────┐    ┌────────────────────┐      │
│  │   Storage   │◀───│  Mutations   │───▶│     Actions        │      │
│  │             │    │              │    │                    │      │
│  │ • CSV Files │    │ • Auth Check │    │ • CSV Parsing      │      │
│  │ • Temp Hold │    │ • Rate Limit │    │ • Batch Process    │      │
│  │ • Auto Clean│    │ • Dedupe     │    │ • Username Detect  │      │
│  └─────────────┘    └──────────────┘    └────────────────────┘      │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐    │
│  │                      Database Tables                        │    │
│  │  • users        • posts          • analyticsPatterns        │    │
│  │  • files        • dailyMetrics   • analyticsTopics          │    │
│  │  • rateLimits   • computedAnalytics  *to work on            │    │
│  └─────────────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────┐
│                         Next.js Frontend                            │
│  ┌─────────────┐    ┌──────────────┐    ┌────────────────────┐      │
│  │  Clerk Auth │    │  Dashboard   │    │   Real-time Data   │      │
│  │             │    │              │    │                    │      │
│  │ • SSO       │    │ • File List  │    │ • Live Updates     │      │
│  │ • MFA       │    │ • Analytics  │    │ • Progress Track   │      │
│  │ • Sessions  │    │ • Metrics    │    │ • Error Display    │      │
│  └─────────────┘    └──────────────┘    └────────────────────┘      │
└─────────────────────────────────────────────────────────────────────┘
```

## ✨ Features

### 🔐 Security First

| Feature                      | Description                                                                                                             |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **Zero Downloads**           | CSV files never touch your disk - direct memory transfer                                                                |
| **Account Linking**          | First upload permanently links your account to specific @username (Need to prevent bug if user change their X username) |
| **Row-Level Security**       | Users only see their own data via Convex RLS                                                                            |
| **Rate Limiting**            | 50 uploads/day per user (configurable)                                                                                  |
| **CSP Headers**              | Strict Content Security Policy on all pages                                                                             |
| **CSV Injection Protection** | Sanitizes cells starting with `=`, `+`, `-`, `@`                                                                        |

### ⚡ Performance

| Feature                 | Description                                                   |
| ----------------------- | ------------------------------------------------------------- |
| **Parallel Processing** | All 3 CSV types extracted simultaneously                      |
| **Smart Waiting**       | Dynamic page ready detection instead of fixed delays          |
| **Batch Operations**    | Processes 200 rows at a time to prevent timeouts *to improve* |
| **Deduplication**       | SHA-256 hashing prevents duplicate uploads                    |
| **Real-time Updates**   | Live progress tracking via Convex subscriptions               |

### 📊 Analytics Types

|CSV Type|Data Extracted|
|---|---|
|**Overview**|Daily metrics, engagement rates, follower growth|
|**Content**|Individual tweet performance, impressions, engagement|
|**Video**|Video-specific metrics, watch time, completion rates|

## 🚀 Getting Started

### Prerequisites

- Node.js 18+
- Chrome browser
- Convex account
- Clerk account
- X/Twitter account with premium (to access analytics)

### Installation

1. **Clone the repository**
    
    ```bash
    git clone https://github.com/yourusername/x-analytics-plus.git
    cd x-analytics-plus
    ```
    
2. **Install dependencies**
    
    ```bash
    npm install
    ```
    
3. **Set up environment variables**
    
    ```bash
    # .env.local
    NEXT_PUBLIC_CONVEX_URL=your_convex_url
    NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=your_clerk_key
    CLERK_SECRET_KEY=your_clerk_secret
    CLERK_WEBHOOK_SECRET=your_webhook_secret
    ```
    
4. **Initialize Convex**
    
    ```bash
    npx convex init
    npx convex deploy
    ```
    
5. **Run the development server**
    
    ```bash
    npm run dev
    ```
    
6. **Load the Chrome extension**
    
    - Open Chrome and navigate to `chrome://extensions/`
    - Enable "Developer mode"
    - Click "Load unpacked" and select the `extension` folder

## 🎯 Usage

### For Users

1. **Sign In**: Click the extension icon and sign in with Clerk
2. **Navigate**: Go to any X.com page
3. **Extract**: Click "Fetch & Upload All CSV"
4. **View**: Check your dashboard for analytics

### For Developers

**Key Extension Files:**

```
extension/
├── manifest.json      # Chrome extension config
├── worker.js         # Service worker orchestration
├── content.js        # Minimal message relay
├── popup.js/html     # User interface
└── convexHelpers.js  # API wrapper
```

**Key Backend Files:**

```
convex/
├── uploads.ts        # File upload & CSV processing
├── posts.ts         # Tweet data management
├── analytics.ts     # Metrics calculation
├── files.ts         # File operations
└── schema.ts        # Database schema
```

## 🛡️ Security Measures

### Data Protection

- **No Local Storage**: CSV data never saved to disk
- **Memory Only**: All processing happens in RAM
- **Auto Cleanup**: Files older than 30 days automatically deleted
- **Encrypted Transport**: All API calls use HTTPS/WSS

### Access Control

- **Username Lock**: Once linked, account only accepts data from that @username
- **JWT Authentication**: Short-lived tokens (50 min expiry)
- **Two-Factor Ready**: Clerk supports MFA/SSO
- **Rate Limited**: Prevents abuse with configurable limits

## 🔧 Configuration

### Rate Limits (convex/uploads.ts)

```typescript
// Current: 50 files/day for testing
if (rateLimit && rateLimit.uploadsToday >= 50n) {
  throw new Error("Daily limit reached (50 files)");
}
```

### Processing Limits (convex/uploads.ts)

```typescript
// Allow up to 3 concurrent files
if (processing.length >= 3) {
  throw new Error("Too many files processing (max 3)");
}
```

### File Size (convex/uploads.ts)

```typescript
// 2MB limit
if (args.size > 2 * 1024 * 1024) {
  throw new Error("File too large (max 2MB)");
}
```

## 📊 Database Schema

```typescript
// Users - Linked to Clerk
users: {
  clerkId: string
  email?: string
  name?: string
  createdAt: number
  xUsername?: string  // Locked after first upload
}

// Files - Upload tracking
files: {
  userId: Id<"users">
  hash: string
  storageId: string
  size: number
  uploadedAt: number
  processingStatus: string
  rowCount?: bigint
  xUsername?: string
}

// Posts - Tweet data
posts: {
  fileId: Id<"files">
  userId: Id<"users">
  tweetId: string
  publishedAt: number
  text: string
  impressions: bigint
  engagement: bigint
  hashtags: string[]
}

// Daily Metrics - Aggregate data
dailyMetrics: {
  userId: Id<"users">
  date: string
  impressions: bigint
  engagements: bigint
  profileVisits: bigint
  followersGained: bigint
  engagementRate: number
}
```

## 🐛 Troubleshooting

### Common Issues

| Issue                    | Solution                                                                                |
| ------------------------ | --------------------------------------------------------------------------------------- |
| "Authentication expired" | Sign out and sign in again *to fix*                                                     |
| "No CSV data extracted"  | Ensure you're on the analytics page and data has loaded                                 |
| "Daily limit reached"    | Wait 24 hours or contact admin to increase limit *diy*                                  |
| "Username mismatch"      | You can only upload data from your linked @username (or create another acc if required) |

## 🙏 Very helpful
- **Convex** - Real-time backend infrastructure (best db)
- **Clerk** - Authentication and user management (ai don't seem to like convex+betterauth sadly)
- **Next.js** - React framework
- **Claude.ai** - The best AI (*literally*, Github integration is fire)

---
