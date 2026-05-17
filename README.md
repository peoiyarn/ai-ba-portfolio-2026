# AI-augmented onboarding: payments fintech

> Diagnosed a 4.2-day new-customer onboarding cycle at a mid-sized payments fintech — where 1.8 days were misattributed to compliance and 18% of applicants were dropping off — and designed an AI-assisted compliance queue routing engine targeting ≤ 2.5-day cycle time, with a full eval framework and governance assessment covering the EU AI Act and ECOA/Reg B.

**Built during:** Techademy AI for Business Analysts Bootcamp (Day 4 capstone) · May 2026  
**Domain:** FinServ — payments fintech, new-customer onboarding  
**Status:** v1 — Day 4 capstone build. v2 planned (see "What I'd Do With More Time").

---

## The case

A mid-sized payments fintech was onboarding new customers across five disconnected systems, producing a 4.2-day median cycle time and an 18% applicant drop-off rate. Of the 4.2 days, 1.8 were attributed to compliance — but no one had separated mandated review time from queue routing latency, leaving the largest reducible bottleneck invisible. The business case for AI intervention existed; the measurement foundation to justify it did not.

**Stakeholders interviewed (imagined for the capstone):**

- Liam, Head of Onboarding — owns the process across five systems; estimates at least 0.6 of the 1.8 compliance days is queue wait, not review; burned by two prior transformation projects that delivered decks and no change
- Aisha, Compliance Lead — accountable for ECOA and Reg B; will not engage without a written compliance gate signed by counsel; the single veto point on any automation touching decisioning logic
- Carlos, SVP Risk & Internal Controls — sets risk appetite for the project; requires a KYC false-negative rate baseline before any automation is scoped; preparing for a spring regulatory exam
- Tom, Engineering Lead — holds the budget and the system logs; concerned about scope creep; needs a bounded phase-1 with a stop/go gate
- Reina, Customer Experience Lead — holds step-level drop-off data; her data is currently disconnected from the internal operational event logs that would explain why customers are leaving

**Constraints that shaped the design:**

- ECOA and Reg B: any change to decisioning-adjacent logic — including routing logic — requires a fair-lending review before scoping. This pushed the AI feature toward pure queue routing (no decisioning) and made Aisha's compliance gate a hard prerequisite, not a soft dependency
- No shared measurement baseline: the KYC false-negative rate, per-step dwell time, and rework loop frequency were all unmeasured at the start of the project. Every opportunity statement that required a number was marked [NEEDS SOURCE] until instrumentation could produce it — which constrained what could be committed versus what could only be directional

---

## What I did — the six stages

| Stage | Artifact | What it shows |
|---|---|---|
| 1. Discovery plan | [`01_discovery_plan.md`](/Day4_Capstone/01_discovery_plan.md) | The riskiest assumption named explicitly: that compliance review time can be reduced without increasing KYC/AML false-negative rate — with a 3-check, 30-day verification protocol |
| 2. Stakeholder synthesis | [`02_themes.md`](/Day4_Capstone/02_themes.md) | 5 orthogonal themes with verbatim quote citations, persona attribution, and BABOK v3 area mapping |
| 3. Requirements + BRD section | [`03_user_stories.md`](/Day4_Capstone/03_user_stories.md) | 5 user stories with 6–7 Given/When/Then acceptance criteria each — including happy path, negative path, accessibility (WCAG 2.2 AA), and performance (sourced only) |
| 4. BPMN (validated) | [`04_bpmn_validated.md`](/Day4_Capstone/04_bpmn_validated.md) | BPMN-from-text across 6 swimlanes + 6-error validation checklist with 7 corrections documented |
| 5. Process mining | [`05_opportunities.md`](/Day4_Capstone/05_opportunities.md) | 1 bottleneck + 1 rework loop identified; 3 BRD-ready opportunities in metric / baseline / target / intervention / evidence format |
| 6. Evals + governance | [`06_eval_and_governance.md`](/Day4_Capstone/06_eval_and_governance.md) | 10-input golden set (4 happy path, 3 edge, 2 adversarial, 1 out-of-scope) + 4-dimension rubric (Accuracy / Tone / Safety / Citation) + 2 domain-specific governance risks (EU AI Act Annex III point 5(b) + ECOA/Reg B disparate treatment) with pre-ship mitigations |

---

## Hardest decision

Whether to scope the AI feature as a full compliance routing-and-triage engine — one that surfaces a recommended disposition alongside the assignment — or as a pure queue router that assigns cases to analysts without any disposition signal.

