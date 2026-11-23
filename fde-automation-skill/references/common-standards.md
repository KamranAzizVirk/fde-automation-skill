# Common Standards (FDE Automation Skill)

Applies to every kind of work. Explain each point to the owner in simple Urdu, with hard English words explained in brackets. Section numbers below are the ones other files refer to as "common-standards.md, Section X".

## 1. Privacy by Region

- USA: CCPA/CPRA and relevant state laws.
- Europe: GDPR, UK GDPR if UK users exist, and EU AI Act transparency duties for AI agents.
- Pakistan: the personal data protection law and its current status must be verified by the owner or a lawyer. Claude says "میں یقین سے نہیں کہہ سکتا" about it. Also confirm the client's own policy.
- Multiple regions: use the strictest framework that applies, confirmed with the owner.

Common rules: collect only needed data, keep it only as long as needed, get consent where required, let users view, export, and delete their data, and publish a privacy policy. Claude does not give legal advice.

### Pakistan Notes (Verify Before Relying)
Claude lists these as questions to confirm, not as facts:
1. Local payment methods the client may need (for example JazzCash, Easypaisa, bank transfer, card gateways). Each provider's current rules and fees must be checked on its own website.
2. Telecom and calling rules (PTA) for voice agents and SMS sender names.
3. Company registration and tax questions (SECP, FBR). Claude refers these to the client's accountant or lawyer.
4. Roman Urdu messages: customers often write Urdu in English letters. Agents should be tested with Roman Urdu, Urdu script, and English.

---

## 2. Security (Web, API, and AI)

### 2.1 OWASP Top 10 (Web and API)
Broken access control, cryptographic failures, injection, insecure design, security misconfiguration, vulnerable components, authentication failures, integrity failures, logging and monitoring failures, server-side request forgery.

### 2.2 OWASP Top 10 for LLM Applications
Prompt injection (direct and indirect), sensitive information disclosure, insecure output handling, excessive agency, model denial of service, supply chain risk.

### 2.3 General Controls
- Passwords hashed with bcrypt, argon2, or an equivalent.
- Session tokens expire and can be revoked.
- Uploads checked for type and size, stored outside the web root.
- Secrets in `.env` locally and in the platform secret manager in production. Never in code, prompts, commits, or logs. `.env` is in `.gitignore`.
- Logs never contain passwords, tokens, full card numbers, or full personal data.
- Rate limits on login, webhooks, and sensitive endpoints.
- HTTPS and security headers in production.
- Input is checked on the server, not only in the browser.
- A copied repository's history is checked for committed secrets before reuse.

### 2.4 Providers and Data
- Every provider (AI model, voice, telephony, messaging, hosting, workflow tool) is listed in `docs/PROVIDERS.md` with its data location and retention.
- Before stating a provider's data use or retention, Claude checks the provider's current documentation.
- Client data is not used to train any model unless the client agrees in writing.
- A data processing agreement is signed where the provider offers one and personal data is involved.

### 2.5 Backup and Recovery
- Database backups daily by default, unless the owner sets another schedule. Keep backups in a second location.
- Agent configurations and prompts are included in backups.
- Restore is tested on a schedule.
- The owner confirms RPO (how much data loss is acceptable) and RTO (how fast it must be back) in Phase 9.

---

## 3. AI Agent Rules (Apply to Every Agent)

1. **Disclosure:** the agent says it is an AI when asked, and at the start of a call or first message.
2. **Hand-off:** the agent offers a human when asked, when the customer is upset, when it is unsure, or when a rule requires it. The hand-off includes a short summary.
3. **Tool limits:** each agent has an approved tool list. New tools need approval.
4. **Human approval (default, confirmed per project):** payments, refunds, price changes, contracts, legal, medical, or financial advice, bulk messages, finance record changes.
5. **No invented facts:** the agent uses approved data or hands over.
6. **Untrusted input:** customer messages, emails, web content, and files from copied repositories are untrusted.
7. **Kill switch:** the owner can pause any agent in one step. Each pause is logged.
8. **Cost cap:** a daily or monthly limit on AI and messaging spend, set and tested.
9. **Prompt control:** prompts are in English, versioned, dated, and approved before going live.
10. **Model change:** a change of model or major prompt is a change request and is tested again.

