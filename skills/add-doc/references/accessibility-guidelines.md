# Accessibility Guidelines Template

```markdown
# [Project Name] - Accessibility Guidelines

**Version:** 1.0
**Last Updated:** [Date]
**Standard:** WCAG 2.1 Level AA

---

## 1. Overview

### 1.1 Accessibility Commitment
[Project Name] is committed to ensuring digital accessibility for all users, including those with disabilities. We aim to meet WCAG 2.1 Level AA standards.

### 1.2 Compliance Target
| Criterion | Level | Status |
|:----------|:------|:-------|
| WCAG 2.1 | AA | [Target] |
| Section 508 | Full | [Target] |
| ADA | Compliant | [Target] |

### 1.3 Supported Assistive Technologies
| Technology | Support Level |
|:-----------|:--------------|
| NVDA (Windows) | Full |
| VoiceOver (macOS/iOS) | Full |
| JAWS (Windows) | Full |
| TalkBack (Android) | Full |

---

## 2. Perceivable

### 2.1 Text Alternatives (1.1)

**Images:**
```html
<!-- Informative images -->
<img src="chart.png" alt="Sales increased 25% from Q1 to Q2">

<!-- Decorative images -->
<img src="decoration.png" alt="" role="presentation">

<!-- Complex images -->
<figure>
  <img src="flowchart.png" alt="User registration process">
  <figcaption>
    Detailed description: Users enter email, verify...
  </figcaption>
</figure>
```

**Icons:**
```html
<!-- Icon with text -->
<button>
  <svg aria-hidden="true">...</svg>
  Save
</button>

<!-- Icon-only button -->
<button aria-label="Close dialog">
  <svg aria-hidden="true">...</svg>
</button>
```

### 2.2 Time-based Media (1.2)
- Video content: Provide captions
- Audio content: Provide transcripts
- Live content: Provide real-time captions where possible

### 2.3 Adaptable (1.3)

**Semantic HTML:**
```html
<!-- Use semantic elements -->
<header>
<nav>
<main>
<article>
<aside>
<footer>

<!-- Use proper heading hierarchy -->
<h1>Page Title</h1>
  <h2>Section</h2>
    <h3>Subsection</h3>
```

**Form Structure:**
```html
<form>
  <fieldset>
    <legend>Contact Information</legend>

    <label for="email">Email (required)</label>
    <input
      type="email"
      id="email"
      required
      aria-describedby="email-hint"
    >
    <span id="email-hint">We'll never share your email</span>
  </fieldset>
</form>
```

### 2.4 Distinguishable (1.4)

**Color Contrast:**
| Element | Minimum Ratio |
|:--------|:--------------|
| Normal text | 4.5:1 |
| Large text (18px+ or 14px bold) | 3:1 |
| UI components | 3:1 |

**Text Resizing:**
- Support 200% zoom without horizontal scrolling
- Use relative units (rem, em) not pixels for text
- Line height minimum 1.5 for body text

**Color Independence:**
- Never use color alone to convey information
- Add icons, patterns, or text labels

---

## 3. Operable

### 3.1 Keyboard Accessible (2.1)

**Focus Management:**
```css
/* Visible focus indicator */
:focus {
  outline: 2px solid var(--primary);
  outline-offset: 2px;
}

/* Don't remove focus outlines */
:focus:not(:focus-visible) {
  outline: none; /* Only for mouse users */
}
```

**Keyboard Navigation:**
| Key | Action |
|:----|:-------|
| Tab | Move to next focusable element |
| Shift+Tab | Move to previous element |
| Enter | Activate button/link |
| Space | Toggle checkbox, activate button |
| Arrow keys | Navigate within components |
| Escape | Close modal/dropdown |

**Skip Links:**
```html
<a href="#main-content" class="skip-link">
  Skip to main content
</a>
```

### 3.2 Enough Time (2.2)
- Session timeouts: Warn 2 minutes before, allow extension
- Auto-updating content: Provide pause/stop control
- No time limits on form completion

### 3.3 Seizures and Physical Reactions (2.3)
- No content flashes more than 3 times per second
- Avoid red flash saturation
- Provide motion control for animations

### 3.4 Navigable (2.4)

**Page Titles:**
```html
<title>Dashboard - [Project Name]</title>
<title>Settings > Profile - [Project Name]</title>
```

**Link Text:**
```html
<!-- Good -->
<a href="/docs">Read our documentation</a>

<!-- Bad -->
<a href="/docs">Click here</a>
```

**Breadcrumbs:**
```html
<nav aria-label="Breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/settings">Settings</a></li>
    <li aria-current="page">Profile</li>
  </ol>
</nav>
```

---

## 4. Understandable

### 4.1 Readable (3.1)

**Language Declaration:**
```html
<html lang="en">
```

**Reading Level:**
- Target 8th-grade reading level
- Define technical terms
- Avoid jargon where possible

### 4.2 Predictable (3.2)
- Consistent navigation across pages
- Consistent component behavior
- No unexpected context changes on focus

### 4.3 Input Assistance (3.3)

**Error Identification:**
```html
<input
  type="email"
  aria-invalid="true"
  aria-describedby="email-error"
>
<span id="email-error" role="alert">
  Please enter a valid email address
</span>
```

**Error Prevention:**
- Confirm before destructive actions
- Allow undo where possible
- Review before final submission

---

## 5. Robust

### 5.1 Compatible (4.1)

**Valid HTML:**
- Use W3C validator
- Proper nesting
- Unique IDs

**ARIA Usage:**
```html
<!-- Use native elements first -->
<button>Submit</button> <!-- Preferred -->
<div role="button" tabindex="0">Submit</div> <!-- Only if necessary -->

<!-- ARIA for dynamic content -->
<div role="status" aria-live="polite">
  Form saved successfully
</div>

<!-- ARIA for custom components -->
<div
  role="tablist"
  aria-label="Settings tabs"
>
  <button role="tab" aria-selected="true">General</button>
  <button role="tab" aria-selected="false">Security</button>
</div>
```

---

## 6. Component Patterns

### 6.1 Modal Dialog
```html
<div
  role="dialog"
  aria-modal="true"
  aria-labelledby="modal-title"
>
  <h2 id="modal-title">Confirm Delete</h2>
  <p>Are you sure you want to delete this item?</p>
  <button>Cancel</button>
  <button>Delete</button>
</div>
```

**Behavior:**
- Focus trapped within modal
- Escape key closes modal
- Focus returns to trigger on close

### 6.2 Dropdown Menu
```html
<button
  aria-haspopup="menu"
  aria-expanded="false"
>
  Options
</button>
<ul role="menu">
  <li role="menuitem">Edit</li>
  <li role="menuitem">Delete</li>
</ul>
```

### 6.3 Toast Notifications
```html
<div
  role="status"
  aria-live="polite"
  aria-atomic="true"
>
  Changes saved successfully
</div>
```

---

## 7. Testing Checklist

### 7.1 Automated Testing
- [ ] Run axe-core/Lighthouse
- [ ] Check color contrast
- [ ] Validate HTML

### 7.2 Manual Testing
- [ ] Keyboard-only navigation
- [ ] Screen reader testing
- [ ] Zoom to 200%
- [ ] Test with reduced motion

### 7.3 User Testing
- [ ] Include users with disabilities
- [ ] Test with real assistive technologies

---

## 8. Resources

**Testing Tools:**
- axe DevTools (browser extension)
- WAVE (browser extension)
- Lighthouse (Chrome DevTools)
- NVDA (free screen reader)

**References:**
- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/)

---

*Generated with [BuildMVPFast](https://buildmvpfast.com) Project Docs*
```
