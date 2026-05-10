# Automation QA Workflow

Use this when the user asks to: generate Playwright test scripts, create Page Object Model, automate test cases, set up test fixtures, review or refactor existing spec files.

## Overview

5-step workflow orchestrated by `#qa-automation-workflow` agent:

```
Step 1: #qa-context-roleplay    → Establish automation context & scope
Step 2: #qa-ui-recon            → Map selectors + navigation flow + auth strategy
Step 3: #qa-pom-generation      → Generate Page Object Model classes
Step 4: #qa-script-generation   → Write .spec.ts files (mode: generate)
Step 5: #qa-script-generation   → Review + fix anti-patterns (mode: review-and-fix)
```

## How to invoke

```bash
# Full workflow
playwright-cli "Run #qa-automation-workflow for module: [module name], app URL: [staging URL]"

# With existing manual test cases
playwright-cli "Run #qa-automation-workflow for module: Customer, app URL: https://staging.example.com, manual TC file: specs/customer-test-cases.md"

# Single step
playwright-cli "Run #qa-ui-recon for module: Customer, app URL: https://staging.example.com"
playwright-cli "Run #qa-pom-generation for module: Customer"
playwright-cli "Run #qa-script-generation for module: Customer, mode: review-and-fix, spec files: tests/customer/"
```

## Output files

| Step | Output |
|------|--------|
| Step 2 | `specs/[module]-selector-inventory.md` |
| Step 3 | `pages/[module]/[Module]ListPage.ts` |
| Step 3 | `pages/[module]/[Module]FormPage.ts` |
| Step 3 | `pages/BasePage.ts` (if not exists) |
| Step 4 | `tests/[module]/[module]-create.spec.ts` |
| Step 4 | `tests/[module]/[module]-update.spec.ts` |
| Step 4 | `tests/[module]/[module]-delete.spec.ts` |

## Selector priority (enforced by #qa-ui-recon)

1. `data-testid` — preferred, ask Dev to add if missing
2. `getByRole('button', { name: '...' })`
3. `getByLabel('...')`
4. `getByPlaceholder('...')`
5. CSS selector — last resort only

## Anti-patterns blocked by #qa-script-generation review

- `waitForTimeout()` / `sleep()` → replaced with Playwright auto-wait
- Hardcoded URLs / credentials → moved to `.env`
- CSS dynamic class selectors → replaced with semantic locators
- Missing `expect()` in test body
- Test data shared across tests → isolated per test

## Test tags convention

```typescript
test('should ... @smoke @p1', ...)   // critical, run before every deploy
test('should ... @p2', ...)          // run in full regression
test('should ... @p3', ...)          // run weekly / on demand
```

## Auth setup (storageState — recommended)

```bash
# Run once to save session
playwright-cli "Run global-setup to save auth state for admin and sales roles"

# Config reference
# playwright.config.ts → use.storageState: 'playwright/.auth/admin.json'
```