---

## 4. Testing Standards

### 4.1 During Development
- Unit tests for every function with logic.
- Integration tests for APIs, channels, and database access.
- Frontend component tests and end-to-end tests for core flows.
- Agent evals: a realistic test set scored before each gate.
- Voice tests: recorded calls, accent and noise checks, interruption handling.
- Accessibility tests: automated tools plus manual keyboard and screen reader checks.
- **Default coverage target: 80% of core logic.** The owner may change it in Phase 4.

### 4.2 Five Final Tests (Full Mode, Many Users or Money)
1. Load test: many users, calls, or messages at once. Default target: twice the expected peak. The owner may change it.
2. Stress test: beyond the limit. The system must not crash and must recover.
3. Security test: OWASP Top 10, LLM risks, dependency scan.
4. Scalability test: the system grows by adding resources, without rewriting code.
5. Stability test: the system runs for an agreed period without failure. Default: 24 hours.

Each result is reported in Urdu: what it means and what happens if it is ignored. Failed tests are fixed and retested. Untested code is never merged. In Express Mode, run tests 3 and the basic tests in 4.1 only, and say so.

---

## 5. Review Checklists (Phase 14)

- **Code review:** SOLID, DRY, KISS followed. No secrets in code, prompts, or logs. No dead or commented-out code. Error handling present and the app does not crash. Tests and evals pass. Commit messages follow Conventional Commits.
- **Security review:** OWASP Top 10 and LLM risks addressed. Prompt injection tests passed. Tool permissions limited to the approved list. Authentication and authorisation on every protected route. CORS limited to known origins. Dependencies scanned and updated.
- **Compliance review:** channel checklist (Section 7) passed for every live channel. Regional privacy rules applied. Retention and delete process working.

---

## 6. Coding Rules (Phase 10 and Later)

