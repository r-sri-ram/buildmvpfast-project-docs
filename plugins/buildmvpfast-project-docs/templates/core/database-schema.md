# Database Schema Design Template

## Document Header

```markdown
# [Project Name] - Database Schema Design

**Version:** 1.0
**Last Updated:** [Date]
**Status:** Draft | In Review | Approved
**Database:** [PostgreSQL | MySQL | MongoDB | etc.]
**ORM:** [Prisma | TypeORM | Drizzle | etc.]

---
```

## 1. Overview

```markdown
## 1. Overview

### 1.1 Database Architecture
**Type:** [Relational | Document | Graph | Hybrid]
**Primary Database:** [PostgreSQL, MySQL, MongoDB, etc.]
**Secondary Storage:** [Redis for caching, S3 for files, etc.]

### 1.2 Design Principles
- **DB-PRINCIPLE-1:** Normalize to 3NF for transactional data
- **DB-PRINCIPLE-2:** Denormalize for read-heavy operations
- **DB-PRINCIPLE-3:** Use UUIDs for primary keys (distributed-friendly)
- **DB-PRINCIPLE-4:** Soft delete with `deleted_at` timestamps

### 1.3 Schema Summary
| Table | Purpose | PRD Reference |
|:------|:--------|:--------------|
| users | User accounts | FR-1, FR-2 |
| [table] | [purpose] | [FR-X] |
| [table] | [purpose] | [FR-X] |

### 1.4 Entity Relationship Diagram

```
┌──────────────┐       ┌──────────────┐       ┌──────────────┐
│    users     │       │   projects   │       │    tasks     │
├──────────────┤       ├──────────────┤       ├──────────────┤
│ id (PK)      │───┐   │ id (PK)      │───┐   │ id (PK)      │
│ email        │   │   │ user_id (FK) │◀──┘   │ project_id(FK)│◀──┘
│ name         │   │   │ name         │       │ title        │
│ created_at   │   │   │ created_at   │       │ status       │
└──────────────┘   │   └──────────────┘       │ created_at   │
                   │                          └──────────────┘
                   │   ┌──────────────┐
                   └──▶│  user_roles  │
                       ├──────────────┤
                       │ user_id (FK) │
                       │ role         │
                       └──────────────┘
```
```

## 2. Core Tables

```markdown
## 2. Core Tables

### DB-1: users

**Purpose:** Store user account information
**Implements:** FR-1 (Registration), FR-2 (Authentication)

| Column | Type | Constraints | Description |
|:-------|:-----|:------------|:------------|
| id | UUID | PK, DEFAULT uuid_generate_v4() | Primary key |
| email | VARCHAR(255) | UNIQUE, NOT NULL | User email address |
| password_hash | VARCHAR(255) | NULL | Bcrypt hashed password (NULL for OAuth) |
| name | VARCHAR(100) | NOT NULL | Display name |
| avatar_url | TEXT | NULL | Profile picture URL |
| email_verified | BOOLEAN | DEFAULT false | Email verification status |
| email_verified_at | TIMESTAMP | NULL | When email was verified |
| created_at | TIMESTAMP | DEFAULT NOW() | Account creation time |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update time |
| deleted_at | TIMESTAMP | NULL | Soft delete timestamp |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_users_email ON users(email) WHERE deleted_at IS NULL;
CREATE INDEX idx_users_created_at ON users(created_at);
```

**Sample Data:**
```sql
INSERT INTO users (id, email, name, email_verified) VALUES
  ('uuid-1', 'user@example.com', 'John Doe', true);
```

---

### DB-2: sessions

**Purpose:** Store active user sessions
**Implements:** FR-2 (Authentication), TS-7 (JWT/Session strategy)

| Column | Type | Constraints | Description |
|:-------|:-----|:------------|:------------|
| id | UUID | PK | Session identifier |
| user_id | UUID | FK → users(id), NOT NULL | User reference |
| token | VARCHAR(255) | UNIQUE, NOT NULL | Session token (hashed) |
| user_agent | TEXT | NULL | Browser/device info |
| ip_address | INET | NULL | Last known IP |
| expires_at | TIMESTAMP | NOT NULL | Session expiration |
| created_at | TIMESTAMP | DEFAULT NOW() | Session start |

**Indexes:**
```sql
CREATE INDEX idx_sessions_user_id ON sessions(user_id);
CREATE INDEX idx_sessions_expires_at ON sessions(expires_at);
```

**Cleanup Job:**
```sql
-- Run daily to remove expired sessions
DELETE FROM sessions WHERE expires_at < NOW();
```

---

### DB-3: oauth_accounts

**Purpose:** Link OAuth providers to user accounts
**Implements:** FR-1 (OAuth Registration), FR-2 (OAuth Login)

