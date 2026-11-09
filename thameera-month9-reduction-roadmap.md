# Month 9 – Debt Reduction Roadmap

**Author:** Thameera Tharmakulasingam
**Role:** Senior Software Engineer, Bistec Global Services
**System in scope:** Nevis Island Company Registry platform
**Date:** 8 November 2026
**Based on:** Month 9 Technical Debt Inventory (19 items) and Debt Cost Model (top 5 items).

> **Planning assumptions:** staffing is **3 engineers total: one full-time lead and two shared engineers at 50% each** (2.0 FTE, about 260 productive hours/month, 780 h/quarter, 3,120 h/year). At this capacity, the 6,690-hour scope takes about **26 months**. The Q1–Q4 work sequence and quarterly effort profile below were built for a 10-engineer pool and are not achievable as currently dated; re-phase the milestones and quarterly budget before approval. The **$50/hour** rate and total effort-based estimate remain unchanged. The plan is aligned with the .NET 10 / Angular 22 upgrade that is already in progress.

---

## 1. Prioritization Framework

### 1.1 Criteria and weights

| # | Criterion | Weight | What it measures |
|---|-----------|-------:|------------------|
| S | **Severity / risk** | 25% | Impact of leaving the debt in place (from the inventory severity rating) |
| C | **Cost of carrying** | 20% | Monthly drag, blocked features and incident exposure (from the cost model; estimated for items outside the top 5) |
| B | **Blocking / enabler value** | 20% | Whether the item blocks other work or makes other fixes cheaper and safer |
| E | **Ease (inverse of effort)** | 15% | Smaller effort scores higher, which favours quick wins |
| R | **Safety (inverse of change risk)** | 10% | Lower regression risk and less testing needed scores higher |
| A | **Age / growth** | 10% | Older debt that compounds faster scores higher |

Severity, cost and blocking value total 65% because the aim is to reduce risk and cost first. Ease and safety total 25% to pull in cheap, low-risk wins. Age is a smaller adjustment.

### 1.2 Scoring methodology

Each item is scored **1–5** on every criterion using the scale below. The weighted score is the sum of (score × weight), for a maximum of 5.0.

| Score | S: Severity | C: Cost of carrying | B: Blocking | E: Ease | R: Safety | A: Age |
|------:|-------------|---------------------|-------------|---------|-----------|--------|
| 5 | Critical | ≥ $8k/month | Blocks the migration or most other items | S effort (days) | Low risk, little testing | 5+ years |
| 4 | High | $5k–8k/month | Blocks one or two items | M effort, small | Low–medium | 4 years |
| 3 | Medium | $3k–5k/month | Makes other fixes easier | M effort | Medium | 3 years |
| 2 | – | $1k–3k/month | Minor dependency | L effort | Medium–high | 1–3 years |
| 1 | Low | < $1k/month | Independent | XL effort | High risk, heavy testing | < 1 year |

**Tiers:** Tier 1 = score ≥ 3.5 · Tier 2 = 3.0–3.49 · Tier 3 = below 3.0.
**Tie-breaks:** higher blocking score first, then higher severity, then older age, then the item that unblocks the most other work.

### 1.3 Scoring results

