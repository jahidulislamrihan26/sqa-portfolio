# SQA Portfolio

Four types of software testing, each in its own folder.

| Folder | Type | Target | Tool |
|---|---|---|---|
| `manual-testing/` | Manual | https://www.saucedemo.com | Test plan, test cases (CSV), bug reports |
| `api-testing/` | API | https://restful-booker.herokuapp.com | Postman collection + Newman, JSON Schema, data-driven |
| `performance-testing/` | Performance | Local demo shop API (plus a tiny public baseline) | k6: smoke, load, stress, spike, soak |
| `automation-testing/` | UI automation | https://www.saucedemo.com | Playwright |

## Run locally

```bash
# API tests
cd api-testing && npm install && npm test

# Performance test (needs k6)
k6 run performance-testing/load-test.js

# UI automation
cd automation-testing && npm install && npx playwright install chromium && npm test
```

CI (`.github/workflows/ci.yml`) runs API and UI tests on every push.
