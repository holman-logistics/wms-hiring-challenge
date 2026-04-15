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

## Part 3: Operating Edge

### AI Fluency

*Note: As Claude, I don't have personal implementation experience to describe. A human candidate should name the specific workflow, the before-state numbers (hours/week, error rate, cycle time), the AI-augmented after-state numbers, and the one thing that broke. A strong answer is specific to WMS / ops work — an AI-assisted SKU master cleanse, an auto-generated SOP draft flow, an outbound cycle-count variance triage. "I use ChatGPT to write emails" does not beat this baseline.*

**Which parts of Cardinal I'd hand to an AI agent:**
- SKU master cleanse and validation (structured work, clear pass/fail rules)
- First-draft SOP generation from HighJump configuration specs
- UAT test case generation from the EDI specification
- Daily cutover standup notes and defect summaries

**Refuse to hand off:**
- The go/no-go call on Sunday night. Non-reversible, requires judgment under ambiguity.
- The pushback conversation with commercial. Relationships don't delegate.
- Slotting decisions for A-items. Requires physical understanding of the building and the team.
- Any EDI partner testing where money moves. Verification has to be human.

### Botched Implementation

*Note: Claude cannot describe a real implementation failure. A strong human answer names what broke (EDI not tested with a real partner, slotting done by the vendor without floor input, labor ramped too late), when the candidate realized (ideally before the customer did), and what they did that week — usually a combination of comms transparency, scope cuts, and a very specific rescue plan. Candidates who can't name one are either too junior or not self-aware.*

### Commercial Pushback

*Note: Claude cannot demonstrate real pushback. This is one of the hardest things to do in a 3PL, because commercial closed the deal and ops eats the variance. The best human answers describe: the specific config or SLA they pushed back on, how they framed it (usually in P&L or SLA terms the commercial team couldn't ignore), and what the outcome was — including the cases where they lost the fight and what they learned.*

---

*Claude's answer is intentionally weaker on the Botched Implementation and Commercial Pushback prompts — these are where real 3PL implementation leads have the biggest advantage.*
