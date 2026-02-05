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

**IMPORTANT: Use the `AskUserQuestion` tool for ALL selection questions.** This provides clickable options instead of requiring manual typing.

**Note:** The AskUserQuestion tool automatically includes an "Other" option that allows users to type custom responses. You don't need to add it explicitly.

Group related questions and confirm understanding before proceeding:

**Essential Questions (Required) - USE AskUserQuestion for ALL:**

1. **Project Name & Vision** - USE AskUserQuestion tool:
   ```
   AskUserQuestion({
     questions: [
       {
         question: "What's your project called?",
         header: "Name",
         options: [
           { label: "I'll type it", description: "Select 'Other' to enter your project name" }
         ],
         multiSelect: false
       }
     ]
   })
   ```
   Then ask for vision:
   ```
   AskUserQuestion({
     questions: [{
       question: "In one sentence, what problem does it solve?",
       header: "Vision",
       options: [
         { label: "I'll describe it", description: "Select 'Other' to describe your project" }
       ],
       multiSelect: false
     }]
   })
   ```

2. **Project Type** - USE AskUserQuestion tool:
   ```
   AskUserQuestion({
     questions: [{
       question: "What type of project is this?",
       header: "Project Type",
       options: [
         { label: "SaaS", description: "Software as a Service - subscription-based web app" },
         { label: "E-commerce", description: "Online store selling products or services" },
         { label: "Mobile App", description: "iOS/Android native or hybrid application" },
         { label: "API/Backend", description: "API service, microservice, or backend system" }
       ],
       multiSelect: false
     }]
   })
   ```

3. **Target Audience** - USE AskUserQuestion tool:
   ```
   AskUserQuestion({
     questions: [
       {
         question: "Who are your primary users?",
         header: "Users",
         options: [
           { label: "Businesses (B2B)", description: "Companies, teams, enterprises" },
           { label: "Consumers (B2C)", description: "Individual end users" },
           { label: "Developers", description: "Software engineers, technical users" },
           { label: "Both B2B & B2C", description: "Multiple user segments" }
         ],
         multiSelect: false
       },
       {
         question: "What are their main pain points?",
         header: "Pain Points",
         options: [
           { label: "Time/Efficiency", description: "Tasks take too long, need automation" },
           { label: "Cost", description: "Current solutions too expensive" },
           { label: "Complexity", description: "Existing tools hard to use" },
           { label: "Collaboration", description: "Team coordination issues" }
         ],
         multiSelect: true
       }
     ]
   })
   ```

4. **Core Features** - USE AskUserQuestion tool:
   ```
   AskUserQuestion({
     questions: [{
       question: "What are the most important features? Select all that apply.",
       header: "Features",
       options: [
         { label: "User Authentication", description: "Login, signup, OAuth" },
         { label: "Dashboard/Analytics", description: "Data visualization, reports" },
         { label: "Payments/Billing", description: "Subscriptions, checkout" },
         { label: "Real-time Features", description: "Chat, notifications, live updates" }
       ],
       multiSelect: true
     }]
   })
   ```
   Then ask for unique value:
   ```
   AskUserQuestion({
     questions: [{
       question: "What makes your solution unique?",
       header: "Differentiator",
       options: [
         { label: "Better UX", description: "Simpler, more intuitive than competitors" },
         { label: "Lower Price", description: "More affordable solution" },
         { label: "More Features", description: "Comprehensive all-in-one platform" },
         { label: "Niche Focus", description: "Specialized for specific industry/use case" }
       ],
       multiSelect: false
     }]
   })
   ```

5. **Tech Stack Preferences** - USE AskUserQuestion tool:
   ```
   AskUserQuestion({
     questions: [
       {
         question: "What frontend framework do you prefer?",
         header: "Frontend",
         options: [
           { label: "React", description: "Most popular, large ecosystem" },
           { label: "Vue", description: "Progressive, easy learning curve" },
           { label: "Next.js", description: "React with SSR, recommended for SEO" },
           { label: "No preference", description: "Let me suggest based on project needs" }
         ],
         multiSelect: false
       },
       {
         question: "What backend/database do you prefer?",
         header: "Backend",
         options: [
           { label: "Node.js + PostgreSQL", description: "JavaScript full-stack, relational DB" },
           { label: "Python + PostgreSQL", description: "Django/FastAPI, great for AI/ML" },
           { label: "Supabase", description: "Firebase alternative, includes auth & DB" },
           { label: "No preference", description: "Let me suggest based on project needs" }
         ],
         multiSelect: false
       }
     ]
   })
   ```

**Optional Questions (Ask if relevant) - USE AskUserQuestion for ALL:**

