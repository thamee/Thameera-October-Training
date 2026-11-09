# Month 9 – Debt Cost Model

**Author:** Thameera Tharmakulasingam
**Role:** Senior Software Engineer, Bistec Global Services
**System in scope:** Nevis Island Company Registry platform
**Date:** 8 November 2026
**Based on:** Month 9 Technical Debt Inventory (19 items). This model costs the **top 5 items** from section 4.1 of that document: TD-ARC-001, TD-COD-001, TD-COD-008, TD-COD-002 and TD-TST-001.

> **Note on figures:** all dollar and hour figures are estimates built from the assumptions in section 1.3. They show the method and the relative scale of each item. Replace the inputs with real data (time tracking, ticket history, incident log) before presenting them as final. The model recalculates from the formulas in section 1.1.

---

## 1. Cost Framework

### 1.1 Cost components

| Component | Definition | Formula |
|-----------|------------|---------|
| **Fix cost** | One-time cost to remediate the debt | Hours to remediate × hourly rate |
| **Carry cost** | Recurring extra effort paid every month because the debt exists (workarounds, manual work, slower changes, extra debugging) | Extra hours per month × hourly rate |
| **Opportunity cost** | Value lost because features are delayed or blocked by the debt | Features delayed per month × value per feature |
| **Risk cost** | Expected loss from incidents the debt makes more likely | Annual incident probability × impact ÷ 12 (per month) |

**Monthly cost of carrying a debt item** = carry cost + opportunity cost + monthly risk cost.
**Benefit of fixing it** = the part of that monthly cost that disappears after remediation (see 1.3).

### 1.2 Measurement methodology

1. **Fix cost:** estimate remediation hours per item from the same breakdown used for the upgrade work (per-service tasks), with a contingency included in the hours.
2. **Carry cost:** estimate extra hours per month by comparing how long typical work takes with the debt against a clean baseline (e.g. a cross-service change, a manual regression cycle, a security review).
3. **Opportunity cost:** count features or change requests that were delayed or could not be started because of the debt, and multiply by the value per feature.
4. **Risk cost:** estimate how likely an incident is in a year (probability) and what it would cost (impact: recovery effort, client impact, penalties, reputation), then multiply.
5. **Trend:** apply monthly growth rates to show how the cost changes if nothing is done (section 3).
6. **ROI:** compare the fix cost against the monthly benefit to get payback, 24-month ROI and sensitivity (section 4).

### 1.3 Assumptions

| Assumption | Value | Basis |
|------------|-------|-------|
| Blended loaded engineering rate | **$50 / hour** | Estimate for a mixed senior/mid team with overheads |
| Value of one average feature | **$10,000** | Assumed ≈ 100 dev hours ($5,000) at 2× business value |
| Remediation removes carry and opportunity cost | **100%** | Once fully fixed, the extra work and blocked features go away |
| Remediation reduces incident risk | **90%** | Some residual risk always remains |
| Delivery time before benefits start | **6 months** | Used in the trend break-even calculation |
| Growth if nothing is done | Carry + opportunity **2% / month**; risk **3% / month**; fix cost **1.5% / month** | Version gaps widen, more code is built on the debt, vulnerabilities accumulate |
| Currency | USD | |

### 1.4 Data sources

| Input | Source to use or confirm |
|-------|--------------------------|
| Remediation hours | Upgrade task breakdown and sprint-planning estimates (7 backend services, Angular frontend) |
| Extra hours per month | Time tracking, bug and change-request tickets, deployment and regression-test records |
| Features delayed | Backlog and delivery history (items postponed or blocked by the debt) |
| Incident probability and impact | Incident and bug history, Cycode findings (vulnerable dependencies, secrets, code issues), vendor end-of-support dates |
| Hourly rate | Finance or project cost-rate card |

---

## 2. Cost Calculations (Top 5 Debt Items)

### 2.1 Per-item calculations

**TD-ARC-001: Over-fragmented microservices**

| Cost type | Calculation | Result |
|-----------|-------------|--------|
| Fix cost | 1,200 h (consolidate ~7 services into fewer, rebuild shared flows) × $50 | **$60,000** |
| Carry cost | 80 h/month (cross-service changes, latency debugging, multi-service releases) × $50 | **$4,000 / month** |
| Opportunity cost | 0.5 features/month delayed × $10,000 | **$5,000 / month** |
| Risk cost | 30% annual chance of a major latency/outage incident × $20,000 impact = $6,000/year | **$500 / month** |
| **Monthly cost of carrying** | $4,000 + $5,000 + $500 | **$9,500 ($114,000 / year)** |

**TD-COD-001: Out-of-support backend runtimes**

| Cost type | Calculation | Result |
|-----------|-------------|--------|
| Fix cost | 1,000 h (~140 h per service across 7 services) × $50 | **$50,000** |
| Carry cost | 40 h/month (workarounds, old tooling, no modern libraries) × $50 | **$2,000 / month** |
| Opportunity cost | 0.25 features/month blocked × $10,000 | **$2,500 / month** |
| Risk cost | 25% annual chance of an unsupported-component incident × $60,000 = $15,000/year | **$1,250 / month** |
| **Monthly cost of carrying** | $2,000 + $2,500 + $1,250 | **$5,750 ($69,000 / year)** |

