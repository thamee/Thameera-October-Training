# Executive Investment Proposal: Modernising the Nevis Registry Platform

**Prepared by:** Thameera Tharmakulasingam, Senior Software Engineer, Bistec Global Services
**Prepared for:** Executive and delivery leadership
**Date:** 8 November 2026
**Decision requested by:** end of November 2026, for a December 2026 start
**Supporting documents:** Technical Debt Inventory, Debt Cost Model, Debt Reduction Roadmap (all Month 9)

---

## 1. Executive Summary

### The ask

**Approve the $334,500 modernisation programme for the Nevis registry platform (6,690 engineering hours), starting December 2026, staffed by one lead engineer and two 50%-shared engineers.** This provides about 2.0 engineer-equivalents (260 hours/month), so delivery is estimated at about 26 months; the original 12-month milestones and related payback timing must be re-baselined.

### The headline

> **The five biggest problems in the platform cost about $30,000 every month and the cost is rising. Fixing them costs $205,000 once and pays for itself about 7 months after delivery. Waiting a year costs about $417,000 and makes the fix about $40,000 more expensive.**

### Why this matters to the business

The platform handles company registrations, renewals, name reservations, filings, payment approvals and certificates for agents, accountants and the registrar. Over its life it has accumulated shortcuts and outdated technology (technical debt). The result today is:

- **Slower delivery:** every change takes longer and breaks more often than it should.
- **Slower service:** the platform is split into about seven tightly linked parts that call each other constantly, which slows common filing steps.
- **Security exposure:** user-facing forms lack consistent input protection, known weaknesses are flagged but not systematically cleared, and the core technology no longer receives vendor security updates.
- **Fragile releases:** testing relies heavily on manual checks, so defects reach clients.

### What we get

| Business outcome | Target at programme completion (dates to be re-baselined) |
|------------------|--------------------|
| Monthly cost of the top 5 problems | From **$30,208 to about $400** |
| Production defects per release | **−40%** |
| Time to deliver a change request | **−25%** |
| Failed changes | **−30%** |
| Speed of core filing steps (p95 response time) | **−30%** |
| Hosting cost per environment | **−15%** |
| Critical risk items open | **3 → 0** |
| Platform on supported technology | Yes (.NET 10, Angular 22) |

### Decisions needed

1. Approve the **$334,500** budget and **6,690 hours** of programme work.
2. Protect the capacity of **one full-time lead engineer plus two engineers shared at 50% each** (about 2.0 FTE total).
3. Re-baseline the 12-month milestone sequence to an estimated **26-month delivery window** before committing to dates; the current quarter-by-quarter plan exceeds available capacity.
4. Name an **executive sponsor** and agree to a **quarterly review**.

*All figures are estimates built on stated assumptions (appendix B). They are designed to be replaced with measured data in the first month of the programme.*

---

## 2. Business Case

### 2.1 Current state and pain points

| Pain point | What the business experiences |
|------------|------------------------------|
| **Platform built on out-of-support technology** | No vendor security fixes; newer tools and libraries cannot be used; hiring and onboarding are harder |
| **Over-fragmented back end (about 7 linked services)** | Slow responses on filing steps; a simple change touches several parts; more places to fail |
| **Aging user interface (two or more generations behind)** | The gap grows with every release; upgrades get more expensive; limits what clients can be offered |
| **Security weaknesses** | Weak protection on user-facing filing and payment forms; security scan findings are raised but not systematically closed |
| **Low automated testing** | Defects found late or by clients; every release carries more risk and manual effort |
| **Inconsistent deployment and monitoring** | Slow releases; slow diagnosis when something goes wrong |
| **Outdated and duplicated code and libraries** | Extra effort on every change; some components are abandoned by their vendors |
| **Knowledge held by individuals** | Little current documentation; delivery depends on a few people |

**The scale of the problem (Technical Debt Inventory):** 19 specific problems across five areas (code, architecture, infrastructure, testing, documentation). Three are rated Critical and seven High. **Sixteen of the 19 (84%) have existed for 3 or more years**, which is why the cost has compounded.

### 2.2 Cost of inaction (quantified)

The five highest-impact problems alone:

