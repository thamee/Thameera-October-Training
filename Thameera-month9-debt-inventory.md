# Month 9 – Technical Debt Inventory

**Author:** Thameera Tharmakulasingam
**Role:** Senior Software Engineer, Bistec Global Services
**System in scope:** Nevis Island Company Registry platform (company registration, renewals, name reservations, filings, payment approvals and certificates)
**Date:** 8 November 2026

---

## 1. Inventory Structure

### 1.1 Debt item identification system

Each item uses the ID format **`TD-<CAT>-<NNN>`**, where `<CAT>` is a three-letter category code and `<NNN>` is a sequential number within that category (e.g. `TD-ARC-001`). IDs are never reused, even if an item is resolved.

### 1.2 Categorization scheme

| Code | Category | Covers |
|------|----------|--------|
| COD | Code | Outdated runtimes/frameworks, duplicated logic, unused or risky dependencies, poor code quality/best-practice violations, security weaknesses (vulnerabilities, input validation, HTML sanitization) |
| ARC | Architecture | Service boundaries, coupling, API contracts |
| INF | Infra | CI/CD, environments, configuration, monitoring, resource efficiency (green/sustainability) |
| DOC | Doc | Architecture docs, API docs, runbooks |
| TST | Test | Unit, integration and regression/E2E coverage |

### 1.3 Severity / priority classification

Severity is judged on business impact, risk and cost of delay.

| Severity | Definition | Target action |
|----------|------------|---------------|
| **Critical** | Causes active user-facing impact, security/support risk, or blocks delivery | Address in current upgrade cycle |
| **High** | Significantly slows delivery or raises defect risk | Schedule within next 1–2 quarters |
| **Medium** | Causes friction or minor risk; manageable short-term | Address opportunistically or alongside related work |
| **Low** | Minor inconvenience | Backlog |

### 1.4 Age buckets

< 1 year · 1 – < 3 years · 3 – < 5 years · 5+ years (age measured from when the debt was introduced or became debt, as of Nov 2026).

### 1.5 Effort, change risk and priority

Severity (section 1.3) measures the impact of leaving the debt in place. It is kept separate from the cost and risk of fixing it, so that a big or risky fix never inflates an item's severity.

| Field | Values | Meaning |
|-------|--------|---------|
| Effort | S / M / L / XL | S = days, M = 1-3 weeks, L = 1-2 months, XL = multi-month programme |
| Change Risk | Low / Med / High | Chance of regression, based on how many systems are touched and how much testing is needed |
| Priority | P1 - P4 | Planning order, derived from severity, effort and change risk using the rules below |

**Priority rules**

- **P1 (do now / current cycle):** every Critical item, plus High items with effort S-M. Critical items with XL effort are planned as a programme, not skipped.
- **P2 (schedule next, bundle with related work):** High items with effort L-XL, and any Medium item that blocks a P1 or P2 item (e.g. stale tests that block the migration).
- **P3 (opportunistic):** other Medium items. A Medium item with High change risk (large impact, heavy testing) stays Medium and is **bundled into the migration** so the testing is shared, not fixed on its own.
- **P4 (backlog):** Low items.

---

## 2. Debt Register

