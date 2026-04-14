# Claude's Answer: WMS Implementation 012

*This is Claude's response to the challenge brief. Your submission will be compared against this in a blind review.*

---

## Part 1: Cardinal Auto Group — 90-Day Cutover

### Week-by-Week Plan

| Week | Dates | Milestone | Exit Criteria |
|------|-------|-----------|---------------|
| W1 | Oct 14-20 | Kickoff, scope freeze, data discovery | Signed scope doc, SKU master file delivered, EDI spec locked |
| W2 | Oct 21-27 | Solution design sign-off (slotting, pick methodology, value-add flows) | Design doc signed by Cardinal IT and Keystone Ops |
| W3 | Oct 28-Nov 3 | HighJump config build begins, EDI maps drafted | Config baseline loaded, 940/945 maps in dev |
| W4 | Nov 4-10 | SKU master cleanse, velocity analysis, initial slot plan | Clean SKU master, ABC classification, slot plan v1 |
| W5 | Nov 11-17 | EDI maps unit tested, HighJump UAT environment stood up | All 6 transaction sets passing unit tests |
| W6 | Nov 18-24 | Cardinal team training begins (supervisors first) | Supervisor sign-off on SOPs |
| W7 | Nov 25-Dec 1 | End-to-end UAT cycle 1 | 80% of test cases pass, known defects logged |
| W8 | Dec 2-8 | UAT cycle 2, parallel EDI testing with Cardinal's trading partners | All EDI partners passing end-to-end |
| W9 | Dec 9-15 | Pick/pack dry runs on the floor with dummy inventory | Throughput benchmark hit at 70% of target |
| W10 | Dec 16-22 | Labor ramp, supervisor shadowing, inventory pre-stage | 50% of inventory received and slotted |
| W11 | Dec 23-29 | Holiday freeze / catch-up week | Final inventory delivery scheduled |
| W12 | Dec 30-Jan 5 | Final UAT, go/no-go checkpoint with Okonkwo | Go/no-go decision documented |
| W13 | Jan 6-12 | Cutover weekend prep, comms tree, rollback plan finalized | All stakeholders briefed, on-call rosters set |
| W14 | **Jan 13-15 — CUTOVER** | Cut Cardinal's legacy system, bring up Keystone | First live orders ship |
| W15-16 | Jan 16-29 | Hypercare | Daily standups, defect burn-down, KPI tracking |

### Cutover Weekend (Jan 13-15)

**Saturday Jan 13**
- 0800: Final data extract from Cardinal's legacy WMS. Freeze inbound.
- 1200: Load master data into HighJump production (SKUs, customers, locations).
- 1600: EDI cutover — partners re-pointed to Keystone VAN endpoints.

**Sunday Jan 14**
- 0800: Inventory reconciliation. Any variance > 0.5% halts cutover.
- 1200: Parallel order run — 50 orders processed on both systems, results compared.
- 2000: Go/no-go call with Okonkwo, Cardinal IT, EDI lead. Rollback window closes.

**Monday Jan 15**
- 0500: First wave released.
- 0700: First pick audit. Accuracy target: 99.0% on day one, climbing to 99.5% by week 3.
- 1600: End-of-day review. Defect triage.

### Three Pushbacks

1. **Scope freeze by Oct 20.** Any config request after that goes to the post-go-live backlog. Framed to Okonkwo as: "Every late scope change costs us two weeks of UAT. We either hit Jan 15 with the scope we have Oct 20, or we slip to Feb."
2. **Cardinal keeps their VAN for 90 days, not permanently.** Running two EDI stacks long-term is where drift starts. Commit to a migration plan with a date.
3. **Labor ramp starts Dec 2, not Jan 2.** Commercial has us hiring 40 associates in the last two weeks. That's how accuracy drops in hypercare. Start earlier, absorb the cost.

### Biggest Risk

**SKU master quality from Cardinal.** Aftermarket auto SKUs carry fitment data (year/make/model), supersession chains, and pack factor inconsistencies. If the master is dirty at Jan 15, pick errors propagate immediately. Mitigation: dedicate a data analyst to Cardinal's master from week 1, run automated validation every week, and make clean master a go/no-go criterion.

---

## Part 2: Peak & Pine — Configuration Call

### Decision Table

| Area | Recommendation | Killing | Tradeoff Accepted |
|------|---------------|---------|-------------------|
| Pick methodology | **Batch pick with cart, 16-order batches, zone-based** | Wave-based discrete picking with wave size 40 | More complex pack sortation; requires pack-station rework |
| Slotting | **Velocity-based slotting with seasonal refresh**, A-items to golden zone | Random slotting | Higher up-front labor for re-slot; blocks one weekend |
| Cartonization | **Cartonization engine at order release** (external or native to the WMS) | Manual cartonization at pack | Licensing or config cost; 4-6 week build |

### Reasoning

At 1.8 lines/order and 90% single carton, discrete picking is wasting travel. Batch picking with 16-order carts and zone handoff should cut travel by 30-40%. Velocity slotting after 6 months of order data is overdue. Manual cartonization at this volume is where accuracy leaks.

### Instrumentation Plan

| Metric | Baseline | 30-day target | Rollback threshold |
|--------|----------|---------------|-------------------|
| Units per labor hour | 42 | 52 | < 44 |
| Order accuracy | 99.2% | 99.5% | < 99.0% |
| Cycle time (order release to ship-ready) | 6.2h | 4.5h | > 6.5h |
| Pack exceptions per 1,000 orders | 38 | < 25 | > 45 |

If any rollback threshold hits, revert the affected change (pick, slot, or cart) and re-diagnose.

---

