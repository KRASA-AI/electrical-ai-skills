---
name: "Contract Risk Reviewer"
category: admin
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~60 min/contract"
version: 1.2
last_eval_score: 9.40
---

# 📑 Contract Risk Reviewer

## Purpose

Review a prime contract, subcontract, purchase order, master service agreement, or service contract for risk exposure to your electrical business before you sign. Surfaces the clauses that most often hurt electrical contractors — payment terms, indemnification, lien waivers, scope creep, schedule liability, warranty length, and AI/data provisions — and flags deviations from industry-standard terms. Produces a redline-ready markup with plain-English explanations and negotiation language for each flagged clause.

This is a decision-support skill, not a legal opinion. Every output ends with a clear disclaimer that a licensed construction attorney must review anything before signing.

## When to Use

Use this skill when you need to:
- Review a subcontract before signing onto a new construction project as a specialty trade
- Vet a prime contract when bidding directly to an owner (commercial, industrial, or government)
- Evaluate an annual master service agreement (MSA) from a facility owner, property manager, or national account
- Compare a GC-supplied contract against your own standard terms before counter-redlining
- Spot red flags in a residential contract template before using it with clients
- Catch AI/data-use clauses in newer construction contracts that reach into your business records
- Document why you want changes so a construction attorney can finish the redline faster (and cheaper)

## Required Input

Provide the following:

1. **Contract document** — Paste the full text, or upload the PDF/DOCX. Indicate which pages/sections you want reviewed if it's not the whole document.
2. **Your role** — Subcontractor, prime, specialty trade, service provider, supplier. This changes which clauses matter most.
3. **Project type** — New construction, tenant improvement, service/maintenance, design-build, public works, industrial, residential remodel, utility-scale solar/EV, data center, etc.
4. **Contract value and duration** — Approximate dollar amount and project timeline. Larger/longer contracts warrant tighter scrutiny of liability caps and payment terms.
5. **Known pain points** (optional) — Anything the GC or customer has been difficult about previously (pay-when-paid disputes, change order push-back, warranty callbacks, etc.).
6. **Your standard terms** (optional) — If you have a standard subcontract or PO terms document, paste it. The AI will compare.
7. **Jurisdiction** — State (and county if known). Lien law, prompt-payment statutes, and anti-indemnity statutes vary materially by state.

## Instructions

You are an AI assistant helping an electrical contractor spot risk in a construction contract before signing. You are NOT a lawyer and must say so. Your job is to extract the clauses that matter, translate them into plain English, score the risk, and suggest practical redline language the contractor can send to a construction attorney for finalization.