| Cost | Per month | Per year |
|------|----------:|---------:|
| Extra effort to work around the problems | $12,500 | $150,000 |
| Features delayed or blocked | $13,500 | $162,000 |
| Expected losses from incidents | $4,208 | $50,500 |
| **Total** | **$30,208** | **$362,500** |

**And the cost is growing.** If nothing is done:

| Time from today | Monthly cost | Total cost so far |
|-----------------|-------------:|------------------:|
| Now | $30,208 | $0 |
| 6 months | $34,305 | $195,329 |
| 12 months | $38,974 | **$417,205** |
| 24 months | $50,374 | $956,012 |
| 36 months | $65,234 | $1,653,165 |

**The fix itself gets more expensive with delay:** $205,000 today, about $224,000 in 6 months and about $245,000 in 12 months.

**The incident exposure is real.** The largest single item is a security incident on the user-facing forms, estimated at **$150,000** impact with about a **15%** chance in any year. An incident tied to the out-of-support platform is estimated at **$60,000** and a **25%** chance. These come from the cost model assumptions, not from known incidents.

### 2.3 Proposed investment and returns

#### Investment

| Scope | Hours | Cost |
|-------|------:|-----:|
| **Top 5 problems** (highest return; fully costed) | 4,100 | **$205,000** |
| Remaining 14 problems | 2,590 | $129,500 |
| **Full programme** | **6,690** | **$334,500** |

#### Return on the top 5 (the part we can fully quantify)

| Problem fixed | Investment | Monthly benefit | Payback after delivery |
|---------------|-----------:|----------------:|-----------------------:|
| Security weaknesses | $20,000 | $3,688 | **5.4 months** |
| Low test coverage | $30,000 | $5,300 | **5.7 months** |
| Fragmented back end | $60,000 | $9,450 | **6.3 months** |
| Aging user interface | $45,000 | $5,725 | **7.9 months** |
| Out-of-support platform | $50,000 | $5,625 | **8.9 months** |
| **Top 5 together** | **$205,000** | **$29,788** | **6.9 months** |

- **24-month return after delivery: 249%** (every $1 invested returns about $3.50 in avoided cost over 24 months).
- **Payback from programme start:** the existing month-18 estimate assumes the old 12-month delivery schedule and is not valid for the reduced staffing plan. Recalculate after the delivery milestones and benefits timing are re-baselined.
- **Not counted:** the other 14 problems (about $129,500 of the investment) have benefits that were not quantified: faster releases, easier onboarding, better documentation, lower hosting cost. The return figures are therefore conservative.

#### How robust is it? (sensitivity)

| If… | Payback | 24-month return |
|-----|--------:|----------------:|
| Base case | 6.9 months | 249% |
| Fix costs 50% more | 10.3 months | 132% |
| Only 60% of the benefits are achieved | 11.5 months | 109% |
| Incident risk is half what we assume | 7.3 months | 227% |
| **Worst case:** costs +50%, drag −30%, delayed features −50%, risk −50% | 17.7 months | **+36%** |

**The investment stays positive in every case tested.** Even with the incident risk set to zero, payback is about 8 months, so the case does not depend on a security incident happening.

### 2.4 Risk if not addressed

| Risk | Consequence |
|------|-------------|
| **Security incident** | Client data exposure, loss of trust with agents and the registrar, remediation and possible contractual or regulatory penalties (estimated $150,000 per incident) |
| **Unsupported platform** | No vendor security patches; rising exposure and audit findings; harder to hire and support |
| **Delivery slowdown** | Features arrive later and cost more; demand from clients cannot be met on time |
| **Cost spiral** | The monthly cost rises about 29% in a year, and the fix gets more expensive each month |
| **Reliability** | Releases that break filing or payment flows, with direct client impact |
| **Key-person risk** | Knowledge is concentrated in a few people; losing one slows or stops delivery |
| **Larger forced upgrade later** | Support deadlines and the growing version gap force the same work under time pressure and at higher cost |

**The choice is not whether to pay, but whether to pay $205,000 on our schedule or a growing amount later, with the risk in the meantime.**

---

## 3. Proposal Details

### 3.1 The specific ask