| ID | Category | Description | Origin | Systems Affected | Age | Owner | Severity | Effort | Change Risk | Priority |
|----|----------|-------------|--------|------------------|-----|-------|----------|--------|-------------|----------|
| TD-ARC-001 | Arch | Backend split into ~7 fine-grained microservices with chatty inter-service calls, causing high latency on common filing flows | Premature decomposition at initial build; boundaries drawn by feature rather than by data/transaction ownership | All backend services | ~5 yrs | Tech Lead / Architect | **Critical** | XL | High | **P1** |
| TD-COD-001 | Code | Backend on legacy .NET runtimes (.NET Framework / .NET Core 3.1), out of vendor support and blocking modern libraries | Original stack choice never upgraded as support windows ended | All backend services | ~4 yrs (since EOL) | Backend Lead | **Critical** | XL | High | **P1** |
| TD-COD-002 | Code | Frontend on Angular 6/8, several major versions behind; outdated tooling, limited library compatibility | Delivery pressure; no scheduled framework-upgrade cadence | Angular frontend | ~5 yrs | Frontend Lead | High | XL | High | **P2** |
| TD-COD-003 | Code | Registration/renewal validation and fee-calculation logic duplicated across multiple services | Copy-paste reuse to meet deadlines when services were split | Backend services (filing modules) | ~3 yrs | Backend Lead | High | M | Med | **P1** |
| TD-COD-004 | Code | Large number of unused/redundant NuGet and npm packages and outdated versions | Libraries added per feature and never pruned | Backend services, frontend | ~3 yrs | Backend + Frontend Leads | Medium | S | Low | **P3** |
| TD-COD-005 | Code | AutoMapper v15+ dual-licensing creates licensing/compliance risk for the upgrade path | Vendor licensing change; dependency adopted as default mapper | Backend services | < 1 yr | Backend Lead | Medium | S | Low | **P3** |
| TD-COD-006 | Code | Deprecated or end-of-life libraries still in use (backend packages and npm modules); no security fixes, and many lack .NET 10 / Angular 22 support so they must be replaced, not just upgraded | Libraries adopted early and never reviewed as vendors deprecated or abandoned them | Backend services, frontend | ~3 yrs | Backend + Frontend Leads | High | M | Med | **P1** |
| TD-COD-007 | Code | Poor code quality and best-practice violations: oversized classes/methods, magic values, inconsistent naming and error handling, weak separation of concerns; no enforced coding standards, linters or static analysis, making code hard to read, debug and change safely | Tight deadlines on features and change requests; no review or quality gates; no shared coding standards | Backend services, frontend | ~4 yrs | Tech Lead / Dev Team | High | M | Low | **P1** |
| TD-COD-008 | Code | Security debt: weak input validation and missing or inconsistent HTML sanitization (XSS risk) on user-facing filing and payment forms, plus known vulnerabilities in outdated runtimes and dependencies left unpatched; Cycode scan findings (vulnerable dependencies, hardcoded secrets, code-level security issues) are flagged but not systematically triaged or remediated | Security not treated as a requirement; out-of-support runtimes and libraries stopped receiving patches; Cycode findings are not tied to a remediation process, owners or SLAs, so the backlog grows | Backend services, Angular frontend (user-facing forms) | ~4 yrs | Tech Lead / DevOps Lead | **Critical** | M | Med | **P1** |
| TD-ARC-002 | Arch | No API versioning; frontend tightly coupled to backend contracts, so changes ripple across both | Contracts evolved ad hoc with a single client | Frontend ↔ backend APIs | ~3 yrs | Architect | Medium | L | High | **P3** |
| TD-TST-001 | Test | Low automated test coverage on core filing workflows; regressions caught late by manual testing | Tests deprioritised during fast delivery of features and change requests | Backend services, frontend | ~5 yrs | QA Lead / Dev Team | High | L | Low | **P2** |
| TD-TST-002 | Test | No automated end-to-end regression suite for payment approval and certificate issuance | Cross-service flows hard to test; no test environment strategy | End-to-end platform flows | ~3 yrs | QA Lead | High | L | Med | **P2** |
| TD-TST-003 | Test | Existing unit/integration tests are outdated, disabled/skipped, brittle or flaky; they no longer reflect current behavior and will break during the .NET 10 / Angular 22 migration | Tests not updated alongside code changes; tests skipped to get builds through and never restored | Backend services, frontend | ~3 yrs | Dev Team / QA Lead | Medium | M | Low | **P2** |
| TD-INF-001 | Infra | Inconsistent or manual build/deploy processes across services; no uniform CI/CD pipeline | Each service set up independently over time | All services, release process | ~3 yrs | DevOps Lead | High | M | Med | **P1** |
| TD-INF-002 | Infra | Environment configuration and secrets handled inconsistently between services/environments | Config copied per service; no central standard | All services | ~3 yrs | DevOps Lead | Medium | M | Med | **P3** |
| TD-INF-003 | Infra | Limited centralized logging, monitoring and distributed tracing, making root-causing cross-service issues slow | Observability not planned when services were split | Backend services | ~2 yrs | DevOps Lead | Medium | M | Low | **P3** |
| TD-INF-004 | Infra | "Green" / sustainability debt: wasteful resource use, including always-on over-provisioned environments for ~7 services, no autoscaling or right-sizing, redundant inter-service calls burning compute, and no tracking of resource/energy cost | Capacity sized for peak and never revisited; chatty service design; efficiency never a requirement | All services, hosting environments | ~3 yrs | DevOps Lead | Medium | M | Low | **P3** |
| TD-DOC-001 | Doc | No current architecture or service-dependency documentation; knowledge held by individuals | Docs never maintained alongside code; team turnover | Platform-wide | ~5 yrs | Architect / Tech Lead | Medium | M | Low | **P3** |
| TD-DOC-002 | Doc | API documentation (Swagger/OpenAPI) outdated and incomplete; no runbook for bug triage and change-request handling | API changes made without doc updates | Backend APIs, support process | ~2 yrs | Backend Lead | Low | S | Low | **P4** |

