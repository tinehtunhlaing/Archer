# Archer test coverage findings

- **Inspected:** 2026-11-12 16:23 (Asia/Rangoon)
- **Scope:** Workspace `app`, `api`, and `mobile`; confirmed web-first MVP behavior and durable security/data invariants.
- **Method:** Static inspection of workspace/project `AGENTS.md`, `SPEC.md`, READMEs, package manifests, test inventory, and relevant API/client source. No tests, coverage tools, builds, typechecks, or project commands were run.
- **Execution status:** Tests were not run. No measured coverage is available from the inspected files.

## Overall assessment

The API has one Node integration-test file using an isolated temporary SQLite database. It covers selected auth, role/ownership, proposal creation, participant messaging, message ordering, and localized validation-error behavior. The web and mobile projects have no discovered test files or test scripts. The web client implements confirmed MVP workflows without automated client-level regression coverage. API coverage is strongest for the create/deny paths and thinner for read/list/query behavior and the full proposal lifecycle.

## Prioritized findings

### P1 — Add client-side coverage for confirmed web workflows and API contract handling

- **Missing scenarios:** Exercise web registration/sign-in and session restoration; job browsing/search/currency filtering and pagination; job posting; proposal submission and withdrawal; client proposal review/rejection; profile editing; and participant messaging. Include representative loading, empty, and API-error states where they affect a workflow.
- **Why it matters:** These are confirmed MVP flows, and the UI coordinates routing, React Query cache updates, session state, and API response shapes. The API suite cannot detect client regressions in those interactions or mismatches in the client’s handling of the published contract.
- **Relevant paths:** `app/package.json` (no test script/dependencies), `app/src/lib/api.ts`, `app/src/lib/auth-context.tsx`, `app/src/pages/jobs-page.tsx`, `app/src/pages/job-detail-page.tsx`, `app/src/pages/proposals-page.tsx`, `app/src/pages/messages-page.tsx`, `app/src/pages/profile-page.tsx`.
- **Existing coverage to retain:** API integration coverage for registration/login basics, job creation, proposal creation, and message send/read is in `api/tests/api.test.ts`.

### P1 — Cover API auth rejection and role boundaries across protected route groups

- **Missing scenarios:** Verify missing, malformed, expired, and invalidly signed bearer tokens are rejected; verify unauthenticated access to each protected route group is rejected; and verify the wrong role cannot list or mutate profiles/jobs/proposals/conversations. Include that an invalid optional bearer token cannot read a public job route.
- **Why it matters:** The API is the authorization boundary for every client. Current tests check unauthenticated `/auth/me`, a freelancer denied job creation, and several ownership/participant denials, but do not systematically protect all route groups or token-validation branches from regressions.
- **Relevant paths:** `api/src/auth.ts` (`requireAuth`, `optionalAuth`, `requireRole`), `api/src/routes/profile.routes.ts`, `api/src/routes/jobs.routes.ts`, `api/src/routes/proposals.routes.ts`, `api/src/routes/conversations.routes.ts`, `api/tests/api.test.ts`.

### P1 — Cover job browse, search, filtering, visibility, and pagination contract

- **Missing scenarios:** Assert that unauthenticated browse returns only open jobs; search matches the supported title/description fields; USD/MMK filtering preserves currency; cursor pagination returns non-overlapping pages and a correct `nextCursor`; and closed jobs are hidden publicly but remain visible to their owning client through `/jobs/mine` and the owner detail route.
- **Why it matters:** Job discovery is a core confirmed workflow. The web client depends on query filters and cursor metadata to render results and fetch subsequent pages, yet the current API test only creates and closes jobs.
- **Relevant paths:** `api/src/routes/jobs.routes.ts` (`GET /`, `/mine`, `/:jobId`), `app/src/pages/jobs-page.tsx`, `api/tests/api.test.ts`.

### P1 — Cover the proposal lifecycle and review/list responses

- **Missing scenarios:** Assert a freelancer can list only their own proposals, a job owner can list that job’s proposals, pending proposals can be withdrawn by their owner, non-pending proposals cannot be withdrawn/rejected, and proposal submission without a bid preserves a null bid/currency. Include bid currency/decimal preservation for both supported currencies.
- **Why it matters:** The MVP depends on proposals as the bridge between job discovery and messaging. Current tests validate submission, conversation creation, duplicate rejection, ownership denials, and client rejection, but do not verify the freelancer withdrawal success path or either list response.
- **Relevant paths:** `api/src/routes/proposals.routes.ts`, `api/tests/api.test.ts`, `app/src/pages/proposals-page.tsx`, `app/src/pages/job-detail-page.tsx`.

### P2 — Cover conversation inbox and message history pagination

- **Missing scenarios:** Verify `/conversations` returns only conversations for the caller; verify message `before` pagination returns the expected older page in chronological order; cover an unknown conversation and message-length boundaries; and assert user-authored message text is returned unchanged.
- **Why it matters:** Messaging is a confirmed client/freelancer workflow. Existing tests cover participant denial, blank input, two-party send/read, and a one-message limit, but not inbox isolation or cursor-based history retrieval used to load older messages.
- **Relevant paths:** `api/src/routes/conversations.routes.ts`, `api/tests/api.test.ts`, `app/src/pages/messages-page.tsx`.

## Product clarification (not counted as a confirmed test gap)

- The spec defers mobile workflows until they are specified, while `mobile/README.md` describes implemented API-backed workflows for jobs, proposals, profiles, and messaging. `mobile/package.json` has no test script and no test files were found. Confirm the intended mobile MVP scope before prioritizing behavioral mobile tests; once confirmed, cover its API adapter, authentication/token lifecycle, and selected screens against the API contract.
- Currency precision and display rules, including fractional MMK values, remain open in `SPEC.md`. Existing tests can protect exact decimal-string transport and storage, but should not assert unresolved formatting policy as a product requirement.

## Next implementation priorities

1. Add focused web workflow tests around the confirmed MVP and its API interactions.
2. Expand API integration tests for protected-route authentication/role boundaries and job browse/list behavior.
3. Complete proposal lifecycle and conversation inbox/history coverage.
4. Resolve mobile scope and currency display decisions before setting corresponding behavioral test expectations.
