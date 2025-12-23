# CodeFlow Website - Technical Debt Registry

This registry tracks all technical debt items identified during the Phase 0-9 audit cycle. Items are organized by priority and include estimated effort for remediation.

---

## Registry Summary

| Priority | Count | Total Effort |
|----------|-------|--------------|
| Critical | 2 | M + M |
| High | 5 | S + M + S + L + S |
| Medium | 6 | S + M + S + M + M + M |
| Low | 4 | S + S + S + S |

---

## Critical Priority

### DEBT-01: No Test Infrastructure

| Field | Value |
|-------|-------|
| **ID** | DEBT-01 |
| **Source** | BUG-03, Phase 4 Audit |
| **Category** | Testing |
| **Description** | No test framework configured; `npm test` script missing from package.json |
| **Impact** | No regression protection; CI cannot validate code changes |
| **Effort** | M (Medium) |
| **Dependencies** | None |
| **Recommended Action** | Install Vitest/Jest, configure test script, add initial test suite |

### DEBT-02: CI Tests Non-Blocking

| Field | Value |
|-------|-------|
| **ID** | DEBT-02 |
| **Source** | Phase 9 Review, `.github/workflows/ci.yml:52,70` |
| **Category** | DevOps |
| **Description** | CI workflow uses `|| true` for test and lint steps, making failures non-blocking |
| **Impact** | Broken code can be merged to main; quality gates ineffective |
| **Effort** | M (Medium) - requires test infrastructure first |
| **Dependencies** | DEBT-01 |
| **Recommended Action** | Remove `|| true` after test infrastructure is established |

---

## High Priority

### DEBT-03: UTF-8 Encoding Corruption

| Field | Value |
|-------|-------|
| **ID** | DEBT-03 |
| **Source** | BUG-01, Phase 4 Audit |
| **Category** | Code Quality |
| **Description** | UTF-8 BOM markers and corrupted emoji characters in source files |
| **Impact** | Visual degradation; emojis display incorrectly |
| **Effort** | S (Small) |
| **Files** | `app/page.tsx`, `app/components/PromoBanner.tsx` |
| **Recommended Action** | Re-save files with UTF-8 encoding (no BOM); add pre-commit hook to detect BOM |

### DEBT-04: Undefined CSS Custom Properties

| Field | Value |
|-------|-------|
| **ID** | DEBT-04 |
| **Source** | BUG-02, Phase 4 Audit |
| **Category** | Styling |
| **Description** | 21 CSS custom properties referenced in `globals.css` but never defined locally |
| **Impact** | Skip link, focus rings, scrollbars may not render correctly |
| **Effort** | M (Medium) |
| **Files** | `app/globals.css` |
| **Dependencies** | Access to codeflow-desktop design tokens |
| **Recommended Action** | Mirror essential design tokens locally or remove unused references |

### DEBT-05: Missing Error Boundaries

| Field | Value |
|-------|-------|
| **ID** | DEBT-05 |
| **Source** | BUG-07, Phase 4 Audit |
| **Category** | Error Handling |
| **Description** | No error boundary components or Next.js `error.tsx` files |
| **Impact** | Component errors crash entire application |
| **Effort** | S (Small) |
| **Files** | `app/layout.tsx`, route directories |
| **Recommended Action** | Add `error.tsx` to app directory; wrap critical components with ErrorBoundary |

### DEBT-06: CSP Security Headers Incomplete

| Field | Value |
|-------|-------|
| **ID** | DEBT-06 |
| **Source** | BUG-08, Phase 4 Audit |
| **Category** | Security |
| **Description** | CSP allows `unsafe-inline` for styles; security headers incomplete |
| **Impact** | Potential XSS vulnerability via inline style injection |
| **Effort** | L (Large) - requires Tailwind nonce strategy |
| **Files** | `public/staticwebapp.config.json` |
| **Recommended Action** | Implement nonce-based CSP for Tailwind; add missing security headers |

### DEBT-07: localStorage Without Error Handling

| Field | Value |
|-------|-------|
| **ID** | DEBT-07 |
| **Source** | BUG-09, Phase 4 Audit |
| **Category** | Error Handling |
| **Description** | Direct localStorage access without try-catch |
| **Impact** | Application may crash in private browsing or storage-disabled environments |
| **Effort** | S (Small) |
| **Files** | `app/components/ThemeProvider.tsx:51,84` |
| **Recommended Action** | Wrap localStorage calls in try-catch with graceful fallback |

---

## Medium Priority

### DEBT-08: AnimatedBackground Resize Not Debounced

| Field | Value |
|-------|-------|
| **ID** | DEBT-08 |
| **Source** | BUG-04, Phase 4 Audit |
| **Category** | Performance |
| **Description** | Window resize handler triggers particle re-initialization without debouncing |
| **Impact** | Performance degradation on rapid resize events |
| **Effort** | S (Small) |
| **Files** | `app/components/AnimatedBackground.tsx:137-141` |
| **Recommended Action** | Add debounce utility; wrap resize handler |

### DEBT-09: Footer Component Duplication

| Field | Value |
|-------|-------|
| **ID** | DEBT-09 |
| **Source** | BUG-05, Phase 4 Audit |
| **Category** | Maintainability |
| **Description** | Identical footer JSX duplicated across 4 page files |
| **Impact** | Maintenance burden; inconsistent updates |
| **Effort** | M (Medium) |
| **Files** | `app/page.tsx`, `app/installation/page.tsx`, `app/integration/page.tsx`, `app/download/page.tsx` |
| **Recommended Action** | Extract to shared `Footer.tsx` component |

### DEBT-10: Theme Detection Via DOM Class