**Total items: 19 across 5 categories** (requirement: ≥ 10 items, ≥ 3 categories).

> Note: owners are listed by role; replace with named individuals as appropriate for your team.

---

## 3. Categorization Analysis

### 3.1 Distribution by category

| Category | Count | % of total |
|----------|-------|-----------|
| Code | 8 | 42.1% |
| Infra | 4 | 21.1% |
| Test | 3 | 15.8% |
| Arch | 2 | 10.5% |
| Doc | 2 | 10.5% |
| **Total** | **19** | **100%** |

*(Percentages are rounded and may not sum exactly to 100%.)*

### 3.2 Distribution by severity

| Severity | Count | % of total |
|----------|-------|-----------|
| Critical | 3 | 15.8% |
| High | 7 | 36.8% |
| Medium | 8 | 42.1% |
| Low | 1 | 5.3% |
| **Total** | **19** | **100%** |

### 3.3 Category × severity matrix

| Category | Critical | High | Medium | Low | Total |
|----------|:-------:|:----:|:------:|:---:|:-----:|
| Code | 2 | 4 | 2 | 0 | 8 |
| Arch | 1 | 0 | 1 | 0 | 2 |
| Infra | 0 | 1 | 3 | 0 | 4 |
| Test | 0 | 2 | 1 | 0 | 3 |
| Doc | 0 | 0 | 1 | 1 | 2 |
| **Total** | **3** | **7** | **8** | **1** | **19** |

### 3.4 Distribution by age

| Age bucket | Count | % of total | Items |
|------------|-------|-----------|-------|
| < 1 year | 1 | 5.3% | COD-005 |
| 1 – < 3 years | 2 | 10.5% | INF-003, DOC-002 |
| 3 – < 5 years | 12 | 63.2% | COD-001, COD-003, COD-004, COD-006, COD-007, COD-008, ARC-002, TST-002, TST-003, INF-001, INF-002, INF-004 |
| 5+ years | 4 | 21.1% | ARC-001, COD-002, TST-001, DOC-001 |
| **Total** | **19** | **100%** | |

**Observation:** 84% of the debt (16 of 19 items) is 3+ years old. (Percentages are rounded.) Debt is mostly legacy that has compounded, not recent shortcuts. All three Critical items are 4+ years old.

### 3.5 Distribution by priority, effort and change risk

| Priority | Count | Items |
|----------|-------|-------|
| P1 | 7 | ARC-001, COD-001, COD-008, COD-003, COD-006, COD-007, INF-001 |
| P2 | 4 | COD-002, TST-001, TST-002, TST-003 |
| P3 | 7 | COD-004, COD-005, ARC-002, INF-002, INF-003, INF-004, DOC-001 |
| P4 | 1 | DOC-002 |
| **Total** | **19** | |

| Effort | Count | | Change risk | Count |
|--------|-------|-|-------------|-------|
| S | 3 | | Low | 9 |
| M | 10 | | Med | 6 |
| L | 3 | | High | 4 |
| XL | 3 | | | |

**Observations:**
- **Severity and priority differ for risky items.** ARC-002 is Medium severity but High change risk and L effort, so it is held at P3 and bundled with the migration instead of being fixed alone.
- **TST-003 is Medium but P2** because stale tests block the safe delivery of the migration work.
- **Quick wins:** COD-004, COD-005 and DOC-002 are small, low-risk items that can be cleared alongside the larger work.
- **The three XL/High-risk items** (ARC-001, COD-001, COD-002) are the programme-level work; the test, security and pipeline items support them.

---

## 4. Hot Spots

### 4.1 Top 5 highest-impact items