| | |
|---|---|
| **Budget** | **$334,500** for 6,690 engineering hours; quarterly spend profile must be re-baselined |
| **Time** | Estimated **26 months** from December 2026, subject to milestone re-baselining |
| **People** | **3 engineers:** 1 full-time lead and 2 shared engineers at 50% each (2.0 FTE total; about 260 productive hours/month) |
| **Capacity** | About **3,120 hours/year** available against **6,690 planned hours**; the current 12-month plan exceeds capacity by about **3,570 hours** |
| **Tooling** | No major new spend assumed; uses existing security scanning, build pipelines and standard code-quality tools |
| **Governance** | Executive sponsor, quarterly review, protected minimum capacity |

**The trade-off, stated plainly:** the current quarterly work plan requires more capacity than this staffing model provides. Re-phase the milestones and feature commitments before approval; the previous quarter-specific feature-capacity percentages no longer apply.

### 3.2 Timeline and milestones

The quarter labels below preserve the original work sequence; they are not achievable dates under the revised 2.0-FTE staffing plan. Re-phase the milestones across the estimated 26-month delivery window before using them as commitments.

| Quarter | Focus | Milestone at quarter end |
|---------|-------|--------------------------|
| **Q1** (Dec–Feb) | Stabilise and secure; build the safety net | No Critical security findings open; test baseline repaired; first services on standard pipelines; consolidation design approved; first service proven on the new technology |
| **Q2** (Mar–May) | Consolidate and migrate the back end | First consolidated services live on the new technology; no High security findings; security checks block unsafe releases; all services on standard pipelines |
| **Q3** (Jun–Aug) | Finish the back end; upgrade the user interface; automate end-to-end tests | Back end complete on supported technology; deprecated libraries gone; 70% test coverage on core filing flows; stable interfaces between front and back end |
| **Q4** (Sep–Nov) | Cut over, harden, document | Live on .NET 10 and Angular 22; old platform retired; automated end-to-end tests gate every release; documentation complete |

**Decision gates:** the executive sponsor reviews at the end of each quarter against the milestone and the measures in Appendix A. If a gate is missed, the plan is re-phased before the next quarter begins.

### 3.3 Quick wins within 90 days

Results the business sees in the first quarter (December–February):

| Quick win | Business benefit |
|-----------|------------------|
| **All Critical security scan findings cleared** | Immediate reduction in the largest single risk |
| **Protection added to the highest-risk filing and payment forms** | Closes the most exposed entry points for user-facing attacks |
| **Test suite repaired** (stale and disabled tests fixed or removed) | A reliable safety net before any larger change |
| **Unused and redundant software components removed** | Less to upgrade, smaller attack surface |
| **Licensing decision made on a key component** | Removes a compliance uncertainty |
| **First services on a standard, repeatable release pipeline** | Faster, more reliable releases |
| **Documentation and support runbook refreshed** | Faster issue handling |
| **First service proven on the new technology** | Confirms the approach and de-risks the larger migration |
| **Consolidation design approved** | A clear, reviewed plan for the biggest item |

These cost about **$75,500** in Q1 and prove the approach before the larger spend begins in Q2.

---

## Appendix A: Supporting Data

### A.1 Debt inventory summary (19 items)

| Category | Items | | Severity | Items | | Age | Items |
|----------|------:|-|----------|------:|-|-----|------:|
| Code | 8 | | Critical | 3 | | Under 1 year | 1 |
| Infrastructure | 4 | | High | 7 | | 1 to under 3 years | 2 |
| Testing | 3 | | Medium | 8 | | 3 to under 5 years | 12 |
| Architecture | 2 | | Low | 1 | | 5 years or more | 4 |
| Documentation | 2 | | | | | | |

The back-end services carry **16 of the 19** items, including all three Critical ones.

### A.2 Top 5 cost detail

