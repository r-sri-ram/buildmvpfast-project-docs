---
name: analyze-project
description: Analyze existing codebase and generate documentation. Use when documenting existing code, reverse engineering docs, or when user says "document this codebase", "analyze project", "generate docs from code".
context: fork
allowed-tools: Read, Glob, Grep, Bash
---

# Codebase Analysis & Documentation Generator

Analyze an existing codebase and generate comprehensive documentation by extracting project context from code.

## Analysis Process

### Step 1: Project Structure Discovery

```bash
# Get overall structure
tree -L 3 -I 'node_modules|.git|dist|build|__pycache__|venv|.venv|vendor|target' . 2>/dev/null || find . -type d -maxdepth 3 | grep -v -E '(node_modules|\.git|dist|build|__pycache__|venv)' | head -50
```

### Step 2: Identify Project Type & Tech Stack

Check for these files in priority order:

**JavaScript/TypeScript:**
- `package.json` - Dependencies, scripts, project name
- `tsconfig.json` - TypeScript configuration
- `next.config.js`, `nuxt.config.js`, `vite.config.ts` - Framework config

**Python:**
- `requirements.txt`, `Pipfile`, `pyproject.toml` - Dependencies
- `manage.py` - Django indicator
- `app.py`, `main.py` - Flask/FastAPI indicator

**Other Languages:**
- `Cargo.toml` - Rust
- `go.mod` - Go
- `pom.xml`, `build.gradle` - Java
- `composer.json` - PHP
- `Gemfile` - Ruby

### Step 3: Database Schema Extraction

Search for database definitions:

```
# Prisma
prisma/schema.prisma

# SQL Migrations
migrations/*.sql
db/migrate/*.rb
alembic/versions/*.py

# ORM Models
models/*.py
models/*.ts
src/models/*
app/models/*

# TypeORM/Sequelize
src/entities/*.ts
src/database/entities/*
```

Extract:
- Table/Collection names
- Fields and types
- Relationships (foreign keys, references)
- Indexes

### Step 4: API Route Extraction

Search for route definitions:

**Express/Node:**
```javascript
// Pattern: router.get, router.post, app.get, app.post
app.get('/api/users', ...)
router.post('/auth/login', ...)
```

**FastAPI/Flask:**
```python
# Pattern: @app.route, @router.get
@app.route('/users')
@router.get('/items/{item_id}')
```

**Next.js/Nuxt:**
```
# File-based routing
pages/api/*
app/api/*
```

Extract:
- Endpoints (method + path)
- Request/response patterns
- Authentication requirements

### Step 5: Feature Discovery

Identify features from:

**Frontend Components:**
- `src/components/*`
- `src/pages/*`, `app/*`
- `src/features/*`

**Backend Services:**
- `src/services/*`
- `src/controllers/*`
- `src/handlers/*`

**Configuration:**
- Feature flags
- Environment variables
- Config files

### Step 6: Present Analysis Summary

```markdown
## 🔍 Codebase Analysis Complete

### Project Overview
- **Name:** [from package.json or folder name]
- **Description:** [from package.json or README]
- **Type:** [SaaS/API/Mobile/etc. - inferred]

### Tech Stack Detected

| Layer | Technology | Version |
|:------|:-----------|:--------|
| Language | [TypeScript/Python/etc.] | [version] |
| Frontend | [React/Vue/None] | [version] |
| Backend | [Express/FastAPI/etc.] | [version] |
| Database | [PostgreSQL/MongoDB/etc.] | [detected] |
| ORM | [Prisma/TypeORM/SQLAlchemy] | [version] |

### Key Dependencies
- [dependency 1] - [purpose]
- [dependency 2] - [purpose]
- [dependency 3] - [purpose]

### Features Discovered
1. **[Feature Name]** - [Brief description from code]
2. **[Feature Name]** - [Brief description]
3. **[Feature Name]** - [Brief description]

### Database Models
| Model | Fields | Relationships |
|:------|:-------|:--------------|
| [Model1] | [field list] | [relations] |
| [Model2] | [field list] | [relations] |

### API Endpoints
| Method | Path | Description |
|:-------|:-----|:------------|
| GET | /api/users | [inferred] |
| POST | /api/auth/login | [inferred] |

### Architecture Pattern
- **Pattern:** [MVC/Feature-based/Layered/etc.]
- **Structure:** [Monolith/Microservices/Serverless]

---

**Ready to generate documentation based on this analysis?**

I'll create:
- 01-prd.md - Product Requirements (from features)
- 02-tech-stack.md - Technical Architecture (from dependencies)
- 03-user-flows.md - User Flows (from routes/pages)
- 04-database-schema.md - Database Schema (from models)
- 05-design-guidelines.md - Design Guidelines (from components)
- 06-task-list.md - Task List (future improvements)

Type "yes" to proceed or provide corrections.
```

### Step 7: Generate Documentation

After confirmation, generate all 6 core documents using the analyzed context:

1. **PRD**: Create requirements from discovered features
2. **Tech Stack**: Document detected technologies and architecture
3. **User Flows**: Map user journeys from routes/pages
4. **Database Schema**: Formalize discovered models
5. **Design Guidelines**: Extract patterns from components
6. **Task List**: Suggest improvements and missing features

### Step 8: Save Documents

Save to `./docs/` with proper cross-referencing:

```
docs/
├── README.md
├── 01-prd.md
├── 02-tech-stack.md
├── 03-user-flows.md
├── 04-database-schema.md
├── 05-design-guidelines.md
└── 06-task-list.md
```

## Analysis Tips

### For Incomplete Codebases

If the codebase is incomplete or early stage:
- Note missing components
- Suggest typical requirements for the project type
- Mark assumptions clearly with [ASSUMPTION] tags

### For Large Codebases

If the codebase is very large:
- Focus on main application code
- Ignore test files for feature discovery
- Sample representative files, don't analyze everything
- Look for README or existing docs for context

### For Monorepos

If multiple packages detected:
- Ask which package to document
- Or offer to document the main application
- Note the monorepo structure in Tech Stack

## Completion Message

```
✅ Codebase analyzed and documented!

📁 Location: ./docs/

📊 Analysis Summary:
   - Files analyzed: [count]
   - Features documented: [count]
   - API endpoints found: [count]
   - Database models documented: [count]

📄 Documents created:
   - 01-prd.md
   - 02-tech-stack.md
   - 03-user-flows.md
   - 04-database-schema.md
   - 05-design-guidelines.md
   - 06-task-list.md

💡 Next steps:
   - Review generated docs for accuracy
   - Use /update-docs to refine requirements
   - Use /add-doc to add API specs or testing docs

---
Generated with BuildMVPFast Project Docs • https://buildmvpfast.com
```
