# Performance Optimization Template

```markdown
# [Project Name] - Performance Optimization Guide

**Version:** 1.0
**Last Updated:** [Date]

---

## 1. Performance Goals

### 1.1 Core Web Vitals Targets
| Metric | Target | Current |
|:-------|:-------|:--------|
| LCP (Largest Contentful Paint) | < 2.5s | - |
| FID (First Input Delay) | < 100ms | - |
| CLS (Cumulative Layout Shift) | < 0.1 | - |
| TTFB (Time to First Byte) | < 200ms | - |

### 1.2 API Performance Targets
| Metric | Target |
|:-------|:-------|
| p50 Response Time | < 100ms |
| p95 Response Time | < 200ms |
| p99 Response Time | < 500ms |
| Error Rate | < 0.1% |

---

## 2. Frontend Optimization

### 2.1 Code Splitting
**Implements:** NFR-1

```typescript
// Route-based splitting
const Dashboard = lazy(() => import('./pages/Dashboard'));
const Settings = lazy(() => import('./pages/Settings'));

// Component-based splitting
const HeavyChart = lazy(() => import('./components/HeavyChart'));
```

### 2.2 Asset Optimization

**Images:**
- Use WebP format with JPEG fallback
- Implement responsive images with srcset
- Lazy load below-fold images
- Use CDN for image delivery

```html
<img
  src="image.webp"
  srcset="image-400.webp 400w, image-800.webp 800w"
  sizes="(max-width: 600px) 400px, 800px"
  loading="lazy"
  alt="Description"
/>
```

**JavaScript:**
- Tree shaking enabled in bundler
- Minification in production
- Compression (Brotli/Gzip)

**CSS:**
- PurgeCSS for unused styles
- Critical CSS inlining
- CSS-in-JS with extraction

### 2.3 Caching Strategy

**Static Assets:**
```
Cache-Control: public, max-age=31536000, immutable
```

**API Responses:**
```
Cache-Control: private, max-age=60
ETag: "abc123"
```

### 2.4 Bundle Analysis
- Maximum initial bundle: 200KB (gzipped)
- Maximum lazy chunk: 100KB (gzipped)
- No single dependency > 50KB

---

## 3. Backend Optimization

### 3.1 Database Optimization

**Indexing Strategy:**
```sql
-- Frequently queried columns
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_projects_user_id ON projects(user_id);

-- Composite indexes for common queries
CREATE INDEX idx_tasks_project_status ON tasks(project_id, status);

-- Partial indexes
CREATE INDEX idx_active_users ON users(created_at)
WHERE deleted_at IS NULL;
```

**Query Optimization:**
- Use EXPLAIN ANALYZE for slow queries
- Avoid N+1 queries (use joins or batch loading)
- Paginate large result sets
- Use database connection pooling

### 3.2 Caching Layer

**Redis Caching:**
```typescript
// Cache frequently accessed data
async function getUser(id: string) {
  const cacheKey = `user:${id}`;
  const cached = await redis.get(cacheKey);

  if (cached) return JSON.parse(cached);

  const user = await db.users.findById(id);
  await redis.setex(cacheKey, 3600, JSON.stringify(user));

  return user;
}
```

**Cache Invalidation:**
| Event | Action |
|:------|:-------|
| User updated | Delete user:* keys |
| Project updated | Delete project:* keys |
| [Event] | [Action] |

### 3.3 API Optimization

**Response Compression:**
```typescript
app.use(compression({
  level: 6,
  threshold: 1024,
  filter: (req, res) => {
    return compression.filter(req, res);
  }
}));
```

**Rate Limiting:**
```typescript
const limiter = rateLimit({
  windowMs: 60 * 1000,
  max: 100,
  standardHeaders: true,
});
```

---

## 4. Infrastructure Optimization

### 4.1 CDN Configuration
- Static assets served from CDN edge
- API responses cached at edge (where appropriate)
- Geographic distribution for global users

### 4.2 Auto-Scaling
- Horizontal scaling based on CPU/memory
- Database read replicas for heavy read loads
- Queue workers scale with queue depth

### 4.3 Connection Optimization
- HTTP/2 enabled
- Keep-alive connections
- Connection pooling for databases

---

## 5. Monitoring & Alerting

### 5.1 Performance Monitoring
| Tool | Purpose |
|:-----|:--------|
| Lighthouse CI | Core Web Vitals |
| DataDog/New Relic | APM |
| Custom metrics | Business KPIs |

### 5.2 Alerts
| Metric | Threshold | Action |
|:-------|:----------|:-------|
| p95 latency | > 500ms | Page on-call |
| Error rate | > 1% | Slack alert |
| Memory usage | > 80% | Auto-scale |

---

## 6. Performance Checklist

### 6.1 Before Launch
- [ ] Run Lighthouse audit (score > 90)
- [ ] Test on slow 3G connection
- [ ] Verify CDN configuration
- [ ] Enable compression
- [ ] Set up monitoring

### 6.2 Ongoing
- [ ] Weekly performance review
- [ ] Monthly bundle analysis
- [ ] Quarterly load testing

---

*Generated with [BuildMVPFast](https://buildmvpfast.com) Project Docs*
```
