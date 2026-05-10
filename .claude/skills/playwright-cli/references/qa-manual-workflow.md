# Manual QA Workflow

Use this when the user asks to: write test cases, create test plan, analyze requirements for QA, build traceability matrix, generate test cases from user stories or specs.

## Overview

6-step workflow orchestrated by `#qa-manual-workflow` agent:

```
Step 1: #qa-context-roleplay   → Establish QA context & scope
Step 2: #qa-analysis-decompose → Risk analysis + feature tree + AC
Step 3: #qa-tc-generation      → Write test cases (RBT, P1→P2→P3)
Step 4: #qa-traceability       → Build RTM + export
```

## How to invoke

```bash
# Full workflow
playwright-cli "Run #qa-manual-workflow for feature: [feature name]"

# With requirements pasted inline
playwright-cli "Run #qa-manual-workflow for feature: Customer CRUD, requirements: [content]"

# Single step only
playwright-cli "Run #qa-context-roleplay for feature: Login"
playwright-cli "Run #qa-tc-generation for feature: Login"
```

## Output files (saved under specs/ by default)

| Step | Output |
|------|--------|
| Step 2 | `specs/[feature]-feature-tree.md` |
| Step 3 | `specs/[feature]-test-cases.md` |
| Step 4 | `specs/[feature]-rtm.md` |

## Test case priority convention

- **P1** — Block release if fail (happy path + critical validation + permission)
- **P2** — Business logic, edge cases
- **P3** — Regression, cosmetic, future sprint

## Test case types

`Functional` | `Negative` | `Edge Case` | `Permission` | `Integration`

## RTM status convention

`TODO` → `DRAFT` → `READY` → `APPROVED` | `BLOCKED`

## Export formats supported

Pass `export-format` param to `#qa-traceability`:
- `markdown` — default, saved as `.md`
- `csv` — for Excel / Google Sheets import
- `gherkin` — for Cucumber / BDD frameworks