| Rank | ID | S | C | B | E | R | A | **Score** | Tier | Start → done |
|-----:|----|--:|--:|--:|--:|--:|--:|----------:|:----:|:-------------|
| 1 | ARC-001 | 5 | 5 | 5 | 1 | 1 | 5 | **4.00** | 1 | Q1 → Q4 |
| 2 | TST-001 | 4 | 4 | 4 | 2 | 4 | 5 | **3.80** | 1 | Q1 → Q3 |
| 3 | COD-001 | 5 | 4 | 5 | 1 | 1 | 4 | **3.70** | 1 | Q1 → Q4 |
| 4 | COD-008 | 5 | 3 | 3 | 3 | 3 | 4 | **3.60** | 1 | Q1 → Q2 |
| 5 | INF-001 | 4 | 3 | 4 | 3 | 3 | 3 | 3.45 | 2 | Q1 → Q2 |
| 6 | COD-006 | 4 | 3 | 4 | 3 | 3 | 3 | 3.45 | 2 | Q1 → Q3 |
| 7 | COD-007 | 4 | 3 | 3 | 3 | 4 | 4 | 3.45 | 2 | Q1 → Q3 |
| 8 | COD-002 | 4 | 4 | 4 | 1 | 1 | 5 | 3.35 | 2 | Q1 → Q4 |
| 9 | TST-003 | 3 | 2 | 5 | 3 | 4 | 3 | 3.30 | 2 | Q1 → Q1 |
| 10 | COD-003 | 4 | 3 | 3 | 3 | 3 | 3 | 3.25 | 2 | Q2 → Q3 |
| 11 | TST-002 | 4 | 3 | 3 | 2 | 3 | 3 | 3.10 | 2 | Q2 → Q4 |
| 12 | COD-004 | 3 | 2 | 2 | 5 | 5 | 3 | 3.10 | 2 | Q1 → Q1 |
| 13 | DOC-001 | 3 | 2 | 2 | 3 | 5 | 5 | 3.00 | 2 | Q4 → Q4 |
| 14 | COD-005 | 3 | 1 | 2 | 5 | 5 | 1 | 2.70 | 3 | Q1 → Q1 |
| 15 | INF-002 | 3 | 2 | 2 | 3 | 3 | 3 | 2.60 | 3 | Q2 → Q2 |
| 16 | INF-003 | 3 | 2 | 2 | 3 | 4 | 2 | 2.60 | 3 | Q3 → Q3 |
| 17 | INF-004 | 3 | 2 | 1 | 3 | 4 | 3 | 2.50 | 3 | Q4 → Q4 |
| 18 | ARC-002 | 3 | 2 | 2 | 2 | 1 | 3 | 2.25 | 3 | Q2 → Q3 |
| 19 | DOC-002 | 1 | 1 | 1 | 5 | 5 | 2 | 2.10 | 3 | Q1 → Q1 |

**Tier counts:** Tier 1 = 4 items, Tier 2 = 9 items, Tier 3 = 6 items.

### 1.4 Trade-off decisions

The score sets the ranking, but the roadmap order also follows dependencies, risk and quick wins. These are the deliberate departures from pure score order.

| # | Decision | Reason |
|---|----------|--------|
| 1 | **Repair stale tests (TST-003, rank 9) first** | Ranked 9th, but it blocks safe migration. Every later change needs a working test baseline. |
| 2 | **Security (COD-008, rank 4) starts in Q1, not at its rank position behind the big programmes** | Security impact is non-linear: one incident outweighs months of drag. It is also cheap to fix (400 h) and has the shortest payback. |
| 3 | **ARC-001 (rank 1) is designed in Q1 and executed in stages from Q2** | Highest score, but XL effort and High change risk. A big-bang rewrite is too risky, so services are consolidated in groups with tests in place first. |
| 4 | **Consolidate services before migrating them (ARC-001 before most of COD-001)** | Migrating seven services to .NET 10 and then merging them would mean doing the work twice. One service is migrated early as a pilot to prove the path. |
| 5 | **Do not refactor code that is about to be retired** | COD-007 clean-up is applied only to code being rewritten or kept; code in services being merged away is left alone. |
| 6 | **Quick wins go early despite low rank (COD-004, COD-005, DOC-002)** | Small, low-risk, and they cut noise before the migration (e.g. removing unused packages reduces what has to be upgraded). |
| 7 | **Defer docs, observability and right-sizing until after consolidation (DOC-001, INF-003, INF-004)** | Document, instrument and size the new architecture once, not twice. |
| 8 | **COD-002 (Angular) ranks 8th but finishes last** | It depends on deprecated-library replacement (COD-006), test coverage (TST-001) and API versioning (ARC-002), so its main effort lands in Q3–Q4. |
| 9 | **Accept a low-priority residual** | DOC-002 (outdated API docs) is a Low item and is handled as a small quick win; no further investment is made beyond it. |

---

## 2. 12-Month Roadmap

### 2.1 Overview

● = work in the quarter · ✔ = completed in the quarter

