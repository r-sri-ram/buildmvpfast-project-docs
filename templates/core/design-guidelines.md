# Design System & Guidelines Template

## Document Header

```markdown
# [Project Name] - Design System & Guidelines

**Version:** 1.0
**Last Updated:** [Date]
**Status:** Draft | In Review | Approved
**Design Tool:** [Figma | Sketch | Adobe XD]

---
```

## 1. Design Principles

```markdown
## 1. Design Principles

### DG-PRINCIPLE-1: Clarity Over Cleverness
Every element should serve a purpose. Avoid decorative elements that don't aid comprehension.

### DG-PRINCIPLE-2: Consistency
Maintain consistent patterns across all screens and interactions. Users should never have to relearn how things work.

### DG-PRINCIPLE-3: Progressive Disclosure
Show only what's needed at each step. Complex features should unfold naturally as users need them.

### DG-PRINCIPLE-4: Accessibility First
Design for all users. Every feature should be accessible via keyboard and screen readers.

### DG-PRINCIPLE-5: Mobile-First
Start with mobile constraints and scale up. Ensure core functionality works on any device.
```

## 2. Color System

```markdown
## 2. Color System

### DG-1: Primary Colors

| Name | Hex | RGB | Usage |
|:-----|:----|:----|:------|
| Primary | #3B82F6 | 59, 130, 246 | Main actions, links, active states |
| Primary Dark | #2563EB | 37, 99, 235 | Hover states, emphasis |
| Primary Light | #93C5FD | 147, 197, 253 | Backgrounds, highlights |

### DG-2: Neutral Colors

| Name | Hex | RGB | Usage |
|:-----|:----|:----|:------|
| Gray 900 | #111827 | 17, 24, 39 | Headings, primary text |
| Gray 700 | #374151 | 55, 65, 81 | Body text |
| Gray 500 | #6B7280 | 107, 114, 128 | Secondary text, icons |
| Gray 300 | #D1D5DB | 209, 213, 219 | Borders, dividers |
| Gray 100 | #F3F4F6 | 243, 244, 246 | Backgrounds |
| White | #FFFFFF | 255, 255, 255 | Cards, surfaces |

### DG-3: Semantic Colors

| Name | Hex | Usage |
|:-----|:----|:------|
| Success | #10B981 | Success messages, positive actions |
| Warning | #F59E0B | Warnings, cautions |
| Error | #EF4444 | Errors, destructive actions |
| Info | #3B82F6 | Information, tips |

### DG-4: Color Usage Guidelines

**Do:**
- Use primary color for main CTAs
- Use semantic colors consistently for their purpose
- Ensure 4.5:1 contrast ratio for text

**Don't:**
- Use more than 3 colors per screen
- Use red for non-destructive actions
- Use low-contrast color combinations
```

## 3. Typography

```markdown
## 3. Typography

### DG-5: Font Family

**Primary:** Inter (or system-ui fallback)
```css
font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
```

**Monospace:** JetBrains Mono (for code)
```css
font-family: 'JetBrains Mono', 'Fira Code', monospace;
```

### DG-6: Type Scale

| Name | Size | Weight | Line Height | Usage |
|:-----|:-----|:-------|:------------|:------|
| Display | 48px / 3rem | 700 | 1.1 | Hero headlines |
| H1 | 36px / 2.25rem | 700 | 1.2 | Page titles |
| H2 | 30px / 1.875rem | 600 | 1.25 | Section headers |
| H3 | 24px / 1.5rem | 600 | 1.3 | Subsection headers |
| H4 | 20px / 1.25rem | 600 | 1.4 | Card titles |
| Body Large | 18px / 1.125rem | 400 | 1.6 | Intro text |
| Body | 16px / 1rem | 400 | 1.5 | Default body text |
| Body Small | 14px / 0.875rem | 400 | 1.5 | Secondary text |
| Caption | 12px / 0.75rem | 400 | 1.4 | Labels, captions |

### DG-7: Typography Guidelines

**Headings:**
- Use sentence case ("Create new project" not "Create New Project")
- Keep headings under 60 characters
- Use only one H1 per page

**Body Text:**
- Maximum line length: 65-75 characters
- Paragraph spacing: 1rem between paragraphs
- Use bold sparingly for emphasis
```

## 4. Spacing System

```markdown
## 4. Spacing System

### DG-8: Spacing Scale

Based on 4px base unit:

| Token | Value | Usage |
|:------|:------|:------|
| space-1 | 4px | Tight spacing (icon padding) |
| space-2 | 8px | Small gaps, icon margins |
| space-3 | 12px | Form element padding |
| space-4 | 16px | Default element spacing |
| space-5 | 20px | Card padding (small) |
| space-6 | 24px | Card padding (medium) |
| space-8 | 32px | Section spacing |
| space-10 | 40px | Large section spacing |
| space-12 | 48px | Page section dividers |
| space-16 | 64px | Major section spacing |

### DG-9: Layout Grid

**Desktop (>1024px):**
- Max content width: 1280px
- Columns: 12
- Gutter: 24px
- Margin: 32px

**Tablet (768-1024px):**
- Columns: 8
- Gutter: 20px
- Margin: 24px

**Mobile (<768px):**
- Columns: 4
- Gutter: 16px
- Margin: 16px
```

## 5. Components

