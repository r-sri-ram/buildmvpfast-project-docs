---
name: update-docs
description: Update existing project documentation with new features, changes, or refinements. Use when user says "add feature", "update docs", "change requirement", or describes modifications to existing project.
allowed-tools: Read, Write, Glob, Grep
---

# Update Project Documentation (Command Mode)

Update existing documentation when requirements change, features are added, or modifications are needed.

## Prerequisites

Verify documentation exists:
```bash
# Check for docs folder
ls ./docs/
```

If no docs found:
```
⚠️ No existing documentation found in ./docs/

Please run /project-docs first to generate documentation,
then use /update-docs to make changes.
```

## Update Process

### Step 1: Parse the Update Command

Extract the change request from the user's command:

**Example commands:**
- `/update-docs "Add Stripe payment integration"`
- `/update-docs "Add user authentication with OAuth"`
- `/update-docs "Remove the analytics feature"`
- `/update-docs "Change database from PostgreSQL to MongoDB"`
- `/update-docs "Add real-time notifications"`

Identify:
- **Action type:** Add, Remove, Change, Update
- **Feature/Component:** What's being modified
- **Details:** Any specific requirements mentioned

### Step 2: Read Existing Documentation

Load all current documents:
- `./docs/01-prd.md`
- `./docs/02-tech-stack.md`
- `./docs/03-user-flows.md`
- `./docs/04-database-schema.md`
- `./docs/05-design-guidelines.md`
- `./docs/06-task-list.md`
- Any custom docs in `./docs/custom/`

Extract:
- Current feature list with FR-X numbers
- Current tech stack with TS-X numbers
- Highest existing numbers for each prefix

### Step 3: Analyze Impact

Determine which documents need updates:

| Change Type | Documents Affected |
|:------------|:-------------------|
| New feature | PRD, User Flows, Database (if data), Task List |
| New integration | PRD, Tech Stack, Integration doc (if exists), Task List |
| Database change | Database Schema, Tech Stack, Task List |
| UI change | Design Guidelines, User Flows, Task List |
| Auth change | PRD, Tech Stack, User Flows, Database, Task List |
| Remove feature | All docs that reference it |

### Step 4: Present Update Plan

Before making changes, show the user:

```markdown
## 📋 Update Plan

**Change Request:** [User's command]

### Documents to Update:

1. **01-prd.md**
   - Add: FR-[next] [New requirement]
   - Add: FR-[next] [Related requirement]

2. **02-tech-stack.md**
   - Add: TS-[next] [New technology/service]

3. **03-user-flows.md**
   - Add: UF-[next] [New user flow]

4. **04-database-schema.md**
   - Add: DB-[next] [New table/collection]

5. **06-task-list.md**
   - Add: Task-[next] [Implementation tasks]

### Cross-References to Add:
- Task-[X] will reference FR-[X], TS-[X], UF-[X]

**Proceed with updates?** (yes/no)
```

### Step 5: Execute Updates

For each affected document:

#### PRD Updates

Add new functional requirements:
```markdown
### [Feature Section]

**FR-[next]: [Requirement Title]**
- Description: [What the feature does]
- User Story: As a [user], I want to [action] so that [benefit]
- Acceptance Criteria:
  - [ ] [Criterion 1]
  - [ ] [Criterion 2]
- Priority: [High/Medium/Low]
- Dependencies: [Any FR-X dependencies]
```

#### Tech Stack Updates

Add new technologies or services:
```markdown
### [Section - e.g., Third-Party Services]

**TS-[next]: [Technology/Service Name]**
- Purpose: [What it does]
- Implements: FR-[X] (reference to PRD)
- Configuration: [Key setup details]
- Documentation: [Link if available]
```

#### User Flows Updates

Add new user journeys:
```markdown
### UF-[next]: [Flow Name]

**Implements:** FR-[X], FR-[Y]

**Steps:**
1. User [action]
2. System [response]
3. User [action]
4. System [response]

**Success Criteria:**
- [Criterion]

**Error States:**
- [Error handling]
```

#### Database Schema Updates