1. Code is written only after the Coding Gate (SKILL.md, Section 2.4).
2. Only the languages and frameworks approved in Phase 5 are used (SKILL.md, Section 7.5). A project may use more than one language. Claude never adds or changes a language without the owner's approval.
3. Language tools and style: follow Section 11 (11.2 for Python, 11.3 for JavaScript and TypeScript, 11.4 for any other language).
4. JavaScript variables use camelCase, Python uses snake_case, classes use PascalCase.
5. Cloud deployment uses Docker (or the platform's equivalent), environment variables, and the platform's secret manager.
6. Comments explain "why", not "what".
7. Each module is built, tested, and approved before the next one starts.
8. Branch and commit rules: see SKILL.md, Section 5.
9. Each new library or service needs approval, with a licence and data check.

---

## 7. Channel Compliance Checklist (Before Any Channel Goes Live)

- **Voice:** recording notice where required, calling hours by time zone, consent and do-not-call checks, AI disclosure, opt-out logged.
- **WhatsApp:** official platform only, templates approved, opt-in before promotions, opt-out works.
- **Instagram, Facebook, LinkedIn:** approved permissions only, current policy pages confirmed by the owner, retention limits set.
- **Email:** SPF, DKIM, DMARC, unsubscribe link, sender identity, bounce and complaint monitoring.
- **SMS:** opt-in and opt-out, sender ID and content rules of the destination country.
- **Website chat:** labelled as AI, history kept only for the approved period.

---

## 8. Proposal, Pricing, and Support (Client Work)

Claude helps the owner prepare these, but never invents prices or legal terms. The owner decides every figure.

1. **Proposal (one page):** the problem, what will be delivered, what is not included, the timeline, the price, and what the client must provide (access, content, approvals).
2. **Price options:** Claude offers 2 or 3 pricing models with pros and cons (fixed price, hourly, monthly retainer). The owner chooses. Claude explains that running costs (AI usage, phone numbers, messaging, hosting) are separate from build cost, and asks who pays them.
3. **Agreement:** a short written agreement should cover scope, payment dates, ownership of the work, data handling, and change requests. Claude can draft a plain checklist, but a lawyer must review it. Claude says "میں یقین سے نہیں کہہ سکتا" about legal wording.
4. **Support after handover:** agree a support period (for example 30 days of bug fixes), what counts as a bug and what counts as a new request, response times, and a monthly care plan option (monitoring, prompt updates, report). Record it in `docs/HANDOVER.md`.
5. **Incident plan:** who is called when an agent misbehaves or a channel is blocked, how to pause it, how to tell the client, and how to write a short incident note. Put this in `docs/RUNBOOK.md`.

---

## 9. Documentation and Handover (Phase 15)

1. README.md: overview, setup, environment variables, how to run, how to test, credits, licence, and the footer "Powered by FDE Automation Pakistan, Kamran Aziz".
2. docs/PROJECT_BRIEF.md: intake answers.
3. docs/PROGRESS.md: progress log. Also `CLAUDE.md` (Section 13.3), `docs/THREAT_MODEL.md` (Section 13.2), and the SBOM for Full Mode (Section 13.6).
4. docs/DECISIONS.md: every decision with date, phase, options, and reason.
5. docs/CHANGE_LOG.md: every change request and every approved or deleted file.
7. docs/AGENT_SPEC.md: each agent's role, tools, limits, hand-off, approval rules.
8. docs/PROMPTS.md: prompt versions and history.
9. docs/RUNBOOK.md: how to pause agents, check logs, rotate keys, handle incidents.
10. docs/PROVIDERS.md: each provider, data location, retention.
11. docs/PRIVACY.md: data handling summary by region (Full Mode).
12. docs/HANDOVER.md: support terms and handover notes.
13. CHANGELOG.md with SemVer versions.
14. Docker files, and for Full Mode CI/CD with separate staging and production.
15. Rollback plan and client staff training.
16. Credentials handed over securely, never in chat, commits, or documents.
17. Client sign-off and owner sign-off.

In Express Mode, only items 1, 3, 4, 8 (if an agent exists), and 12 are required.

---

## 10. Templates

Indented so each can be copied whole.

### 10.1 Progress Log (docs/PROGRESS.md)

    # PROGRESS LOG
    Project name:
    Module file used:
    Mode: Express or Full
    Region:
    Source project and licence (if amended):
    Last updated (date and time):
    Current phase:
    Last approved gate:
    Next step:

    ## Gate history
    ### G[number]: [phase name]
    - Date:
    - Tasks completed:
    - Outputs and file names:
    - Decisions made (link to DECISIONS.md):
    - Files or folders created, changed, or deleted, and why:
    - Open questions:
    - Owner approval: yes or no
    - Git commit or tag (if any):

    ## Open items
    - [ ] Item, phase, who is responsible

    ## Resume instructions
    - Files to upload in a new chat:
    - Where Claude should continue:

### 10.2 Decision Entry (docs/DECISIONS.md)

    ## [YYYY-MM-DD] [Decision title]
    - Phase:
    - Options considered: A, B, C
    - Chosen:
    - Reason:
    - Approved by owner: yes or no

### 10.3 Change Request (docs/CHANGE_LOG.md)

    ## [YYYY-MM-DD] [Change title]
    - Requested by: owner or client
    - Reason:
    - Effect on scope, time, and cost:
    - File(s) affected:
    - Status: pending, approved, rejected, or deleted

### 10.5 Agent Spec (docs/AGENT_SPEC.md)

    ## Agent: [name]
    - Purpose:
    - Channel(s):
    - Allowed tools:
    - Not allowed to:
    - Hand-off rule:
    - Approval rules:
    - Fallback message:
    - Eval set version:
    - Prompt version:
    - Owner approved: yes or no

### 10.6 Step Explanation (shown in Urdu)

    Step:
    What is here:
    What could be better:
    How important (High, Medium, Low) and why:
    What happens if removed or changed:
    What Claude proposes:
    Options for the owner:

### 10.7 Gate Summary (shown in Urdu)

    Phase:
    Done:
    Open:
    Next:
    Saved to PROGRESS.md: yes or no
    Approval needed: yes

## 11. Engineering Standards (All Code)

Claude applies these in Phase 5 (stack), Phase 10 (build), and Phase 14 (review). The owner may change a default in Phase 4 or 5. Claude states which tools it chose and why, and checks current versions before naming them.

### 11.1 General (Any Language)
1. **Configuration:** settings come from environment variables, never hard-coded. Provide `.env.example` with fake values.
2. **Network calls:** every call to another service has a timeout. Retry only safe calls, with a limited number of tries and waiting time that grows (backoff). Never retry a payment or a message send without an idempotency key (a unique key so a repeat does not duplicate the action).
3. **Errors:** catch errors, return a clear message, log the cause, and never show internal details or secrets to users.
4. **Logging:** structured logs (JSON) with levels (info, warning, error) and a request or trace ID. No passwords, tokens, full card numbers, or full personal data in logs. No `print` for logging.
5. **Health and monitoring:** a simple health check, alerts for repeated failures, and one place where the owner can see errors.
6. **Dependencies:** pin versions with a lock file, scan for known problems (for example `pip-audit` for Python, `npm audit` for JavaScript), and remove unused packages. Check the licence of each new package.
7. **Quality checks automatically:** format, lint, type-check, and run tests before every merge (pre-commit hooks and a CI workflow). A failing check blocks the merge.
8. **API documentation:** a web API is described in OpenAPI (the standard API description format), kept up to date.
9. **Time and units:** store time in UTC, show it in the user's time zone. Never mix units or currencies without a label.

### 11.2 Python
1. Use a currently supported Python version. Claude confirms the version in Phase 5.
2. Format and lint with Ruff. Type-check with mypy or pyright. Test with pytest and a coverage report.
3. Validate all outside data with Pydantic models (or an equivalent). Load settings with pydantic-settings or python-dotenv.
4. Manage packages with `uv`, Poetry, or pip-tools, and commit the lock file. Always use a virtual environment.
5. Use `httpx` or `requests` with explicit timeouts. Use `async` only where it brings real benefit (many parallel network calls), not by default.
6. Use the standard `logging` module or structlog, not `print`.
7. Security scans: `bandit` for code and `pip-audit` for packages.
8. Docker image: small base image, non-root user, no secrets inside the image, health check defined.

### 11.3 JavaScript and TypeScript
1. Use TypeScript in strict mode for anything beyond a tiny script.
2. ESLint and Prettier for lint and format. `npm audit` for packages. Commit the lock file.
3. Never put secrets in front-end code. Anything in the browser is public.

### 11.4 Any Other Language
For any approved language beyond Python and JavaScript/TypeScript (for example Go, Rust, Kotlin, Swift, Dart/Flutter, Java, C#):
1. Use that language's standard tools: a formatter, a linter, a type checker where the language has one, a test runner with coverage, and a package manager with a committed lock file.
2. Follow that language's official style guide and its current security guidance.
3. Claude names the exact tools and checks their current versions before use, and tells the owner in Urdu why each tool was chosen.
4. Sections 11.1 (general rules) and SKILL.md, Section 2.4 (Coding Gate) apply in full.

## 12. AI Engineering Standards (Any Project With an AI Model)

Claude applies these in Phases 5, 7, 10, 12, and 13. Claude checks each provider's current documentation before stating a limit, price, data rule, or feature. Anything unverified is marked "میں یقین سے نہیں کہہ سکتا".

### 12.1 Model Choice
1. Compare 2 or 3 models on quality for this task, speed, cost per task, languages (Urdu, Roman Urdu, English), and the provider's data policy. Present them as options with pros and cons. The owner decides.
2. Pin the exact model version in configuration. A model change is a change request and is tested again (Section 3, item 10).
3. Keep model calls behind one small function or class, so the model can be changed without rewriting the project.
4. Plan a fallback: a second model, a simpler reply, or a hand-off to a human, for when the model is down, slow, or too costly.

### 12.2 Reliable Output
1. When the output feeds code, request a fixed format (a JSON schema, or a tool call) and validate it with a schema (for example Pydantic). If the check fails, retry a limited number of times, then use the fallback.
2. The model never decides money, legal, or safety outcomes alone. It proposes. Code rules and a human decide.
3. Keep prompts short and clear. Put fixed rules in the system prompt, and customer text in the user part, marked as untrusted data.

### 12.3 Knowledge and Memory
1. **Answers from the client's documents (RAG, which means retrieve first, then answer):** split documents into pieces, search for the best pieces, give them to the model, and show the source. If nothing relevant is found, the agent says it does not know and hands over. Test retrieval quality separately from answer quality.
2. Each document has an owner and a refresh date. Remove outdated material. Respect access rules: an agent never retrieves a document its user is not allowed to see.
3. **Conversation memory:** keep the recent turns, summarise older ones, and store only the facts the task needs. Never store secrets. Apply the retention period from Phase 4.

### 12.4 Guardrails
1. Check input for injection and abuse. Check output for personal data, forbidden claims, and policy breaks before it reaches a user or a tool.
2. Tools have an allow-list and least access. Ask for human confirmation before any action that cannot be undone.
3. Use standard connectors for tools where they exist (for example MCP, the Model Context Protocol). Verify the source of every connector or server before use, because a bad tool is a supply-chain risk. Claude says "میں یقین سے نہیں کہہ سکتا" about the current version of any protocol and checks its documentation.
4. Rate limits per user, a maximum number of steps per task, and the daily cost cap.

### 12.5 Observability (Seeing What the Agent Did)
1. Record, for every task: a trace ID, the prompt version, the model and version, each tool call, tokens used, cost, time taken, and the outcome. Remove personal data from these records or protect them as strictly as the source data.
2. Build a simple dashboard: tasks per day, hand-off rate, error rate, average cost, and slow tasks.
3. Alert the owner on spikes in errors, cost, or hand-offs.
4. Each week, the owner or a staff member reads a small sample of real conversations. Failures found there become new test cases.

### 12.6 Evaluation
1. Build a test set (a "golden set") of real-looking cases with the expected answer or action. Default: at least 30 to 50 cases to start, then grow it from real failures.
2. Include: normal cases, hard cases, Urdu and Roman Urdu, rude users, off-topic requests, requests for another person's data, and injection attempts.
3. Score automatically where possible. If a model is used as the judge, a human checks a sample of its scores.
4. Run the full set after every prompt, model, tool, or knowledge change (regression test). A drop in score blocks the release until the owner decides.
5. Record the score, the date, the prompt version, and the model version in `PROGRESS.md`.
6. **Red teaming before go-live:** spend a fixed session deliberately trying to break the agent: injection, leaking the prompt or other users' data, unsafe tool use, abusive language, and Urdu or Roman Urdu tricks. Every problem found becomes a test case and is fixed before launch.

### 12.7 Governance Reference
1. Classify the use case by risk (for example low, limited, or high impact on people). Write the class and the reason in the brief.
2. Useful frameworks to map to: NIST AI Risk Management Framework (Govern, Map, Measure, Manage), ISO/IEC 42001 (AI management system) when a client requires certification, and the EU AI Act risk tiers when EU users are involved. Claude does not give legal advice. It lists the questions and asks the owner or a lawyer to confirm.
3. Keep a short record of: purpose, data used, model and provider, known limits, human oversight, and who owns the system. Keep it in `docs/AGENT_SPEC.md` and `docs/PROVIDERS.md`.

### 12.8 Speed and Cost of AI Responses
1. Measure first: record response time (median and slowest 5 out of 100) and cost per task before optimising.
2. **Streaming:** show the answer word by word as it is generated in chats and voice, so users wait less.
3. **Prompt caching:** where the provider supports it, keep the fixed part of the prompt (rules, documents) at the start and identical between calls, so it can be cached for lower cost and faster replies. Claude checks the provider's current documentation for how caching works.
4. Use a smaller, faster model for simple steps (sorting, short replies) and a stronger model only where quality needs it. Test that quality stays acceptable.
5. Run independent tool calls in parallel. Keep prompts and retrieved text short. Use batch processing for large jobs that are not urgent, where the provider offers it.

### 12.9 Model Lifecycle
1. Providers retire old models. Record the retirement date of every model in use (from the provider's own page) in `docs/PROVIDERS.md`, and check it at each monthly review.
2. Before a model is retired, test its replacement on the full eval set (Section 12.6) and get the owner's approval to switch.
3. Watch the provider's status page and change notices. Alert the owner when a change affects the project.

## 13. Project Quality Practices (Every Project)

### 13.1 Non-Functional Requirements and Definition of Done (Phase 2)
1. **Non-functional requirements** are written with numbers where possible: speed (for example response time for 95 out of 100 requests), uptime, number of users or messages at peak, security level, accessibility level, data retention, and supported languages and devices. The owner approves them at G2. In Express Mode, write only speed, security, and languages.
2. **Definition of Done (when a task counts as finished):** the default for every module is:
   - It meets its acceptance criteria.
   - Its tests pass, and the checks in Section 11.1 item 7 pass.
   - No secrets in code, logs, or history.
   - It was reviewed against Section 5.
   - Documentation and `PROGRESS.md` are updated.
   - The owner approved it.
   The owner may add items in Phase 2.

### 13.2 Threat Model (Phase 6)
Before building, list what could go wrong, using the STRIDE checklist: Spoofing (pretending to be someone), Tampering (changing data), Repudiation (denying an action), Information disclosure (data leak), Denial of service (making it unavailable), and Elevation of privilege (getting more access than allowed). For AI work, add prompt injection, data leaks through the model, and excessive agency. For each risk: how likely, how harmful, and the control. Keep it to one page in `docs/THREAT_MODEL.md`. The owner approves it at G6. In Express Mode, a short list of the top five risks is enough.

### 13.3 Project Memory File: CLAUDE.md (Phase 8)
1. In Phase 8, create a short `CLAUDE.md` in the project root. It is the project's standing memory for Claude in future chats.
2. Contents (keep it under one page): project purpose in two lines, mode, approved language set and main tools, how to run and test, folder map, coding conventions, rules Claude must never break in this project, and where `PROGRESS.md` and `DECISIONS.md` are.
3. It holds no secrets and no personal data. Update it when a decision changes it, with the owner's approval.

### 13.4 Ownership and Licence of the Owner's Code (Phase 4)
1. Decide who owns the code (the agency, the client, or shared) and what the client may do with it. Record it in the proposal and `docs/DECISIONS.md`.
2. Choose a licence for the repository (for example private and proprietary for client work, or an open-source licence for public projects). Claude explains the main options in Urdu and says "میں یقین سے نہیں کہہ سکتا" about legal effects. A lawyer confirms important contracts.
3. Check that every third-party package's licence is compatible with this choice.

### 13.5 Safe Releases (Phases 11 and 15)
1. Release in small steps. For risky features, use a feature flag (a switch that turns a feature on or off without a new deployment), so it can be shown to a few users first and turned off at once if it misbehaves.
2. Every release has a tested rollback step.
3. Release notes in `CHANGELOG.md` with a SemVer version number.

### 13.6 Software Bill of Materials (Full Mode)
For Full Mode client work, produce an SBOM (a list of all packages and versions the project uses, in a standard format such as CycloneDX or SPDX) at each release, and keep it with the release. Many larger clients ask for it.
