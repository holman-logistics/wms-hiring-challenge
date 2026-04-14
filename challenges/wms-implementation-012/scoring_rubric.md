# Scoring Rubric: WMS Implementation 012

This challenge-specific rubric supplements the [general scoring methodology](../../SCORING.md).

## Challenge-Specific Evaluation Points

### Implementation Judgment (30 points)

**What we're looking for:**
- Cutover sequencing that reflects real go-live experience (UAT cycles, parallel runs, labor ramp, scope freeze dates)
- A go/no-go framework with named exit criteria, not a vague "assess readiness"
- Pushbacks that name the right three things and frame them in ops and P&L terms Okonkwo would accept
- A Peak & Pine recommendation that actually picks a methodology and kills one
- Diagnosis instincts on Hollister that start with reconciliation, not dashboards

**Red flags:**
- Cutover plan whose first 2-3 weeks are "discovery and stakeholder alignment"
- No kill date for scope changes
- Peak & Pine answer that presents pick methodologies as a menu with no decision
- Hollister triage that starts with "build an integration dashboard"
- No hour-level cutover weekend sequence

### Specificity / Technical Depth (25 points)

**What we're looking for:**
- EDI transaction sets named correctly (940, 943, 944, 945, 846, 856) and used correctly
- Cadences specified (nightly at 0200, every 15 minutes, real-time on event)
- Reconciliation described as a specific activity with a frequency and an owner, not "we'd monitor"
- Slotting and pick methodology decisions with actual labor or accuracy math
- Cutover weekend in hour increments with named stakeholders and named rollback triggers

**Red flags:**
- Describing EDI as "data integration" without naming transactions
- "Real-time inventory sync" with no mention of latency, reconciliation, or drift
- Numbers that sound right but don't tie to the scenario data (e.g., citing throughput targets ungrounded in the 2,800 orders/day volume)
- Missing cutover weekend detail entirely

### AI Fluency (20 points)

**What we're looking for:**
- Specific, named examples of AI-augmented implementation work (not "I use ChatGPT")
- A clear line between AI-handoff tasks and human-only tasks, with reasoning
- A 2028 view that understands the human bottleneck shifts from doing to evaluating
- Recognition that WMS agent systems will need human judgment on non-reversible calls

**Red flags:**
- "I use ChatGPT to write SOPs" as the headline example
- No distinction between tasks AI should and should not handle on a cutover
- A 2028 vision that's indistinguishable from generic "AI will transform X" copy
- Treating AI as a search tool rather than an implementation workforce

### Commercial Instinct (15 points)

**What we're looking for:**
- Identifies at least 2-3 specific configs or commitments worth pushing back on
- Frames pushback in terms Okonkwo would respond to (labor variance, SLA math, cost-to-serve)
- Understands the cost of a yes more than the cost of a no
- On Part 4, names a real moment of commercial pushback with the outcome

**Red flags:**
- "I'd partner closely with commercial" with no concrete pushback
- Accepting Cardinal's 99.5% accuracy SLA without interrogating the ramp curve
- No mention of the Peak & Pine commercial commitment in the config answer
- "I always support the commercial team's decisions" — this is a disqualifier

**Bonus points for:**
- Naming the specific commercial failure pattern at 3PLs (selling a config ops can't profitably deliver)
- Framing pushback as a calibration ritual, not a confrontation

### Communication (10 points)

**What we're looking for:**
- Scannable — tables used where tables belong (cutover weeks, config decisions, EDI architecture)
- Diagnosis before remediation, remediation before monitoring
- Respects the 2-page prose limit; artifacts externalized to tables and timelines
- Writes like someone who's sent "Cutover complete, defects logged, hypercare running" messages

**Red flags:**
- Long narrative paragraphs where a table would work
- Consulting-deck cadence — frameworks without decisions
- Padding the artifacts-don't-count escape hatch with decorative diagrams

---

## Hints for What Beats Claude

Claude's baseline is structured and technically correct:
- Clean week-by-week cutover plan with a defensible hour-level Sunday
- A Peak & Pine decision table that picks batch pick and velocity slotting
- A Hollister architecture with the right transaction sets and cadences
- Honest admission that it can't provide real pushback or cutover war stories

**To beat it, you might:**
- Show the cutover scar tissue — the small rituals real implementation leads develop (the mock-cutover dry run 2 weekends out, the 14-day go-live freeze on the floor, the named person who owns rollback authority)
- Push back on something Claude wouldn't — the 99.5% SLA ramp curve, the Cardinal VAN split, the labor ramp timing, or the fact that parallel EDI testing with real trading partners takes longer than 1 week
- On Peak & Pine, go deeper than "batch pick" — talk about the pack-station rework cost, the sortation handoff, the one thing that breaks at peak when you batch
- On Hollister, go beyond the three most likely causes to the ugly one: a customer-side bug where the 846 drift is compounding because their allocation engine ignores the as-of-timestamp
- Use a real commercial pushback story with names of the config, the SLA, and the specific labor cost that made Okonkwo agree
- Describe a cutover weekend with specifics only an operator knows — the comms ritual, the food order, the rotation, the moment the team almost called rollback and what pulled them back

**The human advantage on this challenge:**
- Part 1 (Cardinal): Claude produces a clean plan. Real implementation leads produce a plan with named people, hard kill dates, and a Sunday night go/no-go ritual that assumes something will go wrong.
- Part 2 (Peak & Pine): Claude will pick a methodology. Real operators know which one breaks at peak and will name the specific failure mode.
- Part 3 (Hollister): Claude will diagnose well. Real operators will start with the reconciliation conversation with Hollister's ERP team, not the Körber support ticket.
- Part 4 (Operator Experience): Claude cannot produce a botched-implementation story, a commercial-pushback story, or a cutover-weekend story from lived experience. This is the widest gap.

### LinkedIn & Background (Evaluated Separately)

**What we're looking for:**
- Multi-year tenure inside 3PLs or large enterprise shippers
- Evidence of owning implementations end-to-end, not just participating (project manager, implementation lead, operations lead)
- Named WMS platforms on their resume with version detail (Körber HighJump is a plus; any named enterprise or mid-market platform beats "WMS experience")
- Progression of implementation scope — solo shipper, then multi-shipper, then greenfield DC

**Red flags:**
- "Familiar with WMS" without naming one
- Only vendor-side or consulting experience with no 3PL operator time
- No implementation they can name with the shipper, dates, and outcome
- Average tenure under 18 months across roles
