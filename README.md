<div align="center">

### AI-Powered Customer Support Copilot

<p>
  <img src="https://img.shields.io/badge/Next.js-14+-000000?style=for-the-badge&logo=nextdotjs&logoColor=white" />
  <img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" />
  <img src="https://img.shields.io/badge/Python-3.11+-3776AB?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" />
  <img src="https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white" />
  <img src="https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white" />
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" />
  <img src="https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white" />
</p>

<p>
  <img src="https://img.shields.io/badge/status-MVP-0A66C2?style=flat-square" />
  <img src="https://img.shields.io/badge/license-MIT-green?style=flat-square" />
  <img src="https://img.shields.io/badge/PRs-welcome-brightgreen?style=flat-square" />
</p>

**An enterprise-grade AI customer support platform with RAG-based chatbot, smart ticketing, knowledge base management, and agent copilot — built for speed, scale, and professional deployment.**

[Getting Started](#getting-started) · [Features](#features) · [API Docs](#api-overview) · [Documentation](#documentation)

</div>

---

## Features

| | Feature | Description |
|--|---------|-------------|
| **AI Chatbot** | RAG-Powered Chat | Streamed AI responses grounded in your knowledge base via WebSocket |
| **Ticketing** | Smart Tickets | Auto ticket creation, priority levels, agent assignment, SLA tracking |
| **Knowledge Base** | Doc Ingestion | Upload PDFs/docs — async pipeline: parse → chunk → embed → store |
| **Agent Copilot** | AI Assist | Suggested replies, ticket summarization, KB retrieval for agents |
| **Admin Panel** | Full Control | AI config, KB management, agent management, system monitoring |
| **Analytics** | Dashboard | Ticket counts, resolution time, open/closed metrics |
| **Auth & RBAC** | Secure Access | Role-based access for admin, agent, and customer via Supabase Auth |

---

## Architecture

```
┌─────────────────────────────────────────────────────┐
│                     Clients                          │
│         Admin Web          User Web (Chat)           │
└──────────────┬──────────────────────┬───────────────┘
               │                      │
               ▼                      ▼
        ┌─────────────────────────────────┐
        │         Next.js Frontend        │
        │   /admin/*        /(user)/*     │
        └──────────────┬──────────────────┘
                       │ REST + WebSocket
                       ▼
        ┌─────────────────────────────────┐
        │         FastAPI Backend          │
        │  Routes → Services → Repos      │
        └──────┬────────────┬─────────────┘
               │            │
       ┌───────▼───┐   ┌────▼──────┐
       │ Supabase  │   │   Redis   │
       │  Postgres │   │  Cache /  │
       │ pgvector  │   │  Queues   │
       └───────────┘   └───────────┘
               │
       ┌───────▼───────┐
       │  Euron EURI   │
       │  AI  API      │
       └───────────────┘
```

---

## Tech Stack

<table>
  <tr>
    <th>Layer</th>
    <th>Technology</th>
    <th>Purpose</th>
  </tr>
  <tr>
    <td>Frontend</td>
    <td>Next.js 14+, TypeScript, Tailwind CSS</td>
    <td>Admin panel + customer-facing UI</td>
  </tr>
  <tr>
    <td>Backend</td>
    <td>Python 3.11+, FastAPI, Pydantic v2</td>
    <td>REST API + WebSocket server</td>
  </tr>
  <tr>
    <td>Database</td>
    <td>Supabase (PostgreSQL + pgvector)</td>
    <td>Data storage + vector similarity search</td>
  </tr>
  <tr>
    <td>Cache / Queue</td>
    <td>Redis</td>
    <td>Caching, rate limiting, job queues</td>
  </tr>
  <tr>
    <td>AI</td>
    <td>Euron EURI API (OpenAI-compatible)</td>
    <td>Chat completions + embeddings</td>
  </tr>
  <tr>
    <td>Auth</td>
    <td>Supabase Auth + JWT</td>
    <td>Authentication + RBAC</td>
  </tr>
  <tr>
    <td>Storage</td>
    <td>AWS S3</td>
    <td>Document file uploads</td>
  </tr>
  <tr>
    <td>Deployment</td>
    <td>Docker, AWS ECS Fargate + ALB</td>
    <td>Containerized cloud deployment</td>
  </tr>
</table>

---

## Project Structure

```
euron/
├── backend/                        # FastAPI backend
│   ├── app/
│   │   ├── main.py                 # App entry point
│   │   ├── core/                   # Config, security, middleware
│   │   ├── routes/                 # Thin API route handlers
│   │   ├── services/               # Business logic layer
│   │   ├── repositories/           # Database access layer
│   │   ├── schemas/                # Pydantic request/response schemas
│   │   ├── models/                 # SQLAlchemy / DB models
│   │   ├── integrations/           # OpenAI, Supabase, Redis clients
│   │   └── workers/                # Async doc ingestion worker
│   ├── .env                        # Backend secrets (not committed)
│   ├── requirements.txt
│   └── Dockerfile
│
├── frontend/                       # Next.js frontend
│   ├── app/
│   │   ├── (auth)/                 # Login, signup pages
│   │   ├── (user)/                 # Chat, tickets, help center
│   │   └── (admin)/                # Dashboard, KB, agents, settings
│   ├── components/                 # Reusable UI components
│   ├── hooks/                      # Custom React hooks
│   ├── lib/                        # API client, utils, constants
│   ├── services/                   # Frontend service layer
│   ├── types/                      # Shared TypeScript types
│   ├── .env.local                  # Frontend secrets (not committed)
│   └── Dockerfile
│
├── docs/                           # PRD, Architecture, API spec, DB schema
├── docker-compose.yml
└── CLAUDE.md
```

---

## Getting Started

### Prerequisites

- **Node.js** 18+
- **Python** 3.11+
- **Docker** & Docker Compose
- **Supabase** account → [supabase.com](https://supabase.com)
- **Redis** (local or managed)

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/euron-ai-support.git
cd euron-ai-support
```

### 2. Set up environment variables

```bash
# Backend
cp backend/.env.example backend/.env
# Open backend/.env and fill in your credentials
```

```bash
# Frontend — edit frontend/.env.local
NEXT_PUBLIC_API_URL=http://localhost:8000/api/v1
NEXT_PUBLIC_WS_URL=ws://localhost:8000
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
```

<details>
<summary>Required environment variables</summary>

| Variable | Description |
|----------|-------------|
| `SUPABASE_URL` | Your Supabase project URL |
| `SUPABASE_ANON_KEY` | Supabase anon/public key |
| `SUPABASE_SERVICE_ROLE_KEY` | Supabase service role key |
| `DATABASE_URL` | PostgreSQL connection string |
| `EURI_API_KEY` | Euron EURI API key |
| `EURI_BASE_URL` | `https://api.euron.one/api/v1/euri` |
| `REDIS_URL` | Redis connection URL |
| `JWT_SECRET` | Secret for JWT signing (min 32 chars) |
| `AWS_S3_BUCKET` | S3 bucket name for file uploads |
| `AWS_ACCESS_KEY_ID` | AWS access key |
| `AWS_SECRET_ACCESS_KEY` | AWS secret key |

</details>

### 3. Run database migrations

```bash
supabase db push
```

### 4. Start the servers

**Option A — Docker Compose (recommended)**

```bash
docker-compose up --build
```

**Option B — Manual (3 terminals)**

```bash
# Terminal 1 — Redis
docker run -p 6379:6379 redis:alpine

# Terminal 2 — Backend
cd backend && pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000

# Terminal 3 — Frontend
cd frontend && npm install && npm run dev
```

### 5. Open the app

| Interface | URL |
|-----------|-----|
| Customer Chat | http://localhost:3000 |
| Admin Panel | http://localhost:3000/admin |
| Backend API | http://localhost:8000 |
| Interactive API Docs | http://localhost:8000/docs |

---

## API Overview

> Base URL: `/api/v1`

<details>
<summary>View all endpoints</summary>

| Module | Method | Endpoint | Description |
|--------|--------|----------|-------------|
| **Auth** | POST | `/auth/login` | Email/password login |
| | POST | `/auth/signup` | Register new user |
| | POST | `/auth/refresh` | Refresh JWT token |
| **Chat** | WS | `/ws/chat/{id}` | Live chat with streamed AI |
| | GET | `/chat/conversations/{id}/history` | Chat history |
| **Tickets** | GET | `/tickets` | List tickets (paginated) |
| | POST | `/tickets` | Create ticket |
| | PATCH | `/tickets/{id}` | Update status / assignee |
| | POST | `/tickets/{id}/messages` | Add message to ticket |
| **Knowledge** | GET | `/knowledge/documents` | List documents |
| | POST | `/knowledge/documents` | Upload + trigger ingestion |
| | DELETE | `/knowledge/documents/{id}` | Remove document |
| **Copilot** | POST | `/copilot/suggest-reply` | AI suggested reply |
| | POST | `/copilot/summarize` | Ticket/conversation summary |
| | POST | `/copilot/retrieve-kb` | KB snippet retrieval |
| **Admin** | GET | `/admin/agents` | List agents |
| | PATCH | `/admin/config/ai` | Update AI config |
| **Analytics** | GET | `/analytics/dashboard` | Aggregated metrics |
| **Health** | GET | `/health` | Service health check |

</details>

Full spec: [docs/API_SPEC.md](docs/API_SPEC.md)

---

## Running Tests

```bash
# Backend
cd backend && pytest

# Frontend
cd frontend && npm test
```

---

## Deployment

Deployed on **AWS** using ECS Fargate, ALB, and S3.

See [docs/DEPLOYMENT.md](docs/DEPLOYMENT.md) for the complete deployment guide.

---

## Documentation

| Document | Description |
|----------|-------------|
| [PRD.md](docs/PRD.md) | Product Requirements Document |
| [ARCHITECTURE.md](docs/ARCHITECTURE.md) | System architecture & design decisions |
| [API_SPEC.md](docs/API_SPEC.md) | Full REST + WebSocket API specification |
| [DB_SCHEMA.md](docs/DB_SCHEMA.md) | Database schema & ER diagram |
| [DEPLOYMENT.md](docs/DEPLOYMENT.md) | AWS deployment guide |

---

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m 'Add your feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a Pull Request

---

<div align="center">

**Built with the Euron EURI AI API**

<img src="https://img.shields.io/badge/license-MIT-green?style=flat-square" />

</div>