**TD-COD-008: Security weaknesses (validation, HTML sanitization, unpatched vulnerabilities, Cycode backlog)**

| Cost type | Calculation | Result |
|-----------|-------------|--------|
| Fix cost | 400 h (sanitization and validation on user-facing forms, clear Cycode backlog, add scan gates) × $50 | **$20,000** |
| Carry cost | 20 h/month (manual triage, rework of findings, security reviews) × $50 | **$1,000 / month** |
| Opportunity cost | 0.1 features/month delayed (client security reviews and approvals) × $10,000 | **$1,000 / month** |
| Risk cost | 15% annual chance of a security incident × $150,000 impact = $22,500/year | **$1,875 / month** |
| **Monthly cost of carrying** | $1,000 + $1,000 + $1,875 | **$3,875 ($46,500 / year)** |

**TD-COD-002: Angular 6/8 frontend**

| Cost type | Calculation | Result |
|-----------|-------------|--------|
| Fix cost | 900 h (Angular 8 → 22 upgrade, UI regression and fixes) × $50 | **$45,000** |
| Carry cost | 50 h/month (workarounds, slow builds, old-library constraints) × $50 | **$2,500 / month** |
| Opportunity cost | 0.3 features/month blocked × $10,000 | **$3,000 / month** |
| Risk cost | 10% annual chance of a frontend incident × $30,000 = $3,000/year | **$250 / month** |
| **Monthly cost of carrying** | $2,500 + $3,000 + $250 | **$5,750 ($69,000 / year)** |

**TD-TST-001: Low test coverage on core filing workflows**

| Cost type | Calculation | Result |
|-----------|-------------|--------|
| Fix cost | 600 h (automated tests for registration, renewal, payment-approval paths) × $50 | **$30,000** |
| Carry cost | 60 h/month (manual regression, late bug fixing, hotfixes) × $50 | **$3,000 / month** |
| Opportunity cost | 0.2 features/month delayed × $10,000 | **$2,000 / month** |
| Risk cost | 40% annual chance of a production defect in a filing flow × $10,000 = $4,000/year | **$333 / month** |
| **Monthly cost of carrying** | $3,000 + $2,000 + $333 | **$5,333 ($64,000 / year)** |

### 2.2 Summary

| Item | Fix cost | Carry / mo | Opportunity / mo | Risk / mo | **Total / mo** | Total / year |
|------|---------:|-----------:|-----------------:|----------:|---------------:|-------------:|
| TD-ARC-001 | $60,000 | $4,000 | $5,000 | $500 | **$9,500** | $114,000 |
| TD-COD-001 | $50,000 | $2,000 | $2,500 | $1,250 | **$5,750** | $69,000 |
| TD-COD-008 | $20,000 | $1,000 | $1,000 | $1,875 | **$3,875** | $46,500 |
| TD-COD-002 | $45,000 | $2,500 | $3,000 | $250 | **$5,750** | $69,000 |
| TD-TST-001 | $30,000 | $3,000 | $2,000 | $333 | **$5,333** | $64,000 |
| **Total** | **$205,000** (4,100 h) | **$12,500** | **$13,500** | **$4,208** | **$30,208** | **$362,500** |

**Observations:**
- The top 5 items cost about **$30,000 per month (~$362,500 per year)** to carry, against a one-time fix cost of **$205,000**.
- **Opportunity cost is the largest component ($13,500/month)**, then carry cost ($12,500/month). Day-to-day drag matters more than the incident risk.
- **TD-ARC-001 is the most expensive to carry** ($9,500/month), driven by carry and opportunity cost.
- **TD-COD-008 is mostly risk cost**: a small monthly drag, but the largest single incident impact ($150,000), so it is cheap to fix relative to the loss it prevents.

---

## 3. Trend Analysis

### 3.1 How debt cost changes over time

Debt cost grows because the version gap widens, more code is built on top of the debt, workarounds multiply, and vulnerabilities accumulate. Assumed growth: carry and opportunity cost **2% per month**, risk cost **3% per month**, fix cost **1.5% per month** (the longer it waits, the more there is to rewrite and retest).

### 3.2 Projection if no action is taken (top 5 items)

| Month | Monthly cost of carrying | Cumulative cost since today |
|------:|-------------------------:|----------------------------:|
| 0 (today) | $30,208 | $0 |
| 6 | $34,305 | $195,329 |
| 12 | $38,974 | $417,205 |
| 18 | $44,299 | $669,346 |
| 24 | $50,374 | $956,012 |
| 36 | $65,234 | $1,653,165 |

- Monthly cost rises about **29% in 12 months** and **67% in 24 months**.
- Doing nothing for 12 months costs roughly **$417,000**, about double the $205,000 it would take to fix these five items today.

### 3.3 Cost of delay on the fix itself