| Field | Value |
|-------|-------|
| **ID** | DEBT-10 |
| **Source** | BUG-06, Phase 4 Audit |
| **Category** | Code Quality |
| **Description** | AnimatedBackground inspects DOM class instead of using ThemeContext |
| **Impact** | Potential theme mismatch; violates single source of truth |
| **Effort** | S (Small) |
| **Files** | `app/components/AnimatedBackground.tsx:144` |
| **Recommended Action** | Use useContext(ThemeContext) instead of DOM inspection |

### DEBT-11: Feature Cards Not Componentized

| Field | Value |
|-------|-------|
| **ID** | DEBT-11 |
| **Source** | Phase 1 Design System Review |
| **Category** | Maintainability |
| **Description** | 3 identical feature card structures inline in page.tsx |
| **Impact** | Code duplication; harder to maintain consistency |
| **Effort** | M (Medium) |
| **Files** | `app/page.tsx` |
| **Recommended Action** | Extract to reusable `FeatureCard.tsx` component |

### DEBT-12: Button Styles Inconsistent

| Field | Value |
|-------|-------|
| **ID** | DEBT-12 |
| **Source** | Phase 1 Design System Review |
| **Category** | Design System |
| **Description** | Multiple button variants (gradient/solid/outline) without clear system |
| **Impact** | Visual inconsistency; harder to maintain design coherence |
| **Effort** | M (Medium) |
| **Files** | Multiple page files |
| **Recommended Action** | Create Button component with defined variants (primary, secondary, outline) |

### DEBT-13: Missing Storybook

| Field | Value |
|-------|-------|
| **ID** | DEBT-13 |
| **Source** | Phase 1 Design System Review |
| **Category** | Documentation |
| **Description** | No Storybook for component documentation and visual testing |
| **Impact** | Component discovery difficult; no visual regression testing |
| **Effort** | M (Medium) |
| **Files** | N/A (new addition) |
| **Recommended Action** | Add Storybook; document existing components with stories |

---

## Low Priority

### DEBT-14: Missing Loading States

| Field | Value |
|-------|-------|
| **ID** | DEBT-14 |
| **Source** | Phase 3 Best Practices Benchmark |
| **Category** | UX |
| **Description** | No `loading.tsx` files for route loading UI |
| **Impact** | No loading indicators during navigation |
| **Effort** | S (Small) |
| **Files** | Route directories |
| **Recommended Action** | Add `loading.tsx` to route segments if navigation latency becomes noticeable |

### DEBT-15: No Error Tracking Integration

| Field | Value |
|-------|-------|
| **ID** | DEBT-15 |
| **Source** | Phase 3 Best Practices Benchmark |
| **Category** | Observability |
| **Description** | No Sentry or similar error tracking service |
| **Impact** | Client-side errors go unnoticed |
| **Effort** | S (Small) |
| **Files** | `app/layout.tsx` |
| **Recommended Action** | Integrate Sentry or Azure Application Insights |

### DEBT-16: No Analytics Integration

| Field | Value |
|-------|-------|
| **ID** | DEBT-16 |
| **Source** | Phase 3 Best Practices Benchmark |
| **Category** | Observability |
| **Description** | No analytics for tracking user behavior and conversion |
| **Impact** | Cannot measure alpha signup conversion or user engagement |
| **Effort** | S (Small) |
| **Files** | `app/layout.tsx` |
| **Recommended Action** | Integrate Plausible, Azure Application Insights, or similar |

### DEBT-17: README Lacks Comprehensive Documentation

| Field | Value |
|-------|-------|
| **ID** | DEBT-17 |
| **Source** | Phase 9 Review |
| **Category** | Documentation |
| **Description** | README is minimal; doesn't reference new docs/ documentation |
| **Impact** | Onboarding friction; documentation discoverability |
| **Effort** | S (Small) |
| **Files** | `README.md` |
| **Recommended Action** | Enhance README with links to docs/, testing instructions, contribution guidelines |

---

## Deferred Items (Future Consideration)

### DEBT-F1: Internationalization (i18n)

| Field | Value |
|-------|-------|
| **ID** | DEBT-F1 |
| **Category** | Feature |
| **Description** | No i18n infrastructure for multi-language support |
| **Effort** | H (High) |
| **Trigger** | International expansion |

### DEBT-F2: Design Token Synchronization

| Field | Value |
|-------|-------|
| **ID** | DEBT-F2 |
| **Category** | Architecture |
| **Description** | Design tokens defined in codeflow-desktop, not synced to this repo |
| **Effort** | M (Medium) |
| **Trigger** | Multi-repo design system maturity |

### DEBT-F3: Accessibility Audit (WCAG 2.1 AA)

| Field | Value |
|-------|-------|
| **ID** | DEBT-F3 |
| **Category** | Accessibility |
| **Description** | Full WCAG 2.1 AA audit not conducted |
| **Effort** | M (Medium) |
| **Trigger** | Pre-production launch |

---

## Resolution Tracking

| ID | Status | Resolved Date | Resolution Notes |
|----|--------|---------------|------------------|
| DEBT-01 | Open | - | - |
| DEBT-02 | Open | - | Blocked by DEBT-01 |
| DEBT-03 | Open | - | - |
| DEBT-04 | Open | - | - |
| DEBT-05 | Open | - | - |
| DEBT-06 | Open | - | - |
| DEBT-07 | Open | - | - |
| DEBT-08 | Open | - | - |
| DEBT-09 | Open | - | - |
| DEBT-10 | Open | - | - |
| DEBT-11 | Open | - | - |
| DEBT-12 | Open | - | - |
| DEBT-13 | Open | - | - |
| DEBT-14 | Open | - | - |
| DEBT-15 | Open | - | - |
| DEBT-16 | Open | - | - |
| DEBT-17 | Open | - | - |

---

Document generated as part of Phase 9 Post-Implementation Review. Last updated: 2025-12-23