| ID | Item | Q1 (Dec–Feb) | Q2 (Mar–May) | Q3 (Jun–Aug) | Q4 (Sep–Nov) |
|----|------|:------------:|:------------:|:------------:|:------------:|
| TST-003 | Repair stale/disabled tests | ✔ | | | |
| COD-008 | Security, sanitization, Cycode backlog | ● | ✔ | | |
| INF-001 | Standard CI/CD pipelines | ● | ✔ | | |
| COD-004 | Remove unused packages | ✔ | | | |
| COD-005 | AutoMapper licensing decision | ✔ | | | |
| DOC-002 | API docs and runbook refresh | ✔ | | | |
| COD-007 | Coding standards, linters, review gates | ● | ● | ✔ | |
| TST-001 | Automated tests, core filing flows | ● | ● | ✔ | |
| COD-006 | Replace deprecated libraries | ● (assess) | ● | ✔ | |
| ARC-001 | Service consolidation | ● (design) | ● | ● | ✔ |
| COD-001 | Backend to .NET 10 | ● (pilot) | ● | ● | ✔ |
| COD-002 | Angular 8 → 22 | ● (spike) | ● | ● | ✔ |
| COD-003 | Remove duplicated logic | | ● | ✔ | |
| INF-002 | Config and secrets standard | | ✔ | | |
| ARC-002 | API versioning | | ● | ✔ | |
| TST-002 | End-to-end regression suite | | ● | ● | ✔ |
| INF-003 | Logging, monitoring, tracing | | | ✔ | |
| INF-004 | Right-size environments (green) | | | | ✔ |
| DOC-001 | Architecture and dependency docs | | | | ✔ |

All 19 inventory items are scheduled.

### 2.2 Quarterly objectives and targeted items

#### Q1 (Dec–Feb): Stabilise, secure and build the safety net

**Objective:** make change safe and remove the highest-risk exposure before the big structural work begins.

| Item | Work in the quarter |
|------|---------------------|
| TST-003 | Triage all tests; fix or delete stale ones, re-enable or remove skipped ones. **Done.** |
| COD-008 | Clear all Critical Cycode findings; add input validation and HTML sanitization on the highest-risk forms (filing and payment). |
| INF-001 | Standard build/test/deploy pipeline template; first services on it. |
| TST-001 | Start automated tests for registration, renewal and payment-approval paths. |
| COD-004, COD-005, DOC-002 | Prune unused packages; decide AutoMapper licensing path; refresh API docs and bug-triage runbook. **All done.** |
| COD-007 | Agree coding standards; switch on linters, static analysis and review checklist for new code. |
| COD-006 | Assess every library, list deprecated ones and choose replacements. |
| ARC-001 | Design: service boundaries, consolidation plan and order. |
| COD-001 | Pilot: migrate one service to .NET 10 to prove the path and the pipeline. |
| COD-002 | Spike: Angular upgrade approach and risks. |

**Exit criteria:** no stale or skipped tests without a ticket; zero Critical Cycode findings; consolidation design approved; pilot service running on .NET 10 in a test environment.

#### Q2 (Mar–May): Consolidate and migrate the backend

**Objective:** start the structural change on top of the tested, secured, standardised base.

| Item | Work in the quarter |
|------|---------------------|
| ARC-001 | First wave of service consolidation. |
| COD-001 | Migrate the first consolidated services to .NET 10. |
| COD-006 | Replace deprecated backend and frontend libraries (first batch). |
| COD-003 | Remove duplicated validation and fee logic as services merge (first half). |
| COD-008 | Clear all High findings; finish sanitization and validation; make Cycode a gating check in the pipeline. **Done.** |
| INF-001 | All services on standard pipelines. **Done.** |
| INF-002 | One standard for configuration and secrets. **Done.** |
| TST-001 | Extend automated tests across core flows. |
| TST-002 | Design the end-to-end regression suite. |
| ARC-002 | Design API versioning. |
| COD-002 | Start Angular upgrade work. |
| COD-007 | Enforce standards on all new and changed code. |

**Exit criteria:** first consolidated services in production on .NET 10; zero High Cycode findings and Cycode gate active; all services on standard pipelines.

#### Q3 (Jun–Aug): Finish the backend, upgrade the frontend, automate end-to-end tests

**Objective:** complete backend consolidation and library replacement, and move the frontend and API contracts onto stable ground.

