# Technical Stack & Architecture Template

## Document Header

```markdown
# [Project Name] - Technical Stack & Architecture

**Version:** 1.0
**Last Updated:** [Date]
**Status:** Draft | In Review | Approved

---
```

## 1. Architecture Overview

```markdown
## 1. Architecture Overview

### 1.1 System Architecture
[High-level description of the system architecture]

**Architecture Pattern:** [Monolith | Microservices | Serverless | Hybrid]

**Key Characteristics:**
- [Characteristic 1]
- [Characteristic 2]
- [Characteristic 3]

### 1.2 Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         Client Layer                             │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │
│  │   Web App   │  │  Mobile App │  │   Admin     │              │
│  │   (React)   │  │  (Optional) │  │   Panel     │              │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘              │
└─────────┼────────────────┼────────────────┼─────────────────────┘
          │                │                │
          ▼                ▼                ▼
┌─────────────────────────────────────────────────────────────────┐
│                         API Layer                                │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                    API Gateway / Load Balancer           │    │
│  └─────────────────────────────────────────────────────────┘    │
│                              │                                   │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │
│  │    Auth     │  │    Core     │  │   [Other]   │              │
│  │   Service   │  │   Service   │  │   Service   │              │
│  └─────────────┘  └─────────────┘  └─────────────┘              │
└─────────────────────────────────────────────────────────────────┘
                              │
┌─────────────────────────────────────────────────────────────────┐
│                         Data Layer                               │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │
│  │  PostgreSQL │  │    Redis    │  │   Storage   │              │
│  │  (Primary)  │  │   (Cache)   │  │   (Files)   │              │
│  └─────────────┘  └─────────────┘  └─────────────┘              │
└─────────────────────────────────────────────────────────────────┘
```

### 1.3 Design Principles
- **TS-PRINCIPLE-1:** [Principle, e.g., "Separation of concerns"]
- **TS-PRINCIPLE-2:** [Principle, e.g., "API-first design"]
- **TS-PRINCIPLE-3:** [Principle, e.g., "Security by default"]
```

## 2. Frontend Stack

```markdown
## 2. Frontend Stack

### TS-1: Frontend Framework

**Technology:** [React | Vue | Next.js | etc.]
**Version:** [Version number]

**Rationale:**
- [Why this choice]
- [Benefits for this project]

**Key Libraries:**
| Library | Version | Purpose |
|:--------|:--------|:--------|
| [Library 1] | [version] | [purpose] |
| [Library 2] | [version] | [purpose] |
| [Library 3] | [version] | [purpose] |

### TS-2: State Management

**Technology:** [Redux | Zustand | Context | etc.]
**Version:** [Version]

**Rationale:**
- [Why this choice]

**Usage Pattern:**
- [How state is organized]
- [Key state slices]

### TS-3: Styling Approach

**Technology:** [Tailwind CSS | CSS Modules | Styled Components | etc.]
**Version:** [Version]

**Design System:**
- [Color palette approach]
- [Typography scale]
- [Component library: shadcn/ui, MUI, etc.]

### TS-4: Build & Development

**Bundler:** [Vite | Webpack | etc.]
**Package Manager:** [npm | pnpm | yarn]

**Key Scripts:**
```bash
npm run dev      # Development server
npm run build    # Production build
npm run test     # Run tests
npm run lint     # Lint code
```
```

## 3. Backend Stack

```markdown
## 3. Backend Stack

### TS-5: Backend Framework

**Technology:** [Node.js/Express | Python/FastAPI | Go | etc.]
**Version:** [Version]
**Runtime:** [Node.js version | Python version | etc.]

**Rationale:**
- [Why this choice]
- [Team expertise]
- [Performance requirements]

**Project Structure:**
```
src/
├── controllers/    # Request handlers
├── services/       # Business logic
├── models/         # Data models
├── middleware/     # Express middleware
├── routes/         # Route definitions
├── utils/          # Utility functions
└── config/         # Configuration
```

### TS-6: API Design

**Style:** [REST | GraphQL | gRPC]
**Documentation:** [OpenAPI/Swagger | GraphQL Schema]

**Endpoint Conventions:**
- Base URL: `/api/v1`
- Resource naming: plural nouns (`/users`, `/projects`)
- Actions: HTTP verbs (GET, POST, PUT, DELETE)

**Response Format:**
```json
{
  "success": true,
  "data": { },
  "meta": {
    "pagination": { }
  },
  "error": null
}
```

### TS-7: Authentication & Authorization

**Strategy:** [JWT | Session | OAuth2]
**Token Storage:** [httpOnly cookies | localStorage]
**Session Duration:** [Duration]

**OAuth Providers:**
- Google
- GitHub
- [Others]

**Authorization Model:**
- [RBAC | ABAC | Custom]
- [Role definitions]
```

## 4. Database

```markdown
## 4. Database

### TS-8: Primary Database

**Technology:** [PostgreSQL | MySQL | MongoDB | etc.]
**Version:** [Version]
**Hosting:** [Self-hosted | Managed: AWS RDS, Supabase, etc.]

**Rationale:**
- [Why this database]
- [Scaling considerations]

### TS-9: ORM / Query Builder

**Technology:** [Prisma | TypeORM | Drizzle | SQLAlchemy | etc.]
**Version:** [Version]

**Migration Strategy:**
- [How migrations are handled]
- [Rollback procedures]

