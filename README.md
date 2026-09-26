<div align="center">

# 🤖 AI Client Onboarding Agent
### Autonomous Discovery System for Digital Agencies

<br/>

[![React](https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://react.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178C6?style=for-the-badge&logo=typescript&logoColor=white)](https://typescriptlang.org)
[![DeepSeek AI](https://img.shields.io/badge/DeepSeek_AI-LLM_Core-6366F1?style=for-the-badge)](https://deepseek.com)
[![Netlify Functions](https://img.shields.io/badge/Netlify_Functions-Backend-00C7B7?style=for-the-badge&logo=netlify&logoColor=white)](https://netlify.com/products/functions)
[![ClickUp](https://img.shields.io/badge/ClickUp-Project_Sync-7B68EE?style=for-the-badge&logo=clickup&logoColor=white)](https://clickup.com)
[![Vite](https://img.shields.io/badge/Vite-7.2-646CFF?style=for-the-badge&logo=vite&logoColor=white)](https://vitejs.dev)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](./LICENSE)

<br/>

> *"Your most experienced senior consultant — available 24/7, never fatigued, never off-script."*

A **premium, AI-driven discovery system** that autonomously conducts client onboarding interviews for digital agencies. It adapts its questioning in real-time, identifies project risks, generates budget estimates, produces strategic roadmaps, and syncs everything directly to ClickUp — without a human in the loop.

[**🌐 Live Demo**](https://ai-client-onboarding-agent.netlify.app) · [**✨ Features**](#-key-features) · [**🏗️ Architecture**](#-architecture) · [**🚀 Setup**](#-installation--local-setup)

---

</div>

## 📌 The Problem

Every digital agency loses hours — and clients — to a broken discovery process:

- **Manual interviews** are time-consuming and inconsistent across team members
- **Generic intake forms** miss the nuance that reveals real project risks
- **Risk identification** is an afterthought, not built into the conversation
- **Budget discussions** happen too late, after significant team time is invested
- **Discovery notes** live in scattered docs and never make it into project management tools

**This agent replaces the manual discovery call with a structured, adaptive AI interview — and auto-generates everything that comes after it.**

---

## ✨ Key Features

### 🎯 Adaptive Service Discovery
Three service-specific onboarding flows, each tailored to its domain:

| Service | Questions | Focus Areas |
|---|---|---|
| 🌐 **Website Development** | 25+ questions | Site type, CMS, design system, SEO baseline, integrations, content readiness |
| 🎨 **Branding & Identity** | 20+ questions | Brand personality, visual direction, competitor landscape, deliverable scope |
| ⚡ **Business Automation** | 25+ questions | Workflow pain points, current toolstack, integration targets, data volume |

Each flow is defined as a typed configuration in `src/data/` — making it trivially easy to add new service types or modify existing question sets.

**Conditional Logic**: Questions adapt dynamically based on previous answers. Selecting "E-commerce store" as the website type unlocks a cascade of follow-up questions about payment gateways, inventory systems, and SKU volume that wouldn't appear for a portfolio site.

### 🧠 Agentic AI Strategy Engine
Two distinct AI layers work together to produce the final project intelligence:

**1. AI Agency Brain** (`aiAgencyBrain.ts`) — Local, rule-based strategic analysis:
- Runs a **Confidence Score algorithm** (0–100) based on answer completeness and project clarity
- Generates a **Project Readiness Label** (`Project Ready` / `Ready with Caveats` / `Requires Workshop`)
- Produces service-specific **Strategic Insights** (e.g., "SEO Retention Protocol" for redesigns, "Scalability Audit" for automation projects)
- Suggests contextual **Add-ons & Upsells** based on the client's situation

**2. DeepSeek AI** (`deepSeekService.ts`) — Full LLM analysis via secure Netlify Functions:
- **Project Analysis**: Deep strategic review of the full discovery session context
- **Cost Estimation**: Generates a `minBudget`/`maxBudget` range with `estimatedHours` and a `rationale` — returned as structured JSON
- **AI Strategist Chat**: A live conversational interface where the agency team can interrogate the discovery data — *"What are the biggest risks in this project?"*, *"What should our proposal prioritize?"*

### 🛡️ Automated Risk Detection
The `summaryGenerator.ts` runs a **risk detection pass** over every completed session:

| Risk Category | Trigger | Severity |
|---|---|---|
| **Timeline Risk** | Client selects "ASAP (within 2–4 weeks)" | 🔴 High |
| **Scope Risk** | Content not ready or only partially ready | 🟡 Medium |
| **Clarity Risk** | Client identified specific unresolved challenges | 🟢 Low |

Each risk includes a human-readable `description` and an actionable `recommendation` — surfaced directly in the Summary View.

### 🔗 Production-Ready Integrations

- **ClickUp Sync** (`clickUpService.ts` + `netlify/functions/clickup-sync.js`): Pushes the complete discovery summary to a ClickUp list as a fully formatted Markdown task — including all answers, identified risks, and next steps
- **Save & Resume** (`localStorage`): Sessions are persisted locally. Clients can close the browser mid-interview and pick up exactly where they left off
- **Secure Backend Architecture**: API keys for DeepSeek and ClickUp never touch the browser. All sensitive calls are proxied through **Netlify Functions** (`deepseek-agent.js`, `clickup-sync.js`)

### 🎨 Premium Interface
- **Cinematic dark UI** with entrance animations on every screen transition
- **Glassmorphism design system** with custom CSS tokens — no UI framework dependency, pure Vanilla CSS
- **Dynamic cursor-glow effect** and precision hover interactions throughout
- **Canvas Confetti** celebration on session completion
- **Fully responsive** — optimized for smartphones, tablets, and desktops

---

## 🏗️ Architecture

### System Overview

```
┌──────────────────────────────────────────────────────────────────────┐
│                          Client Browser                              │
│                                                                      │
│  ┌─────────────────────────────────────────────────────────────┐     │
│  │                    React Application                        │     │
│  │                                                             │     │
│  │  ServiceSelector → OnboardingWizard → SummaryView          │     │
│  │       │                  │                  │              │     │
│  │       │            QuestionRenderer    AI Strategist       │     │
│  │       │            (conditional       Chat Interface       │     │
│  │       │             question flow)                         │     │
│  │       ▼                  ▼                  ▼              │     │
│  │  ┌──────────┐   ┌──────────────┐   ┌───────────────────┐  │     │
│  │  │  src/    │   │  src/data/   │   │   src/utils/      │  │     │
│  │  │  types/  │   │ websiteFlow  │   │  aiAgencyBrain.ts │  │     │
│  │  │  (shared │   │ brandingFlow │   │  summaryGenerator │  │     │
│  │  │  schemas)│   │ autoFlow     │   │  emailService.ts  │  │     │
│  │  └──────────┘   └──────────────┘   └───────────────────┘  │     │
│  └─────────────────────────────────────────────────────────────┘     │
│                          │                                           │
│               Secure API Calls (fetch)                               │
└──────────────────────────┼───────────────────────────────────────────┘
                           │
              ┌────────────┴────────────┐
              ▼                         ▼
┌─────────────────────────┐  ┌─────────────────────────┐
│  Netlify Function:      │  │  Netlify Function:      │
│  deepseek-agent.js      │  │  clickup-sync.js        │
│                         │  │                         │
│  • Project Analysis     │  │  • Push full discovery  │
│  • Cost Estimation      │  │    to ClickUp task      │
│  • Strategist Chat      │  │  • Format as Markdown   │
│                         │  │                         │
│  → DeepSeek AI API      │  │  → ClickUp REST API     │
└─────────────────────────┘  └─────────────────────────┘
```

### Component Responsibility Map

| Component | File | Responsibility |
|---|---|---|
| **Service Selector** | `ServiceSelector.tsx` | Landing screen — presents the 3 service cards, initiates the onboarding session |
| **Onboarding Wizard** | `OnboardingWizard.tsx` | Drives the multi-step form: renders sections, manages navigation, saves answers to state |
| **Question Renderer** | `QuestionRenderer.tsx` | Renders individual question types (`text`, `textarea`, `select`, `multiselect`, `radio`, `checkbox`) with conditional visibility logic |
| **Summary View** | `SummaryView.tsx` | Full post-interview results panel — answers, risks, AI analysis, cost estimate, ClickUp sync, and AI Strategist chat |
| **ClickUp Settings** | `ClickUpSettings.tsx` | In-app configuration UI for entering ClickUp API credentials |

### Intelligence Layer Map

| Module | Type | Responsibility |
|---|---|---|
| `aiAgencyBrain.ts` | Local logic | Confidence scoring, readiness labeling, strategic insight generation, add-on suggestions |
| `summaryGenerator.ts` | Local logic | Transforms raw session answers into a typed `OnboardingSummary` with identified risks |
| `deepSeekService.ts` | API service | Proxies 3 LLM call types to Netlify Functions: `analysis`, `estimation`, `chat` |
| `deepseek-agent.js` | Netlify Function | Secure server-side handler that calls the DeepSeek API with the appropriate system prompt per call type |
| `clickup-sync.js` | Netlify Function | Pushes formatted project data to ClickUp via their REST API |

---

## 🚀 Installation & Local Setup

### Prerequisites

- **Node.js** `18+` and **npm** `9+`
- A **DeepSeek API Key** (for AI analysis features) — [get one here](https://platform.deepseek.com)
- A **ClickUp API Key** and **List ID** (for project sync) — optional for local dev

---

### Step 1 — Clone & Navigate

```bash
git clone https://github.com/Ismail-2001/AI-Client-Onboarding-System.git
cd AI-Client-Onboarding-System/client-onboarding
```

### Step 2 — Install Dependencies

```bash
npm install
```

### Step 3 — Configure Environment

```bash
cp .env.example .env
```

Edit `.env`:

```env
# DeepSeek AI — powers strategic analysis, cost estimation, and the AI chat
VITE_DEEPSEEK_API_KEY=your_deepseek_api_key_here

# ClickUp — for syncing discovery summaries to your project management tool
VITE_CLICKUP_API_KEY=your_clickup_api_key_here
VITE_CLICKUP_LIST_ID=your_clickup_list_id_here
```

> **Note on API Security**: In production (Netlify), these keys are stored as server-side environment variables and accessed only through Netlify Functions — they never appear in the browser bundle. For local development, Vite exposes them via `VITE_` prefix but limits them to the dev server.

### Step 4 — Run Locally

```bash
npm run dev
```

The app will be running at **`http://localhost:5173`**.

> **ClickUp sync in local dev**: To test Netlify Functions locally, install the Netlify CLI (`npm i -g netlify-cli`) and run `netlify dev` instead of `npm run dev`.

---

## 💡 Usage Walkthrough

### The Onboarding Flow

```
1. SERVICE SELECTION
   Client picks a service card:
   🌐 Website Development | 🎨 Branding & Identity | ⚡ Business Automation

        ↓

2. ADAPTIVE INTERVIEW (OnboardingWizard)
   Multi-section question form with conditional logic.
   Progress bar shows completion.
   Answers auto-save to localStorage — can resume anytime.

        ↓

3. SUMMARY GENERATION (automatic)
   summaryGenerator.ts processes all answers and:
   • Identifies risks (timeline, scope, clarity)
   • Extracts goals and constraints
   • Structures everything into OnboardingSummary

        ↓

4. AI STRATEGIC ANALYSIS (SummaryView)
   aiAgencyBrain.ts runs locally to compute:
   • Confidence Score (0–100)
   • Project Readiness Label
   • Strategic Insights and upsell suggestions

   DeepSeek AI (via Netlify Function) then generates:
   • Full narrative project analysis
   • Budget range + estimated hours (structured JSON)

        ↓

5. EXPORT & SYNC
   • Push to ClickUp as a formatted Markdown task
   • Chat with the AI Strategist for ad-hoc questions
   • Team reviews the complete brief before client call
```

### Adding a New Service Type

Service flows are simple TypeScript configuration objects in `src/data/`. To add a new service (e.g., "SEO Consulting"):

```typescript
// src/data/seoFlow.ts
export const seoFlow: ServiceFlow = {
  serviceName: 'SEO Consulting',
  serviceType: 'seo',
  sections: [
    {
      id: 'discovery',
      title: 'Current SEO Baseline',
      questions: [
        {
          id: 'domain-age',
          type: 'text',
          label: 'How old is your domain?',
          required: true,
        },
        {
          id: 'primary-goal',
          type: 'select',
          label: 'Primary SEO goal?',
          options: ['Increase organic traffic', 'Rank for specific keywords', 'Fix technical issues'],
          required: true,
        }
      ]
    }
  ]
};
```

Then register it in `src/data/index.ts` — and the wizard renders it automatically.

---

## 📂 Project Structure

```text
AI-Client-Onboarding-System/
│
├── client-onboarding/              # Main application directory
│   │
│   ├── netlify/
│   │   └── functions/
│   │       ├── deepseek-agent.js   # Secure DeepSeek API proxy (analysis, estimation, chat)
│   │       └── clickup-sync.js     # ClickUp REST API integration
│   │
│   ├── src/
│   │   ├── components/
│   │   │   ├── ServiceSelector.tsx      # Landing screen with 3 service cards
│   │   │   ├── OnboardingWizard.tsx     # Multi-step interview engine
│   │   │   ├── QuestionRenderer.tsx     # Renders all input types with conditional logic
│   │   │   ├── SummaryView.tsx          # Full results + AI analysis + ClickUp sync
│   │   │   ├── ClickUpSettings.tsx      # In-app ClickUp config UI
│   │   │   └── FormInputs/              # Reusable typed form input components
│   │   │
│   │   ├── data/
│   │   │   ├── websiteFlow.ts           # 25+ website development questions
│   │   │   ├── brandingFlow.ts          # 20+ branding & identity questions
│   │   │   ├── automationFlow.ts        # 25+ business automation questions
│   │   │   └── index.ts                 # Service flow registry
│   │   │
│   │   ├── services/
│   │   │   ├── deepSeekService.ts       # DeepSeek API client (analysis, estimation, chat)
│   │   │   └── clickUpService.ts        # ClickUp task creation client
│   │   │
│   │   ├── utils/
│   │   │   ├── aiAgencyBrain.ts         # Local confidence scoring & strategic insights
│   │   │   ├── summaryGenerator.ts      # Session-to-summary transformer + risk detection
│   │   │   └── emailService.ts          # Email notification utilities
│   │   │
│   │   ├── types/
│   │   │   └── index.ts                 # Shared TypeScript interfaces
│   │   │
│   │   ├── App.tsx                      # Root component and view state machine
│   │   └── index.css                    # Complete design system (tokens, glassmorphism)
│   │
│   ├── package.json
│   ├── vite.config.ts
│   └── netlify.toml
│
├── QUICK_START.md                  # Developer onboarding guide
├── PROGRESS.md                     # Build log and milestone tracking
└── project_requirements.md         # Original project specification
```

---

## 🌍 Deployment

### Deploy to Netlify (Recommended)

The project is pre-configured for Netlify with `netlify.toml` — Netlify automatically detects and deploys the serverless functions in `netlify/functions/`.

**Via Netlify UI:**
1. Push your fork to GitHub
2. Log in to [Netlify](https://app.netlify.com) → **"Add new site"** → **"Import from GitHub"**
3. Set the **Base directory** to `client-onboarding`
4. Build settings auto-detected from `netlify.toml`:
   - **Build Command**: `npm run build`
   - **Publish Directory**: `dist`
   - **Functions Directory**: `netlify/functions` *(auto-detected)*
5. Add **Environment Variables** in **Site Settings → Environment Variables**:

| Variable | Description |
|---|---|
| `VITE_DEEPSEEK_API_KEY` | Your DeepSeek API key |
| `VITE_CLICKUP_API_KEY` | Your ClickUp personal API token |
| `VITE_CLICKUP_LIST_ID` | The ClickUp List ID to push tasks to |

6. Click **Deploy Site** ✅

**Via Netlify CLI:**
```bash
npm install -g netlify-cli
netlify login
cd client-onboarding
npm run build
netlify deploy --prod --dir=dist
```

---

## 🗺️ Roadmap

### ✅ Phase 1 — Foundation (Complete)
- [x] Three adaptive service discovery flows (Website, Branding, Automation)
- [x] Conditional question rendering engine
- [x] `localStorage` save & resume for in-progress sessions
- [x] Automated risk detection (Timeline, Scope, Clarity)
- [x] Local AI Agency Brain with confidence scoring and strategic insights
- [x] DeepSeek AI integration via secure Netlify Functions
- [x] DeepSeek cost estimation with structured JSON output
- [x] AI Strategist chat in the Summary View
- [x] ClickUp task sync via serverless backend
- [x] Canvas Confetti completion animation
- [x] Full glassmorphism design system in Vanilla CSS
- [x] Netlify deployment with CI/CD

### 🔨 Phase 2 — Agency Intelligence (Next)
- [ ] **PDF Export**: Generate a professional discovery brief as a branded PDF
- [ ] **Email Notifications**: Auto-send summary email to agency team on completion (`emailService.ts` scaffolded)
- [ ] **Proposal Generator**: Use the discovery summary to draft an initial project proposal outline
- [ ] **Multi-language Support**: Localized question sets for international clients

### 📋 Phase 3 — Platform Expansion (Planned)
- [ ] **Admin Dashboard**: Agency-side view of all completed onboarding sessions
- [ ] **Notion Sync**: Alternative to ClickUp for Notion-based agencies
- [ ] **Zapier Webhook**: Push session data to any downstream tool via Zapier
- [ ] **Client Portal**: Shareable, password-protected links for clients to complete discovery async

### 🔭 Phase 4 — Full Autonomy (Vision)
- [ ] **Real-time Adaptive Questioning**: LLM dynamically generates follow-up questions based on previous answers, replacing the static flow entirely
- [ ] **Voice Mode**: Conduct discovery sessions via voice using the Web Speech API
- [ ] **White-Label Mode**: Configurable theming and branding for agencies to deploy as their own

---

## 🤝 Contributing

Contributions are welcome! The easiest places to contribute:

- **Adding questions** to an existing service flow in `src/data/`
- **Creating a new service flow** (e.g., SEO, Video Production)
- **Improving risk detection** logic in `summaryGenerator.ts`
- **Adding new strategic insight rules** to `aiAgencyBrain.ts`

To contribute:

1. **Fork** the repository
2. **Create** a feature branch: `git checkout -b feature/your-feature`
3. **Commit** with [Conventional Commits](https://www.conventionalcommits.org/): `git commit -m "feat: add SEO service flow"`
4. **Push** and **open a Pull Request** against `main`

---

## 📄 License

Distributed under the **MIT License**. See [`LICENSE`](./LICENSE) for details.

---

<div align="center">

**Empowering agencies to sell smarter and onboard faster with AI.**

*If this system changed how you think about client onboarding, consider starring ⭐ the repo.*

[![GitHub Stars](https://img.shields.io/github/stars/Ismail-2001/AI-Client-Onboarding-System?style=social)](https://github.com/Ismail-2001/AI-Client-Onboarding-System)

Built with ❤️ by [Ismail Sajid](https://github.com/Ismail-2001)

</div>

## Attribution
This repository is an unmodified copy of [Ismail-2001/AI-Client-Onboarding-System](https://github.com/Ismail-2001/AI-Client-Onboarding-System), imported on 2026-09-23. No code changes have been made yet.
The code is licensed under the MIT License. The LICENSE file and its copyright notice ("Copyright (c) 2026") are preserved unchanged.
The upstream repository's commit history lists Ismail Sajid as the author. That history was not carried over into this import; see the upstream repository for it.
Imported and maintained by [Sami123d](https://github.com/Sami123d). Any future changes will be listed under "Changes in this repository" below.
