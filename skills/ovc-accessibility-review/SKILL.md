---
name: ovc-accessibility-review
description: Review frontend code for accessibility issues. Checks images, forms, semantics, keyboard navigation, ARIA usage, and color. Use for any frontend project to ensure it meets WCAG guidelines. Also trigger on "accessibility review", "a11y check", "accessibility audit", "is this accessible", "WCAG check", "screen reader".
metadata:
  author: renderinc
  version: "1.0"
license: MIT
---

# OVC Accessibility Review

Scan frontend code for common accessibility violations. Accessibility is not optional; it affects real users and is a legal requirement in many jurisdictions. This review catches the issues that static analysis can find. Manual testing with a screen reader and keyboard is still necessary for a complete audit.

## What to check

### Images

- `<img>` tags without `alt` attribute
- `<img>` tags with empty `alt=""` that are not decorative (they convey information)
- Informative images with generic alt text ("image", "photo", "icon")
- Background images used to convey content without text alternative
- SVG icons without accessible labels (`aria-label` or `<title>`)

### Forms

- Input fields without associated `<label>` elements (either wrapping or `htmlFor`/`for`)
- Missing `aria-describedby` for inputs with help text or error messages
- Form validation errors not announced to screen readers (`role="alert"` or `aria-live`)
- Submit buttons without accessible labels
- Custom select/dropdown components without ARIA combobox or listbox roles

### Semantic HTML

- `<div>` or `<span>` used where semantic elements should be:
  - Navigation: `<nav>`
  - Page sections: `<main>`, `<header>`, `<footer>`, `<section>`, `<article>`
  - Headings: `<h1>` through `<h6>` (in order, no skipped levels)
  - Buttons: `<button>` instead of `<div onClick>`
  - Links: `<a>` for navigation, `<button>` for actions
- Missing landmark regions (`<main>`, `<nav>`)
- Heading hierarchy violations (skipping from `<h1>` to `<h3>`)

### Keyboard Navigation

- Interactive elements that are not focusable (clickable `<div>` without `tabIndex`)
- Missing visible focus indicators (`:focus` or `:focus-visible` styles)
- Focus traps in modals or dialogs (focus should be trapped inside when open, restored on close)
- Tab order that does not follow visual order
- Custom keyboard shortcuts that conflict with screen reader or browser shortcuts

### ARIA

- ARIA attributes used where native HTML would suffice (`role="button"` on a `<div>` instead of using `<button>`)
- Missing required ARIA attributes (e.g., `aria-expanded` on toggle buttons, `aria-controls` for disclosure widgets)
- `aria-hidden="true"` on elements that contain visible, meaningful content
- Live regions (`aria-live`) that are too noisy or missing where needed

### Color and Contrast

- Information conveyed by color alone (error states shown only with red, not with text or icons)
- Text likely to have insufficient contrast (light gray on white, etc.)
- Links not distinguishable from surrounding text without color

## Steps

### 1. Determine scope

Ask the user: full frontend codebase, specific pages, or specific components?

### 2. Scan component files

Work through each category above. For each component or page file:
- Check the JSX/HTML output against the checklist
- Note any custom components that wrap native elements (check if they pass through accessibility props)

### 3. Check global patterns

- Is there a consistent focus management strategy for route changes?
- Is there a skip-to-content link?
- Are error pages accessible?
- Does the app set `lang` attribute on `<html>`?

### 4. Report findings

For each issue:
- File, line number, and the element or component
- WCAG criterion violated (e.g., "1.1.1 Non-text Content", "4.1.2 Name, Role, Value")
- WCAG level: A (must fix) or AA (should fix)
- How to fix it
- Whether it can be caught by automated tooling (eslint-plugin-jsx-a11y, axe) or requires manual review

Sort by WCAG level (A first), then by frequency of the pattern.
