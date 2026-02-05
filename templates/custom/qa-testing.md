# QA Testing Strategy Template

## Document Header

```markdown
# [Project Name] - QA Testing Strategy

**Version:** 1.0
**Last Updated:** [Date]
**Test Framework:** [Jest | Vitest | pytest | etc.]
**E2E Framework:** [Playwright | Cypress | etc.]

---
```

## Template Structure

```markdown
## 1. Testing Philosophy

### 1.1 Overview
[Brief description of testing approach and goals]

### 1.2 Testing Pyramid
```
        ╱╲
       ╱  ╲
      ╱ E2E╲        10% - Critical user journeys
     ╱──────╲
    ╱        ╲
   ╱Integration╲    20% - API & service integration
  ╱────────────╲
 ╱              ╲
╱   Unit Tests   ╲  70% - Functions, components
╲────────────────╱
```

### 1.3 Coverage Targets
| Test Type | Target Coverage | Priority |
|:----------|:----------------|:---------|
| Unit Tests | 80% | High |
| Integration Tests | Key flows | Medium |
| E2E Tests | Critical paths | High |

---

## 2. Test Environment

### 2.1 Environments
| Environment | Database | External Services |
|:------------|:---------|:------------------|
| Local | SQLite/Docker | Mocked |
| CI | Docker Postgres | Mocked |
| Staging | Staging DB | Sandbox APIs |

### 2.2 Test Data Strategy
- **Factories:** Use test factories for consistent data generation
- **Fixtures:** Static fixtures for complex scenarios
- **Seeding:** Database seeded before test suites
- **Cleanup:** Data reset between test runs

---

## 3. Unit Testing

### 3.1 Framework Configuration
**Framework:** [Jest/Vitest]

```javascript
// vitest.config.ts
export default {
  test: {
    coverage: {
      provider: 'v8',
      reporter: ['text', 'html'],
      threshold: { lines: 80, branches: 75 }
    }
  }
}
```

### 3.2 Testing Patterns

**Component Testing (Frontend):**
```typescript
describe('Button', () => {
  it('renders with correct text', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByText('Click me')).toBeInTheDocument();
  });

  it('calls onClick when clicked', async () => {
    const onClick = vi.fn();
    render(<Button onClick={onClick}>Click</Button>);
    await userEvent.click(screen.getByRole('button'));
    expect(onClick).toHaveBeenCalledOnce();
  });
});
```

**Service Testing (Backend):**
```typescript
describe('UserService', () => {
  it('creates user with hashed password', async () => {
    const user = await userService.create({
      email: 'test@example.com',
      password: 'password123'
    });

    expect(user.passwordHash).not.toBe('password123');
    expect(await bcrypt.compare('password123', user.passwordHash)).toBe(true);
  });
});
```

### 3.3 Test Cases by Feature

#### Authentication (FR-1, FR-2, FR-3)

| ID | Test Case | Expected Result |
|:---|:----------|:----------------|
| UT-AUTH-1 | Register with valid data | User created, verification email sent |
| UT-AUTH-2 | Register with existing email | Error: Email already exists |
| UT-AUTH-3 | Login with valid credentials | Token returned |
| UT-AUTH-4 | Login with invalid password | Error: Invalid credentials |
| UT-AUTH-5 | Password reset request | Reset email sent |

#### [Feature 2] (FR-X)

| ID | Test Case | Expected Result |
|:---|:----------|:----------------|
| UT-[FEAT]-1 | [Test case] | [Expected] |
| UT-[FEAT]-2 | [Test case] | [Expected] |

---

## 4. Integration Testing

### 4.1 API Integration Tests

**Framework:** Supertest / HTTPie

```typescript
describe('POST /api/auth/register', () => {
  it('creates user and returns token', async () => {
    const response = await request(app)
      .post('/api/auth/register')
      .send({
        email: 'new@example.com',
        password: 'Password123',
        name: 'Test User'
      });

    expect(response.status).toBe(201);
    expect(response.body.data.user.email).toBe('new@example.com');
  });

  it('returns 409 for duplicate email', async () => {
    // Create user first
    await createUser({ email: 'existing@example.com' });

    const response = await request(app)
      .post('/api/auth/register')
      .send({ email: 'existing@example.com', password: 'Pass123' });

    expect(response.status).toBe(409);
  });
});
```

### 4.2 Database Integration Tests

| ID | Test Case | Tables | Expected |
|:---|:----------|:-------|:---------|
| IT-DB-1 | User cascade delete | users, sessions | Sessions deleted with user |
| IT-DB-2 | Foreign key constraint | projects, tasks | Task creation fails without project |

---

## 5. End-to-End Testing

### 5.1 Framework Configuration
**Framework:** Playwright

```typescript
// playwright.config.ts
export default {
  testDir: './e2e',
  use: {
    baseURL: 'http://localhost:3000',
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
  },
  projects: [
    { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
    { name: 'mobile', use: { ...devices['iPhone 13'] } },
  ],
};
```

### 5.2 Critical User Journeys

#### E2E-1: User Registration Flow (UF-1)

**Implements:** FR-1, UF-1

```typescript
test('user can register and verify email', async ({ page }) => {
  await page.goto('/signup');

  await page.fill('[name="email"]', 'test@example.com');
  await page.fill('[name="password"]', 'Password123');
  await page.fill('[name="name"]', 'Test User');
  await page.click('button[type="submit"]');

  await expect(page.getByText('Check your email')).toBeVisible();

  // Simulate email verification
  const verificationLink = await getVerificationLink('test@example.com');
  await page.goto(verificationLink);

  await expect(page.getByText('Welcome')).toBeVisible();
});
```

#### E2E-2: User Login Flow (UF-2)

**Implements:** FR-2, UF-2

| Step | Action | Expected |
|:-----|:-------|:---------|
| 1 | Navigate to /login | Login form displayed |
| 2 | Enter valid credentials | Credentials accepted |
| 3 | Click "Sign In" | Redirected to dashboard |
| 4 | Check session | User is authenticated |

#### E2E-3: [Core Feature Flow] (UF-X)

[Document critical feature flows...]

### 5.3 E2E Test Matrix

| Test ID | Flow | Desktop | Mobile | Priority |
|:--------|:-----|:--------|:-------|:---------|
| E2E-1 | Registration | ✅ | ✅ | P0 |
| E2E-2 | Login | ✅ | ✅ | P0 |
| E2E-3 | [Feature] | ✅ | ✅ | P0 |
| E2E-4 | [Feature] | ✅ | ⬜ | P1 |

---

## 6. Performance Testing

### 6.1 Load Testing
**Tool:** k6

```javascript
// load-test.js
export default function () {
  http.get('https://api.example.com/health');
}

export let options = {
  vus: 100,
  duration: '5m',
  thresholds: {
    http_req_duration: ['p(95)<200'],
  },
};
```

### 6.2 Performance Targets
| Metric | Target | Method |
|:-------|:-------|:-------|
| API p95 latency | < 200ms | k6 load test |
| Page load (LCP) | < 2.5s | Lighthouse |
| Time to Interactive | < 3.5s | Lighthouse |

---

## 7. CI/CD Integration

### 7.1 Pipeline Stages
```yaml
test:
  - unit-tests        # Run on every push
  - integration-tests # Run on every push
  - e2e-tests        # Run on PR to main
  - performance-tests # Run weekly
```

### 7.2 GitHub Actions Example
```yaml
name: Tests
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npm run test:unit
      - run: npm run test:integration
      - run: npm run test:e2e
```

---

## 8. Bug Tracking

### 8.1 Bug Report Template
```markdown
**Summary:** [Brief description]
**Steps to Reproduce:**
1. [Step 1]
2. [Step 2]
**Expected:** [Expected behavior]
**Actual:** [Actual behavior]
**Environment:** [Browser, OS, etc.]
**Screenshots:** [If applicable]
```

### 8.2 Severity Levels
| Level | Description | Response Time |
|:------|:------------|:--------------|
| Critical | System down, data loss | Immediate |
| High | Major feature broken | Same day |
| Medium | Feature degraded | Within sprint |
| Low | Minor issue | Backlog |

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
- "Tests FR-X (PRD)"
- "Covers UF-X flow (User Flows)"
- "Validates DB-X schema (Database Schema)"
