# End-to-End Payments Testing Framework & Strategy

I built this framework to demonstrate how I approach quality assurance for payment systems in banking. The focus is on clarity, risk-based testing, and compliance awareness.
## What this project covers

- **SEPA credit transfers** with IBAN validation
- **Strong Customer Authentication (SCA)** redirect flow simulation
- **Duplicate payment detection**
- **Currency conversion** with FX margin checks
- **API contract smoke tests** using mocked payment gateway
- **Test strategy** covering risk areas, compliance (PSD2, GDPR), and coverage approach

## Why I structured it this way

I wanted a framework that:
- Runs locally with zero external dependencies
- Validates real payment scenarios banks care about
- Shows my thinking on test design, not just execution
- Can be reviewed in under 10 minutes

## Tech stack

- **Playwright** for E2E UI tests
- **Postman + Newman** for API smoke tests
- **json-server** as a lightweight mock payment gateway
- **GitHub Actions** for CI/CD

## How to run

**Prerequisites:**
- Node.js 18+

**Setup and test:**
```bash
npm install
npm test              # Run all Playwright tests
npm run test:api      # Run API smoke tests with Newman
npm run test:all      # Run both UI and API tests
```

**Run with UI visible:**
```bash
npm run test:headed
```

## Project structure

```
├── app/                      # Demo payment UI (static HTML)
├── tests/
│   ├── ui/                   # Playwright E2E tests
│   └── api/                  # Postman collections
├── docs/
│   ├── test-strategy.md      # My test strategy and risk analysis
│   ├── compliance-notes.md   # PSD2, GDPR, AML considerations
│   └── traceability.md       # Requirements to test cases mapping
├── mock-api/
│   └── db.json              # Mock payment gateway data
└── .github/workflows/        # CI pipeline
```

## What I validated

### Payment scenarios
- ✅ Valid SEPA transfer (SE to SE, SE to DE)
- ✅ Invalid IBAN handling
- ✅ Amount boundary checks (min 1 SEK, max 999,999 SEK)
- ✅ Duplicate payment detection within 60 seconds
- ✅ SCA redirect flow (simulated 3DS)
- ✅ Currency conversion with FX margin display

### API contracts
- ✅ POST /payments — successful initiation
- ✅ GET /payments/:id — status retrieval
- ✅ Error responses (400, 422, 409)
- ✅ Response time <500ms

### Compliance checks
- ✅ SCA triggered for payments >30 EUR equivalent
- ✅ No PII in logs or error messages
- ✅ Consent display before payment submission

## Compliance and risk notes

I documented:
- **PSD2 SCA requirements** and when they apply
- **GDPR considerations** (data minimization, no PII leakage)
- **AML transaction limits** awareness
- **Risk matrix** covering payment rails, error scenarios, and security

See full details in `docs/test-strategy.md` and `docs/compliance-notes.md`.

## CI/CD

GitHub Actions runs:
- All Playwright tests on every push
- API smoke tests
- Generates HTML report artifacts

## What I learned building this

- Balancing test coverage with execution speed
- Designing for compliance early (not as an afterthought)
- Using lightweight mocks to avoid flaky external dependencies
- Writing tests that serve as living documentation

## Next steps if this were production

- Add contract tests with Pact for producer/consumer validation
- Integrate with actual PSP sandbox (Stripe, Adyen)
- Performance testing with k6 for SLA validation
- Expand to cover instant payments (SEPA Instant, Swish)

---

**Built by:** Malsha Jayawardena  