| Item | Work in the quarter |
|------|---------------------|
| ARC-001, COD-001 | Continue consolidation and migration; backend code-complete by the end of the quarter. |
| COD-003 | Finish removing duplicated logic. **Done.** |
| COD-006 | Finish replacing deprecated libraries. **Done.** |
| TST-001 | Core flow coverage target reached. **Done.** |
| TST-002 | Build the end-to-end suite for payment approval and certificate issuance. |
| ARC-002 | Introduce API versioning to decouple frontend and backend. **Done.** |
| INF-003 | Centralised logging, monitoring and distributed tracing for the new service layout. **Done.** |
| COD-002 | Main Angular upgrade work. |
| COD-007 | Quality gates enforced in the pipeline. **Done.** |

**Exit criteria:** backend code-complete on .NET 10 with no deprecated libraries; coverage target on core flows met; API versioning live; tracing working across all services.

#### Q4 (Sep–Nov): Cut over, harden and document

**Objective:** finish the modernisation, retire the old stack and capture the new architecture.

| Item | Work in the quarter |
|------|---------------------|
| ARC-001, COD-001 | Final consolidation, production cutover and stabilisation; old runtime retired. **Done.** |
| COD-002 | Angular 22 cutover and stabilisation. **Done.** |
| TST-002 | Finish and run the end-to-end suite as the release gate. **Done.** |
| INF-004 | Right-size environments and add autoscaling for the consolidated platform. **Done.** |
| DOC-001 | Architecture and service-dependency documentation for the new platform. **Done.** |
| Review | 12-month retrospective, re-score the remaining and new debt, plan year two. |

**Exit criteria:** production on .NET 10 and Angular 22; old stack decommissioned; all Critical inventory items closed; end-to-end suite gating releases.

### 2.3 Dependencies and sequencing

| Dependency | Why |
|------------|-----|
| TST-003 → COD-001, COD-002 | Migrations need a working test baseline to detect regressions |
| TST-001 → ARC-001 consolidation | Core-flow tests are the safety net when services are merged |
| INF-001 → COD-001 pilot and all deployments | Migrated services need a standard, repeatable pipeline |
| COD-006 assessment → COD-001, COD-002 | Deprecated libraries must be replaced before they block the upgrades |
| ARC-001 design → COD-003, COD-001 | Merge first, then dedupe logic and migrate once |
| COD-008 (Cycode gate) → all later releases | Stops new security debt entering during the migration |
| ARC-001 → INF-003, INF-004, TST-002, DOC-001 | Instrument, size, test end-to-end and document the final architecture once |
| ARC-002 + COD-006 + TST-001 → COD-002 cutover | The Angular upgrade needs stable contracts, supported libraries and coverage |

**Critical path:** TST-003 → INF-001 → ARC-001 / COD-001 (staged) → COD-002 cutover (Q4). Anything that slips on this path moves the end date, so it gets first call on capacity.

### 2.4 Resource requirements

#### Hours by item and quarter

| Item | Q1 | Q2 | Q3 | Q4 | Total |
|------|---:|---:|---:|---:|------:|
| **Lane 1: Modernisation programme** | | | | | |
| ARC-001 | 200 | 350 | 350 | 300 | 1,200 |
| COD-001 | 150 | 300 | 300 | 250 | 1,000 |
| COD-002 | 70 | 150 | 230 | 450 | 900 |
| COD-006 | 80 | 100 | 70 | 0 | 250 |
| COD-003 | 0 | 100 | 100 | 0 | 200 |
| *Lane 1 subtotal* | *500* | *1,000* | *1,050* | *1,000* | *3,550* |
| **Lane 2: Ongoing debt allocation** | | | | | |
| TST-001 | 120 | 250 | 230 | 0 | 600 |
| COD-008 | 250 | 150 | 0 | 0 | 400 |
| TST-002 | 0 | 50 | 200 | 100 | 350 |
| ARC-002 | 0 | 100 | 200 | 0 | 300 |
| TST-003 | 250 | 0 | 0 | 0 | 250 |
| INF-001 | 150 | 100 | 0 | 0 | 250 |
| COD-007 | 80 | 80 | 80 | 0 | 240 |
| INF-003 | 0 | 0 | 200 | 0 | 200 |
| INF-002 | 0 | 150 | 0 | 0 | 150 |
| INF-004 | 0 | 0 | 0 | 120 | 120 |
| DOC-001 | 0 | 0 | 0 | 120 | 120 |
| COD-004 | 60 | 0 | 0 | 0 | 60 |
| DOC-002 | 60 | 0 | 0 | 0 | 60 |
| COD-005 | 40 | 0 | 0 | 0 | 40 |
| *Lane 2 subtotal* | *1,010* | *880* | *910* | *340* | *3,140* |
| **Total** | **1,510** | **1,880** | **1,960** | **1,340** | **6,690** |