```markdown
## 5. Components

### DG-10: Buttons

**Primary Button:**
```css
background: var(--primary);
color: white;
padding: 12px 24px;
border-radius: 8px;
font-weight: 500;
```

| State | Background | Text | Border |
|:------|:-----------|:-----|:-------|
| Default | Primary | White | None |
| Hover | Primary Dark | White | None |
| Active | Primary Dark | White | None |
| Disabled | Gray 300 | Gray 500 | None |

**Secondary Button:**
- Background: White
- Border: 1px solid Gray 300
- Text: Gray 700

**Destructive Button:**
- Background: Error
- Text: White
- Use sparingly, require confirmation

**Button Sizes:**
| Size | Padding | Font Size |
|:-----|:--------|:----------|
| Small | 8px 16px | 14px |
| Medium | 12px 24px | 16px |
| Large | 16px 32px | 18px |

### DG-11: Form Elements

**Text Input:**
```css
padding: 12px 16px;
border: 1px solid var(--gray-300);
border-radius: 8px;
font-size: 16px;
```

| State | Border Color | Background |
|:------|:-------------|:-----------|
| Default | Gray 300 | White |
| Focus | Primary | White |
| Error | Error | Error/10% |
| Disabled | Gray 200 | Gray 100 |

**Labels:**
- Position: Above input
- Font: 14px, 500 weight, Gray 700
- Required indicator: Red asterisk

**Error Messages:**
- Position: Below input
- Font: 14px, Error color
- Icon: Warning icon before text

### DG-12: Cards

**Default Card:**
```css
background: white;
border: 1px solid var(--gray-200);
border-radius: 12px;
padding: 24px;
box-shadow: 0 1px 3px rgba(0,0,0,0.1);
```

**Card Variants:**
| Variant | Background | Border | Shadow |
|:--------|:-----------|:-------|:-------|
| Default | White | Gray 200 | Small |
| Elevated | White | None | Medium |
| Outlined | Transparent | Gray 300 | None |

### DG-13: Navigation

**Top Navigation:**
- Height: 64px
- Background: White
- Border bottom: 1px Gray 200
- Logo: Left aligned
- Nav items: Center
- User menu: Right aligned

**Sidebar Navigation:**
- Width: 240px (expanded), 64px (collapsed)
- Background: Gray 50
- Active item: Primary background (10%), Primary text

### DG-14: Modals

**Modal Container:**
- Max width: 500px (small), 700px (medium), 900px (large)
- Border radius: 16px
- Padding: 24px
- Overlay: Black at 50% opacity

**Modal Structure:**
1. Header (title + close button)
2. Body (scrollable if needed)
3. Footer (actions, right-aligned)
```

## 6. Icons

```markdown
## 6. Icons

### DG-15: Icon Library
**Primary:** Lucide Icons (or Heroicons)

### DG-16: Icon Sizes
| Size | Dimensions | Usage |
|:-----|:-----------|:------|
| XS | 12px | Inline with small text |
| SM | 16px | Buttons, form elements |
| MD | 20px | Default, navigation |
| LG | 24px | Feature highlights |
| XL | 32px | Empty states, heroes |

### DG-17: Icon Guidelines
- Stroke width: 2px (default)
- Color: Inherit from text color
- Touch target: Minimum 44x44px
```

## 7. Motion & Animation

```markdown
## 7. Motion & Animation

### DG-18: Timing Functions

| Name | Value | Usage |
|:-----|:------|:------|
| ease-out | cubic-bezier(0, 0, 0.2, 1) | Enter animations |
| ease-in | cubic-bezier(0.4, 0, 1, 1) | Exit animations |
| ease-in-out | cubic-bezier(0.4, 0, 0.2, 1) | Move/resize |

### DG-19: Durations

| Name | Duration | Usage |
|:-----|:---------|:------|
| instant | 75ms | Micro-interactions (hover) |
| fast | 150ms | Buttons, toggles |
| normal | 200ms | Modals, dropdowns |
| slow | 300ms | Page transitions |

### DG-20: Animation Guidelines
- Prefer CSS transitions over JavaScript
- Respect `prefers-reduced-motion`
- Animate transforms and opacity only for performance
- Never animate layout properties (width, height, top, left)
```

## 8. Responsive Design

```markdown
## 8. Responsive Design

### DG-21: Breakpoints

| Name | Width | Description |
|:-----|:------|:------------|
| mobile | < 640px | Small phones |
| mobile-lg | 640px - 768px | Large phones |
| tablet | 768px - 1024px | Tablets |
| desktop | 1024px - 1280px | Laptops |
| desktop-lg | > 1280px | Large screens |

### DG-22: Responsive Patterns

**Stack → Row:**
```css
/* Mobile: Stack */
.container { flex-direction: column; }

/* Desktop: Row */
@media (min-width: 768px) {
  .container { flex-direction: row; }
}
```

**Hide/Show:**
- Navigation → Hamburger menu on mobile
- Sidebar → Off-canvas drawer on mobile
- Tables → Card layout on mobile
```

## Document Footer

```markdown
---

## Appendix

### A. Design Tokens (CSS Variables)
```css
:root {
  --color-primary: #3B82F6;
  --color-primary-dark: #2563EB;
  --color-success: #10B981;
  --color-warning: #F59E0B;
  --color-error: #EF4444;
  --font-family: 'Inter', sans-serif;
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --shadow-sm: 0 1px 2px rgba(0,0,0,0.05);
  --shadow-md: 0 4px 6px rgba(0,0,0,0.1);
}
```

### B. Accessibility Checklist
- [ ] Color contrast meets WCAG AA (4.5:1)
- [ ] Focus states visible for all interactive elements
- [ ] Form labels properly associated
- [ ] Alt text for all images
- [ ] Keyboard navigation works

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
- All design guidelines: DG-1, DG-2, DG-3... (sequential)
- Design principles: DG-PRINCIPLE-1, DG-PRINCIPLE-2

### Cross-Reference Format
- "Supports UF-1 registration flow (User Flows)"
- "Implements NFR-7 accessibility requirements (PRD)"
