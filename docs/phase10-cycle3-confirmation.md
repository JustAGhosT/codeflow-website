# Phase 10 - Continuous Debt Resolution: Cycle 3 Planning

**Date**: 2025-12-23
**Cycle**: 3 of N
**Status**: Awaiting User Confirmation

---

## Executive Summary

Cycles 1 and 2 successfully resolved **14 of 17** technical debt items (82% complete). The registry now contains **3 remaining items** requiring resolution. These items represent the most challenging work: external dependencies, large-effort security improvements, and optional tooling.

### Cycle Accomplishments

| Cycle | Items Resolved | Test Count | Key Achievements |
|-------|---------------|------------|------------------|
| Cycle 1 | 8 | 19 → 19 | Test infrastructure, CI gates, error boundaries |
| Cycle 2 | 6 | 19 → 76 | Component library, observability scaffolds, documentation |
| **Total** | **14** | **76** | **82% debt resolution** |

### Remaining Registry Overview

| ID | Description | Priority | Effort | Blocker |
|----|-------------|----------|--------|---------|
| DEBT-04 | Undefined CSS Custom Properties | High | M | External repo (codeflow-desktop) |
| DEBT-06 | CSP Security Headers Incomplete | High | L | Tailwind nonce strategy |
| DEBT-13 | Missing Storybook | Medium | M | Optional tooling |

---

## Cycle 3: Proposed Scope

### Assessment

The remaining 3 items present unique challenges:

1. **DEBT-04** requires access to the `codeflow-desktop` repository to sync design tokens. Without access, we can only define local fallback values.

2. **DEBT-06** is a Large effort security improvement requiring:
   - Tailwind CSS nonce-based CSP strategy
   - Azure Static Web Apps configuration
   - Testing in production environment

3. **DEBT-13** (Storybook) is optional tooling that provides value but is not blocking.

### Recommended Approach

Given the constraints, we recommend a **partial resolution strategy**:

#### Option A: Local Token Fallbacks (DEBT-04 Partial)
- Define local CSS custom property fallbacks in `app/globals.css`
- Document dependency on codeflow-desktop for full synchronization
- Effort: S (Small)

#### Option B: CSP Security Hardening (DEBT-06)
- Implement nonce-based CSP for Tailwind
- Update `staticwebapp.config.json` security headers
- Effort: L (Large) - requires production testing

#### Option C: Storybook Setup (DEBT-13)
- Initialize Storybook for React/Next.js
- Add stories for existing components (Footer, FeatureCard, Button, ThemeToggle)
- Effort: M (Medium)

### Recommended Selection

| ID | Item | Include in Cycle 3? | Rationale |
|----|------|---------------------|-----------|
| DEBT-04 | CSS Token Fallbacks | ✅ Partial | Define local fallbacks; full sync deferred |
| DEBT-06 | CSP Headers | ⚠️ Optional | Large effort; security improvement |
| DEBT-13 | Storybook | ✅ Yes | Component documentation value |

**Proposed Scope**: DEBT-04 (partial), DEBT-13
**Effort**: S + M = ~5-6 hours
**Optional**: DEBT-06 (add only if security is priority)

---

## Implementation Plan

### Phase 1: CSS Token Fallbacks (DEBT-04 Partial)

**Effort**: S (~1 hour)

#### Task: Define Local Fallback Values

Update `app/globals.css` to define CSS custom properties locally:

```css
:root {
  /* Fallback design tokens - sync from codeflow-desktop when available */
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-3: 0.75rem;
  --space-4: 1rem;
  --color-surface: theme('colors.white');
  --color-surface-dark: theme('colors.slate.800');
  /* ... additional tokens as needed */
}
```

**Files**: `app/globals.css`

**Limitation**: Full design token synchronization requires access to `codeflow-desktop` repository.

---

### Phase 2: Storybook Setup (DEBT-13)

**Effort**: M (~4 hours)

#### Tasks

1. **Initialize Storybook**
   ```bash
   npx storybook@latest init
   ```

2. **Configure for Next.js App Router**
   - Update `.storybook/main.ts` for App Router compatibility
   - Configure framework options

3. **Add Component Stories**
   - `Button.stories.tsx` - All variants and sizes
   - `FeatureCard.stories.tsx` - Static and linked variants
   - `Footer.stories.tsx` - Light and dark modes
   - `ThemeToggle.stories.tsx` - Interactive theme switching

