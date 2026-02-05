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

If no argument given, USE AskUserQuestion to select document type:

```
AskUserQuestion({
  questions: [{
    question: "Which custom document would you like to add?",
    header: "Doc Type",
    options: [
      { label: "API Specification", description: "REST/GraphQL endpoints, authentication" },
      { label: "QA Testing", description: "Testing strategy, test cases, coverage" },
      { label: "Integration Architecture", description: "Third-party services, webhooks" },
      { label: "Performance Optimization", description: "Caching, benchmarks, optimization" }
    ],
    multiSelect: false
  }]
})
```

If user needs more options, show second selection:
```
AskUserQuestion({
  questions: [{
    question: "Or choose from these additional documents:",
    header: "More Docs",
    options: [
      { label: "Legal Compliance", description: "Privacy policy, GDPR, terms" },
      { label: "Marketing SEO", description: "Marketing strategy, SEO plan" },
      { label: "Accessibility Guidelines", description: "WCAG compliance, a11y" },
      { label: "Monetization Plan", description: "Pricing tiers, revenue models" }
    ],
    multiSelect: false
  }]
})
```

Additional option: Governance Plan (decision-making, contribution guidelines)

## Document-Specific Wizards

**IMPORTANT:** After selecting document type, run a mini-wizard to gather specific information.

### API Specification Wizard
```
AskUserQuestion({
  questions: [
    {
      question: "What API style will you use?",
      header: "API Style",
      options: [
        { label: "REST", description: "Traditional RESTful API" },
        { label: "GraphQL", description: "Query-based API" },
        { label: "Both", description: "REST + GraphQL" },
        { label: "Not sure", description: "Recommend based on project" }
      ],
      multiSelect: false
    },
    {
      question: "What authentication method?",
      header: "Auth",
      options: [
        { label: "JWT Tokens", description: "JSON Web Tokens" },
        { label: "API Keys", description: "Simple API key auth" },
        { label: "OAuth 2.0", description: "Third-party OAuth" },
        { label: "Session-based", description: "Cookie sessions" }
      ],
      multiSelect: false
    }
  ]
})
```

### QA Testing Wizard
```
AskUserQuestion({
  questions: [
    {
      question: "What's your target test coverage?",
      header: "Coverage",
      options: [
        { label: "60-70%", description: "Basic coverage for MVP" },
        { label: "70-80%", description: "Standard coverage" },
        { label: "80-90%", description: "High coverage" },
        { label: "90%+", description: "Comprehensive coverage" }
      ],
      multiSelect: false
    },
    {
      question: "Which testing types do you need?",
      header: "Test Types",
      options: [
        { label: "Unit Tests", description: "Individual function testing" },
        { label: "Integration Tests", description: "Component interaction testing" },
        { label: "E2E Tests", description: "Full user flow testing" },
        { label: "All of the above", description: "Comprehensive testing" }
      ],
      multiSelect: true
    }
  ]
})
```

### Integration Architecture Wizard
```
AskUserQuestion({
  questions: [{
    question: "Which integrations do you need? Select all that apply.",
    header: "Integrations",
    options: [
      { label: "Payment (Stripe/PayPal)", description: "Payment processing" },
      { label: "Auth (Google/GitHub)", description: "OAuth providers" },
      { label: "Email (SendGrid/Resend)", description: "Email services" },
      { label: "Analytics (GA/Mixpanel)", description: "Tracking & analytics" }
    ],
    multiSelect: true
  }]
})
```

### Performance Optimization Wizard
```
AskUserQuestion({
  questions: [
    {
      question: "What's your primary performance concern?",
      header: "Focus",
      options: [
        { label: "Page Load Speed", description: "Frontend performance" },
        { label: "API Response Time", description: "Backend performance" },
        { label: "Database Queries", description: "Query optimization" },
        { label: "All areas", description: "Comprehensive optimization" }
      ],
      multiSelect: false
    },
    {
      question: "What caching strategy do you prefer?",
      header: "Caching",
      options: [
        { label: "CDN + Browser", description: "Static asset caching" },
        { label: "Redis/Memcached", description: "Server-side caching" },
        { label: "Both", description: "Multi-layer caching" },
        { label: "Not sure", description: "Recommend based on stack" }
      ],
      multiSelect: false
    }
  ]
})
```

### Legal Compliance Wizard
```
AskUserQuestion({
  questions: [
    {
      question: "Which regions will you operate in?",
      header: "Regions",
      options: [
        { label: "US only", description: "US regulations" },
        { label: "EU (GDPR)", description: "GDPR compliance required" },
        { label: "Global", description: "Multiple jurisdictions" },
        { label: "Not sure yet", description: "General compliance" }
      ],
      multiSelect: false
    },
    {
      question: "What data do you collect?",
      header: "Data Types",
      options: [
        { label: "Basic (email, name)", description: "Standard user data" },
        { label: "Payment info", description: "Financial data" },
        { label: "Health/Medical", description: "HIPAA considerations" },
        { label: "Children's data", description: "COPPA considerations" }
      ],
      multiSelect: true
    }
  ]
})
```

### Marketing SEO Wizard
```
AskUserQuestion({
  questions: [
    {
      question: "What's your primary marketing channel?",
      header: "Channel",
      options: [
        { label: "SEO/Content", description: "Organic search traffic" },
        { label: "Social Media", description: "Social platforms" },
        { label: "Paid Ads", description: "PPC advertising" },
        { label: "All channels", description: "Multi-channel approach" }
      ],
      multiSelect: false
    },
    {
      question: "What's your target audience?",
      header: "Audience",
      options: [
        { label: "B2B", description: "Business customers" },
        { label: "B2C", description: "Consumer market" },
        { label: "Developers", description: "Technical audience" },
        { label: "Mixed", description: "Multiple segments" }
      ],
      multiSelect: false
    }
  ]
})
```

### Accessibility Guidelines Wizard
```
AskUserQuestion({
  questions: [{
    question: "What WCAG compliance level do you need?",
    header: "WCAG Level",
    options: [
      { label: "WCAG 2.1 Level A", description: "Basic accessibility" },
      { label: "WCAG 2.1 Level AA", description: "Standard (recommended)" },
      { label: "WCAG 2.1 Level AAA", description: "Highest standard" },
      { label: "Not sure", description: "Recommend Level AA" }
    ],
    multiSelect: false
  }]
})
```

### Monetization Plan Wizard
```
AskUserQuestion({
  questions: [
    {
      question: "What's your pricing model?",
      header: "Pricing",
      options: [
        { label: "Subscription", description: "Monthly/yearly plans" },
        { label: "Usage-based", description: "Pay per use" },
        { label: "Freemium", description: "Free tier + paid" },
        { label: "One-time", description: "Lifetime purchase" }
      ],
      multiSelect: false
    },
    {
      question: "How many pricing tiers?",
      header: "Tiers",
      options: [
        { label: "2 tiers", description: "Free + Paid" },
        { label: "3 tiers", description: "Free + Pro + Enterprise" },
        { label: "4+ tiers", description: "Multiple plans" },
        { label: "Custom", description: "Enterprise quotes only" }
      ],
      multiSelect: false
    }
  ]
})
```

### Governance Plan Wizard
```
AskUserQuestion({
  questions: [{
    question: "What type of project governance?",
    header: "Governance",
    options: [
      { label: "Open Source", description: "Community-driven" },
      { label: "Internal Team", description: "Company project" },
      { label: "Client Project", description: "Agency/consulting" },
      { label: "Startup", description: "Founder-led" }
    ],
    multiSelect: false
  }]
})
```

---

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
- **Wizard responses** to customize content
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