| Column | Type | Constraints | Description |
|:-------|:-----|:------------|:------------|
| id | UUID | PK | Primary key |
| user_id | UUID | FK → users(id), NOT NULL | User reference |
| provider | VARCHAR(50) | NOT NULL | OAuth provider (google, github) |
| provider_user_id | VARCHAR(255) | NOT NULL | Provider's user ID |
| access_token | TEXT | NULL | OAuth access token (encrypted) |
| refresh_token | TEXT | NULL | OAuth refresh token (encrypted) |
| created_at | TIMESTAMP | DEFAULT NOW() | Link creation time |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_oauth_provider_user ON oauth_accounts(provider, provider_user_id);
CREATE INDEX idx_oauth_user_id ON oauth_accounts(user_id);
```

**Constraints:**
```sql
-- Each user can only have one account per provider
ALTER TABLE oauth_accounts
ADD CONSTRAINT unique_user_provider UNIQUE (user_id, provider);
```
```

## 3. Feature Tables

```markdown
## 3. Feature Tables

### DB-4: [Primary Resource - e.g., projects]

**Purpose:** [What this table stores]
**Implements:** FR-4, FR-5

| Column | Type | Constraints | Description |
|:-------|:-----|:------------|:------------|
| id | UUID | PK | Primary key |
| user_id | UUID | FK → users(id), NOT NULL | Owner reference |
| name | VARCHAR(255) | NOT NULL | Project name |
| description | TEXT | NULL | Project description |
| status | VARCHAR(50) | DEFAULT 'active' | Project status |
| settings | JSONB | DEFAULT '{}' | Flexible settings storage |
| created_at | TIMESTAMP | DEFAULT NOW() | Creation time |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update |
| deleted_at | TIMESTAMP | NULL | Soft delete |

**Indexes:**
```sql
CREATE INDEX idx_projects_user_id ON projects(user_id) WHERE deleted_at IS NULL;
CREATE INDEX idx_projects_status ON projects(status) WHERE deleted_at IS NULL;
```

**Status Enum Values:**
- `draft` - Not yet active
- `active` - Currently active
- `archived` - No longer active
- `deleted` - Soft deleted

---

### DB-5: [Secondary Resource - e.g., tasks]

**Purpose:** [What this table stores]
**Implements:** FR-6, FR-7

| Column | Type | Constraints | Description |
|:-------|:-----|:------------|:------------|
| id | UUID | PK | Primary key |
| project_id | UUID | FK → projects(id), NOT NULL | Parent project |
| title | VARCHAR(255) | NOT NULL | Task title |
| description | TEXT | NULL | Task description |
| status | VARCHAR(50) | DEFAULT 'todo' | Task status |
| priority | INTEGER | DEFAULT 0 | Sort priority |
| due_date | DATE | NULL | Due date |
| assigned_to | UUID | FK → users(id), NULL | Assignee |
| created_at | TIMESTAMP | DEFAULT NOW() | Creation time |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update |

**Indexes:**
```sql
CREATE INDEX idx_tasks_project_id ON tasks(project_id);
CREATE INDEX idx_tasks_assigned_to ON tasks(assigned_to);
CREATE INDEX idx_tasks_status ON tasks(status);
CREATE INDEX idx_tasks_due_date ON tasks(due_date) WHERE due_date IS NOT NULL;
```

---

### DB-6: [Junction Table - e.g., project_members]

**Purpose:** Many-to-many relationship between projects and users
**Implements:** FR-8 (Team collaboration)

| Column | Type | Constraints | Description |
|:-------|:-----|:------------|:------------|
| id | UUID | PK | Primary key |
| project_id | UUID | FK → projects(id), NOT NULL | Project reference |
| user_id | UUID | FK → users(id), NOT NULL | User reference |
| role | VARCHAR(50) | NOT NULL | Member role |
| invited_by | UUID | FK → users(id), NULL | Who invited |
| joined_at | TIMESTAMP | DEFAULT NOW() | When joined |

**Indexes:**
```sql
CREATE UNIQUE INDEX idx_project_members_unique ON project_members(project_id, user_id);
CREATE INDEX idx_project_members_user ON project_members(user_id);
```

**Role Values:**
- `owner` - Full control, can delete project
- `admin` - Can manage members
- `editor` - Can edit content
- `viewer` - Read-only access
```

## 4. Audit & Activity Tables

