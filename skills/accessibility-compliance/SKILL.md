---
name: accessibility-compliance
description: Dedicated skill to audit and refactor web applications for Web Content Accessibility Guidelines (WCAG) 2.1 AA/AAA compliance, including keyboard navigation, ARIA attributes, semantic HTML, and screen reader compatibility.
---
# Accessibility Compliance (a11y) Specialist

This skill provides step-by-step guidance on designing and implementing interfaces that are fully accessible to individuals with visual, auditory, motor, or cognitive disabilities.

## Key Compliance Guidelines

### 1. Semantic HTML Over "Div Soup"
- Always use semantic tags rather than generic `<div>` or `<span>` elements for structural elements:
  - `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, `<footer>`.
- **Buttons vs. Links:** Use `<button>` for in-page actions (like triggering modals or form submissions) and `<a>` (with a valid `href`) for URL navigation. Do not place click handlers on raw `<div>` or `<span>` elements.

### 2. Keyboard Navigation & Focus Management
- **Focus Rings:** Never remove focus outlines (`outline: none` or `outline: 0`) without providing a distinct custom focus style (using `:focus-visible`).
  ```css
  .button-custom:focus-visible {
    outline: 2px solid var(--accent-primary);
    outline-offset: 4px;
  }
  ```
- **Tab Order:** Ensure standard keyboard tab flow matches visual ordering. Avoid positive `tabindex` values; keep items focusable naturally or use `tabindex="0"` for custom interactive widgets.
- **Focus Trapping:** When modals, drawers, or dialog boxes are opened, keyboard focus must be trapped within that element and returned to the trigger button upon closure.

### 3. ARIA Attributes & Accessible Names
- **Rule 1 of ARIA:** If you can use a native HTML element instead, do so.
- **Alt Text:** Every image (`<img>`) must have an `alt` attribute. Use descriptive alt text for informative images, and empty alt text (`alt=""`) for purely decorative images.
- **Accessible Labels:** Provide explanatory text for icon-only buttons using `aria-label` or `aria-labelledby`.
  ```html
  <button aria-label="Close menu" onclick="closeMenu()">
    <svg>...</svg>
  </button>
  ```
- **State Indicators:** Use `aria-expanded="true/false"` for accordion/menu triggers, and `aria-hidden="true"` to hide decorative or offscreen elements from screen readers.

### 4. Color Contrast & Motion
- **Contrast Ratios:** Ensure text color meets the minimum WCAG 2.1 AA requirement of **4.5:1** contrast against its background (or **3:1** for large text/headings).
- **Reduced Motion Support:** Respect user preference for reduced motion using media queries:
  ```css
  @media (prefers-reduced-motion: reduce) {
    * {
      animation-duration: 0.01ms !important;
      animation-iteration-count: 1 !important;
      transition-duration: 0.01ms !important;
      scroll-behavior: auto !important;
    }
  }
  ```

---

## Auditing & Verification Workflow

1. **Keyboard-Only Test:** Unplug/disable the mouse and navigate the entire application page using only `Tab`, `Shift + Tab`, `Enter`, `Space`, and Arrow keys. Ensure all controls are fully reachable and interactive.
2. **Automated Audits:** Run Lighthouse Accessibility audits or use axe-core CLI integrations in E2E tests to identify structural errors.
3. **Screen Reader Verification:** Test application layouts with common screen readers (VoiceOver on macOS, NVDA or Narrator on Windows) to verify screen reader pronunciation and linear document flow.
