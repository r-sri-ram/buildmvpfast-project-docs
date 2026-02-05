# Cross-Referencing System Guide

This guide ensures consistent numbering and cross-references across all documents.

## Numbering Prefixes

| Document | Prefix | Example |
|:---------|:-------|:--------|
| PRD | FR- (functional), NFR- (non-functional) | FR-1, FR-2.1, NFR-3 |
| Tech Stack | TS- | TS-1, TS-2.1, TS-PRINCIPLE-1 |
| User Flows | UF- | UF-1, UF-2, UF-E1 (errors) |
| Database Schema | DB- | DB-1, DB-2, DB-PRINCIPLE-1 |
| Design Guidelines | DG- | DG-1, DG-2, DG-PRINCIPLE-1 |
| Task List | Task- | Task-1, Task-1.1, Task-1.2 |

## Numbering Rules

### Sequential Numbering
Numbers are assigned sequentially within each document:
- FR-1, FR-2, FR-3...
- TS-1, TS-2, TS-3...

### Sub-numbering
For detailed breakdowns, use dot notation:
- FR-1.1, FR-1.2, FR-1.3 (sub-requirements of FR-1)
- Task-3.1, Task-3.2 (subtasks of Task-3)

### Category Prefixes
Some documents have category-specific prefixes:
- PRD: FR- (functional) and NFR- (non-functional)
- User Flows: UF- (normal) and UF-E (error flows)
- Tech Stack: TS- (items) and TS-PRINCIPLE- (principles)

## Cross-Reference Format

### In-Document References
When referencing within the same document:
```markdown
See FR-3 above for authentication requirements.
This relates to DB-2 (users table).
```

### Cross-Document References
When referencing other documents, include the document name:
```markdown
Implements FR-3, FR-4 (PRD)
Uses TS-7 authentication approach (Tech Stack)
Follows UF-2 login flow (User Flows)
Creates DB-1 users table (Database Schema)
Applies DG-5 typography scale (Design Guidelines)
```

## Cross-Reference Matrix

### How Documents Connect

```
PRD (Requirements)
 │
 ├── Tech Stack (How to build it)
 │    └── References: FR-X requirements it implements
 │
 ├── User Flows (How users interact)
 │    └── References: FR-X requirements it implements
 │
 ├── Database Schema (How data is stored)
 │    └── References: FR-X requirements, TS-X tech choices
 │
 ├── Design Guidelines (How it looks)
 │    └── References: UF-X flows it styles, NFR-X accessibility
 │
 └── Task List (How to implement)
      └── References: ALL documents
```

### Task List Cross-References

Tasks should reference all relevant items:

```markdown
### Task-5: Implement User Authentication

**Implements:**
- FR-1: User registration (PRD)
- FR-2: User login (PRD)
- FR-3: Password recovery (PRD)

**Uses:**
- TS-7: JWT authentication strategy (Tech Stack)
- TS-8: PostgreSQL database (Tech Stack)

**Creates:**
- DB-1: users table (Database Schema)
- DB-2: sessions table (Database Schema)

**Follows:**
- UF-1: Registration flow (User Flows)
- UF-2: Login flow (User Flows)
- UF-3: Password reset flow (User Flows)

**Applies:**
- DG-10: Form component styles (Design Guidelines)
- DG-11: Button styles (Design Guidelines)
```

## Maintaining Consistency

### When Adding New Items

1. **Find the highest existing number** in that document
2. **Assign the next sequential number**
3. **Add cross-references** to related items in other docs

### When Updating Documents

If you add FR-15 to the PRD:
1. Check if User Flows needs a new UF-X
2. Check if Database needs a new DB-X
3. Add Task-X to Task List
4. Reference FR-15 in all related items

### When Removing Items

If you remove FR-5 from the PRD:
1. **Don't reuse the number** - leave a gap or mark as "Removed"
2. Remove references from other documents
3. Mark related tasks as "Cancelled" or remove them

## Examples

### PRD Entry
```markdown
**FR-5: Payment Processing**
- Description: Users can make payments via credit card
- User Story: As a customer, I want to pay for my subscription
- Priority: High
- Dependencies: FR-1 (must have account)

Related:
- TS-15: Stripe integration (Tech Stack)
- UF-7: Checkout flow (User Flows)
- DB-6: payments table (Database Schema)
- Task-12: Implement payment processing (Task List)
```

### Tech Stack Entry
```markdown
**TS-15: Payment Processing**

**Technology:** Stripe
**Implements:** FR-5 (Payment Processing from PRD)

Configuration:
- API Version: 2024-01-01
- Webhook endpoint: /api/webhooks/stripe

Related:
- DB-6: payments table for storing payment records
- UF-7: Checkout flow implementation
```

### Database Entry
```markdown
**DB-6: payments**

**Purpose:** Store payment transaction records
**Implements:** FR-5 (Payment Processing from PRD)

| Column | Type | Description |
|:-------|:-----|:------------|
| id | UUID | Primary key |
| user_id | UUID | FK → users(id) |
| amount | DECIMAL | Payment amount |
| status | VARCHAR | Payment status |
| stripe_id | VARCHAR | Stripe payment ID |
| created_at | TIMESTAMP | Transaction time |

Related:
- TS-15: Stripe configuration
- UF-7: Checkout flow
```

### User Flow Entry
```markdown
**UF-7: Checkout Flow**

**Implements:** FR-5 (Payment Processing from PRD)
**Uses:** TS-15 Stripe integration (Tech Stack)
**Stores:** DB-6 payments table (Database Schema)

Steps:
1. User reviews order summary
2. User enters payment details (Stripe Elements)
3. System processes payment via Stripe
4. System creates payment record in DB-6
5. System confirms order and redirects
```

### Task List Entry
```markdown
**Task-12: Implement Payment Processing**

**Implements:** FR-5 (PRD)
**Tech:** TS-15 Stripe (Tech Stack)
**Database:** DB-6 payments (Database Schema)
**Flow:** UF-7 checkout (User Flows)

Subtasks:
- Task-12.1: Configure Stripe SDK
- Task-12.2: Create payments table (DB-6)
- Task-12.3: Build checkout UI
- Task-12.4: Implement Stripe webhook handler
- Task-12.5: Add payment confirmation flow

Dependencies: Task-3 (Authentication must be complete)
```

## Quick Reference Card

When writing documents, use this reference:

```
PRD references:     "Implements FR-X (PRD)"
Tech references:    "Uses TS-X (Tech Stack)"
Flow references:    "Follows UF-X (User Flows)"
DB references:      "Creates/Uses DB-X (Database Schema)"
Design references:  "Applies DG-X (Design Guidelines)"
Task references:    "Completed in Task-X (Task List)"
```
