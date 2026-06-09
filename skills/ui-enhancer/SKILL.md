---
name: ui-enhancer
description: Dedicated skill to audit, polish, and transform web interface designs into premium, high-aesthetic, responsive layouts featuring modern color palettes, glassmorphism, and micro-animations.
---
# UI Enhancer Specialist

This skill guides the agent in auditing, refining, and implementing premium user interfaces. It enforces advanced design principles and ensures high visual standards.

## Core Design Principles

### 1. Color Palette & Harmonious Schemes
- **Avoid Generic Colors:** Do not use primary red (`#FF0000`), green (`#00FF00`), or blue (`#0000FF`).
- **Use Curated Palettes:** Leverage HSL-tailored schemes, dark themes with deep greys, or vibrant gradients.
  - *Example Slate/Indigo scheme:*
    ```css
    :root {
      --bg-primary: #0a0e17;
      --bg-secondary: #121824;
      --accent-primary: #4f46e5;
      --accent-hover: #4338ca;
      --text-main: #f3f4f6;
      --text-muted: #9ca3af;
      --border-color: #1e293b;
    }
    ```
- **Consistent Contrast:** Ensure text remains highly readable over backgrounds (aim for WCAG AA standard or better).

### 2. Glassmorphism & Modern Textures
- To create premium container elements, combine semi-transparent backgrounds, blur filters, and thin borders:
  ```css
  .glass-card {
    background: rgba(255, 255, 255, 0.03);
    backdrop-filter: blur(12px);
    -webkit-backdrop-filter: blur(12px);
    border: 1px solid rgba(255, 255, 255, 0.08);
    border-radius: 16px;
    box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.3);
  }
  ```

### 3. Micro-animations & Interactive States
- Every interactive element (buttons, links, inputs, cards) must have smooth transitions:
  ```css
  .btn-interactive {
    transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
  }
  .btn-interactive:hover {
    transform: translateY(-2px);
    filter: brightness(1.1);
    box-shadow: 0 4px 12px rgba(79, 70, 229, 0.3);
  }
  .btn-interactive:active {
    transform: translateY(0);
  }
  ```
- **Loaders & Skeletons:** Use pulse or shimmer animations instead of static text placeholders.

### 4. Typography
- **Google Fonts integration:** Import premium web fonts (e.g., Inter, Outfit, Plus Jakarta Sans, Playfair Display) rather than browser defaults.
  ```html
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700&display=swap" rel="stylesheet">
  ```
- Apply hierarchical font sizing and weights (`font-weight: 600` for headings, `line-height: 1.6` for body text).

---

## Verification & Polish Workflow

1. **Local Dev Setup:** Run the development server locally (e.g., `npm run dev` or equivalent static server) using the `run_command` tool.
2. **Visual Audits via Browser Subagent:** Use the `browser_subagent` tool to load the application.
3. **Capture UI State:** Take screenshots at various viewport sizes to inspect responsiveness (mobile, tablet, desktop).
4. **Identify Flaws:** Look for text clipping, alignment issues, unbalanced spacing, or plain layouts.
5. **No Placeholders:** If realistic images are required, use `generate_image` to create contextual assets rather than using basic placeholders.