#### Staffing and cost

| | Q1 | Q2 | Q3 | Q4 | Year |
|---|---:|---:|---:|---:|-----:|
| Hours | 1,510 | 1,880 | 1,960 | 1,340 | 6,690 |
| Engineer equivalents (390 h per FTE per quarter) | 3.9 | 4.8 | 5.0 | 3.4 | n/a |
| Cost at $50/hour | $75,500 | $94,000 | $98,000 | $67,000 | **$334,500** |

Lane 1 costs $177,500 and Lane 2 costs $157,000. The five top items alone cost $205,000 (as in the cost model); the rest of the inventory adds about $129,500.

#### Roles needed

| Role | Main items |
|------|-----------|
| Backend engineers | ARC-001, COD-001, COD-003, COD-006, COD-008, ARC-002 |
| Frontend engineers | COD-002, COD-006 (npm), COD-008 (form sanitization) |
| QA / test engineer | TST-001, TST-002, TST-003 |
| DevOps engineer | INF-001 to INF-004, Cycode gating in COD-008 |
| Tech lead / architect | ARC-001 design, ARC-002, DOC-001, standards and reviews (COD-007) |

**Tooling:** no major new spend is assumed. The plan uses the existing Cycode scanning, the CI/CD platform and standard linters/static analysers.

---

## 3. Capacity Allocation

### 3.1 Capacity feasibility

| | Q1 | Q2 | Q3 | Q4 | Year 1 |
|---|---:|---:|---:|---:|-------:|
| Capacity (2.0 FTE) | 780 h | 780 h | 780 h | 780 h | 3,120 h |
| Planned debt work | 1,510 h | 1,880 h | 1,960 h | 1,340 h | 6,690 h |
| **Capacity shortfall** | **730 h** | **1,100 h** | **1,180 h** | **560 h** | **3,570 h** |

- The current quarterly work estimates exceed available capacity in every quarter. Re-phase Lane 1 and Lane 2 over an estimated 26 months before committing to milestones or feature dates.
- The prior feature-capacity percentages and year-two allocation target assumed a 10-engineer pool; set replacement targets during the re-baseline.

### 3.2 How debt work integrates with feature work

1. **One backlog, one planning process.** Debt items are written as backlog stories with the same estimates and acceptance criteria as features. Each sprint reserves its share of capacity before feature work is planned.
2. **Touch it, improve it.** When a feature or change request touches a module, a small allowance (about 10–15% of that story) covers clean-up, tests and removal of deprecated usage in that area. This keeps debt reduction connected to real work.
3. **Bundle debt with related features.** If a feature is planned in an area that has a debt item (for example, filing forms and sanitization), the two are scheduled together to share testing and regression effort.
4. **Definition of Done includes quality gates.** Tests written, linters and static analysis clean, Cycode scan clean, no new deprecated libraries. This stops new debt entering while old debt is paid down.
5. **Migration work doubles as debt work.** Where a feature must be built on a service that is being migrated, build it on the new stack instead of the old one.
6. **Visible to stakeholders.** Debt hours and their effect on features are reported in the same sprint and quarterly reviews as feature delivery.

### 3.3 Flexibility provisions

| Provision | Detail |
|-----------|--------|
| **Capacity buffer** | No delivery buffer is included until the work is re-phased against the 2.0-FTE allocation. |
| **Borrowing rule** | Set feature-work borrowing limits after re-baselining; the current quarter plan has no spare capacity. |
| **Protected work** | Critical-severity items (ARC-001, COD-001, COD-008) are prioritised during re-phasing. |
| **Re-prioritisation** | Items are re-scored every quarter. New debt can enter the roadmap; items can drop out, for example when the code they sit in is retired. |
| **Phase-shift option** | Re-sequence non-critical items (INF-004, DOC-001, INF-003) to fit available capacity while protecting the critical path. |

---

## 4. Success Metrics

> Baselines marked *TBD* are measured in Month 1 of the roadmap, before targets are locked.

### 4.1 Leading indicators

Early signals that the plan is on track. Reviewed every sprint.