Add new tables/collections:
```markdown
### DB-[next]: [Table Name]

**Purpose:** [What data it stores]
**Implements:** FR-[X]

| Column | Type | Constraints | Description |
|:-------|:-----|:------------|:------------|
| id | UUID | PK | Primary key |
| [column] | [type] | [constraints] | [description] |

**Relationships:**
- [Relation to other tables]

**Indexes:**
- [Index definitions]
```

#### Task List Updates

Add implementation tasks:
```markdown
### Task-[next]: [Task Title]

**Implements:** FR-[X]
**Tech:** TS-[X]
**Database:** DB-[X]
**Flow:** UF-[X]

**Subtasks:**
- [ ] Task-[next].1: [Subtask]
- [ ] Task-[next].2: [Subtask]
- [ ] Task-[next].3: [Subtask]

**Dependencies:** [Any Task-X dependencies]
**Priority:** [High/Medium/Low]
```

### Step 6: Verify Cross-References

After updates, ensure:
- All new FR-X numbers are unique
- All cross-references (FR-X, TS-X, etc.) are valid
- Task list references all new requirements
- No orphaned references to removed features

### Step 7: Update Completion

```markdown
✅ Documentation updated!

### Changes Made:

**01-prd.md:**
- Added FR-[X]: [Title]
- Added FR-[Y]: [Title]

**02-tech-stack.md:**
- Added TS-[X]: [Technology]

**03-user-flows.md:**
- Added UF-[X]: [Flow name]

**04-database-schema.md:**
- Added DB-[X]: [Table name]

**06-task-list.md:**
- Added Task-[X]: [Task title]
- Added Task-[Y]: [Task title]

### Cross-References Added:
- Task-[X] → FR-[X], TS-[X], UF-[X], DB-[X]

---
Generated with BuildMVPFast Project Docs • https://buildmvpfast.com
```

---

## Special Cases

### Removing Features

When removing a feature:
1. Identify all references (FR-X, related TS-X, UF-X, DB-X, Task-X)
2. Mark as deprecated or remove entirely (based on user preference)
3. Update cross-references in other documents
4. Add note in changelog section if exists

### Changing Tech Stack

When changing technology:
1. Update TS-X entry
2. Review all dependent tasks
3. Update database schema if ORM/DB changes
4. Note migration requirements in Task List

### Major Refactors

For large changes:
1. Create a summary of all changes needed
2. Ask for confirmation before proceeding
3. Update in logical order (PRD → Tech → DB → Flows → Tasks)

---

## Examples

### Example 1: Add Payment Integration

**Command:** `/update-docs "Add Stripe payment integration"`

**Updates:**
- PRD: FR-15 Payment processing, FR-16 Invoice generation
- Tech Stack: TS-8 Stripe SDK configuration
- User Flows: UF-9 Checkout and payment flow
- Database: DB-7 payments table, DB-8 invoices table
- Tasks: Task-20 through Task-25 for implementation

### Example 2: Add Authentication

**Command:** `/update-docs "Add user authentication with OAuth"`

**Updates:**
- PRD: FR-1 User registration, FR-2 Login, FR-3 OAuth providers
- Tech Stack: TS-3 OAuth configuration (Google, GitHub)
- User Flows: UF-1 Registration, UF-2 Login, UF-3 OAuth flow
- Database: DB-1 users, DB-2 sessions, DB-3 oauth_accounts
- Tasks: Task-1 through Task-8 for auth implementation

### Example 3: Remove Feature

**Command:** `/update-docs "Remove the chat feature"`

**Updates:**
- PRD: Mark FR-12, FR-13 as removed
- Tech Stack: Remove TS-6 WebSocket configuration
- User Flows: Remove UF-7 chat flow
- Database: Mark DB-5 messages as deprecated
- Tasks: Remove related tasks, add cleanup task

---

## No Argument Handling

If user types just `/update-docs` with no argument:

```
## Update Documentation

What would you like to update?

Please describe the change you want to make:

**Examples:**
- "Add Stripe payment integration"
- "Add user authentication with OAuth"
- "Change database from PostgreSQL to MongoDB"
- "Add real-time notifications feature"
- "Remove the analytics dashboard"

**Usage:** /update-docs "your change description"
```
