# Frontend Audit Checklist

Load this checklist when the project has a frontend layer (React, Vue, Svelte, Next.js, Nuxt, SvelteKit, Astro, or `*.tsx`/`*.jsx`/`*.vue`/`*.svelte` files).

## Accessibility

- [ ] Do all `<img>` tags have meaningful `alt` attributes (not empty on informative images, not generic like "image" or "photo")?
- [ ] Do all form inputs have associated `<label>` elements (wrapping or via `htmlFor`/`for`)?
- [ ] Are interactive elements using the correct HTML elements (`<button>` for actions, `<a>` for navigation -- not clickable `<div>` or `<span>` with `onClick`)?
- [ ] Are landmark elements present (`<main>`, `<nav>`, `<header>`, `<footer>`)?
- [ ] Is the heading hierarchy correct (no skipping from `<h1>` to `<h3>`, one `<h1>` per page)?
- [ ] Do SVG icons have accessible labels (`aria-label` or `<title>`)?
- [ ] Is there a visible focus indicator for keyboard navigation (`:focus` or `:focus-visible` styles)?
- [ ] Are modals or dialogs trapping focus correctly (focus trapped inside when open, restored on close)?
- [ ] Is information conveyed by more than color alone (error states have text or icons, not just red)?

## Bundle and Loading

- [ ] Are full libraries imported where only specific functions are needed (`import _ from 'lodash'` instead of `import groupBy from 'lodash/groupBy'`)?
- [ ] Are heavy routes or components lazy-loaded (`React.lazy`, dynamic `import()`, framework equivalent)?
- [ ] Are images optimized (width/height set, `loading="lazy"` on below-fold images, modern formats like WebP/AVIF, srcset for responsive sizes)?
- [ ] Are there large dependencies that have smaller alternatives (e.g., moment.js vs date-fns)?

## Rendering Performance

- [ ] Are there state updates that trigger re-renders of large component trees? Look for state lifted unnecessarily high.
- [ ] Is expensive derived data memoized (`useMemo`/`computed`/framework equivalent) or recalculated on every render?
- [ ] Are list items using stable, unique `key` props (not array index on dynamic lists)?
- [ ] Are there unnecessary re-renders from new object/function references created on every render (inline objects or arrow functions in JSX props without memoization)?
