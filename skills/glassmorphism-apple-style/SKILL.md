---
name: glassmorphism-apple-style
description: Apply a style-only Apple-system-font glassmorphism visual treatment to an existing repo while preserving its business logic, behavior, data flow, and light/dark mode mechanisms.
metadata:
  short-description: Style-only Apple glassmorphism theme
---

# Glassmorphism Apple Style

Use this skill when the user asks to apply the visual style from this repo's `src/style.css` to another project: Apple system typography, restrained font weights, glassy translucent surfaces, rounded controls, subtle motion, and matching light/dark themes.

This is a style-only skill. Do not implement, refactor, infer, rename, remove, or "clean up" business logic. Do not change data fetching, state management, validation, routing, event handlers, API calls, domain copy, permissions, calculations, persistence, or workflow semantics. Read application code only as needed to locate style entry points, component class names, and theme hooks.

## Source Style

The visual source has these traits:

- Font stack: `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif`.
- Font smoothing: `-webkit-font-smoothing: antialiased`.
- Core accent: Apple blue `#007AFF`, with white primary content.
- Action color system: primary actions are outline-first in light mode and white-outline in dark mode; secondary actions are filled Apple blue in light mode and filled white in dark mode.
- Light base: near-white app backgrounds, white sidebars, subtle gray borders, dark gray text.
- Dark base: black or near-black app backgrounds, muted blue-gray secondary text, white foregrounds.
- Glass surfaces: semi-transparent backgrounds such as `white / 80%` or app-surface `/ 80%`, `backdrop-blur-md`, thin borders, and soft shadows.
- Shape language: pill buttons, round icon buttons, compact rounded inputs, and larger rounded workflow cards.
- Motion: `transition-all duration-200`, small hover lift `translateY(-0.125rem)`, restrained shadow changes.
- Weights: regular body text, medium labels/status, semibold compact pills and metadata, bold only for strong categorical text.

## Guardrails

- Keep diffs cosmetic: CSS, design tokens, theme variables, class names, `className`/`class`, inline styles only when the repo already uses them, and purely presentational component wrappers.
- Preserve the repo's existing styling stack. Use CSS variables in vanilla CSS, Tailwind utilities in Tailwind projects, CSS modules when already present, styled-components/emotion when already present, etc.
- Preserve the existing dark-mode trigger. If the repo uses `[data-theme="dark"]`, keep it. If it uses `.dark`, keep it. If it uses `prefers-color-scheme`, extend that media query. Do not replace the theme system.
- Do not introduce new UI libraries, state libraries, theme providers, fonts, or runtime dependencies unless the user explicitly asks.
- Do not alter visible product/domain text except labels that are purely part of new presentational controls requested by the user.
- Do not change component hierarchy in ways that affect keyboard flow, form submission, routing, or event propagation.
- Before finishing, review the diff and confirm no business logic files were changed except presentational class/style edits.

## Workflow

1. Locate style entry points: global CSS, Tailwind config, theme files, component-level style files, and shared UI primitives.
2. Identify the repo's dark-mode mechanism and token naming conventions.
3. Add or map design tokens first, then update components to consume them.
4. Apply Apple font stack globally and set font weights deliberately.
5. Convert eligible surfaces to glass treatment: app headers, navs, sidebars, panels, cards, toolbars, inputs, popovers, and compact controls.
6. Keep domain-specific components functionally intact. For component files, limit changes to styling attributes and presentational wrapper classes.
7. Validate with the repo's normal checks where available: lint, typecheck, test, build, and visual inspection for light and dark modes.

## Design Tokens

Prefer these token values unless the target repo already has equivalent names:

```css
:root {
  --color-bg-code: #f9fafb;
  --color-bg-base: #f9fafb;
  --color-bg-sidebar: #ffffff;
  --color-border-subtle: #e5e7eb;
  --color-text-main: #1f2937;
  --color-text-muted: #6b7280;
  --color-text-white: #ffffff;
  --color-primary: #007AFF;
  --color-primary-hover: #3394FF;
  --color-primary-active-bg: rgba(0, 122, 255, 0.12);
  --color-primary-content: #ffffff;
  --color-action-dark: #ffffff;
  --color-action-dark-content: #000000;
  --color-action-dark-hover: #e5e5e5;
  --color-action-dark-active-bg: rgba(255, 255, 255, 0.8);
  --color-main: #1d1d1f;
  --color-muted: #86868b;
}

[data-theme="dark"] {
  --color-bg-code: #2c2c2e;
  --color-bg-base: #1c1c1e;
  --color-bg-sidebar: #000000;
  --color-border-subtle: #000000;
  --color-text-main: #ffffff;
  --color-text-muted: #d6e0eb;
}
```

If the repo uses `.dark`, translate the dark selector to `.dark`. If it uses media queries, place the dark overrides inside `@media (prefers-color-scheme: dark)`.

## Global Typography

Apply the font globally:

```css
body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  color: var(--color-text-main);
  background: var(--color-bg-base);
  font-weight: 400;
}
```

Use these weights:

- `400` for body text, inputs, buttons, and standard labels.
- `500` for form labels, section labels, active nav items, and small statuses.
- `600` for badges, pills, metadata, compact headings, and emphasized controls.
- `700` only for categorical log levels, numeric emphasis, or existing strong headings.

Avoid making the whole app heavy. Apple-like UI should feel crisp, quiet, and readable.

## Action Color System

Preserve this action hierarchy when styling buttons, toolbar actions, call-to-action links, and confirmation controls:

- Primary action: outline-first in light mode, Apple blue border/text, transparent background.
- Primary hover: filled Apple blue `#007AFF`, white text.
- Primary active: pale Apple blue active surface `rgba(0, 122, 255, 0.12)`, inset press shadow, Apple blue text and border.
- Primary focus: Apple blue focus ring `0 0 0 3px rgba(0, 122, 255, 0.3)`.
- Secondary action: filled Apple blue `#007AFF`, white text.
- Secondary hover: brighter blue `#3394FF`, white text.
- Dark-mode primary: white outline/text, transparent background.
- Dark-mode primary hover: filled white, black text.
- Dark-mode primary active: `rgba(255, 255, 255, 0.8)`, white border, black text.
- Dark-mode focus: white focus ring `0 0 0 3px rgba(255, 255, 255, 0.3)`.
- Dark-mode secondary: filled white, black text.
- Dark-mode secondary hover: soft gray `#E5E5E5`, black text.

Use this mapping even if the target repo uses different class names such as `.button-primary`, `.cta`, `.submit`, `.cancel`, `.toolbar-action`, or component variants like `variant="primary"` and `variant="secondary"`. Only change styles and variant styling definitions; do not change what an action does.

## Glassmorphism Recipe

Use this reusable pattern for major surfaces:

```css
.glass-surface {
  background: rgba(255, 255, 255, 0.8);
  border: 1px solid var(--color-border-subtle);
  box-shadow: 0 8px 24px rgba(15, 23, 42, 0.08);
  backdrop-filter: blur(12px) saturate(180%);
  -webkit-backdrop-filter: blur(12px) saturate(180%);
}

[data-theme="dark"] .glass-surface {
  background: rgba(0, 0, 0, 0.72);
  border-color: rgba(255, 255, 255, 0.12);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.32);
}
```

Use lower opacity for overlays and headers if the app content must remain visible behind them. Use `rgba(255, 255, 255, 0.05)` for dark-mode inset fields, code blocks, and subtle panels.

## Controls

Buttons should be compact, rounded, and calm:

```css
.btn-primary,
.btn-secondary {
  cursor: pointer;
  border-radius: 22px;
  border: 1px solid;
  padding: 6px 12px;
  font-size: 1rem;
  font-weight: 400;
  letter-spacing: 0;
  transition: all 200ms ease;
}

.btn-primary {
  background: transparent;
  border-color: var(--color-primary);
  color: var(--color-primary);
}

.btn-primary:hover {
  background: var(--color-primary);
  color: var(--color-primary-content);
}

.btn-primary:active {
  background: var(--color-primary-active-bg);
  box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.08);
  border-color: var(--color-primary);
  color: var(--color-primary);
}

.btn-primary:focus-visible {
  outline: none;
  box-shadow: 0 0 0 3px rgba(0, 122, 255, 0.3);
}

.btn-secondary {
  background: var(--color-primary);
  border-color: var(--color-primary);
  color: var(--color-primary-content);
}

.btn-secondary:hover {
  background: var(--color-primary-hover);
  border-color: var(--color-primary-hover);
  color: var(--color-primary-content);
}
```

For dark mode, make primary buttons white-outline by default and white-filled on hover; make secondary buttons white-filled:

```css
[data-theme="dark"] .btn-primary {
  border-color: var(--color-action-dark);
  color: var(--color-action-dark);
}

[data-theme="dark"] .btn-primary:hover {
  background: var(--color-action-dark);
  color: var(--color-action-dark-content);
}

[data-theme="dark"] .btn-primary:active {
  background: var(--color-action-dark-active-bg);
  border-color: var(--color-action-dark);
  color: var(--color-action-dark-content);
}

[data-theme="dark"] .btn-primary:focus-visible {
  box-shadow: 0 0 0 3px rgba(255, 255, 255, 0.3);
}

[data-theme="dark"] .btn-secondary {
  background: var(--color-action-dark);
  border-color: var(--color-action-dark);
  color: var(--color-action-dark-content);
}

[data-theme="dark"] .btn-secondary:hover {
  background: var(--color-action-dark-hover);
  border-color: var(--color-action-dark-hover);
  color: var(--color-action-dark-content);
}
```

Inputs should be full-width, compact, rounded, and focus-visible:

```css
.input-primary {
  width: 100%;
  border: 1px solid #e5e7eb;
  border-radius: 0.5rem;
  padding: 0.5rem 0.75rem;
  color: var(--color-text-main);
  box-shadow: 0 1px 2px rgba(15, 23, 42, 0.05);
  transition: all 200ms ease;
}

.input-primary:focus {
  outline: none;
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px rgba(0, 122, 255, 0.2);
}
```

## Tailwind Mapping

If the repo uses Tailwind, prefer existing utilities and component layers:

```css
@layer components {
  .glass-header {
    @apply bg-[var(--color-bg-sidebar)]/80 backdrop-blur-md border-b border-gray-200;
  }

  .glass-panel {
    @apply bg-white/80 backdrop-blur-md border border-gray-200 shadow-sm;
  }

  [data-theme="dark"] .glass-panel {
    @apply bg-black/70 border-white/10 shadow-black/30;
  }

  .icon-button {
    @apply p-1 rounded-full transition-all duration-200 hover:-translate-y-0.5;
  }
}
```

Keep Tailwind class changes presentational. Do not alter conditional rendering, handlers, stores, props, or emitted events.

## Validation Checklist

- Light mode and dark mode both use the same component structure.
- Text remains readable on glass surfaces and dark-mode inset panels.
- Focus states remain visible for keyboard users.
- Hover motion is subtle and does not shift layout.
- No business logic, data model, API, route, state, validation, or workflow behavior changed.
- Any component-file edits are limited to classes, style attributes, or presentational wrappers.