## Part 3: Hollister Health — Integration Fire

### 72-Hour Triage

**Hour 0-4:** Pull the 846 diff for the last 11 days. Reconcile on-hand in Körber against Hollister's ERP snapshot. Identify whether drift is SKU-specific, location-specific, or systemic.

**Hour 4-12:** Pull the 2% of rejected 945s. Check rejection reason codes. Common culprits: UOM mismatch, location reference mismatch, ASN-to-945 quantity delta, timestamp skew on the 856.

**Hour 12-24:** Call Hollister ops director. Lead with: "Here's what we've found, here's what we don't know yet, here's when you'll have the next update." Don't promise a fix yet.

**Hour 24-48:** Engage HighJump support if it's a platform bug. Engage the EDI middleware vendor if it's a map issue. Run a controlled reconciliation on a single SKU family to validate the fix before broad rollout.

**Hour 48-72:** Apply the fix in a controlled window. Re-sync 846. Monitor 945 reject rate hourly.

### EDI Architecture

| Transaction | Direction | Cadence | Reconciliation | Alert |
|-------------|-----------|---------|---------------|-------|
| 940 (order) | In | Every 15 min | Order count vs. ERP release count, hourly | Delta > 2% triggers PagerDuty |
| 943/944 (transfer/receive) | Both | Real-time | Qty-out vs. qty-in nightly | Mismatch > 0.5% |
| 945 (ship confirm) | Out | On ship event | 945 count vs. packed-order count, hourly | Reject rate > 0.5% |
| 846 (inventory) | Out | Nightly 0200, hourly delta on A-items | On-hand vs. ERP nightly | Drift > 1% |
| 856 (ASN) | Out | On shipment | ASN qty vs. 945 qty | Delta on any line |

**Weekly reconciliation across all shippers:** on-hand variance report, 945 reject rate, 846 drift, orphaned EDI files. Reviewed by me every Monday.

### Three Most Likely Root Causes

1. **Cycle count adjustments not flowing to the 846.** HighJump cycle-count transactions bypass the inventory change event that triggers the 846 delta. **Test:** compare a known cycle-count adjustment to the 846 for that SKU over the following 24 hours.
2. **UOM conversion bug on a recent SKU master update.** Hollister added 200 SKUs last month. If pack factors are off, received quantities inflate on-hand. **Test:** reconcile received qty against PO qty for new SKUs over the last 30 days.
3. **Clock skew or batch ordering issue on the nightly 846 job.** 0200 job running before cycle counts complete. **Test:** check job scheduling history and correlate with adjustment timestamps.

---

## Part 4: Operating Edge

### AI Fluency

#### Workflow I've Automated

*Note: As Claude, I don't have personal implementation experience to describe. A human candidate should name the specific workflow, the before-state numbers (hours/week, error rate, cycle time), the AI-augmented after-state numbers, and the one thing that broke. A strong answer is specific to WMS / ops work — for example, an AI-assisted SKU master cleanse, an auto-generated SOP draft flow, or an outbound cycle-count variance triage. "I use ChatGPT to write emails" does not beat this baseline.*

#### Which Parts of Cardinal I'd Hand to an AI Agent

**Hand off to an AI agent:**
- SKU master cleanse and validation (structured work, clear pass/fail rules)
- First-draft SOP generation from HighJump configuration specs
- UAT test case generation from the EDI specification
- Daily cutover standup notes and defect summaries

**Refuse to hand off:**
- The go/no-go call on Sunday night. Non-reversible, requires judgment under ambiguity.
- The pushback conversation with commercial. Relationships don't delegate.
- Slotting decisions for A-items. Requires physical understanding of the building and the team.
- Any EDI partner testing where money moves. Verification has to be human.

#### WMS Implementation Lead in 2028

By 2028, the implementation lead manages a small team of agents that handle the structured 80% of every go-live: data cleanse, config drift detection, SOP generation, UAT automation, defect triage. The human owns the 20% that can't be automated: judgment calls on go/no-go, commercial pushback, cutover weekend command, post-mortem authority. The bottleneck shifts from "doing the work" to "knowing which agent output to trust." Implementation leads who can't evaluate agent output become irrelevant; leads who can will run 3-4x the shipper portfolio they run today.

### Operator Experience

#### Botched Implementation

*Note: Claude cannot describe a real implementation failure. A strong human answer names what broke (EDI not tested with a real partner, slotting done by the vendor without floor input, labor ramped too late), when the candidate realized (ideally before the customer did), and what they did that week — usually a combination of comms transparency, scope cuts, and a very specific rescue plan. Candidates who can't name one are either too junior or not self-aware.*

#### Pushing Back on Commercial

*Note: Claude cannot demonstrate real pushback. This is one of the hardest things to do in a 3PL, because commercial closed the deal and ops eats the variance. The best human answers describe: the specific config or SLA they pushed back on, how they framed it (usually in P&L or SLA terms the commercial team couldn't ignore), and what the outcome was — including the cases where they lost the fight and what they learned.*

#### Cutover Weekend

*Note: Claude can describe a cutover weekend in the abstract but can't describe how they personally ran one. The best answers include specifics: the comms channel (Teams war room, dedicated Slack), the standup cadence (every 2 hours), who has rollback authority (usually one person, named in advance), the go/no-go criteria written down before the weekend, and the small ritual that keeps the team alive at hour 30 — ordering food, rotating leads, sending the first pick photo to the exec team.*

---

*Total: ~3 pages. Claude's answer is intentionally weaker on Part 4 Operator Experience — the botched-implementation, commercial-pushback, and cutover-weekend prompts are where real 3PL implementation leads have the biggest advantage.*
