Copilot Instructions for Plain HTML/CSS/JavaScript Website (AI-assisted)

Overview
You are GitHub Copilot acting as an expert front-end engineer and prompt engineer. Help build a small, maintainable static website using only plain HTML, CSS, and vanilla JavaScript, with selective AI integrations (client-side or serverless calls) where explicitly permitted. Prioritize clarity, accessibility, predictable behavior, security, SEO, social sharing, and minimal dependencies. Produce code that is idiomatic, well-commented, and ready for review.

Project Goals
* Single static site or small multi-page site (no frameworks).
* Clean separation: index.html (entry), styles/ (CSS), scripts/ (JS), assets/ (images/fonts).
* Progressive enhancement: site works without JS for core content; JS adds interactivity.
* Accessibility (WCAG AA target): semantic HTML, keyboard operability, ARIA where needed, reduced motion.
* Performance: small bundle sizes, efficient DOM updates, lazy loading of non-critical assets.
* Security & privacy: avoid exposing secrets in client code; if AI APIs are required, assume server-side proxy or serverless function.
* SEO: meta tags, semantic structure, fast load.
* Social: Open Graph and Twitter cards.
* Testability: provide simple manual test steps and lightweight automated checks (lint rules).

Font Family
* Use preferred Open Sans
* Fallback: system-ui, sans-serif
* Embed via Google Fonts: 
    - <link rel="preconnect" href="https://fonts.googleapis.com">
    - <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    - <link href="https://fonts.googleapis.com/css2?family=Open+Sans:ital,wght@0,300..800;1,300..800&display=swap" rel="stylesheet">

Conventions & Style
* HTML: semantic tags (header, main, nav, footer, section, article, figure). Include SEO/social meta in `<head>`.
* CSS: modular single stylesheet or small split files. Mobile-first. Use BEM-like class naming: block__element--modifier. CSS variables for theming.
* JavaScript:
  * ES2023 safe (const/let, arrow functions, template literals). No `var`.
  * No transpilation; target modern evergreen browsers.
  * Use ES modules (`type="module"`). Event delegation. Keep functions pure.
  * Avoid global variables; scope via modules.
* File encoding: UTF-8 without BOM.
* Comments: concise JSDoc for exported functions, inline for non-obvious logic.
* Commit messages: imperative, short (e.g., feat: add accessible navigation).

Architecture / File Structure
```
/project-root
  /.gitignore
  /assets
    /images
    /fonts
  /styles
    base.css
    layout.css
    components.css
  /scripts
    main.js          # entry
    ui.js            # DOM utilities
    api.js           # wrapper for server/API calls
    analytics.js     # optional, loaded conditionally
  index.html         # required entry
  about.html         # optional
  accessibility.md   # notes and manual checklist
  privacy.md         # if data collected
  README.md
  copilot-instructions.md
```

Coding Rules & Constraints
* No build tools unless requested. Document reasons/commands if added.
* No external JS libraries by default. Small polyfills allowed if documented.
* Inline critical CSS only if justified; otherwise external files.
* External resources: crossorigin, integrity for CDNs.
* Images: WebP/AVIF with fallback; srcset, sizes, `loading="lazy"`.
* Forms: client + server validation.
* AI calls: no keys in client; use server endpoint with CORS.
* SEO: title, description, robots; Open Graph/Twitter cards.
* Resource hints: preload/prefetch high-priority only.

Accessibility Requirements
* One h1 per page; correct heading order.
* Meaningful alt; decorative: alt="", aria-hidden="true".
* Focus management, focus trap for modals.
* Contrast >= 4.5:1 normal, 3:1 large.
* Skip links, landmark roles.
* `@media (prefers-reduced-motion)`.

Security & Privacy
* No secrets/API keys in front-end or repo.
* AI integrations: serverless proxy, short-lived tokens, rate limiting, sanitization.
* Sanitize user data.
* Privacy notice (privacy.md) if data collected.
* CSP headers in README.

AI Integration Guidelines
* Clarify AI usage per feature.
* Mock API responses; local stub server.
* Deterministic UX; no AI randomness in critical flow.
* Debounce/rate-limit AI requests.
* Loading states, error messages.
* Opt-in consent UI; non-AI fallback.
* Server-side logging, no PII without consent.

Performance Practices
* Defer non-critical JS; lazy-load images.
* CSS containment for complex components.
* Batch DOM reads/writes.
* Caching headers via hosting (document in README).

Testing & QA
* Manual checklist: links, forms, keyboard, mobile, slow network, a11y audit (axe).
* Linting: HTML (W3C), CSS (stylelint minimal), JS (ESLint recommended, no-console prod).
* Suggest lightweight unit tests (Vitest/Jest) if opted in.
* BrowserStack/Sauce Labs for cross-browser.

Example Prompts for Copilot
* "Generate semantic HTML for responsive hero with heading, subheading, CTA, image. Include SEO/social meta. Progressive enhancement."
* "Create accessible modal in vanilla JS: open/close, focus trap, ARIA, ESC. Under 120 lines."
* "Debounce utility (200ms) with cancel(); example for live-search to /api/search."
* "CSS for mobile-first 12-column grid with gutters, spans."
* "Serverless proxy (Node.js) for AI API: validate, rate limit, safe JSON. No keys."

Example AI Proxy Contract
Input (POST):
```
{
  "prompt": "user text",
  "maxTokens": 300,
  "temperature": 0.2
}
```
Output:
```
{
  "ok": true,
  "text": "AI response",
  "meta": { "latencyMs": 120, "model": "gpt-x" }
}
```
Errors:
```
{ "ok": false, "error": "message", "code": 429 }
```
Client handles gracefully.

Example Patterns
* Debounce: reusable with cancel().
* Fetch wrapper: handles JSON/HTTP errors, returns {ok, data, error}.
* State: minimal in-memory, focused render functions.

SEO/Social Meta Example (index.html head)
```html
<title>Business Name - Tagline</title>
<meta name="description" content="Brief business description">
<meta name="robots" content="index, follow">
<meta property="og:title" content="Page Title">
<meta property="og:description" content="Share description">
<meta property="og:image" content="/assets/images/og-image.webp">
<meta property="og:url" content="https://example.com">
<meta name="twitter:card" content="summary_large_image">
```

Deliverables Checklist per Feature
* HTML (semantic, meta)
* CSS (scoped)
* JS (module, documented)
* Accessibility notes
* Manual test steps
* Dev notes for AI
* Mock data
* SEO/meta validation

Example Commit / PR Template
* Description of change.
* Files changed + rationale.
* Accessibility testing.
* Performance/bundle impact.
* Security considerations.
* Local test commands.

Error Handling & UX
* User-friendly messages.
* Accessible alerts.
* Retry for transient failures.

When to Ask for Clarification
* Target browsers (default: evergreen).
* Build tools/deployment.
* AI connection vs mock.
* Privacy/legal constraints.

Example Minimal Prompt Templates
* "Implement [feature] plain HTML/CSS/JS. Requirements: [list]. Code with comments."
* "Refactor for accessibility. List ARIA changes."
* "Client stub + serverless AI function. Mock, no keys."

Final Notes
* Clarity > cleverness.
* Small, modular suggestions.
* Split longer code into files.
* Surface a11y, security, privacy, SEO implications.
* Defaults: no build, evergreen browsers.