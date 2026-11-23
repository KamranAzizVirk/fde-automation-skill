# Module: Web App and Mobile App

Part of the FDE Automation Skill. The rules in SKILL.md and references/common-standards.md always apply on top of this file.

Covers: web applications (login, dashboards, data entry, client portals) and mobile apps (Android, iOS, or both).
Roles: Web Frontend Expert (web apps), Mobile Expert (mobile apps), Security Expert, QA Expert, Cloud Automation Expert. Add a Language Expert for each approved language (SKILL.md, Section 7.5).
Express Mode fits a small internal tool with few screens and simple data. Use Full Mode for many users, payments, personal data, or store release.
 If the app handles money, read references/finance.md.

## Extra Intake Questions
1. Type (clickable): web app, Android app, iOS app, web plus mobile.
2. Who uses it, and how many people? (staff, customers, public)
3. Does it need login? What kinds of users (roles)?
4. Main things a user must be able to do (owner types, Claude turns them into a short list).
5. Does it work offline? Does it send notifications?
6. Is there an existing system or data to connect or move in?
7. Should a cross-platform approach (one codebase for Android and iOS) be considered? Claude explains pros and cons in Phase 5.

## Workflow (Added to the Shared Phases)

### A. Roles and Permissions (Phase 2)
Write a table: each user role, what it can see, what it can change. Default is least access. The owner approves it at G2.

### B. Screens and Flow (Phase 7)
1. Design the main screens and navigation in the Artifact Panel. Show 2 to 3 options for the home screen. The owner approves.
2. Plan empty states, loading states, and error messages in simple language.
3. Plan Urdu and RTL layout from the start if Urdu is needed.

### C. Login and Security (Phase 5)
1. Approve the login method (email and password, one-time code, or a sign-in provider). Compare options with pros and cons.
2. Passwords are hashed. Sessions expire. Login attempts are limited. Roles are checked on the server for every protected action.
3. Apply OWASP Top 10, plus the mobile risks the Security Expert lists for the chosen platform.

### D. Data Model (Phase 9)
1. Explain the tables and data flow in simple Urdu before building.
2. Schema changes only through migrations. Never edit a live database by hand.
3. Backup plan and RPO and RTO approved here (common-standards.md, Section 2.5).

### E. API Design (Phases 6 and 10)
1. Use versioned routes (for example `/api/v1/`).
2. Validate all input on the server. Return clear error messages that do not reveal internal details.
3. Rate limit sensitive endpoints.

### F. Build (Phase 10)
1. Only after the Coding Gate (SKILL.md, Section 2.4).
2. One feature at a time, each in its own branch, each tested before approval.
3. Keep the first release small: only the "must have" features from Phase 3.

### G. Mobile Additions (Only for Mobile Apps)
1. Platforms: Android, iOS, or both. The owner confirms minimum OS versions.
2. Offline behaviour is written in the requirements: what works without internet and how data syncs later.
3. Ask for a device permission only when it is needed, and show the reason to the user.
4. Push notifications only with the user's permission.
5. **Store readiness:** privacy labels, permission explanations, screenshots, and store policy checks. Store rules and fees change. Claude says "میں یقین سے نہیں کہہ سکتا" and asks the owner to confirm them on the store's own pages. Allow extra time for store review.
6. Test on devices and OS versions the owner picks, including a low-end phone and a slow connection.

### H. Testing (Phase 13)
1. Unit, integration, and end-to-end tests for the main flows (common-standards.md, Section 4).
2. Test each user role: can it do what it should, and can it not do what it should not?
3. Test wrong input, double clicks, lost connection, and expired login.
4. Accessibility checks: labels, contrast, keyboard or screen reader use.

### I. Deployment and Handover
1. Use Docker or the platform's equivalent, environment variables, and the secret manager. Separate staging and production for Full Mode.
2. Give the owner a rollback plan.
3. Fresh owner confirmation before every release or store submission.
4. Handover: README, runbook, user guide in simple language, and the support terms (common-standards.md, Sections 8 and 9).

### J. Standards Baseline
1. **Security baseline:** use OWASP ASVS (Application Security Verification Standard) as the checklist for web apps and OWASP MASVS (the mobile equivalent) for mobile apps. Pick the level with the owner in Phase 4. Say "میں یقین سے نہیں کہہ سکتا" about the current version and check the OWASP site.
2. **API contract:** describe the API in OpenAPI before building the screens that use it. Keep it in the repository.
3. **Accessibility:** web apps target WCAG 2.2 level AA. Mobile apps follow the platform's own accessibility guidance (screen reader labels, text size, touch target size).
4. **Error and crash reporting:** add a crash and error reporting tool so problems are seen before users complain. Remove personal data from reports. Analytics only with consent where the region requires it.
5. **Performance budget:** agree targets in Phase 4, for example screen load time on a mid-range phone and app size. Measure before optimising.
6. **Environments:** local, staging, and production, each with its own keys and database. Never test with production data.
7. **App signing keys (mobile):** keep signing keys and store credentials in a secure place, never in the repository. Back them up. Losing them can block updates. Record who holds them.
8. **Account deletion and data export:** let users delete their account and request their data. App stores can require in-app account deletion. Claude says "میں یقین سے نہیں کہہ سکتا" and the owner confirms the current store rules.
9. **Release steps (mobile):** internal test, then closed beta, then store release. Allow time for store review. Keep a rollback or hotfix plan.
10. **Offline and sync conflicts:** decide who wins when the same data is changed offline on two devices, and test it.
11. If the app includes an AI chatbot or agent, read references/single-agent.md and follow common-standards.md, Sections 3 and 12 from there.

## Gates Checklist for This Module
- G2: roles, permissions, and main user actions approved.
- G5: login method, hosting, and stack approved.
- G7: screens approved.
- G9: data model approved, then the Coding Gate.
- G13: role tests and main flow tests passed.
- Release: fresh owner confirmation.
