---
name: add-doc
description: Add a custom document type to existing project documentation. Use when user wants to add API docs, testing strategy, integration docs, or other specific document types.
argument-hint: [api|testing|integration|performance|legal|marketing|accessibility|monetization|governance]
allowed-tools: Read, Write, Glob
---

# Add Custom Document to Project Documentation

Add individual custom documents to existing project documentation.

## Prerequisites

Before running, verify existing documentation exists:
- Check for `./docs/` folder
- Read `./docs/01-prd.md` for project context
- Read `./docs/02-tech-stack.md` for technical context

If no existing docs found:
```
⚠️ No existing documentation found in ./docs/

Please run /project-docs first to generate core documentation,
then use /add-doc to add custom documents.
```

## Available Document Types

| Argument | Document | Description |
|:---------|:---------|:------------|
| `api` | API Specification | REST/GraphQL endpoint documentation |
| `testing` | QA Testing | Testing strategy and test cases |
| `integration` | Integration Architecture | Third-party service integrations |
| `performance` | Performance Optimization | Caching, performance guidelines |
| `legal` | Legal Compliance | Privacy policy, GDPR, terms |
| `marketing` | Marketing SEO | Marketing strategy, SEO plan |
| `accessibility` | Accessibility Guidelines | WCAG compliance, a11y |
| `monetization` | Monetization Plan | Pricing strategy, revenue |
| `governance` | Governance Plan | Project governance |

## No Argument Provided

If no argument given, show the menu:

```
## Add Custom Document

Which document would you like to add?

1. **api** - API Specification
   REST/GraphQL endpoints, request/response formats, authentication

2. **testing** - QA Testing Strategy
   Testing approach, test cases, coverage requirements

3. **integration** - Integration Architecture
   Third-party services, webhooks, external APIs

4. **performance** - Performance Optimization
   Caching strategies, optimization guidelines, benchmarks

5. **legal** - Legal Compliance
   Privacy policy, GDPR compliance, terms of service

6. **marketing** - Marketing & SEO
   Marketing strategy, SEO optimization, content plan

7. **accessibility** - Accessibility Guidelines
   WCAG compliance, screen reader support, keyboard navigation

8. **monetization** - Monetization Plan
   Pricing tiers, revenue models, conversion strategy

9. **governance** - Governance Plan
   Decision-making process, contribution guidelines, roadmap

Enter your choice (e.g., "api" or "1"):
```

## Generation Process

### Step 1: Load Project Context

Read existing documentation to extract:
- Project name and description (from PRD)
- Feature list with FR-X numbers (from PRD)
- Tech stack with TS-X numbers (from Tech Stack)
- User flows with UF-X numbers (from User Flows)
- Database schema with DB-X numbers (from Database Schema)

### Step 2: Generate Document

Generate the custom document using:
- Template from `templates/custom/[type].md`
- Cross-references to existing FR-X, TS-X, etc.
- Project-specific context

### Step 3: Save Document

Save to `./docs/custom/[document-name].md`

If `./docs/custom/` doesn't exist, create it.

### Step 4: Update Index

Update `./docs/README.md` to include the new custom document.

---

## Document Templates

### API Specification (`/add-doc api`)

Generate comprehensive API documentation:

```markdown
# API Specification

## Overview
[Project name] API documentation covering all endpoints.

## Base URL
- Development: `http://localhost:3000/api`
- Production: `https://api.[domain].com`

## Authentication
[Based on TS-X auth approach from Tech Stack]

## Endpoints

### [Feature Area] (implements FR-X)

#### GET /api/[resource]
**Description:** [What it does]

**Request:**
- Headers: `Authorization: Bearer {token}`
- Query Params: [if any]

**Response:**
```json
{
  "data": [...],
  "meta": { "total": 100, "page": 1 }
}
```

**Errors:**
| Code | Description |
|:-----|:------------|
| 401 | Unauthorized |
| 404 | Not found |

[Continue for all endpoints discovered from routes or PRD features]

## Rate Limiting
[Standard rate limiting info]

## Versioning
[API versioning strategy]

---
*Generated with [BuildMVPFast](https://buildmvpfast.com) Project Docs*
```

### QA Testing (`/add-doc testing`)

```markdown
# QA Testing Strategy

## Testing Philosophy
[Approach based on project complexity]

## Test Types

### Unit Tests
- Coverage target: 80%
- Framework: [Based on TS-X]

### Integration Tests
- API endpoint testing
- Database integration

### E2E Tests
- Critical user flows (UF-X references)
- Framework: [Playwright/Cypress based on stack]

## Test Cases

### [Feature Area] (FR-X)

| ID | Test Case | Steps | Expected Result |
|:---|:----------|:------|:----------------|
| TC-1 | [Case name] | [Steps] | [Result] |

[Continue for key features]

## CI/CD Integration
[Based on project setup]

---
*Generated with [BuildMVPFast](https://buildmvpfast.com) Project Docs*
```

### Integration Architecture (`/add-doc integration`)

```markdown
# Integration Architecture

## Third-Party Services

### [Service Name]
- **Purpose:** [What it does]
- **Implements:** FR-X
- **Configuration:** TS-X.X

#### Setup
[Configuration steps]

#### API Keys Required
| Key | Environment Variable | Purpose |
|:----|:--------------------|:--------|

#### Data Flow
[Diagram or description]

[Continue for each integration]

## Webhooks

### Incoming Webhooks
[List webhook endpoints]

### Outgoing Webhooks
[Events that trigger external calls]

## Error Handling
[Integration failure handling]

---
*Generated with [BuildMVPFast](https://buildmvpfast.com) Project Docs*
```

[Templates continue for all 9 custom document types...]

---

## Completion Message

```
✅ Custom document added!

📄 Created: ./docs/custom/[document-name].md
📊 Word count: [X] words
🔗 Cross-references: [X] links to existing docs

📝 Index updated: ./docs/README.md

💡 Tip: Use /update-docs to modify this document when requirements change.

---
Generated with BuildMVPFast Project Docs • https://buildmvpfast.com
```
