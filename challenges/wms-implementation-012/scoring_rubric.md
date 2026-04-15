# Scoring Rubric: WMS Implementation 012

This challenge-specific rubric supplements the [general scoring methodology](../../SCORING.md).

## Challenge-Specific Evaluation Points

### Implementation Judgment (30 points)

**What we're looking for:**
- Cutover sequencing that reflects real go-live experience (UAT cycles, parallel runs, labor ramp, scope freeze dates)
- A go/no-go framework with named exit criteria, not a vague "assess readiness"
- Pushbacks that name the right three things and frame them in ops and P&L terms Okonkwo would accept
- A Peak & Pine recommendation that actually picks a methodology and kills one

**Red flags:**
- Cutover plan whose first 2-3 weeks are "discovery and stakeholder alignment"
- No kill date for scope changes
- Peak & Pine answer that presents pick methodologies as a menu with no decision
- No hour-level cutover weekend sequence

### Specificity / Technical Depth (25 points)

**What we're looking for:**
- Real cadences (UAT cycle dates, scope freeze date, labor ramp start)
- Slotting and pick methodology decisions with actual labor or accuracy math
- Cutover weekend in hour increments with named stakeholders and named rollback triggers
- Instrumentation plan with specific metrics and rollback thresholds

**Red flags:**
- Generic "we'd monitor" language without named metrics or thresholds
- Numbers that sound right but don't tie to the scenario data (e.g., citing throughput targets ungrounded in the 2,800 orders/day volume)
- Missing cutover weekend detail entirely

### AI Fluency (20 points)

**What we're looking for:**
- Specific, named example of AI-augmented implementation work (not "I use ChatGPT")
- A clear line between AI-handoff tasks and human-only tasks on the Cardinal cutover, with reasoning
- Recognition that WMS agent systems will need human judgment on non-reversible calls

**Red flags:**
- "I use ChatGPT to write SOPs" as the headline example
- No distinction between tasks AI should and should not handle on a cutover
- Treating AI as a search tool rather than an implementation workforce

### Commercial Instinct (15 points)

**What we're looking for:**
- Identifies at least 2-3 specific configs or commitments worth pushing back on
- Frames pushback in terms Okonkwo would respond to (labor variance, SLA math, cost-to-serve)
- Understands the cost of a yes more than the cost of a no
- Names a real moment of commercial pushback in Part 3, with the outcome

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
- Scannable — tables used where tables belong (cutover weeks, config decisions)
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
- Honest admission that it can't provide real pushback or war stories

**To beat it, you might:**
- Show the cutover scar tissue — the small rituals real implementation leads develop (the mock-cutover dry run 2 weekends out, the 14-day go-live freeze on the floor, the named person who owns rollback authority)
- Push back on something Claude wouldn't — the 99.5% SLA ramp curve, the Cardinal VAN split, the labor ramp timing
- On Peak & Pine, go deeper than "batch pick" — talk about the pack-station rework cost, the sortation handoff, the one thing that breaks at peak when you batch
- Use a real commercial pushback story with names of the config, the SLA, and the specific labor cost that made the COO agree
- Describe a botched implementation with the specific moment you knew it was going sideways and the call you made that week

**The human advantage on this challenge:**
- Part 1 (Cardinal): Claude produces a clean plan. Real implementation leads produce a plan with named people, hard kill dates, and a Sunday night go/no-go ritual that assumes something will go wrong.
- Part 2 (Peak & Pine): Claude will pick a methodology. Real operators know which one breaks at peak and will name the specific failure mode.
- Part 3 (Operating Edge): Claude cannot produce a botched-implementation story or a commercial-pushback story from lived experience. This is the widest gap.

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
