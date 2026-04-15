# Challenge: WMS Implementation 012

## Running Implementations Inside a Multi-Tenant 3PL

### The Situation

You're the new Head of WMS Implementations at a mid-market 3PL. The incumbent WMS is stretched, the shipper mix is getting more complex, and the implementation queue has two live problems before you've finished your first coffee.

**The Company: Keystone Logistics**
- $180M revenue, 4 DCs (Columbus OH hub, Reno, Atlanta, Allentown)
- 24 shipper clients across retail, aftermarket auto, consumer health, apparel, industrial MRO
- ~600 warehouse associates, mixed shifts, ~1.1M sqft total
- Incumbent WMS: **Körber HighJump** (on-prem, 12 years old, heavily customized, multi-tenant)
- Evaluating two finalist WMS platforms for a planned greenfield 5th DC in 2027
- TMS, parcel rate-shopping / labeling, and yard tooling all in place (yard is still manual + spreadsheet)
- EDI middleware is aging and under review for replatform

**The COO: James Okonkwo**
- 18 years in 3PL ops. Former Director of Ops at XPO.
- Obsession: cost-to-serve per shipper. Won't sign off on a new client without a modeled P&L.
- His read on the last two implementations: "We said yes to config requests we shouldn't have. We ate the labor variance for nine months. That's on us, not the shippers."
- You report to him. He has 20 minutes for you Monday morning.

**Your Remit**
- Own the implementation roadmap for new shipper go-lives and existing shipper re-configurations
- Partner with the integration team on EDI / API work, but you're accountable for go-live readiness
- Push back on commercial when the configuration they sold will hurt ops or the customer

---

## Your Task

Two problems are already on your desk, plus a short section on you. Tables and diagrams don't count toward the page limit. Use them.

---

### Part 1: The 90-Day Cutover — Cardinal Auto Group

**The shipper:** Cardinal Auto Group. Aftermarket specialty auto parts distributor. Consolidating two of their own warehouses into our Columbus DC. **Go-live: January 15.** Today is October 10.

**Profile:**
- 18,000 active SKUs. 40% slow movers (< 4 lines/month). 60% movers, heavy Q4/Q1 seasonality (winter product).
- ~3,500 B2B orders/day average. 12,000/day peak (first two weeks of November).
- Order shape: avg 4 lines. Mix of full case, inner pack, and each picks. 15% of orders require kitting (winter prep kits) or light assembly.
- Carriers: UPS, FedEx parcel. R+L, SAIA, Estes LTL.
- Leaving a tier-1 enterprise WMS. Their IT wants to keep their existing EDI VAN; our ops wants them on our middleware.
- Commercial has committed to a 99.5% order accuracy SLA and 24-hour order-to-ship cycle for standard SKUs.

**Deliver:**
- A **week-by-week cutover plan** from Oct 14 through Jan 29 (two weeks post go-live). Include the cutover weekend sequence at hour granularity.
- The **three things you would push back on** (config, commercial, or timeline) and how you'd frame the pushback to Okonkwo.
- The **single biggest risk** to hitting Jan 15 and your mitigation.

---

### Part 2: The Configuration Call — Peak & Pine Apparel

**The shipper:** Peak & Pine. DTC outdoor apparel. Live on our WMS six months. Contract SLA: 99.5% accuracy.

**The problem:**
- 45,000 SKUs (style × color × size). 2,800 B2C orders/day average. Nov-Dec peak: 12,000/day.
- Avg 1.8 lines/order. Single carton 90% of the time.
- Current config: **discrete single-order picking**, wave size 40, **random slotting** (they moved in 5 months ago), **manual cartonization** at pack.
- Labor hours per unit picked: up 14% in the last 4 months.
- Rolling accuracy: 99.2%. Missed SLA in 3 of the last 8 weeks.

**Deliver:**
- A **configuration decision table** with your recommendations across pick methodology, slotting strategy, and cartonization. For each, show the option you're picking, the option you're **explicitly killing**, and the tradeoff you're accepting.
- Your **instrumentation plan** — how you'll know in 30 days whether the changes worked. Name the 3-4 metrics and the thresholds for "roll back."

---

### Part 3: Your Operating Edge

Three short prompts. One paragraph each is enough.

- **AI fluency.** Describe one WMS / ops workflow you've personally automated or augmented with AI. Before/after numbers. Then say which specific tasks in Part 1 (Cardinal cutover) you'd hand to an AI agent, and which you'd refuse to.
- **Botched implementation.** Tell us about an implementation that went wrong (yours or one you inherited). What broke, when you realized it, and what you did that week.
- **Commercial pushback.** Tell us about a time commercial sold a configuration that wouldn't work in ops. What did you push back on, how did you frame it, and what was the outcome.

---

## What We're Evaluating

| Criterion | Weight | What We're Looking For |
|-----------|--------|------------------------|
| Implementation judgment | 30% | Cutover sequencing, risk calls, and config decisions that reflect real go-live scar tissue |
| Specificity / technical depth | 25% | Real cadences, concrete slotting/pick calls, hour-level cutover plan, named exit criteria |
| AI fluency | 20% | Treats AI agents as implementation workforce, not a demo. Knows where they help and where they break |
| Commercial instinct | 15% | Pushes back on the three things. Understands SLA/billing math and the cost of saying yes |
| Communication | 10% | Scannable, tables where tables belong, no padding |

## What Will Lose

- A cutover plan whose first month is "discovery and stakeholder alignment"
- A configuration answer that lists pick methodologies without picking one or killing one
- "I'd use ChatGPT to draft SOPs" as your AI edge
- Refusing to push back on Cardinal's config or Peak & Pine's sales commitment
- Any answer that applies equally to a WMS, a TMS, and an ERP

## What Will Win

- A cutover plan that is boring in the right places (UAT, parallel runs, labor ramp) and sharp in the others (kill dates for scope, hour-by-hour cutover weekend, specific exit criteria for each phase)
- A Peak & Pine config recommendation that names a methodology, kills one explicitly, and tells Okonkwo what the labor math actually looks like
- Evidence you've been in the WMS at 2am on a cutover weekend, not just next to it

---

## Format

**Maximum 2 pages.** Diagrams, tables, and cutover timelines don't count toward the limit. Estimated time: 30-45 minutes.

Your submission is scored alongside Claude's baseline answer in a blind review. See [SCORING.md](../../SCORING.md) for the general rubric and [scoring_rubric.md](scoring_rubric.md) for this challenge's specifics.

---

**Questions?** Open an issue in this repo.