4. **Add to CI Pipeline**
   - Build Storybook as part of CI
   - Optional: Deploy Storybook to static hosting

**Files**:
- `.storybook/main.ts`
- `.storybook/preview.ts`
- `app/components/*.stories.tsx`

---

### Phase 3: CSP Security (DEBT-06) - Optional

**Effort**: L (~6-8 hours)

If security hardening is prioritized:

1. **Research Tailwind CSP Strategy**
   - Evaluate nonce vs hash approaches
   - Check Azure Static Web Apps CSP header support

2. **Implement Nonce Generation**
   - Server-side nonce generation in middleware
   - Pass nonce to Tailwind/style injection

3. **Update Security Headers**
   - Remove `unsafe-inline` from CSP
   - Add nonce-based style-src directive
   - Complete other security headers

4. **Production Testing**
   - Deploy to staging environment
   - Verify styles render correctly
   - Test across browsers

**Files**:
- `middleware.ts` (new)
- `public/staticwebapp.config.json`
- `app/layout.tsx`

---

## Testing Strategy

### New Test Suites for Cycle 3

| Test Type | Framework | Target | Priority |
|-----------|-----------|--------|----------|
| Visual Regression | Storybook + Chromatic | All component stories | Medium |
| Interaction Tests | Storybook play functions | Button, ThemeToggle | Low |

### Coverage Expectations

| Metric | Current | Target |
|--------|---------|--------|
| Test Count | 76 | 80+ |
| Statements | 60%+ | 65%+ |
| Storybook Stories | 0 | 4+ |

---

## Risk Assessment

| Risk | Mitigation |
|------|------------|
| DEBT-04 requires external repo | Define fallbacks; document limitation |
| Storybook setup complexity | Use official Next.js integration |
| CSP breaks styles | Thorough testing; rollback plan |
| Time overrun on CSP | Make optional; defer if needed |

---

## Decision Points

### User Confirmation Required

Please review and respond with:

1. **`CONTINUE`** - Proceed with DEBT-04 (partial) + DEBT-13 (Storybook)
2. **`CONTINUE WITH CSP`** - Include DEBT-06 (CSP security hardening)
3. **`CONTINUE NO FILE CHANGES`** - Documentation-only review
4. **`REVISE`** - Request scope adjustments

### Questions for User

1. **External Repo Access**: Do you have access to `codeflow-desktop` repository for design token sync?
2. **Security Priority**: Should CSP hardening (DEBT-06) be included in Cycle 3?
3. **Storybook Deployment**: Should Storybook be deployed to a public URL?

---

## Success Metrics

### Cycle 3 Completion Criteria

- [ ] CSS custom properties have local fallbacks (DEBT-04 partial)
- [ ] Storybook initialized with component stories (DEBT-13)
- [ ] All existing tests passing (76+)
- [ ] CI pipeline includes Storybook build
- [ ] Documentation updated

### Post-Cycle 3 Status

| Scenario | Items Resolved | Remaining |
|----------|---------------|-----------|
| Minimal (DEBT-04 partial, DEBT-13) | 15.5/17 | 1.5 |
| Full (+ DEBT-06) | 17/17 | 0 |

---

## Context Budget Status

| Phase | Tokens | Budget | Status |
|-------|--------|--------|--------|
| Cycle 1 | ~600 | 500-1000 | ✅ Complete |
| Cycle 2 | ~500 | 500-1000 | ✅ Complete |
| Cycle 3 Plan | ~400 | 500-1000 | ✅ Within budget |

---

## Blockers

1. **External Dependency**: Full DEBT-04 resolution requires `codeflow-desktop` repository access
2. **Production Environment**: DEBT-06 CSP changes require production testing
3. **Time Investment**: DEBT-06 is Large effort and may extend timeline

---

## Next Steps

1. **Awaiting User Response**: Review this planning document
2. **Upon CONTINUE**: Begin with DEBT-04 partial (CSS fallbacks)
3. **After Cycle 3**: Evaluate remaining items for final cycle

---

**Document Status**: Ready for user confirmation
**Proposed File Changes**: 0 (planning phase only)
**Ready State**: Awaiting `CONTINUE`, `CONTINUE WITH CSP`, `CONTINUE NO FILE CHANGES`, or `REVISE`

---

*Generated as part of Phase 10 Continuous Debt Resolution - Cycle 3 Planning*
*Last Updated: 2025-12-23*