| Rank | ID | Item | Why it ranks here |
|------|----|------|-------------------|
| 1 | TD-ARC-001 | Over-fragmented microservices | Directly causes user-visible latency; amplifies every other backend issue (duplication, deployment, tracing) |
| 2 | TD-COD-001 | Out-of-support backend runtimes | Security/support exposure; blocks modern libraries and tooling |
| 3 | TD-COD-008 | Security weaknesses (input validation, HTML sanitization, unpatched vulnerabilities) | Direct exposure on user-facing filing and payment forms in a fintech-style system; a breach affects clients, data and reputation |
| 4 | TD-COD-002 | Angular 6/8 frontend | Large version gap makes the upgrade effort grow every release; limits hiring and library choice |
| 5 | TD-TST-001 | Low test coverage on filing workflows | Makes every change (including the upgrades above) risky and slow to verify |

*Next in line: TD-INF-001 (inconsistent CI/CD), which slows every release across 7 services.*

### 4.2 Systems with most debt concentration

Items can touch more than one system, so counts overlap.

| System | Items affecting it | Count |
|--------|--------------------|-------|
| Backend services (~7 microservices) | ARC-001, ARC-002, COD-001, COD-003, COD-004, COD-005, COD-006, COD-007, COD-008, TST-001, TST-003, INF-001, INF-002, INF-003, INF-004, DOC-002 | 16 |
| Angular frontend | ARC-002, COD-002, COD-004, COD-006, COD-007, COD-008, TST-001, TST-003 | 8 |
| Build/deploy & environments | INF-001, INF-002, INF-004 | 3 |
| Platform-wide / end-to-end | DOC-001, TST-002 | 2 |

**The backend services carry 16 of 19 items (≈84%)**, including all three Critical items. They are the clear concentration point and the priority for remediation.

### 4.3 Patterns observed

1. **Architecture debt multiplies other debt.** The fragmented service layout (ARC-001) drives duplicated logic (COD-003), repeated pipelines/config (INF-001, INF-002) and tracing difficulty (INF-003). Fixing boundaries, for example by merging related services, reduces several items at once.
2. **Delivery pressure left tests and docs behind.** Test and Doc debt (5 items, including tests left stale or disabled) all trace to features and change requests being prioritised over supporting work, a recurring pattern in sustained bug-fix/CR-driven delivery.
3. **Nothing was scheduled for upgrades.** Runtime and framework debt (COD-001, COD-002, COD-004, COD-006) comes from having no recurring upgrade cadence, so version gaps grew until a large migration was unavoidable.
4. **Knowledge concentrated in individuals.** Missing docs (DOC-001, DOC-002) plus weak observability (INF-003) make root-causing slow and dependent on a few people.
5. **Dependency choices carry hidden cost.** The AutoMapper licensing change (COD-005) and the unused-package sprawl (COD-004) and the deprecated libraries (COD-006) show dependencies need periodic review, not just addition.
6. **No quality gates.** Poor code quality (COD-007) sits alongside duplicated logic (COD-003) and weak tests (TST-001, TST-003). Without enforced standards, code review checks and static analysis, shortcuts accumulate and make every later change and bug fix slower.
7. **Security is a by-product of other debt.** Weak sanitization and unpatched vulnerabilities (COD-008) are worsened by out-of-support runtimes (COD-001), deprecated libraries (COD-006), missing quality gates (COD-007) and no enforced remediation of Cycode findings in CI/CD (INF-001). Upgrading alone will not fix input validation and HTML sanitization; those need deliberate fixes and testing.

### 4.4 Suggested sequencing (brief)

1. Rewrite/upgrade effort (COD-001, COD-002) combined with service consolidation (ARC-001) and library pruning/replacement (COD-004, COD-005, COD-006), refactoring poor-quality code in the modules being rewritten and introducing coding standards, linters and review gates (COD-007).
2. Triage and repair outdated/disabled tests (TST-003) before migrating, then add automated tests for core filing and payment flows (TST-001, TST-002) as migration safety nets.
3. Standardise CI/CD, config and observability (INF-001 to INF-003), and right-size environments with autoscaling (INF-004). Consolidating services under step 1 also reduces compute waste.
4. Treat security as a parallel, early workstream: patch or replace vulnerable dependencies, add input validation and HTML sanitization on user-facing forms, and triage and clear the open Cycode findings (vulnerable dependencies, hardcoded secrets, code issues), then make Cycode scans a gating check in the pipeline with owners and remediation deadlines (COD-008).
5. Document the new architecture and APIs as the rewrite lands (DOC-001, DOC-002).