The disposition signal would have been more useful to Aisha's team in the short term. It would have surfaced low-confidence AML flags before the analyst opened the case, potentially reducing review time on straightforward cases. But Aisha's stated requirement — *"if you're changing decisioning logic at all, that's a fair-lending review, full stop"* — and Carlos's demand for a false-negative rate baseline before any automation is scoped made the disposition-signal version impossible to ship within the project's 30-day discovery window.

The pure queue router was chosen: it assigns cases to the right analyst type based on flags and queue depth, surfaces case information in the handoff notes, and stops there. The analyst makes every judgment call. The cost is that the routing engine delivers no reduction in active review time — only in queue wait time (~0.6 days). The benefit is that it requires no fair-lending model risk review, does not touch the KYC/AML false-negative rate baseline, and can be built and evaluated before the compliance gate is fully executed. It is the version that can actually ship without creating the regulatory exposure that would cause Aisha to block the entire project.

The disposition-signal version is documented as a v2 candidate — contingent on the baseline report (US-03) establishing a defensible false-negative rate, and the compliance gate (US-02) explicitly covering disposition signals as within scope.

---

## What I'd do with more time

- **10-input golden set is too small for production.** The eval framework uses 10 synthetic golden inputs — enough to catch structural failures in the routing logic, but not representative of the full distribution of edge cases in a live onboarding population. A production eval set would need 50–100 inputs minimum, drawn from a stratified sample of real historical cases (with PII removed), covering the long tail of applicant segment combinations and multi-flag concurrent scenarios that the synthetic inputs cannot fully anticipate.

- **No A/B test against the human routing baseline.** The 0.6-day queue-to-touch time baseline is Liam's estimate, not a measured figure. The routing engine's performance cannot be evaluated against a baseline that does not yet exist. Before any pilot, US-01 instrumentation must produce a verified baseline, and the routing engine must be run in shadow mode against live cases — logging what it would have assigned versus what the human router actually assigned — for at least four weeks before any live routing decision is delegated to it.

- **Governance mapping covers EU AI Act and federal ECOA/Reg B only.** The two governance risks documented in stage 6 are the most material for this feature. But a full pre-ship governance assessment should also cover: state-level fair lending laws in every state of operation (several states have broader protected class definitions than federal ECOA); FinCEN guidance on automated AML screening tools; and, if the entity is publicly traded, SOX Section 404 implications for a material change to the onboarding control environment. All three are flagged as [NEEDS SOURCE] pending legal confirmation of the entity's operational footprint and public/private status.

- **Reina's swimlane has no formal escalation path.** The BPMN identified that Reina holds drop-off data but has no formal handoff to onboarding operations in the current process. The joined dataset (US-05) addresses the analytical gap but not the operational one. A v2 design should specify a formal escalation trigger — when Reina's monitoring detects a step-level drop-off spike above a threshold, what is the operational response and who owns it? This was out of scope for the capstone but is a real process gap.

---

## How to reproduce

If you want to re-run this work yourself:

1. **Clone this repo:**
   ```
   git clone https://github.com/{your-username}/ai-ba-portfolio-2026.git
   ```
2. **Read the stage artifacts in order:** 01 → 02 → 03 → 04 → 05 → 06.
3. **For stage 5 (process mining path):** the three BRD-ready opportunities in `05_opportunities.md` reference a bottleneck and rework loop identified from the BPMN in `04_bpmn_validated.md`. Read these two together — the BPMN is the source of truth for the process structure; the opportunities document is the analytical output.
4. **For stage 6:** the eval rubric in `06_eval_and_governance.md` is runnable as a manual scoring exercise. Take any output from the AI routing engine (or a simulated output), score it against the 4-dimension rubric for each of the 10 golden inputs, and check the result against the shipping threshold (all dimensions ≥ 4 on 9 of 10 inputs, with no Safety score below 3 on any input).

**Stack used:**
- Anthropic Claude (claude-sonnet-4-6) — LLM used throughout for synthesis, drafting, and structured analysis
- GitHub Desktop — version control and publishing
- Markdown — all artifacts are plain markdown, no proprietary tooling required

---

## About me
Samantha Kong, Product Management, https://www.linkedin.com/in/peoiyarn/

Built as part of the [Techademy AI for Business Analysts Bootcamp](https://techademy.com/ai-for-ba) · May 2026. Feedback welcome — open an issue or DM on LinkedIn.

---

*License: MIT — see [LICENSE](LICENSE)*
