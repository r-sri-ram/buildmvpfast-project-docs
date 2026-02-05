# User Flows & Journey Maps Template

## Document Header

```markdown
# [Project Name] - User Flows & Journey Maps

**Version:** 1.0
**Last Updated:** [Date]
**Status:** Draft | In Review | Approved

---
```

## 1. Overview

```markdown
## 1. Overview

### 1.1 Purpose
This document maps all user journeys and interaction flows within [Project Name], ensuring consistent user experience across all features.

### 1.2 User Personas Reference
| Persona | Primary Flows | PRD Reference |
|:--------|:--------------|:--------------|
| [Persona 1] | UF-1, UF-2, UF-3 | Section 2.1 |
| [Persona 2] | UF-4, UF-5 | Section 2.2 |

### 1.3 Flow Categories
1. **Authentication Flows** (UF-1 to UF-3)
2. **Core Feature Flows** (UF-4 to UF-X)
3. **Settings & Account Flows** (UF-X to UF-X)
4. **Admin Flows** (UF-X to UF-X) [if applicable]
```

## 2. Authentication Flows

```markdown
## 2. Authentication Flows

### UF-1: User Registration

**Implements:** FR-1 (User Registration)
**Actors:** New User
**Preconditions:** User is not logged in
**Postconditions:** User has active account, is logged in

**Flow Diagram:**
```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Landing   │────▶│   Signup    │────▶│   Email     │
│    Page     │     │    Form     │     │ Verification│
└─────────────┘     └─────────────┘     └──────┬──────┘
                                               │
                    ┌─────────────┐     ┌──────▼──────┐
                    │  Dashboard  │◀────│   Welcome   │
                    │   (Home)    │     │   Screen    │
                    └─────────────┘     └─────────────┘
```

**Steps:**

| Step | User Action | System Response | UI Element |
|:-----|:------------|:----------------|:-----------|
| 1 | Clicks "Sign Up" | Displays registration form | Button |
| 2 | Enters email, password | Validates input in real-time | Form |
| 3 | Clicks "Create Account" | Sends verification email | Button |
| 4 | Clicks email link | Verifies email, logs user in | Email link |
| 5 | - | Redirects to welcome/onboarding | Auto redirect |

**Alternative Paths:**
- **A1:** User chooses OAuth (Google/GitHub)
  - Step 2a: Clicks OAuth provider button
  - Step 2b: Authenticates with provider
  - Step 2c: Returns to app, account created

**Error States:**
| Error | Trigger | Message | Recovery |
|:------|:--------|:--------|:---------|
| E1 | Email already exists | "Email already registered" | Show login link |
| E2 | Weak password | "Password too weak" | Show requirements |
| E3 | Invalid email | "Invalid email format" | Highlight field |

**Success Criteria:**
- [ ] User can complete registration in < 60 seconds
- [ ] Error messages are clear and actionable
- [ ] OAuth flow completes without manual email entry

---

### UF-2: User Login

**Implements:** FR-2 (User Login)
**Actors:** Registered User
**Preconditions:** User has verified account
**Postconditions:** User is authenticated, session created

**Flow Diagram:**
```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Landing   │────▶│   Login     │────▶│  Dashboard  │
│    Page     │     │    Form     │     │             │
└─────────────┘     └─────────────┘     └─────────────┘
                          │
                          ▼ (if 2FA enabled)
                    ┌─────────────┐
                    │  2FA Input  │
                    └─────────────┘
```

**Steps:**

| Step | User Action | System Response | UI Element |
|:-----|:------------|:----------------|:-----------|
| 1 | Clicks "Log In" | Displays login form | Button |
| 2 | Enters credentials | Validates format | Form |
| 3 | Clicks "Sign In" | Authenticates user | Button |
| 4 | (Optional) Enters 2FA code | Verifies code | Input |
| 5 | - | Creates session, redirects | Auto redirect |

**Alternative Paths:**
- **A1:** Forgot password flow → UF-3
- **A2:** OAuth login (Google/GitHub)

**Error States:**
| Error | Trigger | Message | Recovery |
|:------|:--------|:--------|:---------|
| E1 | Invalid credentials | "Invalid email or password" | Retry, show forgot password |
| E2 | Account locked | "Account temporarily locked" | Show unlock instructions |
| E3 | Unverified email | "Please verify your email" | Resend verification |

---

### UF-3: Password Recovery

**Implements:** FR-3 (Password Recovery)
**Actors:** User who forgot password
**Preconditions:** User has account
**Postconditions:** User has new password, can log in

**Steps:**

| Step | User Action | System Response |
|:-----|:------------|:----------------|
| 1 | Clicks "Forgot Password" | Shows email input |
| 2 | Enters email | Sends reset link (if account exists) |
| 3 | Clicks email link | Shows password reset form |
| 4 | Enters new password | Updates password |
| 5 | - | Redirects to login with success message |

**Security Considerations:**
- Reset links expire after 24 hours
- Old sessions are invalidated on password change
- Rate limiting on reset requests (3 per hour)
```

## 3. Core Feature Flows

