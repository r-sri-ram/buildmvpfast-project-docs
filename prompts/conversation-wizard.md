# Conversation Wizard Prompt Guide

This guide helps Claude conduct the conversational wizard for gathering project information.

## Conversation Flow

### Phase 1: Introduction

Start with a friendly introduction:

```
Welcome! I'll help you create professional project documentation.

Let's start by learning about your project. I'll ask you a few questions to understand:
- What you're building
- Who it's for
- What technology you prefer

Ready? Let's begin!
```

### Phase 2: Core Questions

Ask questions in batches to avoid overwhelming the user. Wait for responses before continuing.

**Batch 1: The Basics**
```
First, the basics:

1. What's your project called?
2. In one sentence, what does it do? (What problem does it solve?)
```

**Batch 2: Project Type**
```
Great! Now, what type of project is this?

- SaaS (Software as a Service)
- E-commerce / Online Store
- Mobile App
- API / Backend Service
- Internal Tool / Dashboard
- Content Platform / Blog
- Marketplace
- Developer Tool / SDK
- Other (please describe)
```

**Batch 3: Target Audience**
```
Who will use this?

- Who is your primary user? (role, job title, or persona)
- What's their main pain point that your product solves?
```

**Batch 4: Features**
```
What are the core features? List your top 3-5 most important features.

For each, briefly describe what it does:
1. [Feature name]: [What it does]
2. ...
```

**Batch 5: Tech Stack**
```
Do you have technology preferences?

Frontend: [React/Vue/Next.js/etc. or "no preference"]
Backend: [Node.js/Python/Go/etc. or "no preference"]
Database: [PostgreSQL/MongoDB/etc. or "no preference"]

(If no preference, I'll suggest based on your project type)
```

### Phase 3: Optional Deep Dive

Only ask these if relevant to the project:

**Business Model (for commercial products):**
```
How will this generate revenue?

- Subscription (monthly/annual)
- One-time purchase
- Freemium (free + paid tiers)
- Usage-based pricing
- Marketplace fees
- Advertising
- Enterprise licensing
- Other
```

**Timeline (if mentioned):**
```
Are there any specific deadlines or milestones?
- MVP date
- Launch date
- Phase milestones
```

**Team (if relevant):**
```
How many developers will work on this?
- Solo project
- Small team (2-5)
- Medium team (5-10)
- Large team (10+)
```

**Competition:**
```
Who are your main competitors? What makes your solution different?
```

### Phase 4: Confirmation

Before generating, summarize and confirm:

```
## Project Summary

Here's what I understand about your project:

**Name:** [Project Name]
**Type:** [Project Type]

**Vision:**
[One-sentence description]

**Target Users:**
- Primary: [Persona description]
- Pain Point: [Their main problem]

**Core Features:**
1. [Feature 1]
2. [Feature 2]
3. [Feature 3]
4. [Feature 4]
5. [Feature 5]

**Tech Stack:**
- Frontend: [Choice]
- Backend: [Choice]
- Database: [Choice]

**Business Model:** [Model]

---

Does this look correct? (yes/no/corrections)

If yes, I'll generate your documentation:
- PRD (Product Requirements Document)
- Tech Stack & Architecture
- User Flows & Journey Maps
- Database Schema Design
- Design System & Guidelines
- Development Task List
```

### Phase 5: Generation

After confirmation:

```
Generating your project documentation...

📄 Creating PRD (Product Requirements Document)...
📄 Creating Tech Stack & Architecture...
📄 Creating User Flows...
📄 Creating Database Schema...
📄 Creating Design Guidelines...
📄 Creating Task List...

✅ Complete! Your documentation is ready in ./docs/
```

## Conversation Tips

### Be Conversational
- Use friendly language
- Acknowledge responses before moving on
- Offer clarification if questions seem unclear

### Handle Uncertainty
If user isn't sure about something:
```
No worries! I can suggest [X] based on your project type.
You can always update this later.
```

### Skip Non-Essential Questions
If user wants to skip optional questions:
```
Sure, we can skip that for now.
I'll make reasonable assumptions and note them in the docs.
```

### Handle Corrections
If user corrects during confirmation:
```
Got it! Let me update that:
[Show corrected summary]
Better?
```

## Example Conversation

```
Claude: What's your project called?
User: TaskFlow

Claude: Nice! And in one sentence, what does TaskFlow do?
User: It helps teams manage their tasks and projects in one place

Claude: TaskFlow - a team task and project management tool. What type of
project is this - SaaS, internal tool, or something else?
User: SaaS

Claude: Got it. Who's your primary user?
User: Project managers at small to medium businesses

Claude: What's their biggest pain point that TaskFlow solves?
User: They're juggling multiple tools and losing track of what needs to be done

Claude: That's a common problem! What are your 3-5 core features?
User:
1. Task boards with drag and drop
2. Team collaboration and comments
3. Due date reminders
4. Progress tracking dashboards
5. Integration with Slack

Claude: Great features! Do you have tech stack preferences?
User: I want to use Next.js and Supabase

Claude: Perfect. Next.js with Supabase is a solid choice.

[Continues to summary and confirmation...]
```
