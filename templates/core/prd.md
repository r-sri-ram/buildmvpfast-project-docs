# Product Requirements Document (PRD) Template

## Document Header

```markdown
# [Project Name] - Product Requirements Document

**Version:** 1.0
**Last Updated:** [Date]
**Status:** Draft | In Review | Approved

---
```

## 1. Executive Summary

```markdown
## 1. Executive Summary

### 1.1 Product Vision
[One paragraph describing the product vision - what problem does it solve and for whom]

### 1.2 Product Overview
[Brief description of what the product is and its core value proposition]

### 1.3 Key Objectives
- [Objective 1]
- [Objective 2]
- [Objective 3]

### 1.4 Success Metrics
| Metric | Target | Measurement Method |
|:-------|:-------|:-------------------|
| [Metric 1] | [Target] | [How measured] |
| [Metric 2] | [Target] | [How measured] |
```

## 2. Target Audience

```markdown
## 2. Target Audience

### 2.1 Primary User Persona

**Name:** [Persona Name]
**Role:** [Job title/role]
**Demographics:** [Age range, location, tech-savviness]

**Goals:**
- [Goal 1]
- [Goal 2]

**Pain Points:**
- [Pain point 1]
- [Pain point 2]

**How [Product] Helps:**
[How the product addresses their needs]

### 2.2 Secondary User Persona

[Repeat structure if applicable]

### 2.3 User Segments
| Segment | Characteristics | Priority |
|:--------|:----------------|:---------|
| [Segment 1] | [Description] | High |
| [Segment 2] | [Description] | Medium |
```

## 3. Functional Requirements

```markdown
## 3. Functional Requirements

### 3.1 [Feature Category 1 - e.g., User Authentication]

**FR-1: User Registration**
- **Description:** Users can create an account using email/password or OAuth providers
- **User Story:** As a new user, I want to create an account so that I can access the platform
- **Acceptance Criteria:**
  - [ ] User can register with email and password
  - [ ] Email verification is sent upon registration
  - [ ] Password must meet security requirements (8+ chars, mixed case, number)
  - [ ] User receives welcome email after verification
- **Priority:** High
- **Dependencies:** None

**FR-2: User Login**
- **Description:** Registered users can authenticate and access their account
- **User Story:** As a registered user, I want to log in so that I can access my data
- **Acceptance Criteria:**
  - [ ] User can log in with email/password
  - [ ] User can log in with OAuth (Google, GitHub)
  - [ ] "Remember me" option persists session
  - [ ] Failed login attempts are rate-limited
- **Priority:** High
- **Dependencies:** FR-1

**FR-3: Password Recovery**
- **Description:** Users can reset their password via email
- **User Story:** As a user who forgot my password, I want to reset it so I can regain access
- **Acceptance Criteria:**
  - [ ] User can request password reset via email
  - [ ] Reset link expires after 24 hours
  - [ ] User is notified of successful password change
- **Priority:** High
- **Dependencies:** FR-1

### 3.2 [Feature Category 2 - e.g., Core Features]

**FR-4: [Feature Name]**
- **Description:** [What the feature does]
- **User Story:** As a [user type], I want to [action] so that [benefit]
- **Acceptance Criteria:**
  - [ ] [Criterion 1]
  - [ ] [Criterion 2]
  - [ ] [Criterion 3]
- **Priority:** [High/Medium/Low]
- **Dependencies:** [FR-X or None]

[Continue with FR-5, FR-6, etc.]

### 3.3 [Feature Category 3]

[Continue pattern...]
```

## 4. Non-Functional Requirements

```markdown
## 4. Non-Functional Requirements

### 4.1 Performance

**NFR-1: Response Time**
- API responses must be < 200ms for 95th percentile
- Page load time < 3 seconds on 3G connection

**NFR-2: Throughput**
- System must handle [X] concurrent users
- Support [X] requests per second

### 4.2 Security

**NFR-3: Data Protection**
- All data encrypted at rest (AES-256)
- All traffic encrypted in transit (TLS 1.3)
- PII data handling compliant with GDPR/CCPA

**NFR-4: Authentication Security**
- Passwords hashed with bcrypt (cost factor 12)
- Session tokens expire after [X] hours of inactivity
- Rate limiting on authentication endpoints

### 4.3 Scalability

**NFR-5: Horizontal Scaling**
- Application must support horizontal scaling
- Database must support read replicas

### 4.4 Availability

**NFR-6: Uptime**
- 99.9% uptime SLA
- Maximum planned downtime: 4 hours/month

### 4.5 Accessibility

**NFR-7: WCAG Compliance**
- WCAG 2.1 AA compliance
- Keyboard navigation support
- Screen reader compatibility
```

## 5. Constraints and Assumptions

```markdown
## 5. Constraints and Assumptions

### 5.1 Constraints
- **Budget:** [Budget constraints if any]
- **Timeline:** [Deadline constraints]
- **Technology:** [Required technologies or limitations]
- **Regulatory:** [Compliance requirements]

### 5.2 Assumptions
- Users have modern web browsers (Chrome, Firefox, Safari, Edge - last 2 versions)
- Users have reliable internet connection
- [Other assumptions]

### 5.3 Dependencies
- [External service dependencies]
- [Third-party API dependencies]
```

## 6. Release Planning

```markdown
## 6. Release Planning

### 6.1 MVP Scope
| Feature | FR Reference | Included |
|:--------|:-------------|:---------|
| [Feature 1] | FR-1, FR-2, FR-3 | ✅ |
| [Feature 2] | FR-4, FR-5 | ✅ |
| [Feature 3] | FR-6 | ❌ (Post-MVP) |

### 6.2 Future Phases
**Phase 2:**
- [Feature list]

**Phase 3:**
- [Feature list]
```

## 7. Appendix

```markdown
## 7. Appendix

### 7.1 Glossary
| Term | Definition |
|:-----|:-----------|
| [Term] | [Definition] |

### 7.2 References
- [Reference documents]
- [Related documentation]

### 7.3 Revision History
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
- Functional Requirements: FR-1, FR-2, FR-3... (sequential)
- Sub-requirements: FR-1.1, FR-1.2 (for complex features)
- Non-Functional Requirements: NFR-1, NFR-2, NFR-3...

### Cross-Reference Format
When referencing from other documents:
- "Implements FR-3 from PRD"
- "Related to FR-5, FR-6"

### Priority Definitions
- **High:** Must have for MVP
- **Medium:** Important but can be deferred
- **Low:** Nice to have, future consideration