```markdown
## 4. Audit & Activity Tables

### DB-7: audit_logs

**Purpose:** Track all significant system events
**Implements:** NFR-3 (Data Protection), Compliance requirements

| Column | Type | Constraints | Description |
|:-------|:-----|:------------|:------------|
| id | UUID | PK | Primary key |
| user_id | UUID | FK → users(id), NULL | Acting user |
| action | VARCHAR(100) | NOT NULL | Action type |
| entity_type | VARCHAR(100) | NOT NULL | Entity being acted upon |
| entity_id | UUID | NULL | Entity ID |
| old_values | JSONB | NULL | Previous state |
| new_values | JSONB | NULL | New state |
| ip_address | INET | NULL | User's IP |
| user_agent | TEXT | NULL | Browser info |
| created_at | TIMESTAMP | DEFAULT NOW() | When action occurred |

**Indexes:**
```sql
CREATE INDEX idx_audit_user_id ON audit_logs(user_id);
CREATE INDEX idx_audit_entity ON audit_logs(entity_type, entity_id);
CREATE INDEX idx_audit_created_at ON audit_logs(created_at);
```

**Retention Policy:**
- Keep for 90 days in hot storage
- Archive to cold storage after 90 days
- Delete after 2 years (or per compliance requirements)

---

### DB-8: notifications

**Purpose:** In-app notifications for users
**Implements:** FR-X (Notifications feature)

| Column | Type | Constraints | Description |
|:-------|:-----|:------------|:------------|
| id | UUID | PK | Primary key |
| user_id | UUID | FK → users(id), NOT NULL | Recipient |
| type | VARCHAR(100) | NOT NULL | Notification type |
| title | VARCHAR(255) | NOT NULL | Notification title |
| body | TEXT | NULL | Notification content |
| data | JSONB | DEFAULT '{}' | Additional data |
| read_at | TIMESTAMP | NULL | When read |
| created_at | TIMESTAMP | DEFAULT NOW() | When created |

**Indexes:**
```sql
CREATE INDEX idx_notifications_user ON notifications(user_id, read_at);
CREATE INDEX idx_notifications_created ON notifications(created_at);
```
```

## 5. Configuration Tables

```markdown
## 5. Configuration Tables

### DB-9: app_settings

**Purpose:** Application-wide configuration
**Implements:** Admin functionality

| Column | Type | Constraints | Description |
|:-------|:-----|:------------|:------------|
| key | VARCHAR(100) | PK | Setting key |
| value | JSONB | NOT NULL | Setting value |
| description | TEXT | NULL | Setting description |
| updated_at | TIMESTAMP | DEFAULT NOW() | Last update |
| updated_by | UUID | FK → users(id), NULL | Who updated |

**Example Settings:**
```json
{"maintenance_mode": false}
{"feature_flags": {"new_dashboard": true}}
{"rate_limits": {"api": 100, "auth": 10}}
```
```

## 6. Migrations

```markdown
## 6. Migrations

### Migration Strategy
- Use [Prisma Migrate | TypeORM migrations | etc.]
- All migrations are version-controlled
- Migrations run automatically on deployment
- Rollback scripts provided for each migration

### Migration Naming Convention
```
YYYYMMDD_HHMMSS_description.sql
Example: 20250105_120000_create_users_table.sql
```

### Initial Migration (Example)
```sql
-- 20250105_120000_create_users_table.sql

-- Up
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255),
  name VARCHAR(100) NOT NULL,
  avatar_url TEXT,
  email_verified BOOLEAN DEFAULT false,
  email_verified_at TIMESTAMP,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW(),
  deleted_at TIMESTAMP
);

CREATE UNIQUE INDEX idx_users_email ON users(email) WHERE deleted_at IS NULL;

-- Down
DROP TABLE IF EXISTS users;
```
```

## Document Footer

```markdown
---

## Appendix

### A. Common Queries

**Get user with projects:**
```sql
SELECT u.*, json_agg(p.*) as projects
FROM users u
LEFT JOIN projects p ON p.user_id = u.id AND p.deleted_at IS NULL
WHERE u.id = $1
GROUP BY u.id;
```

**Get project with members:**
```sql
SELECT p.*, json_agg(json_build_object(
  'user_id', pm.user_id,
  'role', pm.role,
  'name', u.name
)) as members
FROM projects p
LEFT JOIN project_members pm ON pm.project_id = p.id
LEFT JOIN users u ON u.id = pm.user_id
WHERE p.id = $1
GROUP BY p.id;
```

### B. Data Types Reference
| Type | Description | Use Case |
|:-----|:------------|:---------|
| UUID | Universally unique ID | Primary keys |
| VARCHAR(n) | Variable string | Names, emails |
| TEXT | Unlimited string | Descriptions |
| JSONB | Binary JSON | Flexible data |
| TIMESTAMP | Date and time | Timestamps |
| BOOLEAN | True/false | Flags |

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
- All database objects: DB-1, DB-2, DB-3... (sequential)
- Principles: DB-PRINCIPLE-1, DB-PRINCIPLE-2

### Cross-Reference Format
- "Implements FR-1, FR-2 (PRD)"
- "Referenced in UF-3 login flow (User Flows)"
- "Uses TS-9 ORM configuration (Tech Stack)"