```markdown
## 3. Core Feature Flows

### UF-4: [Primary Feature Flow - e.g., Create Project]

**Implements:** FR-4, FR-5
**Actors:** Authenticated User
**Preconditions:** User is logged in
**Postconditions:** [Result of flow]

**Flow Diagram:**
```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Dashboard  │────▶│ Create Form │────▶│   Review    │
│             │     │  (Step 1)   │     │  (Step 2)   │
└─────────────┘     └─────────────┘     └──────┬──────┘
                                               │
                                        ┌──────▼──────┐
                                        │   Success   │
                                        │    View     │
                                        └─────────────┘
```

**Steps:**

| Step | User Action | System Response | UI Element |
|:-----|:------------|:----------------|:-----------|
| 1 | Clicks "Create New" | Opens creation modal/page | Button |
| 2 | Fills required fields | Validates in real-time | Form |
| 3 | Clicks "Continue" | Shows preview/confirmation | Button |
| 4 | Clicks "Create" | Creates resource, shows success | Button |
| 5 | - | Redirects to detail view | Auto redirect |

**Form Fields:**
| Field | Type | Required | Validation |
|:------|:-----|:---------|:-----------|
| [Field 1] | Text | Yes | Max 100 chars |
| [Field 2] | Select | Yes | From predefined list |
| [Field 3] | Text Area | No | Max 500 chars |

**Error States:**
| Error | Trigger | Message |
|:------|:--------|:--------|
| E1 | Missing required field | "[Field] is required" |
| E2 | Duplicate name | "A [resource] with this name exists" |
| E3 | Server error | "Something went wrong. Please try again" |

---

### UF-5: [Secondary Feature Flow]

[Follow same structure...]

---

### UF-6: [Feature Flow with Complex Logic]

[Follow same structure...]
```

## 4. Settings & Account Flows

```markdown
## 4. Settings & Account Flows

### UF-7: Update Profile

**Implements:** FR-X (Profile Management)
**Actors:** Authenticated User

**Steps:**

| Step | User Action | System Response |
|:-----|:------------|:----------------|
| 1 | Navigates to Settings | Displays settings page |
| 2 | Updates profile fields | Validates changes |
| 3 | Clicks "Save" | Saves and shows confirmation |

**Editable Fields:**
- Display name
- Avatar/Profile picture
- Email preferences
- [Other fields]

---

### UF-8: Change Password

**Implements:** FR-X (Security Settings)

**Steps:**

| Step | User Action | System Response |
|:-----|:------------|:----------------|
| 1 | Navigates to Security settings | Displays security page |
| 2 | Clicks "Change Password" | Shows password form |
| 3 | Enters current + new password | Validates requirements |
| 4 | Clicks "Update" | Changes password, invalidates other sessions |

---

### UF-9: Account Deletion

**Implements:** FR-X (Account Management)

**Steps:**

| Step | User Action | System Response |
|:-----|:------------|:----------------|
| 1 | Navigates to Account settings | Displays account page |
| 2 | Clicks "Delete Account" | Shows confirmation modal |
| 3 | Types confirmation phrase | Enables delete button |
| 4 | Clicks "Delete" | Schedules deletion, logs out |

**Data Handling:**
- 30-day grace period for recovery
- All personal data deleted after grace period
- Anonymized data may be retained for analytics
```

## 5. Error & Edge Case Flows

```markdown
## 5. Error & Edge Case Flows

### UF-E1: Session Expiry

**Trigger:** User session expires during activity

**Flow:**
1. User performs action
2. System detects expired session
3. Modal appears: "Your session has expired"
4. User clicks "Log In"
5. User re-authenticates
6. System restores previous context (if possible)

---

### UF-E2: Network Error

**Trigger:** Network request fails

**Flow:**
1. User performs action
2. Request fails
3. Toast notification: "Network error. Retrying..."
4. System retries (up to 3 times)
5. If still failing: "Could not connect. Check your connection"
6. User can click "Retry" manually

---

### UF-E3: Permission Denied

**Trigger:** User tries to access restricted resource

**Flow:**
1. User navigates to restricted page
2. System checks permissions
3. Displays: "You don't have access to this page"
4. Offers: "Request Access" or "Go Back"
```

## 6. Mobile/Responsive Considerations

```markdown
## 6. Mobile/Responsive Considerations

### Touch Interactions
- All clickable areas minimum 44x44px
- Swipe gestures for navigation where appropriate
- Long-press for context menus

### Mobile-Specific Flows
| Desktop Flow | Mobile Adaptation |
|:-------------|:------------------|
| Hover tooltips | Tap to reveal |
| Multi-column forms | Single column, collapsible sections |
| Drag and drop | Touch-friendly alternatives |

### Breakpoint Behaviors
- **Desktop (>1024px):** Full feature set
- **Tablet (768-1024px):** Collapsible sidebar
- **Mobile (<768px):** Bottom navigation, simplified views
```

## Document Footer

```markdown
---

## Appendix

### A. Flow Diagram Legend
| Symbol | Meaning |
|:-------|:--------|
| Rectangle | Screen/Page |
| Arrow | Navigation/Transition |
| Diamond | Decision point |
| Circle | Start/End point |

### B. Revision History
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
- All user flows: UF-1, UF-2, UF-3... (sequential)
- Error flows: UF-E1, UF-E2, UF-E3
- Alternative paths within flows: A1, A2, A3
- Error states within flows: E1, E2, E3

### Cross-Reference Format
- "Implements FR-1, FR-2 (PRD)"
- "Uses TS-7 authentication (Tech Stack)"
- "Related to UF-2 login flow"
