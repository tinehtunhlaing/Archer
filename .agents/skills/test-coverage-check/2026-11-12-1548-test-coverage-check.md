# Test Coverage Check

- **Reviewed:** 2026-11-12 15:48 (Asia/Yangon, UTC+06:30)
- **Scope:** Archer workspace: API, web app, and mobile client
- **Assessment type:** Static review of specs, project guidance, manifests, test inventory, and relevant source
- **Test execution:** None. Tests and coverage commands were not run.
- **Measured coverage:** Unknown. No coverage report was present.

## Summary

The API now has a focused integration suite in `api/tests/api.test.ts` using Node's built-in test runner and a temporary SQLite database. It covers registration/login, profile read/update, basic role and ownership enforcement, job close and budget validation, proposal submission/rejection, participant-only messaging, and localized validation responses. These tests have not been executed, so their runtime status is unknown.

No test files or test scripts were found in `app` or `mobile`. API authorization and core workflow behavior have the strongest initial coverage; API read-scope rules, proposal withdrawal, search/pagination, and currency edge cases remain the most useful next additions. Mobile workflows are marked deferred in `SPEC.md`, so mobile tests should wait until that scope is confirmed.

## Findings for implementation

### P1 — Add tests for protected list and detail read scopes

The current suite tests role denial for job creation, owner denial for job closing and proposal rejection, and outsider denial for message access. It does not verify the filtering behavior of protected reads. Cover that:

- `GET /jobs/mine` returns only the authenticated client's jobs and rejects non-clients.
- `GET /proposals/me` returns only the authenticated freelancer's proposals and rejects non-freelancers.
- `GET /proposals/jobs/:jobId` is limited to the owning client and returns only that job's proposals.
- `GET /conversations` contains only conversations in which the caller participates.
- A closed job is hidden from non-owners through `GET /jobs/:jobId` while remaining visible to its owner.

These checks exercise the API's server-side ownership and privacy boundaries. Relevant implementation: `api/src/routes/jobs.routes.ts`, `api/src/routes/proposals.routes.ts`, and `api/src/routes/conversations.routes.ts`. Existing related coverage: `api/tests/api.test.ts` lines 165–209, 211–265, and 267–319.

### P2 — Cover the successful proposal withdrawal lifecycle

The suite checks that a different freelancer cannot withdraw a proposal and that a client can reject a pending proposal. It does not cover a proposal owner successfully withdrawing their own pending proposal or the resulting status/readback. Add a focused test for successful withdrawal and rejection of subsequent state changes, plus verify the freelancer's own proposal list reflects the status. Relevant implementation: `api/src/routes/proposals.routes.ts` lines 16–25 and 72–79. Existing related coverage: `api/tests/api.test.ts` lines 232–255.

### P2 — Cover job discovery and cursor behavior

No test currently exercises open-job search, currency filtering, exclusion of closed jobs, or `nextCursor` behavior. Add a small integration scenario with enough jobs to cross a page boundary and confirm filters and cursor results are stable. This protects the public discovery workflow and its API contract. Relevant implementation: `api/src/routes/jobs.routes.ts` lines 31–50. The current job tests focus on creation and closing at `api/tests/api.test.ts` lines 165–209.

### P2 — Cover message `before` pagination and conversation-list scoping

Message ordering and `limit=1` are covered, but the `before` query is not. Conversation-list ownership is also untested. Add a case that pages older messages using `before`, and verify each participant sees their conversation while an unrelated user does not. Relevant implementation: `api/src/routes/conversations.routes.ts` lines 21–45. Existing message coverage: `api/tests/api.test.ts` lines 267–319.

### P2 — Add money boundary cases and MMK proposal currency propagation

The suite confirms that budget decimal strings and MMK currency survive job creation, and that a USD proposal bid retains its string and currency. It does not test rejected malformed or over-precision amounts, nor that a bid on an MMK job receives MMK currency. Add boundary cases around the four-fractional-digit request limit and assert the bid currency follows its job. Relevant implementation: `api/src/routes/jobs.routes.ts` lines 9–29 and `api/src/routes/proposals.routes.ts` lines 10–14 and 49–69. Fractional amount display rules remain an open product decision, so tests should assert the current API contract rather than decide formatting policy.

### P3 — Establish web-client test coverage

No web test files, test script, or test framework configuration were found. Once API coverage is in place, add a small number of user-flow tests for sign-in/session restoration, proposal submission, and messaging error states, along with focused coverage of USD/MMK amount formatting. Avoid exhaustive component tests. Relevant client code includes `app/src/lib/api.ts`, `app/src/lib/auth-context.tsx`, and the marketplace pages under `app/src/pages/`.

### Deferred — Mobile tests after workflow scope is confirmed

No mobile test files or test script were found. The product spec defers mobile workflows; do not treat the absence of mobile tests as an MVP release gap until the product owner confirms which mobile workflows are in scope. If that scope is confirmed, prioritize API error handling and the login, job, proposal, and messaging flows in `mobile/src/api.ts` and `mobile/src/screens/marketplace.tsx`.

## Suggested implementation order

1. Add API tests for protected read scoping (P1).
2. Add successful proposal withdrawal coverage (P2).
3. Cover job and message pagination/filter contracts (P2).
4. Add money validation and MMK bid-currency cases (P2).
5. Choose and add a small web test setup (P3); revisit mobile tests after product scope is decided.
