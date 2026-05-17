# 01 — Discovery plan: new-customer onboarding
**Domain:** Mid-sized payments fintech  
**Baseline:** 4.2-day median onboarding · 18% drop-off · 5 systems · 1.8 compliance-attributed days

---

## Objective

Identify the highest-leverage intervention points to reduce onboarding cycle time and drop-off rate without increasing compliance or fair-lending risk.

---

## Week 1 — Instrument the funnel

| Activity | Owner hint | Output |
|---|---|---|
| Pull per-step timestamps across all 5 systems for the last 90 days | Eng / data | Median + P90 dwell time per step |
| Tag every drop-off event by step, channel, and customer segment | Analytics | Drop-off heatmap |
| Map handoff latency between systems (not just in-system time) | Process analyst | Handoff latency matrix |
| Identify which 1.8 compliance days are wait time vs. active review | Compliance + ops | Compliance time-split |

---

## Week 2 — Root-cause interviews

- **Ops staff** handling KYC/AML queues: what triggers manual review, and how often is it a false positive?
- **Customers who dropped off** (exit-survey sample, n ≥ 30): at which step, and why?
- **Risk / compliance leads**: what is the actual regulatory floor on review SLAs? What is discretionary process overhead?
- **Product / engineering**: what data does each of the 5 systems hold, and where are the re-entry points for a customer who pauses?

---

## Week 3–4 — Synthesise and size

- Rank steps by: (dwell time × drop-off rate at that step)
- Separate "fixable with process" vs. "fixable with automation" vs. "regulatory floor"
- Produce a shortlist of ≤5 interventions with estimated cycle-time impact and implementation complexity

---

## Non-negotiable risk assumption

> **Assumed:** Reducing compliance review time is achievable without increasing false-negative rate on KYC/AML decisions, and will not trigger fair-lending or model-risk objections from the compliance team.

This is the single assumption that could invalidate every timeline and ROI projection in this engagement.

### Verifiable check — within 30 days

| Check | How | Pass criterion |
|---|---|---|
| Regulatory floor on KYC/AML SLA | Written confirmation from Chief Compliance Officer or General Counsel | A defined minimum review window exists and is shorter than the current 1.8 days, OR the gap is wholly attributable to resolvable queue/staffing issues |
| False-negative appetite | Review last 12 months of SAR filing rate and post-onboarding fraud rate by review-time cohort | No statistically significant correlation between faster review and elevated fraud/SAR rate |
| Fair-lending model risk | Compliance sign-off that any decisioning automation is reviewed under fair-lending and ECOA obligations | Written go/no-go from compliance before any automation is scoped |

If any check fails, the scope of the engagement pivots immediately to process efficiency (staffing, queue management, system consolidation) rather than automation of compliance steps.

---

## Immediate next actions

1. Secure data access to all 5 systems' event logs — agree schema and export format by end of day 3.
2. Schedule CCO / compliance interview in week 1, not week 2 — this gates everything else.
3. Identify a drop-off cohort for customer callbacks; legal to advise on re-contact consent.

---

*Generated: discovery phase · pre-hypothesis*