| Item | Fix cost | Extra effort / month | Delayed features / month | Incident exposure / month | **Total / month** |
|------|---------:|---------------------:|-------------------------:|--------------------------:|------------------:|
| Fragmented back end (ARC-001) | $60,000 | $4,000 | $5,000 | $500 | **$9,500** |
| Out-of-support platform (COD-001) | $50,000 | $2,000 | $2,500 | $1,250 | **$5,750** |
| Security weaknesses (COD-008) | $20,000 | $1,000 | $1,000 | $1,875 | **$3,875** |
| Aging user interface (COD-002) | $45,000 | $2,500 | $3,000 | $250 | **$5,750** |
| Low test coverage (TST-001) | $30,000 | $3,000 | $2,000 | $333 | **$5,333** |
| **Total** | **$205,000** | **$12,500** | **$13,500** | **$4,208** | **$30,208** |

### A.3 Expected savings as milestones complete

| End of | Top-5 items completed (cumulative) | Monthly cost removed |
|--------|------------------------------------|---------------------:|
| Q1 | Foundations only | $0 |
| Q2 | Security weaknesses | $3,688 |
| Q3 | + Test coverage | $8,988 |
| Q4 | + Back end, platform, user interface | $29,788 |

### A.4 Capacity by quarter

| | Q1 | Q2 | Q3 | Q4 | Year 1 |
|---|---:|---:|---:|---:|-------:|
| Available capacity (2.0 FTE) | 780 h | 780 h | 780 h | 780 h | 3,120 h |
| Planned programme work | 1,510 h | 1,880 h | 1,960 h | 1,340 h | 6,690 h |
| Capacity shortfall | 730 h | 1,100 h | 1,180 h | 560 h | **3,570 h** |

The original Q1–Q4 allocation is not feasible with the revised staffing plan. Re-phase these work packages across an estimated 26 months; feature capacity and payback dates must be recalculated as part of that re-baseline.

### A.5 Success measures and reporting

| Measure | Target |
|---------|--------|
| Monthly cost of the top 5 problems | $26,500 end Q2 · $21,200 end Q3 · about $400 end Q4 |
| Critical security findings open | 0 by end Q1 |
| High security findings open | 0 by end Q2 |
| Test coverage on core filing and payment flows | At least 70% by end Q3 |
| Deprecated components remaining | 0 by end Q3 |
| Production defects per release | −40% by end Q4 |
| Change request lead time | −25% by end Q4 |
| Failed changes | −30% by end Q4 |
| Core filing step response time | −30% by end Q4 |
| Hosting cost per environment | −15% by end Q4 |
| Delivery throughput | Back to or above baseline by Q4 |

**Reporting:** every sprint (progress, blockers) · monthly (dashboard to project owner) · quarterly (executive review, savings and return refreshed) · annually (retrospective and year-two plan). Baselines not yet measured are captured in the first month.

---

## Appendix B: Technical Details for Follow-up

### B.1 Assumptions behind the numbers

| Assumption | Value |
|------------|-------|
| Engineering rate | $50 per hour (blended, loaded) |
| Value of an average feature | $10,000 |
| Engineering pool | 1 full-time lead + 2 shared engineers at 50% each; 2.0 FTE total, about 260 productive hours/month |
| Fix removes ongoing effort and delayed features | 100% |
| Fix reduces incident risk | 90% |
| Cost growth if no action | Ongoing effort and delays +2% per month; incident risk +3% per month; fix cost +1.5% per month |
| Delivery time before benefits (cost model) | 6 months |

These are estimates. The cost model lists where the real data comes from (time tracking, ticket history, incident log, security scan results, vendor support dates).

### B.2 Technical items

| ID | Technical item | Severity | Priority |
|----|----------------|----------|----------|
| ARC-001 | Consolidate about 7 fine-grained services with chatty calls | Critical | P1 |
| COD-001 | Move back-end from legacy .NET runtimes (.NET Framework / .NET Core 3.1) to .NET 10 | Critical | P1 |
| COD-008 | Input validation, HTML sanitization (XSS), unpatched vulnerabilities, security-scan (Cycode) backlog | Critical | P1 |
| COD-002 | Upgrade Angular 6/8 to Angular 22 | High | P2 |
| TST-001 | Automated tests for registration, renewal and payment-approval flows | High | P2 |
| COD-003, COD-006, COD-007, INF-001 | Duplicated logic, deprecated libraries, code quality, CI/CD | High | P1 |
| TST-002 | End-to-end regression suite for payment approval and certificate issuance | High | P2 |
| TST-003 | Repair outdated, disabled and flaky tests | Medium | P2 |
| COD-004, COD-005, ARC-002, INF-002, INF-003, INF-004, DOC-001 | Unused packages, AutoMapper licensing, API versioning, config and secrets, observability, right-sizing (green), architecture docs | Medium | P3 |
| DOC-002 | API docs and runbook | Low | P4 |