6. **Timeline** - USE AskUserQuestion tool:
   ```
   AskUserQuestion({
     questions: [{
       question: "What's your timeline for MVP?",
       header: "Timeline",
       options: [
         { label: "1-2 weeks", description: "Quick prototype or proof of concept" },
         { label: "1-2 months", description: "Basic MVP with core features" },
         { label: "3-6 months", description: "Full-featured MVP" },
         { label: "Flexible", description: "No specific deadline" }
       ],
       multiSelect: false
     }]
   })
   ```

7. **Team Size** - USE AskUserQuestion tool:
   ```
   AskUserQuestion({
     questions: [{
       question: "How many developers will work on this?",
       header: "Team Size",
       options: [
         { label: "Solo", description: "Just me" },
         { label: "2-3 devs", description: "Small team" },
         { label: "4-10 devs", description: "Medium team" },
         { label: "10+ devs", description: "Large team" }
       ],
       multiSelect: false
     }]
   })
   ```

8. **Budget** - USE AskUserQuestion tool:
   ```
   AskUserQuestion({
     questions: [{
       question: "Any budget constraints affecting technology choices?",
       header: "Budget",
       options: [
         { label: "Minimal/Free", description: "Use free tiers, open source only" },
         { label: "Low ($0-100/mo)", description: "Basic paid services OK" },
         { label: "Medium ($100-500/mo)", description: "Standard SaaS tools OK" },
         { label: "Flexible", description: "Budget not a constraint" }
       ],
       multiSelect: false
     }]
   })
   ```

9. **Business Model** - USE AskUserQuestion tool:
   ```
   AskUserQuestion({
     questions: [{
       question: "How will this project generate revenue?",
       header: "Revenue",
       options: [
         { label: "Subscription", description: "Monthly/yearly recurring payments" },
         { label: "Freemium", description: "Free tier with paid upgrades" },
         { label: "One-time purchase", description: "Single payment for lifetime access" },
         { label: "Marketplace fees", description: "Commission on transactions" }
       ],
       multiSelect: false
     }]
   })
   ```

10. **Competition** - USE AskUserQuestion tool:
    ```
    AskUserQuestion({
      questions: [{
        question: "Do you have identified competitors?",
        header: "Competition",
        options: [
          { label: "Yes, direct competitors", description: "I know who they are - select Other to name them" },
          { label: "Indirect competitors", description: "Different solutions to same problem" },
          { label: "Blue ocean", description: "No direct competition, new market" },
          { label: "Not sure yet", description: "Need to research competition" }
        ],
        multiSelect: false
      }]
    })
    ```

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
```

Then USE AskUserQuestion tool to confirm:
```
AskUserQuestion({
  questions: [{
    question: "Ready to generate documentation?",
    header: "Confirm",
    options: [
      { label: "Yes, generate docs", description: "Start generating all 6 core documents" },
      { label: "Let me modify", description: "I want to change some details first" },
      { label: "Start over", description: "Begin the wizard again from scratch" }
    ],
    multiSelect: false
  }]
})
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

After core docs, USE AskUserQuestion tool with multiSelect:

```
AskUserQuestion({
  questions: [{
    question: "Would you like to generate any custom documents? Select all that apply.",
    header: "Custom Docs",
    options: [
      { label: "API Specification", description: "REST/GraphQL endpoint documentation" },
      { label: "QA Testing", description: "Testing strategy and test cases" },
      { label: "Integration Architecture", description: "Third-party service integrations" },
      { label: "None", description: "Skip custom documents for now" }
    ],
    multiSelect: true
  }]
})
```

If user wants more options, offer a second selection:
```
AskUserQuestion({
  questions: [{
    question: "Any additional documents?",
    header: "More Docs",
    options: [
      { label: "Performance Optimization", description: "Caching, optimization guidelines" },
      { label: "Legal Compliance", description: "Privacy policy, GDPR, terms" },
      { label: "Marketing SEO", description: "Marketing strategy, SEO plan" },
      { label: "None", description: "No more documents needed" }
    ],
    multiSelect: true
  }]
})
```

Additional options if needed: Accessibility Guidelines, Monetization Plan, Governance Plan

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

```

Then USE AskUserQuestion tool to confirm:
```
AskUserQuestion({
  questions: [{
    question: "Should I generate documentation based on this analysis?",
    header: "Confirm",
    options: [
      { label: "Yes, generate all docs", description: "Generate all 6 core documents" },
      { label: "Yes, let me customize", description: "I want to modify some details first" },
      { label: "No, analyze again", description: "Re-analyze with different parameters" }
    ],
    multiSelect: false
  }]
})
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