| Start remediation in | Fix cost | Extra vs today |
|----------------------|---------:|---------------:|
| Now | $205,000 | n/a |
| 6 months | $224,156 | +$19,156 |
| 12 months | $245,102 | +$40,102 |

### 3.4 Break-even analysis for remediation

Assumes remediation starts now, benefits begin after a **6-month delivery period**, and the trend growth above applies. Break-even is the month when cumulative avoided cost equals the fix cost.

| Item | Fix cost | Break-even (months from start) |
|------|---------:|-------------------------------:|
| TD-COD-008 | $20,000 | month 11 |
| TD-TST-001 | $30,000 | month 11 |
| TD-ARC-001 | $60,000 | month 12 |
| TD-COD-002 | $45,000 | month 13 |
| TD-COD-001 | $50,000 | month 14 |
| **Top 5 together** | **$205,000** | **month 12** |

In plain terms, the investment is paid back about **6 months after delivery**, roughly a year after starting.

---

## 4. ROI Model

### 4.1 Investment required

| Item | Hours | Investment |
|------|------:|-----------:|
| TD-ARC-001 | 1,200 | $60,000 |
| TD-COD-001 | 1,000 | $50,000 |
| TD-COD-002 | 900 | $45,000 |
| TD-TST-001 | 600 | $30,000 |
| TD-COD-008 | 400 | $20,000 |
| **Total** | **4,100** | **$205,000** |

### 4.2 Payback and ROI

Monthly benefit = carry + opportunity + 90% of monthly risk cost.
Payback (months after delivery) = fix cost ÷ monthly benefit.
24-month ROI = (monthly benefit × 24 − fix cost) ÷ fix cost. Flat benefits, no trend growth, so this is the conservative case.

| Item | Fix cost | Monthly benefit | Payback | 24-month ROI |
|------|---------:|----------------:|--------:|-------------:|
| TD-COD-008 | $20,000 | $3,688 | **5.4 months** | 342% |
| TD-TST-001 | $30,000 | $5,300 | **5.7 months** | 324% |
| TD-ARC-001 | $60,000 | $9,450 | **6.3 months** | 278% |
| TD-COD-002 | $45,000 | $5,725 | **7.9 months** | 205% |
| TD-COD-001 | $50,000 | $5,625 | **8.9 months** | 170% |
| **Portfolio** | **$205,000** | **$29,788** | **6.9 months** | **249%** |

All five items pay back in under 9 months after delivery. Security (COD-008) and test coverage (TST-001) have the fastest payback and are cheapest to start. COD-001 and COD-002 pay back slowest, but they cannot be skipped: the runtime is out of support and the Angular gap keeps growing.

### 4.3 Sensitivity analysis (portfolio)

Each row changes one input from the base case (or several for the worst case).

| Scenario | Monthly benefit | Payback | 24-month ROI |
|----------|----------------:|--------:|-------------:|
| **Base case** | $29,788 | **6.9 months** | 249% |
| Fix cost +50% (overrun) | $29,788 | 10.3 months | 132% |
| Fix cost −20% | $29,788 | 5.5 months | 336% |
| Carry hours −30% | $26,038 | 7.9 months | 205% |
| Carry hours +30% | $33,538 | 6.1 months | 293% |
| Opportunity cost −50% | $23,038 | 8.9 months | 170% |
| Risk cost −50% | $27,894 | 7.3 months | 227% |
| Risk cost +50% | $31,681 | 6.5 months | 271% |
| Rate $40/hour | $27,288 | 6.0 months | 299% |
| Rate $65/hour | $33,538 | 7.9 months | 202% |
| Only 60% of benefits realised | $17,872 | 11.5 months | 109% |
| **Worst case:** fix +50%, carry −30%, opportunity −50%, risk −50% | $17,394 | 17.7 months | 36% |

**Reading the results:**
- The investment stays positive in every scenario, including the worst case (payback ~18 months, ROI still positive over 24 months).
- The most sensitive inputs are **benefit realisation**, **opportunity cost** and **fix-cost overrun**. These are the numbers to validate first.
- **Risk cost matters less than expected for the case itself.** With incident risk set to zero, payback is still about 7.9 months, so the case does not depend on a security incident happening.
- Overlap between items (for example, consolidating services reduces the effort for COD-001 and TST-001) means the portfolio benefit may be slightly lower than the sum of its parts. The "60% of benefits realised" row covers that, and the case still holds.

---

## 5. Conclusions and Recommendation

1. **The top 5 debt items cost roughly $30,000 a month and the cost is rising.** Waiting 12 months costs about $417,000 and raises the fix cost by about $40,000.
2. **Fixing them costs about $205,000 (4,100 hours) and pays back in about 7 months after delivery**, roughly a year after starting.
3. **Start with the quick, high-return items** (TD-COD-008 security and TD-TST-001 test coverage), run them alongside the large programme items (TD-ARC-001 service consolidation, TD-COD-001 and TD-COD-002 upgrades), and use the new tests as the safety net for the migration.
4. **Validate the biggest assumptions first**: the value per feature, the extra hours per month, and the benefit realisation rate, since these drive the result most.