---
name: project-docs
description: Generate professional project documentation. Use when user wants to create PRD, technical docs, or full project documentation. Triggers on "generate docs", "create documentation", "project docs", "PRD", "technical documentation".
argument-hint: [new|analyze]
allowed-tools: Read, Glob, Grep, Write, Bash
---

# BuildMVPFast Project Documentation Generator

Generate comprehensive, professional project documentation with cross-referenced requirement numbering.

## Mode Selection

**If argument is "new" or no codebase exists:**
→ Run Conversational Wizard Mode

**If argument is "analyze" or codebase detected:**
→ Run Codebase Analysis Mode

**If no argument:**
→ Auto-detect: Check for package.json, requirements.txt, Cargo.toml, go.mod, etc.
   - If found: Suggest analysis mode
   - If not found: Start wizard mode

---

## MODE 1: Conversational Wizard (New Projects)

### Step 1: Gather Project Information

Ask the user these questions conversationally (not all at once). Group related questions and confirm understanding before proceeding:

**Essential Questions (Required):**

1. **Project Name & Vision**
   - "What's your project called?"
   - "In one sentence, what problem does it solve?"

2. **Project Type**
   - "What type of project is this?"
   - Options: SaaS, E-commerce, Mobile App, API/Backend Service, Internal Tool, Content Platform, Marketplace, Developer Tool, Other

3. **Target Audience**
   - "Who are your primary users?"
   - "What are their main pain points?"

4. **Core Features**
   - "What are the 3-5 most important features?"
   - "What makes your solution unique?"

5. **Tech Stack Preferences**
   - "Do you have preferences for frontend/backend/database?"
   - "Any specific frameworks or languages required?"

**Optional Questions (Ask if relevant):**

6. **Timeline & Milestones**
   - "Any specific deadlines or milestones?"

7. **Team Size**
   - "How many developers will work on this?"

8. **Budget Considerations**
   - "Any budget constraints affecting technology choices?"

9. **Business Model**
   - "How will this project generate revenue?"
   - Options: Subscription, One-time purchase, Freemium, Advertising, Marketplace fees, Enterprise licensing, Other

10. **Competition & Differentiation**
    - "Who are your main competitors?"
    - "What's your unique advantage?"

### Step 2: Confirm Context

Before generating, summarize the gathered information:

```
## Project Summary

**Name:** [Project Name]
**Type:** [Project Type]
**Vision:** [One-sentence description]

**Target Users:**
- [Primary persona]
- [Secondary persona if any]

**Core Features:**
1. [Feature 1]
2. [Feature 2]
3. [Feature 3]

**Tech Stack:**
- Frontend: [Choice or "To be determined"]
- Backend: [Choice or "To be determined"]
- Database: [Choice or "To be determined"]

**Business Model:** [Model]

Ready to generate documentation? (Yes/No)
```

### Step 3: Generate Documents

Generate documents in this order (each using context from previous):

**Batch 1 (No dependencies):**
- PRD (Product Requirements Document)
- Design Guidelines

**Batch 2 (Uses PRD context):**
- Tech Stack & Architecture
- User Flows & Journey Maps

**Batch 3 (Uses PRD + Tech Stack):**
- Database Schema Design

**Batch 4 (Uses all previous):**
- Development Task List

### Step 4: Custom Documents

After core docs, ask:
```
Core documentation complete! Would you like to generate any custom documents?

Available:
1. API Specification - REST/GraphQL endpoint documentation
2. QA Testing - Testing strategy and test cases
3. Integration Architecture - Third-party service integrations
4. Performance Optimization - Caching, optimization guidelines
5. Legal Compliance - Privacy policy, GDPR, terms of service
6. Marketing SEO - Marketing strategy, SEO plan
7. Accessibility Guidelines - WCAG compliance, a11y standards
8. Monetization Plan - Pricing strategy, revenue models
9. Governance Plan - Project governance, decision-making

Enter numbers (e.g., "1,2,5") or "none" to skip:
```

---

## MODE 2: Codebase Analysis (Existing Projects)

### Step 1: Explore Codebase

Analyze the project structure:

```bash
# Get project structure
tree -L 3 -I 'node_modules|.git|dist|build|__pycache__|venv' .

# Check for dependency files
ls -la package.json requirements.txt Cargo.toml go.mod pom.xml build.gradle composer.json Gemfile 2>/dev/null
```