### TS-10: Caching Layer

**Technology:** [Redis | Memcached | None]
**Use Cases:**
- Session storage
- Query result caching
- Rate limiting data

**Cache Invalidation Strategy:**
- [TTL-based | Event-based | Manual]
```

## 5. Infrastructure & Deployment

```markdown
## 5. Infrastructure & Deployment

### TS-11: Hosting Platform

**Platform:** [Vercel | AWS | GCP | DigitalOcean | etc.]

**Services Used:**
| Service | Purpose |
|:--------|:--------|
| [Service 1] | [Purpose] |
| [Service 2] | [Purpose] |

### TS-12: CI/CD Pipeline

**Platform:** [GitHub Actions | GitLab CI | etc.]

**Pipeline Stages:**
1. **Build:** Compile and bundle
2. **Test:** Unit tests, integration tests
3. **Lint:** Code quality checks
4. **Deploy:** Environment deployment

**Environments:**
| Environment | URL | Branch |
|:------------|:----|:-------|
| Development | [URL] | `develop` |
| Staging | [URL] | `staging` |
| Production | [URL] | `main` |

### TS-13: Containerization

**Technology:** [Docker | None]
**Orchestration:** [Kubernetes | Docker Compose | None]

**Container Strategy:**
- [How services are containerized]
- [Base images used]
```

## 6. Third-Party Services

```markdown
## 6. Third-Party Services

### TS-14: Email Service

**Provider:** [SendGrid | Postmark | Resend | etc.]
**Use Cases:**
- Transactional emails (welcome, password reset)
- Marketing emails (if applicable)

### TS-15: Payment Processing

**Provider:** [Stripe | PayPal | etc.]
**Features Used:**
- [Subscriptions | One-time payments | Both]
- [Webhook handling]

### TS-16: File Storage

**Provider:** [AWS S3 | Cloudflare R2 | Supabase Storage | etc.]
**Use Cases:**
- User uploads
- Generated documents
- Static assets

### TS-17: Monitoring & Logging

**Error Tracking:** [Sentry | Bugsnag | etc.]
**Logging:** [CloudWatch | Datadog | etc.]
**APM:** [DataDog | New Relic | etc.]

### TS-18: Analytics

**Provider:** [Google Analytics | Mixpanel | Amplitude | etc.]
**Events Tracked:**
- [Key events]
```

## 7. Security Measures

```markdown
## 7. Security Measures

### TS-19: Application Security

**Implemented Measures:**
- HTTPS everywhere (TLS 1.3)
- CORS configuration
- Rate limiting
- Input validation and sanitization
- SQL injection prevention (parameterized queries)
- XSS prevention (output encoding)
- CSRF protection

### TS-20: Data Security

**Encryption:**
- At rest: AES-256
- In transit: TLS 1.3

**Secrets Management:**
- Environment variables
- [Secrets manager: AWS Secrets Manager, Vault, etc.]

### TS-21: Compliance

**Standards:**
- [GDPR | CCPA | SOC2 | HIPAA | etc.]

**Data Retention:**
- [Policy details]
```

## 8. Development Workflow

```markdown
## 8. Development Workflow

### TS-22: Version Control

**Platform:** GitHub
**Branching Strategy:** [Git Flow | GitHub Flow | Trunk-based]

**Branch Naming:**
- `feature/[ticket-id]-description`
- `bugfix/[ticket-id]-description`
- `hotfix/[ticket-id]-description`

### TS-23: Code Quality

**Linting:** ESLint
**Formatting:** Prettier
**Pre-commit Hooks:** Husky + lint-staged

**Code Review Requirements:**
- [Number] approvals required
- All CI checks must pass

### TS-24: Testing Strategy

**Unit Tests:** [Jest | Vitest | pytest | etc.]
**Integration Tests:** [Framework]
**E2E Tests:** [Playwright | Cypress | etc.]

**Coverage Targets:**
- Unit: 80%+
- Integration: Key flows
- E2E: Critical paths
```

## Document Footer

```markdown
---

## Appendix

### A. Environment Variables

| Variable | Description | Required |
|:---------|:------------|:---------|
| `DATABASE_URL` | PostgreSQL connection string | Yes |
| `JWT_SECRET` | JWT signing secret | Yes |
| `[VAR]` | [Description] | [Yes/No] |

### B. API Rate Limits

| Endpoint | Limit | Window |
|:---------|:------|:-------|
| `/api/auth/*` | 10 req | 1 min |
| `/api/*` | 100 req | 1 min |

### C. Revision History

| Version | Date | Author | Changes |
|:--------|:-----|:-------|:--------|
| 1.0 | [Date] | [Author] | Initial version |

---

*Generated with [BuildMVPFast](https://buildmvpfast.com) Project Docs skill for Claude Code*
```

---

## Generation Guidelines

### Word Count Target
- **Minimum:** 1,500 words
- **Target:** 2,000-2,500 words
- **Maximum:** 3,000 words

### Numbering Rules
- All tech stack items: TS-1, TS-2, TS-3... (sequential)
- Sub-items: TS-1.1, TS-1.2 (for detailed breakdowns)
- Principles: TS-PRINCIPLE-1, TS-PRINCIPLE-2

### Cross-Reference Format
- "Supports FR-5 authentication requirements (PRD)"
- "Database design detailed in DB-1 through DB-5 (Database Schema)"