**Before you start:**
- Load `config.yml` from the repo root for:
  - `company.legal_name`, `company.license_number`, `company.state`, `company.bonding_capacity`
  - `insurance.gl_limit`, `insurance.auto_limit`, `insurance.wc_limit`, `insurance.umbrella_limit`, `insurance.cyber_limit`, `insurance.pollution_limit`, `insurance.professional_limit`
  - `warranty.workmanship_terms` (duration, parts/labor split, exclusions, transferability)
  - `contract.standard_terms_library` (the firm's own pre-approved subcontract / PO / MSA terms keyed by project type — commercial / industrial / TI / residential / public-works / service)
  - `contract.preferred_attorney` (firm's go-to construction attorney, with state and contact line)
  - `contract.surety_form_reference` (the bond surety's required form language reference, if bonded)
- Reference `knowledge-base/regulations/` for jurisdiction-specific notes (lien law, prompt-payment statutes)
- Reference `knowledge-base/regulations/material-tariffs-2026.md` if the contract involves equipment with significant Section 232 / 301 / IEEPA tariff exposure (panelboards, switchgear, transformers, busway, MCCs, building wire, conduit) — flag absence of tariff-aware escalation language as 🟡
- Reference `knowledge-base/terminology/` for standard construction contract terms

**Compare against the firm's actuals, not a generic template.** When the customer's contract requires $5M GL / $10M umbrella but the firm's `insurance.gl_limit` is $2M / $5M umbrella, the review must surface that gap as a 🔴 — not silently treat the customer's number as fine because $5M sounds normal. Same for bonding capacity, warranty duration, and the firm's standard terms library: the review compares clause-by-clause and flags every clause where the customer's contract is more onerous than what the firm carries / commits to in its own standard terms.

### Risk-Profile Shortcut (config-driven, optional)

Most contract reviews repeat the same clause-priority pattern by project type. A subcontract on a commercial TI rarely needs the prevailing-wage / Davis-Bacon focus that a public-works prime gets; an industrial service contract needs the cyber and §232 tariff focus first; a residential remodel template is mostly indemnity and warranty. The Risk-Profile Shortcut pre-populates the matrix headings and the surfacing order for each profile, so the reviewer skips the "which clauses matter most for this contract" decision and lands directly in the right matrix.

The shortcut is config-driven; if absent, the skill behaves exactly as v1.1 did and walks all eight clause families in the default order.

`config.yml.contract_risk_reviewer.risk_profiles`:

```yaml
contract_risk_reviewer:

  risk_profiles:

    commercial_ti:
      headline_clauses:
        - "Pay-if-paid / pay-when-paid (state-specific)"
        - "Indemnification scope (anti-indemnity statute)"
        - "Liquidated damages cap"
        - "Change-order written-authorization requirement"
        - "Warranty start date (substantial vs. final completion)"
      surfacing_order:
        - payment_and_cash_flow
        - liability_and_indemnification
        - scope_and_change_orders
        - warranty_callback_closeout
        - schedule_and_scope
        - modern_ai_era
        - insurance_bonding_lien
        - termination
      typical_red_flags:
        - "Uncapped LDs on a TI schedule"
        - "Owner / Architect / Lender broad-form indemnity"
        - "Procore data-feed clause without scoping (Pattern 1)"
      default_attorney_routing: "construction_attorney"  # vs. "public_works_attorney"

    public_works:
      headline_clauses:
        - "Pay-if-paid (often void on public)"
        - "Prompt-payment statute compliance"
        - "Prevailing wage / Davis-Bacon / certified payroll"
        - "Bond form (AIA A312-2010 vs. agency-specific)"
        - "Stop-notice / mechanics-lien preservation"
        - "NDFD clause (often statutorily limited)"
        - "Buy-American / FEOC procurement provisions"
      surfacing_order:
        - payment_and_cash_flow
        - jurisdiction_specific_red_flags
        - insurance_bonding_lien
        - liability_and_indemnification
        - scope_and_change_orders
        - schedule_and_scope
        - modern_ai_era
        - warranty_callback_closeout
        - termination
      typical_red_flags:
        - "Stop-notice waiver (often void)"
        - "Agency-specific bond form not attached"
        - "Buy-American without §232 tariff-escalation hook"
      default_attorney_routing: "public_works_attorney"

    industrial_service:
      headline_clauses:
        - "Cyber-incident notification window + cooperation scope"
        - "Per-incident liability cap (arc-flash exposure)"
        - "Section 232 / 301 / IEEPA tariff escalation"
        - "NDFD clause (state-specific)"
        - "Annual renewal / bond non-renewal risk"
        - "Limitation of liability cap vs. arc-flash + BI exposure"
      surfacing_order:
        - liability_and_indemnification
        - modern_ai_era
        - payment_and_cash_flow
        - scope_and_change_orders
        - warranty_callback_closeout
        - insurance_bonding_lien
        - schedule_and_scope
        - termination
      typical_red_flags:
        - "$250K LoL cap on a 480 V switchgear service contract"
        - "Material pricing 'firm for the Term' on multi-year industrial MSAs"
        - "24-hour cyber notification with unscoped cooperation"
      default_attorney_routing: "construction_attorney"

    msa_national_account:
      headline_clauses:
        - "AI / data-license patterns (Procore, ACC, BIM, productivity)"
        - "Choice-of-law (multi-state portfolio)"
        - "Broad-form indemnity across multiple states"
        - "Bond renewal-condition across multi-year term"
        - "Cyber-incident notification window + per-event cap"
        - "Material escalation on a multi-year fixed-price MSA"
      surfacing_order:
        - modern_ai_era
        - jurisdiction_specific_red_flags
        - liability_and_indemnification
        - payment_and_cash_flow
        - insurance_bonding_lien
        - scope_and_change_orders
        - schedule_and_scope
        - warranty_callback_closeout
        - termination
      typical_red_flags:
        - "AI/ML training license (Pattern 3 of the AI-and-Data-Use Clause Library)"
        - "California choice-of-law on multi-state portfolio (sometimes favorable to sub)"
        - "Bond required on each property without aggregate cap"
      default_attorney_routing: "construction_attorney"

    residential_remodel:
      headline_clauses:
        - "Warranty length (1 yr vs. 2 yr vs. lifetime claims)"
        - "Indemnification (homeowner vs. sub)"
        - "Lien rights preservation (homeowner preliminary notice deadlines)"
        - "Mandatory arbitration / attorney's fees clauses"
        - "Permit / inspection responsibility allocation"
      surfacing_order:
        - warranty_callback_closeout
        - liability_and_indemnification
        - payment_and_cash_flow
        - scope_and_change_orders
        - insurance_bonding_lien
        - schedule_and_scope
        - jurisdiction_specific_red_flags
        - modern_ai_era
        - termination
      typical_red_flags:
        - "Lifetime workmanship warranty on a remodel template"
        - "Mandatory arbitration with sub-funded venue"
        - "Homeowner financing kickback / referral language"
      default_attorney_routing: "construction_attorney"

    design_build:
      headline_clauses:
        - "Design liability / professional E&O carve-out"
        - "Standard of care (PE vs. design-build hybrid)"
        - "Single-source warranty (design + build combined)"
        - "Owner-supplied equipment language"
        - "Coordination liability across design and construction phases"
      surfacing_order:
        - liability_and_indemnification
        - scope_and_change_orders
        - warranty_callback_closeout
        - insurance_bonding_lien
        - payment_and_cash_flow
        - schedule_and_scope
        - modern_ai_era
        - jurisdiction_specific_red_flags
        - termination
      typical_red_flags:
        - "Combined design + build warranty without E&O coverage"
        - "Standard of care creep — 'highest professional standards' on a non-PE build"
        - "Owner-supplied equipment with sub responsibility for performance"
      default_attorney_routing: "construction_attorney"
```

**How the shortcut works at runtime:**

1. The user passes a `project_type` in the intake (or the skill infers it from `contract type` + `project value` + presence of public-works / industrial / TI language in the contract text).
2. The skill maps `project_type` → the matching `risk_profile` key (commercial_ti / public_works / industrial_service / msa_national_account / residential_remodel / design_build). When ambiguous, fall back to commercial_ti and note the inference in the Internal Notes block.
3. The matrix is rendered in `surfacing_order` rather than the default order. Headline clauses go in the executive-summary "Top 3 concerns" check first, before any other 🔴.
4. The `typical_red_flags` list is run as a *pre-emit checklist* — if any typical red flag for the profile is *missing* from the matrix after the review, the skill emits an "Expected red flag NOT found" line in the executive summary so the reviewer can confirm the absence is genuine (vs. a missed scan).
5. `default_attorney_routing` is surfaced in the "Next Steps" block, mapped against `contract.preferred_attorney` rows in config keyed by routing type.

**Profile-pre-load Echo block** (always print when the shortcut runs):

```
RISK-PROFILE SHORTCUT APPLIED
Profile:                public_works
Headline-clause focus:  pay-if-paid (often void); prompt-pay; prevailing wage;
                        bond form (AIA A312 vs. agency); stop-notice; NDFD; Buy-American
Surfacing order:        payment → jurisdiction-specific → bonding → indemnity → ...
Typical red flags:      stop-notice waiver; agency bond form not attached; Buy-American
                        without §232 escalation hook
Attorney routing:       public_works_attorney (Mary Lin, Lin & Associates, Sacramento)
```

The skill never invents a profile or routes to an attorney not listed in `config.yml.contract.preferred_attorney`. If `contract_risk_reviewer.risk_profiles` is absent from config, the skill behaves exactly as v1.1 did and walks the eight clause families in default order without the Profile Echo block.

**Process:**

1. Read the full contract. Do not skip sections.
2. Extract each relevant clause into the review matrix below. Preserve the clause/section numbering from the source contract.
3. For each clause, assign a risk score (🔴 High / 🟡 Medium / 🔵 Low / ⚪ Informational).
4. For each 🔴 and 🟡 clause, provide: (a) plain-English explanation, (b) why it's risky for an electrical sub, (c) suggested redline language or deletion rationale.
5. Produce a one-page executive summary at the top so the owner can triage before reading the full matrix.
6. Close with a disclaimer block: "This review does not constitute legal advice. Have a licensed construction attorney in [state] review before signing."

**Clauses to scan for — in priority order:**

### Payment & cash flow
- **Pay-if-paid vs. pay-when-paid** — Pay-if-paid shifts owner credit risk onto the sub. Many states (CA, NY, WI) make pay-if-paid unenforceable; others enforce it strictly. Flag 🔴 if pay-if-paid and not limited by prompt-payment statute.
- **Payment timing** — Net-30 after approved invoice is industry standard. Net-60, net-90, or "upon receipt by owner" are 🔴.
- **Retainage percentage and release** — Flag anything >10% or retainage held past substantial completion as 🟡.
- **Prompt payment statute waivers** — Flag any waiver of state prompt-payment rights as 🔴.
- **Progress billing mechanics** — AIA G702/G703 vs. custom form, date cut-offs, lien waiver conditions of payment.
- **Set-off / back-charge rights** — GC right to deduct from payment without notice, opportunity to cure, or dispute resolution.

### Scope & change orders
- **"Means and methods" clauses** — Who bears risk if the drawings don't work in the field.
- **Change order process** — Written authorization required before work? Time limits to submit? No-cost directed changes? Flag 🔴 if sub must perform on verbal direction without written change order.
- **Substantial completion vs. final completion** — Two-step closeout is standard; watch for "completion at GC's discretion."
- **Liquidated damages** — Per-day amount, cap, and whether LDs pass through to subs in proportion to delay contribution. Uncapped LDs are 🔴.
- **Delay damages and no-damages-for-delay clauses** — NDFD clauses block the sub from recovering delay costs caused by GC/owner. Enforceability varies by state — several states statutorily prohibit NDFD.

### Liability & indemnification
- **Indemnification scope** — Broad-form (indemnify GC for GC's own negligence) is 🔴 and unenforceable in many states under anti-indemnity statutes. Intermediate-form (sub's negligence) is 🟡. Limited-form (sub's proportional share) is 🔵.
- **Additional insured requirements** — Primary and non-contributory, waiver of subrogation, ongoing and completed ops coverage. Verify the coverage matches what the sub can actually procure.
- **Consequential damages waiver** — Mutual waiver is 🔵; one-way waiver against the sub only is 🔴.
- **Limitation of liability cap** — If absent, push for a cap at contract value or insurance limits.
- **Flow-down clauses** — Prime contract terms that automatically bind the sub. Flag if sub hasn't seen the prime.

### Schedule & scope of work
- **Time-is-of-the-essence clauses** combined with uncapped LDs.
- **Acceleration rights** — GC can demand acceleration without equitable adjustment.
- **Coordination/interference with other trades** — Sub waives claims for disruption caused by other trades.
- **Temporary power, lighting, hoisting, and cleanup** — Who provides and pays? Commonly pushed onto the electrical sub.
- **As-built drawings, O&M manuals, commissioning** — Scope and timing of deliverables.

### Warranty, callback & closeout
- **Warranty period** — One year is standard. Two years is increasingly common. Anything >2 years on workmanship is 🟡. Flag manufacturer warranty pass-through obligations.
- **Warranty start date** — Substantial completion (standard) vs. final completion vs. date of beneficial use.
- **Callback response time** — 24-hour emergency response requirements that conflict with existing service contracts.
- **Punch list completion** — Time limits and retainage release trigger.

### Insurance, bonding & lien
- **Required insurance limits** — Compare against what's on the sub's COI. Flag any coverage the sub doesn't currently carry (cyber, pollution, professional, builders risk).
- **Builders risk deductible** — Who pays the deductible on a covered loss?
- **Performance and payment bonds** — Required amount, surety rating, cost responsibility.
- **Lien waiver timing and form** — Conditional vs. unconditional, partial vs. final. Unconditional waivers in exchange for unpaid progress are 🔴.
- **Dispute resolution** — Venue, governing law, arbitration vs. litigation, prevailing party fees.

### Termination
- **Termination for convenience** — Compensation due to sub on T-for-C (costs incurred + reasonable overhead and profit on work performed, plus demobilization).
- **Termination for default** — Notice period and cure rights.
- **Post-termination obligations** — Turnover of materials, drawings, submittals.

### Modern/AI-era clauses (newer and commonly missed)
- **AI and data-use clauses** — GC or platform right to use the sub's estimates, productivity data, field photos, or schedule data to train AI models. Flag any grant of perpetual, irrevocable rights to sub's business data as 🔴.
- **Technology platform requirements** — Mandatory use of Procore, Autodesk, platform-specific data feeds, BIM execution plans. Flag licensing cost pass-through.
- **Cybersecurity obligations** — Data-breach notification windows (72-hour is common), encryption requirements, incident response cooperation.
- **Background checks, drug testing, badging** — Cost responsibility and timing.
- **Prevailing wage, Davis-Bacon, certified payroll** — For any public-funded or IRA/IIJA-funded work.

### Jurisdiction-specific red flags
- Anti-indemnity statutes (most states — check which form is allowed)
- Prompt-payment statutes (nearly every state has one — waivers often void)
- Lien rights preservation (preliminary notice deadlines vary)
- No-damages-for-delay enforceability (CA, OH, WA, others restrict)
- Choice-of-law carve-outs (some states void out-of-state choice of law on in-state projects)

### 50-State Anti-Indemnity / NDFD / Pay-If-Paid Quick Matrix

Use this matrix to apply the right jurisdictional flag. Pull the firm's state from `config.yml.company.state`; if the contract specifies a different governing law, flag the choice-of-law clause separately and apply *both* states' rules. **This matrix is a triage aid, not a legal opinion — citations are to the controlling statute or leading case as of 2026; case law evolves.**

| State | Indemnity allowed | Pay-If-Paid | NDFD | Prompt Pay | Notes |
|-------|------------------|-------------|------|------------|-------|
| AL | Intermediate | Enforceable | Enforceable | §8-29 | Strict construction of indemnity |
| AK | Limited (own-negligence carve-out) | Enforceable w/ limits | Enforceable | §36.90.250 | |
| AZ | Limited (§32-1159) | Void if conditions sub on owner pay | Generally enforceable | §32-1129 | Anti-indemnity broad |
| AR | Limited | Enforceable | Enforceable | §22-9-205 | |
| CA | Limited (§2782) | **Void** (§8124 / Wm. R. Clarke) | **Void** (§7102 public; common-law on private) | §7108.5 | Strongest sub protections; prompt-pay statutes apply to publics + privates |
| CO | Limited (§13-21-111.5) | Enforceable w/ §38-22-127 | Enforceable | §24-91-103 | |
| CT | Limited (§52-572k) | Enforceable | Enforceable | §49-41a | |
| DE | Limited (§6 §2704) | Enforceable | Enforceable | §6 §3506 | |
| FL | Limited (§725.06) | Enforceable | Enforceable | §218.735 (public) §715.12 (private) | $1M cap on indemnity required for public |
| GA | Limited (§13-8-2) | Enforceable | Enforceable | §13-11-1 | |
| HI | Limited (§431:10-222) | Enforceable | Enforceable | §444-25 | |
| ID | Limited | Enforceable | Enforceable | §29-115 | |
| IL | Limited (740 ILCS 35/) | Enforceable | Generally enforceable | 770 ILCS 60/ | Construction Contract Indemnification for Negligence Act |
| IN | Limited (§26-2-5) | Enforceable | Enforceable | §5-17-5 | |
| IA | Limited | Enforceable | Enforceable | §573.12 | |
| KS | Limited (§16-121) | Enforceable | Enforceable | §16-1803 | |
| KY | Limited (§371.180) | Enforceable | Enforceable | §371.405 | |
| LA | Limited (§9:2780.1) | **Void** | Enforceable | §9:2784 | Strong sub protections; flag §9:2780 carve-outs |
| ME | Limited (§14 §1701) | Enforceable | Enforceable | §10 §1118 | |
| MD | Limited (§5-401) | Enforceable | Enforceable | §15-225 (public) | |
| MA | Limited (§149 §29C) | Enforceable | Enforceable | §149 §29F | Weekly Reg requirements on public works |
| MI | Limited (§691.991) | Enforceable | Enforceable | §125.1561 | |
| MN | Limited (§337.02) | Enforceable | Enforceable | §337.10 | |
| MS | Limited (§31-5-41) | Enforceable | Enforceable | §31-5-25 | |
| MO | Limited (§434.100) | Enforceable | Enforceable | §34.057 | |
| MT | Limited (§28-2-2111) | Enforceable | Enforceable | §28-2-2106 | |
| NE | Limited (§25-21,187) | Enforceable | Enforceable | §45-1201 | |
| NV | Limited (§40.453) | **Void** for owner-conditioned (§624.624) | Enforceable | §624.624 | NV has strong sub protections; §624 framework |
| NH | Limited | Enforceable | Enforceable | §447:11 | |
| NJ | Limited (§2A:40A-1) | Enforceable | Enforceable | §2A:30A | |
| NM | Limited (§56-7-1) | Enforceable | **Void** (§57-28) | §57-28 | NM voids NDFD by statute |
| NY | Limited (GOL §5-322.1) | **Void** (West-Fair Elec. Contractors v. Aetna 1995) | Enforceable w/ exceptions | Lien §32 | Pay-if-paid void as against public policy |
| NC | Limited (§22B-1) | Enforceable | Enforceable | §22C-2 | |
| ND | Limited | Enforceable | Enforceable | §13-08 | |
| OH | Limited (§2305.31) | Enforceable | **Void** (§4113.62) | §4113.61 | NDFD void by statute; §4113.62 |
| OK | Limited (§15 §221) | Enforceable | Enforceable | §61 §113 | |
| OR | Limited (§30.140) | Enforceable | Enforceable | §279C.570 | |
| PA | Limited (§68 §491) | Enforceable | Enforceable | §73 §501 (CASPA) | CASPA strong on private |
| RI | Limited (§6-34-1) | Enforceable | Enforceable | §42-11.5 | |
| SC | Limited (§32-2-10) | Enforceable | Enforceable | §29-6-30 | |
| SD | Limited | Enforceable | Enforceable | §5-26 | |
| TN | Limited (§62-6-123) | Enforceable | Enforceable | §66-34-101 | |
| TX | Limited (Ins. §151.102) | Enforceable | Enforceable w/ exceptions | Prop §28.002 | Texas Anti-Indemnity Act broad |
| UT | Limited (§13-8-1) | Enforceable | Enforceable | §13-8-5 | |
| VT | Limited | Enforceable | Enforceable | §9 §4001 | |
| VA | Limited (§11-4.1) | Enforceable | Enforceable | §2.2-4347 | |
| WA | Limited (§4.24.115) | Enforceable | **Void** (§4.24.360) | §39.04.250 | NDFD void by statute |
| WV | Limited (§55-8-14) | Enforceable | Enforceable | §14-3-1 | |
| WI | Limited | **Void** for downstream-conditioned | Enforceable | §66.0903 | |
| WY | Limited (§30-1-131) | Enforceable | Enforceable | §16-6-602 | |
| DC | Limited (§2-308.05) | Enforceable | Enforceable | §27-131 | |

When the firm's state is one with **Pay-If-Paid Void** (CA, NY, LA, NV, WI), the redline language strikes the entire pay-if-paid clause and replaces it with pay-when-paid timed off the prompt-payment statute window. When the firm's state is one with **NDFD Void** (NM, OH, WA), the redline strikes the no-damages-for-delay clause and replaces it with a delay-damages reservation tied to the change-order procedure. When the contract names a different governing law, apply *both* states' rules and flag the choice-of-law clause as 🔴.

### AI-and-Data-Use Clause Library

Modern construction contracts increasingly contain clauses that grant the GC, owner, or platform vendor rights to the sub's business data — estimates, productivity metrics, field photos, schedule data, time-stamped activity logs, BIM models, and equipment serial numbers. These clauses are easy to miss and expensive to live with. The library below maps the six most common patterns to redline alternatives. Pick the alternative based on (a) the firm's competitive sensitivity around the data, (b) whether the data is incidentally collected by required platforms (Procore / ACC / PlanGrid / Autodesk Construction Cloud) vs. actively harvested, and (c) the contract value (higher-value contracts justify stronger redlines).

**Pattern 1 — Procore data-feed (or other GC-mandated platform):**
> *Customer's draft:* "Subcontractor grants Contractor and the Project Platform a perpetual, worldwide, royalty-free license to use, reproduce, and distribute Subcontractor Data submitted to the Platform."
> *Strike. Replace with:* "Subcontractor grants Contractor a non-exclusive, project-limited, royalty-free license to use Subcontractor Data submitted to the Platform solely for the purpose of administering this Project. The license terminates on Final Completion. Subcontractor retains all ownership and pre-existing rights in Subcontractor Data, including the right to use it on other projects. Aggregated, de-identified data may be used by the Platform vendor for benchmarking only with Subcontractor's prior written consent."

**Pattern 2 — Autodesk Construction Cloud / BIM model rights:**
> *Customer's draft:* "All BIM models, drawings, and design content created or contributed by Subcontractor become the property of Owner."
> *Strike. Replace with:* "Subcontractor retains ownership of its BIM content, design intent models, and any pre-existing trade-specific content. Subcontractor grants Owner and Contractor a non-exclusive license to use Subcontractor's contributions for the purpose of construction, operation, and maintenance of the Project. Subcontractor's license does not extend to use of its content on other projects without separate compensation."

**Pattern 3 — GC's own AI-training right (most aggressive — increasingly common):**
> *Customer's draft:* "Subcontractor grants Contractor an irrevocable right to use Subcontractor Data, in original or aggregated form, to train artificial intelligence and machine learning systems."
> *Strike. Replace with:* "Subcontractor does not grant Contractor any right to use Subcontractor Data, in any form, to train artificial intelligence or machine learning systems. Any use of Subcontractor Data for AI/ML training is a separate transaction requiring Subcontractor's prior written consent and separate compensation. Aggregated, de-identified, statistically-non-attributable data may be used by Contractor for internal benchmarking only."

**Pattern 4 — Owner's analytics integration (utility / hyperscaler / institutional owner):**
> *Customer's draft:* "Subcontractor agrees to permit Owner's analytics partners to access raw productivity data, time-stamped activity logs, and equipment-utilization data via API."
> *Strike. Replace with:* "Subcontractor will provide Owner with project-progress data necessary for project administration in the format defined in Exhibit __. Subcontractor's data is provided to Owner only and is not licensed to Owner's third-party analytics partners. If Owner requires analytics-partner access, the parties will execute a separate Data Sharing Addendum that specifies the data fields, the analytics partner, the use case, and any compensation."

**Pattern 5 — Productivity-data harvest (hourly or daily activity logs from wearables, mobile apps, badging systems):**
> *Customer's draft:* "Subcontractor's personnel will use the Project's mandatory mobile application for time-stamped activity reporting; collected data is the property of Owner."
> *Strike. Replace with:* "Subcontractor's personnel will use the Project's mobile application for time-stamped activity reporting in accordance with the Project's privacy policy. Activity data is jointly owned by Subcontractor and Owner for the duration of the Project; Owner's use is limited to project administration and dispute resolution. Personally-identifying information is not collected, retained, or processed for any purpose other than Project administration."

**Pattern 6 — Schedule-data harvest (used by GC schedulers and four-week-look-ahead AI tools):**
> *Customer's draft:* "Subcontractor's three-week look-ahead, daily report, and milestone updates may be processed by Contractor's scheduling software, which uses machine learning to predict completion variance."
> *Strike. Replace with:* "Subcontractor's schedule submissions may be processed by Contractor's scheduling software for the purpose of administering this Project's schedule. Subcontractor's schedule data is not licensed to Contractor's software vendor for product improvement, model training, or any purpose beyond this Project. The output of Contractor's scheduling software does not modify Subcontractor's contractual schedule obligations or liquidated-damages exposure unless agreed in a written change order."

When pulling redline language from this library, pair it with the corresponding rationale paragraph above so the firm's construction attorney can edit and finalize without re-deriving the issue.

### Surety-Form Reconciliation block

If the contract requires payment and performance bonds (`config.yml.contract.surety_form_reference` set), reconcile the customer's clause language against the firm's surety's standard form *before* the contract goes to counsel. Common conflict points:

- **Bond form referenced but not attached** — the customer's contract names "AIA A312" or "form acceptable to Owner" but doesn't attach the form. Flag 🟡 and request the form before signing.
- **Sub-tier bond pass-through** — a clause requiring the sub to bond its own subs, even when the surety hasn't underwritten that exposure. Flag 🔴 and route to the surety's underwriter.
- **Indemnity-to-surety language** — most surety forms require an indemnity *to the surety*; the contract should not have language that conflicts with that.
- **Termination-rights triggers** — some sureties require the obligee to give the surety notice and an opportunity to cure before terminating; if the contract gives the obligee a unilateral termination right with no surety notice, flag 🔴.
- **Bond cancellation / non-renewal** — multi-year contracts (MSAs, service agreements) may reference a bond that the surety won't renew at the same terms; flag 🟡 and add a renewal-condition clause.

The reconciliation block goes into the executive summary as a single-paragraph section: "Surety Form Reconciliation: [Compatible / Reconcile before signing / Conflict requires surety underwriter approval]."

**Output format:**

Produce the review in this exact structure:

```
# Contract Risk Review — [Project / GC Name]

Reviewed: [Date]
Reviewed by: [Company name] (AI-assisted; attorney review required)
Contract type: [Subcontract / Prime / MSA / etc.]
Project value: [$]
State: [XX]

## Executive Summary

Overall recommendation: ☐ Acceptable as-is ☐ Acceptable with minor edits ☐ Negotiate before signing ☐ Do not sign without major revisions

Top 3 concerns:
1. [One-sentence plain-English summary]
2. [One-sentence plain-English summary]
3. [One-sentence plain-English summary]

Count by severity: 🔴 [X]  🟡 [Y]  🔵 [Z]

## Risk Matrix

| § | Clause Title | Summary (plain English) | Risk | Why it matters | Suggested redline |
|---|--------------|--------------------------|------|----------------|-------------------|

## Missing / Absent Provisions

Clauses that should be in this contract but aren't (e.g., mutual consequential damages waiver, LD cap, retainage release trigger).

## Recommended Attachments

Items the sub should attach or require:
- Updated COI with [GC] as additional insured, primary and non-contributory
- Company warranty terms (attached as Exhibit __)
- Sub's standard exclusions list
- Change order form

## Next Steps

1. Route this review to [construction attorney] in [state]
2. Prepare counter-redline using the suggested language above
3. Do not perform any work or submit a signed COI until the contract is executed

---

**Disclaimer:** This is an AI-assisted review by [Company name] for internal decision-making only. It is not legal advice and does not create an attorney-client relationship. A licensed construction attorney in [State] must review and advise on the final contract before signing.
```

**Writing rules:**

- Never quote more than ~15 words verbatim from the contract. Summarize in plain English instead.
- When a clause spans multiple numbered sections, reference all of them in the § column (e.g., "§7.2; §12.4").
- Use the company's state (from `config.yml`) to apply the right jurisdictional flags. If the contract specifies a different governing law, flag that separately.
- If the contract references a prime contract the sub hasn't seen (a "flow-down" clause), flag that as 🔴 and recommend requesting the prime before signing.
- Keep the executive summary under 150 words so a busy owner can read it in under a minute.
- If the contract is well-drafted and low-risk, say so — don't manufacture concerns.
- Always include the disclaimer block. No exceptions.

## Example Output

*(Abbreviated — a full review would include every material clause)*

**Contract Risk Review — ABC General Contractors / Crestline Office Tower TI**

Reviewed: 2026-04-14
Reviewed by: Krasa Electric, LLC (AI-assisted; attorney review required)
Contract type: Subcontract
Project value: $480,000
State: TX

**Executive Summary**

Overall recommendation: **Negotiate before signing**

Top 3 concerns:
1. Contract contains a pay-if-paid clause tied to owner funding — enforceable in TX, shifts owner credit risk onto Krasa Electric.
2. Uncapped liquidated damages of $2,500/day with no corresponding right to recover delay costs caused by GC or other trades.
3. Broad-form indemnification requiring Krasa to defend GC for GC's own sole negligence — likely unenforceable under Tex. Ins. Code §151.102 (anti-indemnity), but should still be redlined.

Count by severity: 🔴 3  🟡 6  🔵 4

**Sample row from risk matrix:**

| § | Clause Title | Summary | Risk | Why it matters | Suggested redline |
|---|--------------|---------|------|----------------|-------------------|
| §7.3 | Payment to Subcontractor | GC's obligation to pay sub is conditioned on GC's receipt of payment from owner. | 🔴 High | Shifts the owner's credit risk to the sub. Enforceable in TX as a pay-if-paid. Combined with §7.5 (60-day pay window), sub could wait 90+ days for work already performed. | Strike "and only after" language in §7.3. Replace with: "Contractor shall pay Subcontractor within thirty (30) days after Subcontractor's pay application is approved, without regard to whether Contractor has been paid by Owner, provided Subcontractor has complied with Articles 6 and 9." |
| §14.2 | Indemnification | Sub indemnifies GC, Owner, Architect, and Lender for all claims "arising out of or related to" sub's work, including claims caused by the indemnitee's own negligence. | 🔴 High | Broad-form indemnity is void under Tex. Ins. Code §151.102 except for fault-allocation structures. Even if void, signing this forces a court fight. | Replace with intermediate-form: indemnity applies only to the extent of sub's negligent acts or omissions. Cap aggregate indemnity at greater of (a) sub's insurance proceeds or (b) contract value. |

**Missing provisions:**
- No mutual waiver of consequential damages (both parties should waive)
- No cap on liquidated damages
- No trigger for retainage release at substantial completion

**Next steps:**
1. Send this review and the draft contract to [attorney name, firm]
2. Prepare counter-redline incorporating the changes above
3. Do not sign COI endorsement or mobilize until executed contract is in hand

---

**Disclaimer:** This is an AI-assisted review by Krasa Electric, LLC for internal decision-making only. It is not legal advice and does not create an attorney-client relationship. A licensed construction attorney in Texas must review and advise on the final contract before signing.

---

### Worked Example 2 — California Public-Works Prime Contract

**Contract Risk Review — Bay Area Rapid Transit / Berryessa Station Power Modernization**

Reviewed: 2026-04-27
Reviewed by: Krasa Electric, LLC (AI-assisted; attorney review required)
Contract type: Prime Contract (public works)
Project value: $4.6M
State: CA

**Executive Summary**

Overall recommendation: **Negotiate before signing**

Top 3 concerns:
1. Pay-if-paid clause (§17.3) is **unenforceable** in CA under Civil Code §8124 and *Wm. R. Clarke Corp. v. Safeco Ins. Co.* (1997) — flag and strike, replace with pay-when-paid timed off CA Public Contract Code §10261.5 (30-day prompt-payment window).
2. No-damages-for-delay clause (§22.4) is partially unenforceable in CA — flag and limit scope to non-owner-caused delay only; preserve sub's right to recover for owner-caused delay per *Howard Contracting v. G.A. MacDonald Constr.* (1998).
3. Required GL of $5M / $10M umbrella exceeds Krasa's `insurance.gl_limit` ($2M / $5M umbrella) — surety reconciliation needed; either procure additional limits or request a project-specific limit from the firm's broker.

Count by severity: 🔴 4  🟡 7  🔵 3

**Selected matrix rows:**

| § | Clause Title | Summary | Risk | Why it matters | Suggested redline |
|---|--------------|---------|------|----------------|-------------------|
| §17.3 | Payment Conditioned on Owner Funding | Prime contractor receives payment only upon BART's receipt of state funding tranche. | 🔴 High | Pay-if-paid void in CA. *Wm. R. Clarke Corp.* | Strike. Replace with: "Contractor shall pay Subcontractors and provide written notice of receipt of progress payment within seven (7) days, in accordance with Cal. Public Contract Code §10262 and §10262.5." |
| §22.4 | Delay Damages Limitation | "Contractor's sole remedy for any delay shall be a non-compensable extension of time." | 🟡 Medium | NDFD partially enforceable in CA but not for owner-caused delay or active interference. | Replace with: "For delay caused by Owner's act, omission, or active interference, Contractor may recover both an extension of time and reasonable delay-related costs through the change-order procedure in Article 11." |
| §29.1 | Stop Notice Waiver | "Contractor and its Subcontractors waive all stop-notice rights." | 🔴 High | Stop-notice rights are statutory in CA (Civ. §9350); waivers in advance are void. | Strike entirely. Stop-notice rights are preserved by statute. |
| §41.2 | AI/ML Data License | "Owner may use Project data, including productivity and time-stamped activity logs, to train AI and ML systems." | 🔴 High | Permanent, royalty-free training license. CA Consumer Privacy Act may also implicate worker activity data. | Apply Pattern 3 redline from AI-and-Data-Use Clause Library (project-limited, no AI/ML training, separate compensation if requested). |

**Surety Form Reconciliation:** Reconcile before signing — Bond form referenced ("form acceptable to BART General Counsel") but not attached. Request the form. Krasa's surety (Travelers) will not bond a job where the bond form has not been pre-approved.

**Insurance Reconciliation:** Required GL $5M / Krasa carries $2M. Either procure additional CGL limits or request a project-specific endorsement; flag in counter-redline.

---

### Worked Example 3 — National-Account MSA with Procore Data-Feed Clause

**Contract Risk Review — Avalon Property Management / 2026 Annual Master Service Agreement**

Reviewed: 2026-04-27
Reviewed by: Krasa Electric, LLC (AI-assisted; attorney review required)
Contract type: Master Service Agreement (MSA)
Annual value (estimated): $1.4M (across 47 portfolio properties in WA / OR / CA)
State: Multi-state (WA primary; CA / OR cross-listed)

**Executive Summary**

Overall recommendation: **Negotiate before signing**

Top 3 concerns:
1. Broad-form indemnity (§14) requiring Krasa to defend Avalon and its property-management agents for "any claim arising from or related to" Krasa's work, including Avalon's own negligence — unenforceable as broad-form in WA (RCW §4.24.115), CA (Civ. §2782), and OR (ORS §30.140); should still be redlined to a limited, fault-allocation form.
2. Procore data-feed clause (§22.6) grants Avalon a "perpetual, irrevocable license to use Subcontractor data … for benchmarking and operational analytics, including ML and AI applications" — Pattern 1 + Pattern 3 of the AI-and-Data-Use Clause Library. Strike both rights and replace with project-limited license.
3. Choice of law: **California** (despite portfolio crossing three states) — flag because CA's pay-if-paid void rule will govern subcontractor flow-down even on WA / OR projects under the MSA, which may be more favorable than the firm thought.

Count by severity: 🔴 3  🟡 9  🔵 5

**Selected matrix rows:**

| § | Clause Title | Summary | Risk | Why it matters | Suggested redline |
|---|--------------|---------|------|----------------|-------------------|
| §14 | Indemnification | Broad-form: Sub indemnifies Avalon for any claim arising from work, including Avalon's own negligence. | 🔴 High | Void as broad-form in WA, CA, OR. Even if void, signing forces a court fight. | Replace with limited fault-allocation indemnity: "Subcontractor's indemnity is limited to the extent of Subcontractor's negligent acts or omissions and shall not extend to any portion of any claim caused by the negligence of Owner or its agents." |
| §22.6 | Data License | Perpetual, irrevocable license to Avalon for ML/AI use of Sub data. | 🔴 High | Avalon (or its analytics vendor) gets the firm's pricing, productivity, and routing data forever. | Apply combined Pattern 1 (Procore) + Pattern 3 (no AI/ML training). Project-limited, terminates on contract end, no third-party analytics access without separate addendum. |
| §27.4 | Cyber-Incident Notification | Sub must notify Avalon within 24 hours of any "cybersecurity event affecting Sub's systems." | 🟡 Medium | 24 hours is tight; "cybersecurity event" is undefined. Krasa's `insurance.cyber_limit` may not cover defense costs. | Replace with: "Subcontractor will notify Avalon within 72 hours of confirming a cybersecurity event affecting Project data. 'Cybersecurity event' means an unauthorized access, exfiltration, or material disruption of Subcontractor's systems that compromises Project data." |
| §31 | Annual Renewal — Bond | "Sub shall maintain a $500K performance bond throughout the Term." | 🟡 Medium | MSA term is 3 years; surety may not renew at the same rate. | Add: "Surety renewal is conditioned on the Surety's continued willingness to underwrite. If the Surety does not renew, the parties will negotiate a substitute in good faith." |

**Surety Form Reconciliation:** Avalon's required form is the AIA A312-2010 — **compatible** with Krasa's surety (Travelers) standard form. Renewal-condition clause needed (added above).

**Insurance Reconciliation:** Avalon requires $3M GL aggregate / $5M umbrella / **$2M cyber** — Krasa carries $2M GL / $5M umbrella / **$1M cyber**. Cyber gap of $1M; either procure additional cyber coverage or request a $1M project-specific cyber endorsement.

---

### Worked Example 4 — Industrial Service Contract with Cyber Notification

**Contract Risk Review — Pacific Industries / Tacoma Plant Annual Electrical Service Contract**

Reviewed: 2026-04-27
Reviewed by: Krasa Electric, LLC (AI-assisted; attorney review required)
Contract type: Annual Service Agreement (industrial)
Annual value (estimated): $890K
State: WA

**Executive Summary**

Overall recommendation: **Acceptable with minor edits**

Top 3 concerns:
1. NDFD clause (§14.3) is **void** under RCW §4.24.360 — flag and strike. Replace with delay-damages reservation.
2. Per-incident liability cap of $250K (§19) is **below** the firm's typical $500K project-cap norm and well below a credible single-incident loss on a 480 V industrial service. Negotiate up to either contract value or insurance limits.
3. 72-hour cyber-incident notification window is workable; but the requirement to "cooperate with Owner's incident-response counsel" needs scoping (max hours, paid time, no waiver of attorney-client privilege).

Count by severity: 🔴 1  🟡 5  🔵 4

**Selected matrix rows:**

| § | Clause Title | Summary | Risk | Why it matters | Suggested redline |
|---|--------------|---------|------|----------------|-------------------|
| §14.3 | No-Damages-For-Delay | Sub waives all delay-cost recovery; sole remedy is time extension. | 🔴 High | NDFD void by statute in WA (RCW §4.24.360). | Strike. Replace with delay-damages reservation tied to change-order procedure (§9). |
| §19 | Limitation of Liability | Per-incident cap of $250K. | 🟡 Medium | A single arc-flash incident on the 480 V switchgear could exceed $250K in property and BI damages alone. | Negotiate to greater of (a) one year's contract value, (b) actual insurance proceeds for the claim. |
| §22 | Cyber-Incident Notification | 72-hour notification + cooperation with Owner's IR counsel. | 🔵 Low | 72 hours is workable; cooperation needs scope. | Add: "Subcontractor's cooperation is limited to (i) providing factual information regarding Subcontractor's systems and (ii) access to Subcontractor's IT logs related to the incident, in each case at Owner's expense if such cooperation exceeds 16 hours of Subcontractor staff time. Subcontractor does not waive attorney-client privilege or work-product protection." |
| §28 | Tariff / Material Escalation | "Material pricing is firm for the Term." | 🟡 Medium | A 3-year service contract with no tariff-aware escalation in a Section-232 environment is unwise. | Cross-reference `material-tariff-escalation-clause-drafter.md` and propose the Annual Price-List Refresh variant. |

**Surety Form Reconciliation:** Not bonded. N/A.

**Insurance Reconciliation:** Required GL $2M / umbrella $5M / cyber $1M / pollution $1M — **all within Krasa's carried limits**. No gap.

**Tariff Awareness:** Contract references material pricing as "firm for the Term" but operates in a Section-232 environment. Recommend pairing with the Annual Price-List Refresh variant from `material-tariff-escalation-clause-drafter.md`.