### B.3 Sequencing logic

1. **Tests and pipelines first** (TST-003, INF-001, COD-008): a safe, secure base to change from.
2. **Consolidate before migrating** (ARC-001 before most of COD-001): avoids upgrading services that will be merged.
3. **One pilot early** (COD-001): proves the path at low risk.
4. **Libraries and interfaces before the front-end upgrade** (COD-006, ARC-002, TST-001 before COD-002).
5. **Document, instrument and right-size the final platform once** (INF-003, INF-004, DOC-001, TST-002).

**Critical path:** repair tests → standard pipelines → staged consolidation and migration → front-end cutover.

### B.3.1 Full documents

The inventory, cost model and roadmap hold the full detail: item register with origins and owners, per-item cost calculations, scoring of all 19 items, hours by item and quarter, and dependencies.

---

## Appendix C: Anticipated Questions

**Why now?**
Three things converge: the core platform is already out of vendor support, the upgrade to current technology is already planned, and the cost of waiting grows every month (about 29% higher in a year, and the fix itself about $40,000 dearer). Doing it now lets us combine the required upgrade with the fixes that make it safe.

**Can we just keep delivering features and fix things gradually?**
We tested this. At the current drag, the five biggest problems cost about $30,000 a month and rise over time. Gradual fixing alongside full feature delivery does not catch up, because the problems slow every feature and the fix cost grows. A protected allocation is the only way the work actually gets finished.

**Does the current schedule fit the proposed team?**
No. One full-time lead and two 50%-shared engineers provide about 3,120 hours per year, while the current plan calls for 6,690 hours. That is about 26 months of delivery at the proposed allocation. The work sequence, quarterly budget profile, feature commitments and payback timing must be re-baselined before dates are committed.

**What will features look like during the programme?**
The prior quarter-specific feature-capacity estimates assumed a 10-engineer pool and no longer apply. Feature commitments should be set after the milestone plan is re-phased against the 2.0-FTE allocation.

**Why not rebuild everything from scratch?**
A full rewrite carries high risk and would pause delivery. The plan instead consolidates and migrates in stages behind automated tests, one pilot first, so the platform keeps running throughout.

**How confident are the numbers?**
They are estimates on stated assumptions, not audited figures. We tested the sensitivity: the investment stays positive even if costs overrun by 50% and benefits come in at half of what we expect (worst case still +36% over 24 months). The first month replaces the key assumptions with measured data, and the quarterly review updates the return.

**What if it overruns?**
The old Q4 buffer assumed a 10-engineer pool and is not available under this staffing model. Use quarterly gates to re-prioritise and reforecast against actual capacity; protect critical security and platform-upgrade work first.

**Is the security risk real or theoretical?**
The weaknesses are real: security scans already flag them. The dollar estimate of an incident is an assumption, but even with the incident risk set to zero the investment pays back in about 8 months, so the case does not rely on it.

**Can we do only the top 5 items?**
Yes. The top 5 cost $205,000 and carry the highest return. The other 14 items add about $129,500, are mostly lower-cost, and make the top 5 safer, faster and cheaper to deliver (for example, repairing the test suite before migrating). We recommend the full programme, but the top 5 are the minimum.

**Why use shared resources?**
The plan assumes one lead engineer plus two shared engineers at 50% allocation. This provides continuity and specialist support while limiting dedicated staffing; the corresponding delivery estimate is about 26 months for the full 6,690-hour scope.

**How will we know it is working?**
Quarterly milestones, savings and the measures in Appendix A. The first visible proof is within 90 days: Critical security findings cleared, a repaired test base and the first service on the new technology.

**What happens after the programme?**
Set a sustainable ongoing debt allocation when the re-baselined schedule is approved. The inventory is re-scored quarterly, and the quality checks added in the programme stop new debt from piling up again.
