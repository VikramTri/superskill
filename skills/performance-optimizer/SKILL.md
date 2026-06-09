---
name: performance-optimizer
description: Dedicated skill to audit and optimize web applications for speed, reducing bundle sizes, deferring non-essential assets, caching queries, and achieving high scores in core web vitals.
---
# Performance Optimizer Specialist

This skill directs the agent on how to optimize page load speeds, execution performance, rendering stability, and server-side responses.

## Key Optimization Vectors

### 1. Front-end Asset Optimization
- **Image Compression:** Convert images to modern formats (`.webp`, `.avif`) using tools/scripts. Implement responsive image formats (`srcset`).
- **Lazy Loading:** Use the native `loading="lazy"` attribute for non-critical images and iframes.
  ```html
  <img src="hero.webp" alt="Hero banner">
  <img src="footer-logo.webp" alt="Footer Logo" loading="lazy">
  ```
- **Fonts:** Preload critical web fonts, and use `font-display: swap` in `@font-face` definitions to prevent invisible text during loading.

### 2. Bundle Size & Script Loading
- **Dynamic Imports:** For React/Next.js/Vite, split bundles by utilizing dynamic imports for components that are not visible immediately (e.g., modals, dashboards).
  ```javascript
  // Example React lazy load
  const LargeChartComponent = React.lazy(() => import('./LargeChart'));
  ```
- **Script Deferral:** Mark third-party analytics or non-critical scripts with `defer` or `async` tags to prevent blocker rendering.
- **Tree-shaking & Minification:** Verify that bundlers (Vite, Webpack, ESBuild) are configured to prune dead code and compress production files.

### 3. Rendering & Layout Stability (Core Web Vitals)
- **Cumulative Layout Shift (CLS):** Always specify explicit `width` and `height` attributes on images, video containers, and ad units to reserve visual space and prevent shifts.
- **Minimize DOM Depth:** Avoid nesting unnecessary wrapper elements. A deep DOM tree degrades rendering speed and interaction responsiveness.
- **Debounce/Throttle Event Listeners:** Apply debounce or throttle decorators/helpers on high-frequency events (like `scroll`, `resize`, `mousemove`).

### 4. Backend & Database Performance
- **Caching Strategies:** Implement middleware-level caching (Redis, Memcached) or HTTP cache headers (`Cache-Control: public, max-age=31536000`).
- **Query Optimization:** Validate that database columns used in `WHERE`, `JOIN`, or `ORDER BY` operations are properly indexed. Avoid N+1 query patterns by using eager loading.
- **Payload Compression:** Implement Gzip or Brotli compression on server responses (e.g., utilizing `compression` middleware in Express).

---

## Auditing & Verification Workflow

1. **Lighthouse Audits:** Generate a Lighthouse report via DevTools to record performance baselines (FCP, LCP, CLS, TBT).
2. **Network Tab Analysis:** Check the waterfall chart in browser DevTools to spot large blocking assets, uncompressed responses, or redundant API requests.
3. **Bundle Analyzer:** If relevant, run bundle analysis tools (e.g., `rollup-plugin-visualizer` or `webpack-bundle-analyzer`) to detect bloated node modules.
4. **Execution Performance:** Profile JavaScript runtime usage in the DevTools Performance panel to detect long tasks (>50ms) causing frame drops.
