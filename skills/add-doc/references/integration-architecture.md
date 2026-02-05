# Integration Architecture Template

```markdown
# [Project Name] - Integration Architecture

**Version:** 1.0
**Last Updated:** [Date]

---

## 1. Overview

### 1.1 Integration Philosophy
[Brief description of integration approach]

### 1.2 Integration Map
```
┌─────────────────────────────────────────────────────┐
│                    [Project Name]                    │
├─────────────────────────────────────────────────────┤
│                                                      │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐             │
│  │  Auth   │  │  Email  │  │ Payment │             │
│  │ (OAuth) │  │ Service │  │ Gateway │             │
│  └────┬────┘  └────┬────┘  └────┬────┘             │
│       │            │            │                   │
└───────┼────────────┼────────────┼───────────────────┘
        │            │            │
        ▼            ▼            ▼
   ┌─────────┐  ┌─────────┐  ┌─────────┐
   │  Google │  │ SendGrid│  │  Stripe │
   │  GitHub │  │ Postmark│  │  PayPal │
   └─────────┘  └─────────┘  └─────────┘
```

---

## 2. Authentication Integrations

### 2.1 Google OAuth
**Implements:** FR-1, FR-2, TS-7

**Purpose:** Social login with Google accounts

**Configuration:**
| Setting | Value |
|:--------|:------|
| Provider | Google OAuth 2.0 |
| Scopes | email, profile |
| Callback URL | /api/auth/callback/google |

**Environment Variables:**
```bash
GOOGLE_CLIENT_ID=your_client_id
GOOGLE_CLIENT_SECRET=your_client_secret
```

**Data Flow:**
1. User clicks "Sign in with Google"
2. Redirect to Google OAuth consent screen
3. User authorizes, Google redirects to callback
4. Backend exchanges code for tokens
5. Fetch user profile, create/link account
6. Issue session/JWT

**Error Handling:**
| Error | Cause | Resolution |
|:------|:------|:-----------|
| access_denied | User declined | Show "Permission required" |
| invalid_grant | Code expired | Restart OAuth flow |

---

### 2.2 GitHub OAuth
**Implements:** FR-1, FR-2, TS-7

[Same structure as Google OAuth...]

---

## 3. Email Service Integration

### 3.1 [Email Provider - SendGrid/Postmark/Resend]
**Implements:** FR-X, TS-14

**Purpose:** Transactional email delivery

**Configuration:**
| Setting | Value |
|:--------|:------|
| Provider | [SendGrid/Postmark/Resend] |
| API Version | v3 |
| From Address | noreply@[domain].com |

**Environment Variables:**
```bash
EMAIL_PROVIDER=sendgrid
SENDGRID_API_KEY=SG.xxxxx
EMAIL_FROM=noreply@example.com
```

**Email Templates:**
| Template | Trigger | Variables |
|:---------|:--------|:----------|
| Welcome | User registration | name, verifyUrl |
| Password Reset | Reset request | name, resetUrl |
| [Template] | [Trigger] | [Variables] |

**Implementation:**
```typescript
async function sendEmail(template: string, to: string, variables: object) {
  await emailClient.send({
    to,
    from: process.env.EMAIL_FROM,
    templateId: templates[template],
    dynamicTemplateData: variables
  });
}
```

**Rate Limits:**
- 100 emails/second (SendGrid)
- Implement queue for bulk sends

---

## 4. Payment Integration

### 4.1 Stripe
**Implements:** FR-X (Payments), TS-15

**Purpose:** Payment processing, subscriptions

**Configuration:**
| Setting | Value |
|:--------|:------|
| API Version | 2024-01-01 |
| Mode | Test / Live |
| Webhook Endpoint | /api/webhooks/stripe |

**Environment Variables:**
```bash
STRIPE_SECRET_KEY=sk_test_xxxxx
STRIPE_PUBLISHABLE_KEY=pk_test_xxxxx
STRIPE_WEBHOOK_SECRET=whsec_xxxxx
```

**Webhook Events:**
| Event | Action |
|:------|:-------|
| checkout.session.completed | Activate subscription |
| invoice.paid | Update billing status |
| invoice.payment_failed | Notify user, retry |
| customer.subscription.deleted | Deactivate subscription |

**Webhook Handler:**
```typescript
app.post('/api/webhooks/stripe', async (req, res) => {
  const sig = req.headers['stripe-signature'];
  const event = stripe.webhooks.constructEvent(
    req.body,
    sig,
    process.env.STRIPE_WEBHOOK_SECRET
  );

  switch (event.type) {
    case 'checkout.session.completed':
      await handleCheckoutComplete(event.data.object);
      break;
    // ... handle other events
  }

  res.json({ received: true });
});
```

**Security:**
- Verify webhook signatures
- Use idempotency keys for charges
- PCI compliance via Stripe Elements

---

## 5. File Storage Integration

### 5.1 [Storage Provider - S3/R2/Supabase]
**Implements:** FR-X, TS-16

**Purpose:** User file uploads, generated exports

**Configuration:**
| Setting | Value |
|:--------|:------|
| Provider | [AWS S3 / Cloudflare R2] |
| Bucket | [bucket-name] |
| Region | [region] |

**Environment Variables:**
```bash
S3_BUCKET=your-bucket
S3_REGION=us-east-1
AWS_ACCESS_KEY_ID=xxxxx
AWS_SECRET_ACCESS_KEY=xxxxx
```

**Upload Flow:**
1. Client requests presigned URL
2. Backend generates URL with expiry
3. Client uploads directly to S3
4. Client notifies backend of completion
5. Backend stores file reference

**File Types Allowed:**
| Type | Max Size | Use Case |
|:-----|:---------|:---------|
| image/* | 5MB | Profile photos |
| application/pdf | 10MB | Documents |
| [type] | [size] | [use case] |

---

## 6. Analytics Integration

### 6.1 [Analytics Provider]
**Implements:** TS-18

**Purpose:** User behavior tracking, conversion metrics

**Configuration:**
```javascript
// Client-side
analytics.init({ writeKey: process.env.ANALYTICS_KEY });
```

**Events Tracked:**
| Event | Properties | Trigger |
|:------|:-----------|:--------|
| page_viewed | path, title | Page load |
| signed_up | method | Registration complete |
| feature_used | feature_name | Feature interaction |
| subscription_started | plan, price | Payment complete |

---

## 7. Error Monitoring

### 7.1 Sentry
**Implements:** TS-17

**Purpose:** Error tracking, performance monitoring

**Configuration:**
```typescript
Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: 0.1,
});
```

**Captured Data:**
- Stack traces
- Request context
- User context (anonymized)
- Breadcrumbs

---

## 8. Integration Security

### 8.1 Secrets Management
- All API keys stored as environment variables
- Secrets rotated quarterly
- Principle of least privilege

### 8.2 Webhook Security
- Verify signatures on all webhooks
- Use HTTPS endpoints only
- Implement replay protection

### 8.3 Rate Limiting
| Integration | Rate Limit | Handling |
|:------------|:-----------|:---------|
| Email | 100/sec | Queue excess |
| Payment | 25/sec | Retry with backoff |
| Storage | 100/sec | Client-side throttle |

---

## 9. Monitoring & Alerts

### 9.1 Health Checks
| Integration | Health Endpoint | Interval |
|:------------|:----------------|:---------|
| Database | SELECT 1 | 30s |
| Redis | PING | 30s |
| [Service] | [Endpoint] | [Interval] |

### 9.2 Alerts
| Condition | Severity | Action |
|:----------|:---------|:-------|
| Integration down | Critical | Page on-call |
| High error rate | High | Slack notification |
| Rate limit approaching | Medium | Email warning |

---

*Generated with [BuildMVPFast](https://buildmvpfast.com) Project Docs*
```