| Indicator | Baseline | Target |
|-----------|----------|--------|
| Debt hours delivered vs plan | n/a | ≥ 90% of planned hours each quarter |
| Open Critical Cycode findings | TBD | 0 by end Q1 |
| Open High Cycode findings | TBD | 0 by end Q2 |
| Services on the standard CI/CD pipeline | TBD | 100% by end Q2 |
| Skipped or stale tests without a ticket | TBD | 0 by end Q1 |
| Test coverage on core filing and payment flows | TBD | ≥ 70% by end Q3 |
| Deprecated libraries remaining | Count from COD-006 assessment | 0 by end Q3 |
| Backend services migrated to .NET 10 | 0 | Pilot in Q1; ~40% by end Q2; code-complete by end Q3; cutover in Q4 |
| Changed code passing linter/static-analysis gates | TBD | 100% from end Q3 |
| Debt items closed vs new debt items logged | n/a | Closed ≥ logged every quarter |

### 4.2 Lagging indicators

Outcomes that show the debt reduction is paying off. Reviewed monthly and quarterly.

| Indicator | Baseline | Target |
|-----------|----------|--------|
| **Monthly cost of carrying the top 5 items** (cost model) | $30,208 | ≈ $26,500 end Q2 · ≈ $21,200 end Q3 · ≈ $400 end Q4 |
| Open Critical inventory items | 3 | 0 by end Q4 |
| Open High inventory items | 7 | 0 by end Q4 (re-scored each quarter; new High items may enter) |
| Change failure rate | TBD | −30% by end Q4 |
| Lead time for a change request | TBD | −25% by end Q4 |
| Escaped production defects per release | TBD | −40% by end Q4 |
| Time to find the root cause of a production issue | TBD | −30% by end Q4 |
| p95 latency on core filing flows | TBD | −30% after consolidation (Q4) |
| Hosting/compute cost per environment | TBD | −15% after right-sizing (Q4) |
| Security incidents from known weaknesses | TBD | 0 |
| Feature throughput per quarter | TBD | Back to or above baseline by Q4 despite the investment |

#### Indicative savings as items complete

| End of | Top-5 items completed (cumulative) | Monthly cost removed |
|--------|------------------------------------|---------------------:|
| Q1 | none (foundations only) | $0 |
| Q2 | COD-008 | $3,688 |
| Q3 | + TST-001 | $8,988 |
| Q4 | + ARC-001, COD-001, COD-002 | $29,788 |

The savings and payback schedule shown here assumes the original Q1–Q4 delivery profile. It must be recalculated after the work is re-phased to the 2.0-FTE capacity; the existing month-18 estimate is no longer applicable.

### 4.3 Reporting cadence

| Cadence | Audience | Content |
|---------|----------|---------|
| **Every sprint (2 weeks)** | Dev team, product owner | Leading indicators, debt hours vs plan, blockers on the critical path |
| **Monthly** | Delivery manager, project owner | Debt dashboard: both indicator sets, cost-of-carrying trend, risks |
| **Quarterly** | Stakeholders, leadership | Roadmap review: exit criteria met, cost savings and ROI refreshed against the cost model, re-scored inventory, capacity adjustments |
| **Annually (month 12)** | Leadership | Retrospective, year-two plan and steady-state allocation |

**Owners:** the tech lead owns the roadmap and the quarterly review; each item has an owner from the inventory; DevOps owns pipeline and security metrics; QA owns test metrics.

---

## 5. Risks to the Roadmap

| Risk | Impact | Mitigation |
|------|--------|-----------|
| Feature pressure squeezes debt capacity | Slips the critical path | Re-baselined feature commitments and protected critical-path work |
| Consolidation or migration surprises | Overruns on ARC-001, COD-001 | Pilot before staged cutover, reforecast at quarterly reviews, tests in place first |
| Hidden library blockers | Delays COD-001 / COD-002 | COD-006 assessment in Q1, before the main migration |
| Key-person dependency | Knowledge held by few people | Pair on migration work, document as the rewrite lands |
| Estimates are optimistic | Cost and timeline overruns | Quarterly re-estimate, ±50% fix-cost sensitivity in the cost model |
| New debt keeps being created | Debt reduction is cancelled out | Quality gates, Definition of Done, closed ≥ logged rule |
