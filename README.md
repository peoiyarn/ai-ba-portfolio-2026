\# \[AI-augmented onboarding: payments fintech]



> Diagnosed a 4.2-day new-customer onboarding cycle at a mid-sized payments fintech — where 1.8 days were misattributed to compliance and 18% of applicants were dropping off — and designed an AI-assisted compliance queue routing engine targeting ≤ 2.5-day cycle time, with a full eval framework and governance assessment covering the EU AI Act and ECOA/Reg B.


\*\*Built during:\*\* Techademy AI for Business Analysts Bootcamp (Day 4 capstone) · May 2026

\*\*Domain:\*\* \[FinServ — payments fintech, new-customer onboarding]

\*\*Status:\*\* v1 — Day 4 capstone build. v2 planned (see "What I'd Do With More Time").


\---



\## The Case



\A mid-sized payments fintech was onboarding new customers across five disconnected systems, producing a 4.2-day median cycle time and an 18% applicant drop-off rate. Of the 4.2 days, 1.8 were attributed to compliance — but no one had separated mandated review time from queue routing latency, leaving the largest reducible bottleneck invisible. The business case for AI intervention existed; the measurement foundation to justify it did not.


\*\*Stakeholders interviewed (or imagined for the capstone):\*\*

\- \Liam, Head of Onboarding — owns the process across five systems; estimates at least 0.6 of the 1.8 compliance days is queue wait, not review; burned by two prior transformation projects that delivered decks and no change

\- \Aisha, Compliance Lead — accountable for ECOA and Reg B; will not engage without a written compliance gate signed by counsel; the single veto point on any automation touching decisioning logic

\- \Carlos, SVP Risk & Internal Controls — sets risk appetite for the project; requires a KYC false-negative rate baseline before any automation is scoped; preparing for a spring regulatory exam

\- \Tom, Engineering Lead — holds the budget and the system logs; concerned about scope creep; needs a bounded phase-1 with a stop/go gate

\- \Reina, Customer Experience Lead — holds step-level drop-off data; her data is currently disconnected from the internal operational event logs that would explain why customers are leaving


\*\*Constraints that shaped the design:\*\*

\- \ECOA and Reg B: any change to decisioning-adjacent logic — including routing logic — requires a fair-lending review before scoping. This pushed the AI feature toward pure queue routing (no decisioning) and made Aisha's compliance gate a hard prerequisite, not a soft dependency

\- \No shared measurement baseline: the KYC false-negative rate, per-step dwell time, and rework loop frequency were all unmeasured at the start of the project. Every opportunity statement that required a number was marked [NEEDS SOURCE] until instrumentation could produce it — which constrained what could be committed versus what could only be directional



\---



\## What I Did — The Six Stages



| Stage | Artifact | What it shows |

|---|---|---|

| 1. Discovery Plan | \[`01\_discovery\_plan.md`](01\_discovery\_plan.md) | The riskiest assumption named explicitly. |

| 2. Stakeholder Synthesis | \[`02\_themes.md`](02\_themes.md) + \[`02\_synthesis\_agent\_output.pdf`](02\_synthesis\_agent\_output.pdf) | 3-5 orthogonal themes with verbatim quote citations. |

| 3. Requirements + BRD section | \[`03\_brd\_section.md`](03\_brd\_section.md) | 5 user stories with 3-5 Given/When/Then acceptance criteria each. |

| 4. BPMN (validated) | \[`04\_bpmn\_validated.md`](04\_bpmn\_validated.md) | BPMN-from-text + 6-error validation checklist with corrections. |

| 5. Mining / Automation | \[`05\_opportunities.md`](05\_opportunities.md) OR \[`05\_make\_blueprint.json`](05\_make\_blueprint.json) | 3 BRD-ready opportunities, OR a runnable Make.com scenario. |

| 6. Evals + Governance | \[`06\_eval\_and\_governance.md`](06\_eval\_and\_governance.md) | 10-input golden set + 4-dimension rubric + 2 domain-specific governance risks. |



