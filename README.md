# Huntd QA Portfolio

## About

This portfolio showcases end-to-end testing artifacts created while testing Huntd — a web3-oriented recruitment platform with shared GraphQL backend services across web and mobile applications.

My approach extends beyond identifying defects to investigating reproduction conditions, isolating failure triggers, and forming evidence-based root cause hypotheses using browser DevTools, GraphQL analysis, ADB logcat, and database error investigation.

---

## Repository Structure

```
├── README.md
├── test-plan.md
├── permission-testing-table.md
├── web-app/
│   ├── defect-reports/             
│   ├── evidence-screenshots/
│   ├── jira-test-runs-summary.md
│   └── requirements-traceability-matrix.md
└── mobile-app/
    ├── defect-reports/             
    ├── evidence-screenshots/
    ├── jira-test-runs-summary.md
    └── requirements-traceability-matrix.md
```

---

## Key Findings

Selected findings that required investigation beyond the surface:

- A `varchar(255)` database constraint with no client-side guard caused silent data loss across multiple flows. On creation, the failure triggered PostgreSQL error `25P02` — aborting the entire transaction block and silently blocking all subsequent saves until page reload, without any user feedback.
- Profile ID regeneration on edit cycles broke recruiter-candidate relationship continuity — duplicate chats created, contact history inflated, previously contacted candidates reappearing as new. Confirmed by tracing profile IDs across edit cycles (`candidateProfileId: 13173 → 13176 → 13179`).
- Cancelled OAuth authorization attempts persisted ghost tokens in the database. All three providers shared the same `providerId` from the initiating account — confirmed via Apollo cache inspection.
- A `Tooltip_toolTipTop__G_POy` component using `opacity: 0` instead of `display: none` remained in the DOM with `z-index: 12`, intercepting clicks on modal dialogs (`z-index: auto`) across five separate pages.
- Candidates List page crashed for Recruiter role during pagination due to unguarded `workPlaces[n].title` access on a profile with `workPlaces: null`. Isolated via DevTools console query; Candidate role unaffected up to offset 400.
- Deleted chat records persisted in the database. Cross-party messaging to deleted chats succeeded, triggering `SequelizeUniqueConstraintError` (`code: 23505`) on subsequent send attempts — captured via GraphQL mutation analysis for both Recruiter and Candidate roles.

---

## Cross-Module Defect Patterns

Testing revealed recurring bug classes that crossed module boundaries:

- **`varchar(255)` silent failure** — same root cause across Experience page (create and edit flows) and Recruiter Company Details, each with a different failure mode: silent discard, form reset, or full mutation abort
- **React state not reset after user interaction** — disabled OAuth buttons not re-enabled, deleted chat UI remaining visible, ghost tokens persisting, required field validation not re-triggered after clear-all — unrelated components, same class of bug
- **Shared component reuse propagating bugs** — `Tooltip` component, `Technologies` multi-select, `Job Experience` dropdown, and salary fields all exhibited identical defects across every page they appeared on

---

## Bug Showcase

A selection of findings that required investigation beyond the surface:

| Report | Finding | Investigation |
|---|---|---|
| [HUNTD-62](./web-app/defect-reports/HUNTD-62-list-crashes-on-workplace-null.md) | Candidates List crashes for Recruiter role during pagination | Production crash isolated by role — Candidate unaffected on identical data. Offending profiles identified via DevTools console query at crash offset |
| [HUNTD-58](./web-app/defect-reports/HUNTD-58-duplicate-chats-and-contact-his.md) | Profile reactivation creates duplicate chats and inflates contact history | Architectural finding — profile ID regeneration traced across edit cycles (`candidateProfileId: 13173 → 13176 → 13179`) breaking all existing recruiter-candidate relationships |
| [HUNTD-73](./web-app/defect-reports/HUNTD-73-cancelled-oauth-creates-connections.md) | Cancelled OAuth attempts persist ghost tokens for all three providers | All providers sharing the same `providerId` from the initiating account — confirmed via Apollo cache inspection |
| [HUNTD-65](./web-app/defect-reports/HUNTD-65-chat-records-persist-after-deletion.md) | Deleted chats persist in DB and trigger `SequelizeUniqueConstraintError` on subsequent messages | Full GraphQL mutation analysis for both Recruiter and Candidate roles — `code: 23505` captured in both directions |
| [HUNTD-81](./web-app/defect-reports/HUNTD-81-form-creation-resets-on-255-char-limit.md) | `varchar(255)` violation triggers PostgreSQL `25P02` transaction abort | Not only does the creation fail silently — the aborted transaction blocks all subsequent saves in the same session until page reload |

---

## Skills Demonstrated

- **Test design** — equivalence partitioning, boundary value analysis, decision table testing, state transition testing, use case testing
- **Grey-box investigation** — browser DevTools (Network, Console, Elements), GraphQL mutation and response analysis, Apollo cache inspection, ADB logcat
- **Mobile testing** — physical device testing on Android, ADB via PowerShell, scrcpy screen mirroring, React Native error identification
- **Defect reporting** — root cause investigation before filing, systemic pattern recognition across modules
- **Test management** — TestRail (test cases, runs, reports), Jira (bug tracking, epics, stories)
- **Documentation** — Test Plan, RTM, Permission Testing Table, Test Summary Report (ISTQB Foundation Level)
