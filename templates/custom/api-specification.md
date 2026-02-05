# API Specification Template

## Document Header

```markdown
# [Project Name] - API Specification

**Version:** 1.0
**Last Updated:** [Date]
**API Version:** v1
**Base URL:** https://api.[domain].com/v1

---
```

## Template Structure

```markdown
## 1. Overview

### 1.1 Introduction
[Brief description of the API and its purpose]

### 1.2 Base URL
| Environment | URL |
|:------------|:----|
| Production | `https://api.[domain].com/v1` |
| Staging | `https://api.staging.[domain].com/v1` |
| Development | `http://localhost:3000/api/v1` |

### 1.3 Response Format
All responses follow this structure:
```json
{
  "success": true,
  "data": { },
  "meta": {
    "timestamp": "2025-01-05T12:00:00Z",
    "requestId": "req_abc123"
  },
  "error": null
}
```

Error Response:
```json
{
  "success": false,
  "data": null,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Email is required",
    "details": [
      { "field": "email", "message": "Required" }
    ]
  }
}
```

---

## 2. Authentication

### 2.1 Overview
**Implements:** FR-2, TS-7

Authentication is handled via Bearer tokens (JWT).

### 2.2 Obtaining a Token

**POST** `/auth/login`

**Request:**
```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "accessToken": "eyJhbG...",
    "refreshToken": "eyJhbG...",
    "expiresIn": 3600,
    "tokenType": "Bearer"
  }
}
```

### 2.3 Using the Token

Include in all authenticated requests:
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

### 2.4 Refreshing Tokens

**POST** `/auth/refresh`

**Request:**
```json
{
  "refreshToken": "eyJhbG..."
}
```

---

## 3. Endpoints

### 3.1 Authentication Endpoints

#### POST /auth/register
**Implements:** FR-1

Create a new user account.

**Request Body:**
| Field | Type | Required | Description |
|:------|:-----|:---------|:------------|
| email | string | Yes | Valid email address |
| password | string | Yes | Min 8 chars, mixed case, number |
| name | string | Yes | Display name |

**Example Request:**
```bash
curl -X POST https://api.example.com/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email":"user@example.com","password":"Password123","name":"John"}'
```

**Success Response (201):**
```json
{
  "success": true,
  "data": {
    "user": {
      "id": "usr_123abc",
      "email": "user@example.com",
      "name": "John",
      "createdAt": "2025-01-05T12:00:00Z"
    },
    "message": "Verification email sent"
  }
}
```

**Error Responses:**
| Code | Description |
|:-----|:------------|
| 400 | Validation error (invalid email, weak password) |
| 409 | Email already registered |

---

#### POST /auth/login
**Implements:** FR-2

Authenticate a user.

[Same structure as above...]

---

### 3.2 [Resource] Endpoints

#### GET /[resources]
**Implements:** FR-X
**Auth Required:** Yes

List all [resources] for the authenticated user.

**Query Parameters:**
| Param | Type | Default | Description |
|:------|:-----|:--------|:------------|
| page | integer | 1 | Page number |
| limit | integer | 20 | Items per page (max 100) |
| sort | string | createdAt | Sort field |
| order | string | desc | Sort order (asc/desc) |
| search | string | - | Search query |
| status | string | - | Filter by status |

**Example Request:**
```bash
curl -X GET "https://api.example.com/v1/resources?page=1&limit=10" \
  -H "Authorization: Bearer {token}"
```

**Success Response (200):**
```json
{
  "success": true,
  "data": [
    {
      "id": "res_123",
      "name": "Example",
      "status": "active",
      "createdAt": "2025-01-05T12:00:00Z"
    }
  ],
  "meta": {
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 45,
      "totalPages": 5
    }
  }
}
```

---

#### POST /[resources]
**Implements:** FR-X
**Auth Required:** Yes

Create a new [resource].

**Request Body:**
| Field | Type | Required | Description |
|:------|:-----|:---------|:------------|
| name | string | Yes | Resource name |
| description | string | No | Description |
| [field] | [type] | [required] | [description] |

**Success Response (201):**
```json
{
  "success": true,
  "data": {
    "id": "res_456",
    "name": "New Resource",
    "createdAt": "2025-01-05T12:00:00Z"
  }
}
```

---

#### GET /[resources]/:id
**Implements:** FR-X
**Auth Required:** Yes

Get a single [resource] by ID.

**Path Parameters:**
| Param | Type | Description |
|:------|:-----|:------------|
| id | string | Resource ID |

**Success Response (200):**
```json
{
  "success": true,
  "data": {
    "id": "res_123",
    "name": "Example",
    "description": "Full details...",
    "createdAt": "2025-01-05T12:00:00Z",
    "updatedAt": "2025-01-05T12:00:00Z"
  }
}
```

**Error Responses:**
| Code | Description |
|:-----|:------------|
| 404 | Resource not found |
| 403 | Access denied |

---

#### PUT /[resources]/:id
**Implements:** FR-X
**Auth Required:** Yes

Update a [resource].

[Same structure...]

---

#### DELETE /[resources]/:id
**Implements:** FR-X
**Auth Required:** Yes

Delete a [resource].

**Success Response (204):**
No content

---

## 4. Error Codes

| Code | HTTP Status | Description |
|:-----|:------------|:------------|
| VALIDATION_ERROR | 400 | Request validation failed |
| UNAUTHORIZED | 401 | Authentication required |
| FORBIDDEN | 403 | Insufficient permissions |
| NOT_FOUND | 404 | Resource not found |
| CONFLICT | 409 | Resource already exists |
| RATE_LIMITED | 429 | Too many requests |
| INTERNAL_ERROR | 500 | Server error |

---

## 5. Rate Limiting

| Endpoint Group | Limit | Window |
|:---------------|:------|:-------|
| Authentication | 10 requests | 1 minute |
| API (authenticated) | 100 requests | 1 minute |
| API (unauthenticated) | 20 requests | 1 minute |

Rate limit headers included in all responses:
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1704456000
```

---

## 6. Webhooks

### 6.1 Overview
[If applicable, document webhooks]

### 6.2 Events
| Event | Description |
|:------|:------------|
| resource.created | New resource created |
| resource.updated | Resource modified |
| resource.deleted | Resource removed |

### 6.3 Payload Format
```json
{
  "event": "resource.created",
  "timestamp": "2025-01-05T12:00:00Z",
  "data": { }
}
```

---

*Generated with [BuildMVPFast](https://buildmvpfast.com) Project Docs*
```

---

## Generation Guidelines

### Word Count Target
- **Minimum:** 1,000 words
- **Target:** 1,500-2,000 words
- **Maximum:** 2,000 words

### Cross-Reference Format
- "Implements FR-X (PRD)"
- "Uses TS-7 authentication (Tech Stack)"
- "Related to DB-X schema (Database Schema)"