\---



\## Hardest Decision



\Whether to build a routing engine that also surfaces a recommended disposition (clear / refer / escalate) alongside each case assignment, or one that purely routes cases to the right analyst and stops there.
The disposition signal would have been more immediately useful — it would have pre-flagged low-confidence AML cases before the analyst opened them. But Aisha's requirement that any change to decisioning logic triggers a full fair-lending review, and Carlos's insistence on a KYC false-negative baseline before any automation is scoped, made it impossible to ship within 30 days.
The pure router was chosen. It costs the project any reduction in active review time — only the ~0.6-day queue wait is recovered. The benefit is that it requires no fair-lending model risk review, clears Aisha's compliance gate more easily, and can actually ship. The disposition-signal version is parked as a v2 candidate, contingent on US-03 (baseline report) and US-02 (compliance gate) being completed first.



\---



\## What I'd Do With More Time



\- \10-input golden set is too small for production. The eval framework uses 10 synthetic golden inputs — enough to catch structural failures in the routing logic, but not representative of the full distribution of edge cases in a live onboarding population. A production eval set would need 50–100 inputs minimum, drawn from a stratified sample of real historical cases (with PII removed), covering the long tail of applicant segment combinations and multi-flag concurrent scenarios that the synthetic inputs cannot fully anticipate.

\- \No A/B test against the human routing baseline. The 0.6-day queue-to-touch time baseline is Liam's estimate, not a measured figure. The routing engine's performance cannot be evaluated against a baseline that does not yet exist. Before any pilot, US-01 instrumentation must produce a verified baseline, and the routing engine must be run in shadow mode against live cases — logging what it would have assigned versus what the human router actually assigned — for at least four weeks before any live routing decision is delegated to it.

\- \Governance mapping covers EU AI Act and federal ECOA/Reg B only. The two governance risks documented in stage 6 are the most material for this feature. But a full pre-ship governance assessment should also cover: state-level fair lending laws in every state of operation (several states have broader protected class definitions than federal ECOA); FinCEN guidance on automated AML screening tools; and, if the entity is publicly traded, SOX Section 404 implications for a material change to the onboarding control environment. All three are flagged as [NEEDS SOURCE] pending legal confirmation of the entity's operational footprint and public/private status.

\- \Reina's swimlane has no formal escalation path. The BPMN identified that Reina holds drop-off data but has no formal handoff to onboarding operations in the current process. The joined dataset (US-05) addresses the analytical gap but not the operational one. A v2 design should specify a formal escalation trigger — when Reina's monitoring detects a step-level drop-off spike above a threshold, what is the operational response and who owns it? This was out of scope for the capstone but is a real process gap.

\---



\## How to Reproduce



If you want to re-run this work yourself:



1\. \*\*Clone this repo:\*\*

&#x20;  ```

&#x20;  git clone https://github.com/{your-username}/ai-ba-portfolio-2026.git

&#x20;  ```

2\. \*\*Read the stage artifacts in order:\*\* 01 → 02 → 03 → 04 → 05 → 06.

3\. \*\*For Stage 5 (if Make.com blueprint):\*\* import `05\_make\_blueprint.json` into a free Make.com account. Connect Anthropic + Slack (free tier credit covers a few hundred runs).

4\. \*\*For Stage 6:\*\* the eval rubric (`06\_eval\_rubric.csv`) is runnable as a manual scoring exercise on any AI output.



\*\*Stack used:\*\*

\- Anthropic Claude (claude-sonnet-4-6) — LLM

\- Markdown — all artifacts are plain markdown, no proprietary tooling required

\- GitHub Desktop — version control \& publishing



\---



\## About me



\Samantha, Product Management, https://www.linkedin.com/in/peoiyarn/



Built as part of the \[Techademy AI for Business Analysts Bootcamp](https://techademy.com/ai-for-ba). Feedback welcome — open an issue or DM on LinkedIn.



\---



\*License: MIT — see \[LICENSE](LICENSE)\*

