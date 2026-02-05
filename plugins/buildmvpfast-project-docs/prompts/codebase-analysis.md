# Codebase Analysis Prompt Guide

This guide helps Claude analyze existing codebases to generate documentation.

## Analysis Strategy

### Step 1: Get Project Structure

```bash
# First, get the directory structure
tree -L 3 -I 'node_modules|.git|dist|build|__pycache__|venv|.venv|vendor|target|.next|coverage' .
```

If `tree` not available:
```bash
find . -type d -maxdepth 3 | grep -v -E '(node_modules|\.git|dist|build|__pycache__|venv)' | head -50
```

### Step 2: Identify Tech Stack

**Check for dependency files in this order:**

| File | Indicates |
|:-----|:----------|
| `package.json` | Node.js project |
| `requirements.txt` / `pyproject.toml` | Python project |
| `Cargo.toml` | Rust project |
| `go.mod` | Go project |
| `pom.xml` / `build.gradle` | Java project |
| `composer.json` | PHP project |
| `Gemfile` | Ruby project |

**Read the main dependency file:**
```
Read package.json (or equivalent) to extract:
- Project name
- Description
- Dependencies (frameworks, libraries)
- Scripts (dev, build, test)
```

### Step 3: Detect Framework

**JavaScript/TypeScript Projects:**

| Indicator | Framework |
|:----------|:----------|
| `next.config.js` | Next.js |
| `nuxt.config.ts` | Nuxt |
| `vite.config.ts` + react | React (Vite) |
| `angular.json` | Angular |
| `svelte.config.js` | SvelteKit |
| `express` in deps | Express.js |
| `fastify` in deps | Fastify |
| `@nestjs/core` | NestJS |

**Python Projects:**

| Indicator | Framework |
|:----------|:----------|
| `manage.py` | Django |
| `app.py` + flask | Flask |
| `main.py` + fastapi | FastAPI |

### Step 4: Find Database Models

**Prisma:**
```
Read prisma/schema.prisma
```

**TypeORM/Sequelize:**
```
Search: src/entities/*.ts or src/models/*.ts
```

**Django:**
```
Search: */models.py
```

**SQL Migrations:**
```
Search: migrations/*.sql or db/migrate/*
```

**Extract from models:**
- Table/model names
- Field names and types
- Relationships (belongsTo, hasMany, references)

### Step 5: Find API Routes

**Express:**
```javascript
// Pattern to find:
router.get('/path', handler)
router.post('/path', handler)
app.use('/api', router)
```

**Next.js API Routes:**
```
pages/api/**/*.ts
app/api/**/route.ts
```

**FastAPI:**
```python
# Pattern to find:
@app.get("/path")
@router.post("/path")
```

### Step 6: Identify Features

**From Routes:**
- `/api/auth/*` → Authentication feature
- `/api/users/*` → User management
- `/api/projects/*` → Project feature
- etc.

**From Components:**
```
src/components/
├── auth/         → Authentication UI
├── dashboard/    → Dashboard feature
├── settings/     → Settings feature
```

**From Services:**
```
src/services/
├── auth.ts       → Auth service
├── email.ts      → Email functionality
├── payment.ts    → Payment processing
```

### Step 7: Analyze Architecture

**Identify Pattern:**

| Structure | Pattern |
|:----------|:--------|
| `controllers/`, `models/`, `views/` | MVC |
| `src/features/[feature]/` | Feature-based |
| `src/[layer]/` | Layered architecture |
| `packages/` or `apps/` | Monorepo |

**Identify Style:**
- Monolith vs Microservices
- Server-rendered vs SPA vs Hybrid
- REST vs GraphQL

## Output Template

After analysis, present findings:

```markdown
## 🔍 Codebase Analysis Complete

### Project Overview
- **Name:** [from package.json or folder]
- **Description:** [from package.json or README]
- **Type:** [SaaS/API/Tool - inferred from structure]

### Tech Stack Detected

| Layer | Technology | Version |
|:------|:-----------|:--------|
| Language | TypeScript | 5.x |
| Frontend | React + Next.js | 14.x |
| Styling | Tailwind CSS | 3.x |
| Backend | Next.js API Routes | - |
| Database | PostgreSQL | - |
| ORM | Prisma | 5.x |
| Auth | NextAuth.js | 4.x |

### Key Dependencies
- **UI:** shadcn/ui, Lucide icons
- **State:** Zustand
- **Forms:** React Hook Form + Zod
- **API:** tRPC (or REST)

### Features Discovered

Based on routes and components:

1. **Authentication**
   - Email/password login
   - OAuth (Google, GitHub)
   - Password reset

2. **[Feature 2]**
   - [Sub-feature]
   - [Sub-feature]

3. **[Feature 3]**
   - [Sub-feature]

### Database Models

From Prisma schema:

| Model | Key Fields | Relationships |
|:------|:-----------|:--------------|
| User | id, email, name | has many Projects |
| Project | id, name, userId | belongs to User |
| Task | id, title, projectId | belongs to Project |

### API Endpoints

| Method | Path | Description |
|:-------|:-----|:------------|
| POST | /api/auth/register | User registration |
| POST | /api/auth/login | User login |
| GET | /api/projects | List projects |
| POST | /api/projects | Create project |
| GET | /api/projects/:id | Get project |

### Architecture

- **Pattern:** Feature-based organization
- **Style:** Full-stack monolith (Next.js)
- **API Style:** REST (or tRPC)

---

**Ready to generate documentation?**

I'll create comprehensive docs based on this analysis:
- PRD with discovered features as requirements
- Tech Stack documenting the detected technologies
- User Flows based on routes and components
- Database Schema from your models
- Design Guidelines from your UI patterns
- Task List for improvements and missing features

Proceed? (yes/no)
```

## Handling Edge Cases

### Incomplete Codebase
```
I notice this appears to be an early-stage project. I found:
- [What exists]

I'll document what's here and suggest what's commonly needed:
- [Suggestions]

Want me to proceed with partial documentation?
```

### Monorepo
```
I detected a monorepo structure with multiple packages:
- packages/web - Frontend application
- packages/api - Backend API
- packages/shared - Shared utilities

Which should I focus on? Or should I document all?
```

### Unknown Tech
```
I see some technologies I'm not certain about:
- [Unknown package or pattern]

Can you clarify what [X] is used for?
```
