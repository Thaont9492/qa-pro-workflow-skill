# qa-pro-workflow-skill

An AI-powered QA skill that combines **Claude AI Agent** and **Playwright CLI** to drive a full QA lifecycle — from manual test case generation all the way to automated test execution — through structured, step-by-step workflows.

## What it does

Given a feature name or module, this skill:

1. Analyzes requirements and decomposes them into a feature tree + acceptance criteria
2. Generates prioritized test cases (P1 → P2 → P3) with a Requirement Traceability Matrix (RTM)
3. Maps UI selectors and navigation flows via live browser recon
4. Generates Page Object Model (POM) classes in TypeScript
5. Writes `.spec.ts` test scripts and reviews them for anti-patterns
6. Executes tests via Playwright CLI and reports results

## Tech Stack

- [Playwright CLI](https://playwright.dev/) (`@playwright/cli@latest`) — browser automation & test execution
- Playwright Test + TypeScript — test framework
- Page Object Model — test architecture
- Claude AI Agent — workflow orchestration & code generation
- Node.js (LTS)
- GitHub Actions — CI/CD

## Project Structure

```
qa-pro-workflow-skill/
├── tests/                    # Generated .spec.ts test files
│   └── [module]/
│       ├── [module]-create.spec.ts
│       ├── [module]-update.spec.ts
│       └── [module]-delete.spec.ts
├── pages/                    # Page Object Model classes
│   ├── BasePage.ts
│   └── [module]/
│       ├── [Module]ListPage.ts
│       └── [Module]FormPage.ts
├── specs/                    # QA documentation artifacts
│   ├── [feature]-feature-tree.md
│   ├── [feature]-test-cases.md
│   ├── [feature]-rtm.md
│   └── [module]-selector-inventory.md
├── .claude/skills/playwright-cli/  # Playwright CLI skill definition
├── .github/workflows/        # CI pipeline
├── playwright.config.ts
└── CLAUDE.md                 # QA workflow config & agent definitions
```

## Setup

```bash
npm install
npx playwright install

# Install Playwright CLI globally
npm install -g @playwright/cli@latest
playwright-cli --help
```

## Available QA Workflows

### Manual QA Workflow — `#qa-manual-workflow`

Generates test documentation from requirements (6 steps).

```bash
# Full workflow
playwright-cli "Run #qa-manual-workflow for feature: [feature name]"

# With requirements pasted inline
playwright-cli "Run #qa-manual-workflow for feature: Customer CRUD, requirements: [content]"

# Single step
playwright-cli "Run #qa-tc-generation for feature: Login"
```

**Output artifacts:**

| Step                | File                              |
| ------------------- | --------------------------------- |
| Feature analysis    | `specs/[feature]-feature-tree.md` |
| Test cases          | `specs/[feature]-test-cases.md`   |
| Traceability matrix | `specs/[feature]-rtm.md`          |

**Test case priorities:**

| Priority | Criteria                                                       |
| -------- | -------------------------------------------------------------- |
| P1       | Blocks release — happy path, critical validations, permissions |
| P2       | Business logic, edge cases                                     |
| P3       | Regression, cosmetic, future sprint                            |

---

### Automation QA Workflow — `#qa-automation-workflow`

Generates POM classes + Playwright spec files, then executes them (5 steps).

```bash
# Full workflow
playwright-cli "Run #qa-automation-workflow for module: [module name], app URL: [staging URL]"

# With existing manual test cases as input
playwright-cli "Run #qa-automation-workflow for module: Customer, app URL: https://staging.example.com, manual TC file: specs/customer-test-cases.md"

# Single step
playwright-cli "Run #qa-ui-recon for module: Customer, app URL: https://staging.example.com"
playwright-cli "Run #qa-pom-generation for module: Customer"
playwright-cli "Run #qa-script-generation for module: Customer, mode: review-and-fix, spec files: tests/customer/"
```

**Output artifacts:**

| Step            | File                                    |
| --------------- | --------------------------------------- |
| UI recon        | `specs/[module]-selector-inventory.md`  |
| POM — list page | `pages/[module]/[Module]ListPage.ts`    |
| POM — form page | `pages/[module]/[Module]FormPage.ts`    |
| Test specs      | `tests/[module]/[module]-*.spec.ts`     |

**Selector priority (enforced by UI recon):**

1. `data-testid` — preferred
2. `getByRole('button', { name: '...' })`
3. `getByLabel` / `getByPlaceholder`
4. CSS selector — last resort only

**Test tags convention:**

```typescript
test('should ... @smoke @p1', ...)  // run before every deploy
test('should ... @p2', ...)         // full regression
test('should ... @p3', ...)         // weekly / on demand
```

## Running Tests

```bash
# Run all tests
npx playwright test

# Run by tag
npx playwright test --grep @smoke
npx playwright test --grep @p1

# Run on a specific browser
npx playwright test --project=chromium
npx playwright test --project=firefox
npx playwright test --project=webkit

# Run with UI mode
npx playwright test --ui

# View HTML report
npx playwright show-report
```

## Auth Setup (storageState)

```bash
# Save session state once (for tests that require login)
playwright-cli "Run global-setup to save auth state for admin and sales roles"
```

Reference in `playwright.config.ts`:

```typescript
use: { storageState: 'playwright/.auth/admin.json' }
```

## CI/CD

GitHub Actions runs the full test suite on every push or pull request to `main`/`master`. HTML reports are uploaded as artifacts and retained for 30 days.

## Core Principles

- Always read project context before doing anything
- Ask when requirements are unclear — never assume
- Follow workflow steps in order, never skip
- Docs in Vietnamese, code in English
- No `waitForTimeout()` / hardcoded credentials / dynamic CSS selectors in generated specs
