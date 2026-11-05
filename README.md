# SQA Portfolio

[![CI](https://github.com/jahidulislamrihan26/sqa-portfolio/actions/workflows/ci.yml/badge.svg)](https://github.com/jahidulislamrihan26/sqa-portfolio/actions/workflows/ci.yml)

**[Open the live test reports](https://jahidulislamrihan26.github.io/sqa-portfolio/)** (UI, API and performance, regenerated on every push).

Four complete software-testing projects, from planning to reports to automated checks in CI. Each folder stands alone with its own README, plan, results and findings. Every number below was measured by running the tests; nothing is estimated.

## The four projects at a glance

| Project | Target | What it shows | Headline result |
|---|---|---|---|
| [**Manual testing**](manual-testing) | [saucedemo.com](https://www.saucedemo.com) (web shop) | Strategy, risk-based plan, 85 test cases, traceability matrix, 6 exploratory charters, defect reports with screenshots, summary report | 85 cases run: 60 pass, 25 fail. **19 defects** (10 genuine), including a login bypass with a forged cookie and checkout accepting empty or repeated orders. Verdict: not ready for release |
| [**API testing**](api-testing) | [Restful-Booker](https://restful-booker.herokuapp.com) (booking API) | Postman and Newman suite built on an OpenAPI 3 spec I wrote: schema contract checks, auth, a 14-body validation matrix, HTTP behaviour (ETag, 304, 415, 406), security checks, data-driven runs, coverage report, self-cleaning test data | 186 assertions: 151 pass, 35 expected failures, **0 unexpected**; all 23 documented responses covered. **13 defects**, e.g. invalid dates stored as `0NaN-aN-aN`, decimals truncated, failed login returns 200 |
| [**Performance testing**](performance-testing) | Demo shop API (run locally) | k6 smoke, load, stress, spike and soak tests with pass/fail targets, an HTML report, and recommendations | Healthy to **about 500 req/s**, then a cliff at 600 (p95 21 ms to 1.5 s). Fails fast, recovers fully. Soak flat |
| [**UI automation**](automation-testing) | [saucedemo.com](https://www.saucedemo.com) | Playwright and TypeScript with the **Page Object Model**, fixtures, data-driven tests, accessibility scans, expected-failure tests for known bugs | 69 tests on **Chromium, Firefox and WebKit**; 208 of 208 runs passed in a flakiness check |

## How the projects connect
```mermaid
flowchart LR
    M[Manual testing<br/>find defects, write<br/>evidence] -->|19 defects, test ideas| A[UI automation<br/>regression suite,<br/>known bugs as tests]
    M -->|same practice site| A
    API[API testing<br/>contract, security,<br/>known bugs as tests] --> CI
    A --> CI[GitHub Actions<br/>every push + nightly]
    P[Performance testing<br/>k6 profile] --> CI
```
The defects found by hand in the manual project reappear as expected-failure tests in the automation project, and the same idea (a test that states the *correct* behaviour and is allowed to fail until the bug is fixed) is used in the API project. CI stays green only for known problems and turns red for anything new.

## Where to start
- **Quick read (5 minutes):** [manual test summary](manual-testing/docs/05-test-summary-report.md), [API summary](api-testing/docs/test-summary-report.md), [performance summary](performance-testing/docs/test-summary-report.md).
- **See the code structure:** [page objects](automation-testing/pages) and [fixtures](automation-testing/fixtures).
- **Open the live reports:** [UI](https://jahidulislamrihan26.github.io/sqa-portfolio/ui/playwright-report/index.html), [API](https://jahidulislamrihan26.github.io/sqa-portfolio/api/functional-report.html), [performance](https://jahidulislamrihan26.github.io/sqa-portfolio/performance/).
- **See evidence:** [defect reports with screenshots](manual-testing/defects), [API defect reports](api-testing/docs/defects/README.md), [Excel workbook](manual-testing/test-suite/SauceDemo-Test-Suite.xlsx).

## Run it yourself
Needs Node 20 or newer. The performance project also needs [k6](https://k6.io/docs/get-started/installation/).
```bash
cd api-testing && npm install && npm test                     # API suite, writes HTML and JUnit reports
cd performance-testing && npm test                            # about 12 minutes; npm run test:ci is the short profile
cd automation-testing && npm install && npx playwright install chromium && npm test
cd manual-testing/tools && npm install && npm run build       # regenerates the Excel workbook and reports
```

CI (GitHub Actions) runs the API suite, the UI suite on three browsers and a short performance profile on every push and nightly.

## License
Released under the [MIT License](LICENSE). Copyright (c) 2026 Md. Jahidul Islam Rihan.