### Step 2: Extract Information

**Package Manager Files:**
- Read package.json, requirements.txt, etc. for dependencies
- Identify frameworks (React, Vue, Django, Rails, etc.)

**Database:**
- Look for migrations folder, schema files, Prisma schema, etc.
- Search for ORM model definitions

**API Routes:**
- Search for route definitions, controllers, handlers
- Look for OpenAPI/Swagger specs

**Architecture:**
- Identify folder structure patterns (MVC, feature-based, etc.)
- Find configuration files

### Step 3: Present Findings

```
## Codebase Analysis Complete

**Detected Tech Stack:**
- Language: [Detected]
- Frontend: [Detected or None]
- Backend: [Detected]
- Database: [Detected or Unknown]
- Key Libraries: [List]

**Project Structure:**
- Architecture Pattern: [MVC/Feature-based/etc.]
- Total Files: [Count]
- Main Directories: [List]

**Discovered Features:**
1. [Feature from routes/components]
2. [Feature]
3. [Feature]

**Database Models Found:**
- [Model 1]
- [Model 2]

Should I generate documentation based on this analysis? (Yes/No)
```

### Step 4: Generate with Inferred Context

Use discovered information to generate all 6 core documents.

---

## Document Generation Templates

When generating each document, use the templates from the `templates/` folder and follow these guidelines:

### Word Count Targets
- Each core document: 1,500-3,000 words
- Each custom document: 1,000-2,000 words

### Cross-Reference Numbering

| Document | Prefix | Format |
|:---------|:-------|:-------|
| PRD | FR-, NFR- | FR-1, FR-2.1, NFR-1 |
| Tech Stack | TS- | TS-1, TS-2.1 |
| User Flows | UF- | UF-1, UF-2 |
| Database Schema | DB- | DB-1, DB-2 |
| Design Guidelines | DG- | DG-1, DG-2 |
| Task List | Task- | Task-1, Task-2.1 |

### Cross-Reference Examples

In Task List:
```markdown
- Task-2.3: Implement user authentication
  - Implements FR-5, FR-6 (PRD)
  - Uses TS-2.1 auth service (Tech Stack)
  - Follows UF-3 login flow (User Flows)
  - Creates DB-2 users table (Database Schema)
```

In Database Schema:
```markdown
- DB-3: Orders Table
  - Supports FR-12 checkout requirements (PRD)
  - Referenced in UF-7 checkout flow (User Flows)
```

---

## Output Location

Save all documents to `./docs/`:

```
docs/
├── README.md              # Auto-generated index
├── 01-prd.md
├── 02-tech-stack.md
├── 03-user-flows.md
├── 04-database-schema.md
├── 05-design-guidelines.md
├── 06-task-list.md
└── custom/                # If custom docs generated
    └── [custom-doc].md
```

### Generate Index README

After all documents, create `docs/README.md`:

```markdown
# [Project Name] Documentation

Generated with [BuildMVPFast](https://buildmvpfast.com) Project Docs

## Core Documents

| Document | Description |
|:---------|:------------|
| [01-prd.md](01-prd.md) | Product Requirements Document |
| [02-tech-stack.md](02-tech-stack.md) | Technical Stack & Architecture |
| [03-user-flows.md](03-user-flows.md) | User Flows & Journey Maps |
| [04-database-schema.md](04-database-schema.md) | Database Schema Design |
| [05-design-guidelines.md](05-design-guidelines.md) | Design System & Guidelines |
| [06-task-list.md](06-task-list.md) | Development Task List |

## Custom Documents

[List any generated custom docs]

---

*Generated with [BuildMVPFast](https://buildmvpfast.com) Project Docs skill for Claude Code*
```

---

## Completion Message

After generation:

```
✅ Documentation generated successfully!

📁 Location: ./docs/

📄 Documents created:
   - 01-prd.md (X words)
   - 02-tech-stack.md (X words)
   - 03-user-flows.md (X words)
   - 04-database-schema.md (X words)
   - 05-design-guidelines.md (X words)
   - 06-task-list.md (X words)
   [+ any custom docs]

💡 Tips:
   - Use /update-docs "your change" to update docs when requirements change
   - Use /add-doc [type] to add more custom documents
   - All documents cross-reference each other with FR-X, TS-X, etc.

---
Generated with BuildMVPFast Project Docs • https://buildmvpfast.com
```
