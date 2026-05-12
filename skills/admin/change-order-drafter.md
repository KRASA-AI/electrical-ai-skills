---
name: "Change Order Drafter"
category: admin
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~15 min/CO"
version: 2.1
last_eval_score: 9.60
---

# 📄 Change Order Drafter

## Purpose

Draft a complete, signature-ready electrical change order that captures scope change, labor/material cost impact, schedule impact, and code/NEC triggers — in the format GCs, owners, and inspectors expect. Supports lump-sum, unit-price, and T&M (time-and-materials) CO formats and handles both construction-project and service-call contexts.

## When to Use

Use this skill any time the scope of an executed contract, quote, or work order shifts and the change needs to be documented and priced before proceeding — or when T&M work needs to be documented after the fact for invoicing:

- **Owner/GC-directed added scope** — "Add 6 recessed lights in the primary bedroom"
- **Unforeseen conditions** — Concealed K&T discovered, rusted service mast, hidden junction box, existing undersized neutral
- **Code-triggered upgrades** — NEC requires AFCI/GFCI expansion, service disconnect relocation, arc-flash label per 110.16
- **Engineer/architect revisions** — ASI, RFI response, or revised drawing changes a circuit, device count, or panel schedule
- **Owner-deletion or scope-reduction** — Credit back labor/material for removed scope
- **Schedule acceleration / overtime request** — Premium labor to meet a moved milestone
- **Allowance reconciliation** — Actual cost of a fixture package vs. the allowance in the contract
- **Unit-price quantity true-up** — Final device count vs. contract estimate (receptacles, can lights, etc.)
- **Back-charge dispute** — GC has issued a back-charge; sub needs to counter with a CO establishing the correct cost and cause

## Required Input

Provide the following:

1. **Project identifier** — Job name, address, and original contract number (or PO #)
2. **Original contract value & format** — Lump sum? Unit price? T&M cap? Prior COs to date (running total)?
3. **What triggered this CO** — GC directive, RFI response, field condition, code trigger, owner request
4. **Scope delta** — What's being added, changed, or removed (original vs. revised — be specific about counts, locations, device types, circuit impacts)
5. **Labor impact** — Hours by classification (journeyman, apprentice, foreman), or T&M estimate
6. **Material impact** — List of materials with quantities; note if owner-furnished vs. contractor-furnished
7. **Subcontractor impact** (if any) — Low-voltage, controls, trenching, crane
8. **Schedule impact** — Days added / compressed / no impact; dependency on other trades
9. **Code / NEC trigger** (if applicable) — Article/section and code edition
10. **Pricing format requested** — Lump sum, T&M not-to-exceed, unit price, or allowance reconciliation
11. **Deadline for signed approval** (optional)

## Instructions

You are an AI assistant drafting electrical change orders for a licensed contractor. The CO needs to stand up if challenged — meaning it clearly shows what changed, why, what it costs, what it delays, and who authorized it. It also needs to read cleanly enough that a GC's PM will actually sign it instead of parking it.

**Before you start — Config-Driven Addressee Routing Block:**

Load `config.yml` and resolve the CO routing for this project. The routing determines the signature-block structure and the "Approved and Accepted" block layout.

| Config key | What it controls | Default if missing |
|---|---|---|
| `company.legal_name` | Contractor signature block | "[Company Name]" + flag |
| `company.license_no` | Contractor signature block | "[License #]" + flag |
| `co.default_addressee_routing` | Project-type routing (residential / commercial-GC / commercial-owner-direct / public-works) | Prompt user to confirm routing |
| `co.owner_direct_templates` | Signature-block templates for owner-direct projects (no GC intermediary) | Standard two-party block (owner + contractor) |
| `co.gc_templates` | Signature-block templates for GC-channel projects (three parties) | Standard three-party block (GC + owner/owner's rep + contractor) |
| `co.validity_days` | Days the CO is valid for signature | 30 |
| `co.payment_terms` | Net-days for payment of approved COs | 30 |
| `labor_rates.*` | Labor rates by classification | Prompt user for missing rates |
| `material_markup_pct` | Markup on material cost | Prompt user |
| `subcontractor_markup_pct` | Markup on sub costs | Prompt user |
| `overtime_premium` | Overtime multiplier | 1.5 (time-and-a-half) |

**Routing logic by project type:**

- **Residential (owner-direct):** Two-party CO. Signature block: Owner (or homeowner) + Electrical Contractor. "Approved and Accepted" block: one signature pair. Pull `co.owner_direct_templates.residential`.
- **Commercial-GC channel:** Three-party CO. Signature block: GC Project Manager + Owner/Owner's Rep (if required by GC's subcontract) + Electrical Sub. Some GCs issue CO approvals without the owner's signature; check the subcontract. Pull `co.gc_templates.commercial`.
- **Commercial owner-direct:** Two-party CO. Signature block: Owner's Representative/PM + Electrical Contractor (prime). Pull `co.owner_direct_templates.commercial`.
- **Public-works:** Three-party CO in most jurisdictions (owner's agent + prime GC + sub); the contractor's signature follows the prime's approval. Retainage and payment terms per the public-works contract, not the firm's standard. Pull `co.gc_templates.public_works`.

If `config.yml` does not have the routing configured, prompt the user for the project type before drafting the CO.

**Before you start — additional config and references:**
- Reference `knowledge-base/regulations/` for NEC code-edition references (including NEC 2026 adoption timing where applicable)
- Reference `knowledge-base/regulations/material-tariffs-2026.md` for any CO triggered by a Section 232 / 301 / IEEPA Tariff Event (see §5d below)
- Reference `knowledge-base/terminology/` for standard construction-contract terms
- Reference `admin/material-tariff-escalation-clause-drafter.md` cross-reference when producing a Tariff-Event CO variant
- If the original contract is attached, extract the CO numbering convention, signature block, and retainage handling

**Process:**

1. Identify the CO type (added scope / changed scope / deleted scope / unforeseen condition / code trigger / acceleration / allowance reconciliation / unit-price true-up)
2. Number this CO sequentially (e.g., "CO-007" — pick up from the most recent CO provided by the user, or ask)
3. Describe the original scope and the revised scope in parallel clauses (original vs. revised), referencing the contract section, drawing sheet, or RFI number that drove the change
4. Itemize **labor** by classification with hours and rate from config. Show overtime premium separately if applicable.
5. Itemize **materials** by line item with unit cost, quantity, subtotal, and markup (use config markup %)
6. Calculate subcontractor cost (cost + config sub-markup %) if applicable
7. State the **schedule impact** explicitly — even if zero, say "No schedule impact"
8. If code-driven, cite the NEC article/section and code edition and explain *why* the upgrade is required (e.g., "NEC 210.8(F) requires GFCI protection for outdoor outlets — this receptacle was added outside the existing GFCI-protected zone")
9. For T&M COs, include a **not-to-exceed (NTE)** number and a statement that remaining value requires a separate CO
10. Include a **protective language** block: "This CO is valid for X days. Work proceeds only upon written signature. Prices assume no further scope changes or schedule compressions beyond those identified above."
11. Close with a clean signature block for owner/GC PM and sub.

**Pricing format rules:**

- **Lump-sum CO** — Show total only on the signature page, but include the labor/material breakdown on an attached backup sheet so the GC can QC it.
- **T&M (not-to-exceed)** — Show the hourly rates, estimated hours, and NTE cap. State that final billing is actual hours × rate + materials at cost + markup.
- **Unit-price true-up** — Show the unit, contract qty, actual qty, delta, and extended price.
- **Allowance reconciliation** — Show allowance carried, actual invoiced cost, over/(under) allowance.
- **Tariff-Event CO** — Use when a Section 232 / 301 / IEEPA Tariff Event (as defined in the project's tariff escalation rider from `admin/material-tariff-escalation-clause-drafter.md`) triggers a material price adjustment after the proposal or PO date. The Tariff-Event CO has a distinct structure (see §5d in the Output Format template below).

**Calculation conventions:**
- Use config-defined rates. Do not invent rates. If config lacks a rate, prompt the user for it before finalizing.
- Apply material markup on cost, not on list price unless the user says otherwise.
- Overtime premium: time-and-a-half after 8 hrs/day or 40 hrs/week per config setting.
- Bond / insurance uplift: apply if the contract requires it on CO value (flag to user).
- Sales tax: apply on materials per the project's jurisdiction. If unknown, flag for user input.

**What NOT to do:**

- Do not bundle unrelated scope changes into a single CO — each trigger gets its own number for a clean paper trail
- Do not quote NEC articles to a residential homeowner without translating in parallel (see customer-explanation-generator)
- Do not proceed on verbal authorization — the CO template includes a line stating written signature is required before work begins
- Do not hide subcontractor markup inside the labor line
- Do not skip the schedule-impact statement even when it's zero
- Do not accept liability for **existing** conditions in the CO body — describe the field condition found, not who caused it

## Output Format

Produce a complete document in this structure:

```
CHANGE ORDER #[CO-XXX]
Date: [YYYY-MM-DD]
Project: [Job name and address]
Original Contract: [Contract #, dated YYYY-MM-DD, $original value]
Prior COs to date: [count, $running total]
This CO (ref): [RFI # / ASI # / Field directive # / Verbal from NAME on DATE]

1. REASON FOR CHANGE
   [1–3 sentence plain description of what triggered this CO]

2. ORIGINAL SCOPE (relevant portion)
   [What the original contract required for this area of work]

3. REVISED SCOPE
   [What the revised scope is, with specific counts / locations / devices / circuits]

4. CODE / NEC TRIGGER (if applicable)
   NEC [year] Article [###] Section [###]: [plain-English reason]

5. PRICING
   [Format: Lump Sum | T&M NTE $[amount] | Unit Price | Allowance Rec]

   a. Labor
      | Classification | Hours | Rate   | Subtotal |
      | Foreman        | xx    | $xx.xx | $xxx.xx  |
      | Journeyman     | xx    | $xx.xx | $xxx.xx  |
      | Apprentice     | xx    | $xx.xx | $xxx.xx  |
      | OT Premium     | xx    | $xx.xx | $xxx.xx  |
      | Labor subtotal |       |        | $xxx.xx  |

   b. Materials
      | Item                         | Qty | Unit Cost | Extended | Markup @ X% | Total    |
      | [e.g., 20A AFCI breaker]     | x   | $xx.xx    | $xxx.xx  | $xx.xx      | $xxx.xx  |
      | Material subtotal            |     |           |          |             | $xxx.xx  |

   c. Subcontractor (if any)
      | Sub         | Scope  | Cost    | Markup @ X% | Total   |
      | [Sub name]  | [...]  | $xxx.xx | $xx.xx      | $xxx.xx |

   d. Other (permits, bond, tax)
      [Itemized]

   TOTAL THIS CO: $[amount]
   REVISED CONTRACT VALUE: $[original + all prior COs + this CO]

   --- FOR TARIFF-EVENT CO VARIANT ONLY ---

   d. Tariff-Event Price Adjustment (in lieu of or in addition to items a–d above)
      Trigger: [Executive Order / Federal Register citation] — effective [date]
      Affected HTS Category: [e.g., "HTSUS 8537.10 — switchgear and switchboards ≤1000V"]
      Pre-event supplier quote: $[amount], dated [YYYY-MM-DD]
      Post-event supplier re-quote: $[amount], dated [YYYY-MM-DD]
      Price delta: $[delta]
      Contractor markup @ [config markup %]: $[markup]
      Tariff-Event adjustment subtotal: $[adjusted total]
      Substantiation: [Supplier invoice or written re-quote attached — see Attachment A]

   SCHEDULE IMPACT (Tariff-Event CO):
      [Lead-time impact in weeks if the Tariff Event caused a lead-time blowout
       separate from the price adjustment. Distinguish: non-compensable time
       extension for lead-time delay vs. compensable price adjustment for the
       tariff delta. Both may apply; do not merge them into one line.]

6. SCHEDULE IMPACT
   [X additional calendar days / No schedule impact / Accelerates by X days]
   Dependency: [e.g., switchgear 6-wk lead time; crane day locked 4/28]

7. TERMS
   - This CO is valid for signature for [30] days from the date above.
   - Work covered by this CO shall not commence until a signed copy is returned.
   - All other terms of the original contract dated [YYYY-MM-DD] remain in effect.
   - [For T&M only] Final billing reflects actual labor and materials; hours will not exceed the NTE above without a further signed CO.

8. APPROVED & ACCEPTED
   OWNER / GC:
       Signature: _______________________  Date: _________
       Printed name / title: __________________________

   ELECTRICAL CONTRACTOR:
       [Company name], License #[license]
       Signature: _______________________  Date: _________
       Printed name / title: __________________________
```

After the CO body, include a short **Internal Notes** block (not for the signature page) covering:
- Assumptions made (lead times, sales tax jurisdiction, bond uplift, subcontractor quote status)
- Items the user should verify before sending (markup %, classifications, NEC edition adopted locally)
- Attachments recommended (mark-up drawing, RFI response, photo of field condition, supplier quote)

## Example Output

**Input:** Field directive from GC Maple Ridge Builders on the 1422 Maple St service upgrade. Customer wants to add 6 recessed lights in the primary bedroom (originally excluded). Labor: 1 foreman hr + 4 journeyman hrs. Materials: 6 × 4" LED can lights @ $42 ea, 125 ft 14/2 NM @ $0.55/ft, 6 plaster frames @ $8 ea, 1 new 15A AFCI breaker @ $48, misc devices & wire nuts ~$35. Config rates: Foreman $95/hr, Journeyman $82/hr, material markup 22%, no OT. No schedule impact. NEC 2023 adopted locally; AFCI required on bedroom circuits per 210.12.

---

**CHANGE ORDER #CO-007**
Date: 2026-04-14
Project: 1422 Maple St — Service Upgrade & Interior Electrical
Original Contract: TE-2026-0142, dated 2026-03-10, $38,650
Prior COs to date: 6 COs, $4,820
This CO (ref): GC Field Directive from Sarah Liu (Maple Ridge Builders), 2026-04-12

**1. REASON FOR CHANGE**
Owner has directed the addition of six (6) recessed LED downlights in the primary bedroom. This scope was not included in the original contract (original scope called for two ceiling-fan boxes only).

**2. ORIGINAL SCOPE (relevant portion)**
Primary bedroom: (2) paddle-fan/light combination boxes with switch legs, on existing bedroom circuit. No recessed lighting included.

**3. REVISED SCOPE**
Primary bedroom: (2) paddle-fan/light combination boxes *as previously scoped* PLUS (6) new 4" LED recessed downlights on a new dedicated 15A AFCI circuit. New circuit originates at Panel P-1 (space available — breaker 18). Switching via a new 3-way pair at the existing switch wall and entry wall. Dimmer-compatible LED drivers specified. Rough-in to align with existing bedroom drywall close-in date of 4/25.

**4. CODE / NEC TRIGGER**
NEC 2023, Article 210.12(A): Branch circuits supplying outlets or devices in bedrooms in dwelling units shall be protected by a listed arc-fault circuit interrupter. New 15A dedicated circuit will be AFCI-protected via a new AFCI breaker at P-1.

**5. PRICING (Lump Sum)**

**a. Labor**

| Classification | Hours | Rate    | Subtotal |
|---|---|---|---|
| Foreman        | 1     | $95.00  | $95.00   |
| Journeyman     | 4     | $82.00  | $328.00  |
| **Labor subtotal** |   |         | **$423.00** |

**b. Materials**

| Item                                | Qty   | Unit Cost | Extended | Markup @ 22% | Total    |
|---|---|---|---|---|---|
| 4" LED recessed downlight (IC-rated)| 6     | $42.00    | $252.00  | $55.44       | $307.44  |
| 14/2 NM-B cable                     | 125 ft| $0.55     | $68.75   | $15.13       | $83.88   |
| Plaster / retrofit frames           | 6     | $8.00     | $48.00   | $10.56       | $58.56   |
| 15A AFCI breaker (to match panel)   | 1     | $48.00    | $48.00   | $10.56       | $58.56   |
| 3-way switches + plates             | 2     | $14.00    | $28.00   | $6.16        | $34.16   |
| Misc devices / wire nuts / staples  | 1 lot | $35.00    | $35.00   | $7.70        | $42.70   |
| **Material subtotal**               |       |           |          |              | **$585.30** |

**c. Other**
- Sales tax on materials @ [confirm jurisdiction rate] — **flagged for user input**
- No bond uplift (below CO threshold in original contract)

**TOTAL THIS CO:** $1,008.30 (excluding sales tax — see Internal Notes)
**REVISED CONTRACT VALUE:** $44,478.30 (original $38,650 + prior COs $4,820 + this CO $1,008.30)

**6. SCHEDULE IMPACT**
No schedule impact. Rough-in for the added recessed lights will align with existing west-wall drywall close-in date (4/25). Work will be sequenced into the already-scheduled 4/22 rough-in day.

**7. TERMS**
- This CO is valid for signature for 30 days from the date above.
- Work covered by this CO shall not commence until a signed copy is returned.
- All other terms of the original contract dated 2026-03-10 remain in effect.

**8. APPROVED & ACCEPTED**

OWNER / GC — Maple Ridge Builders:
Signature: _______________________  Date: _________
Printed name / title: __________________________

ELECTRICAL CONTRACTOR — Torres Electric, LLC, License #EC-00842:
Signature: _______________________  Date: _________
Printed name / title: Mike Torres, Owner

---

**Internal Notes**
- Sales tax deliberately left as a line item flag — confirm the jurisdictional rate before sending (primary bedroom is still in [City], so [X]% is expected).
- AFCI breaker matches existing panel brand (Eaton BR) — confirm with foreman before material pull.
- Assumed dimmer-compatible drivers are acceptable; verify homeowner's dimmer preference (standard vs. low-voltage-compatible) before ordering to avoid a callback.
- Recommend attaching: photo of panel P-1 with breaker 18 space highlighted, LED spec sheet, sketch of can-light layout stamped by Mark (foreman).
- Prior COs to date are assumed — confirm the running total with PM before issuing CO-007.

---

## Example 2 — Tariff-Event Price Adjustment CO (Commercial, Post-Section 232 Switchboard, Cleveland OH)

**Input:** Drysdale Electric, Westmoreland MOB project (Sub #26-118), 14201 W Halsey Ave, Cleveland OH. Original contract TE-MOB-2025-118, dated 2025-12-01, $2,847,000. Prior COs: 10, $88,450 running total. This CO: the April 6, 2026 Section 232 Executive Order restructure impacted the 4000 A 480/277 V 3Φ 4W switchboard SB-MAIN on the BoM. Eaton re-quoted the switchboard from $284,000 (PO release Feb 14, 2026) to $312,000 (re-quote April 14, 2026, citing Section 232 tariff increase on HTSUS 8537.20 — switchgear assemblies >1000 V and associated components). Delta = $28,000. Config: Drysdale Electric, license OH-EC-0031892; material markup 18%; no OT. The project's tariff escalation rider (from `admin/material-tariff-escalation-clause-drafter.md`) is attached to the original sub-bid cover. Schedule impact: Eaton's revised delivery date is 6/22/2026 (up from 6/8/2026), a 2-week slip — non-compensable time extension for the lead-time blowout, separate from the price adjustment. Routing: commercial GC channel (Westmoreland Builders, GC PM Tom Reyes; owner's rep Karen Lai co-approves per subcontract §12.3).

---

**CHANGE ORDER #CO-011**
Date: 2026-04-16
Project: Westmoreland MOB — 14201 W Halsey Ave, Cleveland OH 44109
Original Contract: TE-MOB-2025-118, dated 2025-12-01, $2,847,000
Prior COs to date: 10 COs, $88,450
This CO (ref): Section 232 Tariff Event — EO dated April 6, 2026 / Eaton re-quote April 14, 2026 (Attachment A)

**1. REASON FOR CHANGE**
On April 6, 2026, the President signed an Executive Order restructuring Section 232 duties on steel, aluminum, and related electrical products under HTSUS 8537.20 (switchgear assemblies >1000 V, components). The post-event Section 232 tariff rate on the SB-MAIN switchboard assembly increased the Eaton invoice cost from $284,000 (February 14, 2026 PO-release price) to $312,000 (April 14, 2026 re-quote). This CO documents the pass-through of the Tariff Event price delta per Section 4 of the Tariff & Escalation Rider attached to the sub-bid cover (TE-MOB-2025-118-RIDER, dated 2025-12-01).

**2. ORIGINAL SCOPE (relevant portion)**
Division 26-13: furnish and install 4000 A 480/277 V 3Φ 4W main service switchboard SB-MAIN per spec section 26 13 00 and the Eaton Pow-R-Line C submittal approved 2026-02-05. PO released to Eaton 2026-02-14 at the then-current quote of $284,000.

**3. REVISED SCOPE**
No scope change. Same equipment, same spec, same approved submittal. CO documents the price-only adjustment triggered by the April 6, 2026 Tariff Event per the escalation rider.

**4. CODE / NEC TRIGGER**
Not applicable. This CO is a contractual price-escalation event, not a code-triggered scope change.

**5. PRICING — Tariff-Event Price Adjustment**

| Item | Detail | Amount |
|---|---|---|
| Pre-event Eaton invoice (PO-release price, 2026-02-14) | SB-MAIN — 4000 A 480/277 V 3Φ 4W switchboard | $284,000.00 |
| Post-event Eaton re-quote (2026-04-14) | Same equipment, post-Section 232 tariff | $312,000.00 |
| Tariff-Event price delta | $312,000 − $284,000 | $28,000.00 |
| Contractor markup @ 18% | Per config and escalation rider §4 | $5,040.00 |
| **Tariff-Event adjustment subtotal** | | **$33,040.00** |
| **TOTAL THIS CO** | | **$33,040.00** |
| **REVISED CONTRACT VALUE** | $2,847,000 + $88,450 + $33,040 | **$2,968,490.00** |

Substantiation: Eaton written re-quote dated 2026-04-14 and Section 232 tariff notice attached as Attachment A. Original PO release confirmation attached as Attachment B.

**6. SCHEDULE IMPACT**
Eaton's revised delivery date is **June 22, 2026** (previously June 8, 2026 per the approved submittal long-lead schedule). The 2-week lead-time slip is a **non-compensable time extension** per the escalation rider §5 — no additional compensable delay costs are claimed in this CO; the schedule extension follows the delivery date. Crane day and energization sequencing to be updated in the project schedule by Westmoreland Builders following CO-011 execution.

This CO is limited to the Tariff-Event price adjustment. No other schedule claims are waived by execution of this CO.

**7. TERMS**
- This CO is valid for signature for 30 days from the date above.
- Work covered by this CO is the price adjustment on an already-issued PO; work proceeds per the approved submittal.
- All other terms of the original contract dated 2025-12-01 and the escalation rider remain in effect.
- This CO does not constitute a waiver of any future Tariff-Event claims if the Section 232 tariff rate changes again before the delivery date.

**8. APPROVED & ACCEPTED**

OWNER'S REPRESENTATIVE — Westmoreland Health (co-approval per Subcontract §12.3):
Signature: _______________________  Date: _________
Printed name / title: Karen Lai, Owner's Rep

GC — Westmoreland Builders:
Signature: _______________________  Date: _________
Printed name / title: Tom Reyes, Project Manager

ELECTRICAL SUBCONTRACTOR — Drysdale Electric LLC, License #OH-EC-0031892:
Signature: _______________________  Date: _________
Printed name / title: Mike Drysdale, Owner

---

**Internal Notes**
- Three-party CO per Subcontract §12.3 — owner's rep Karen Lai co-approves alongside GC PM Tom Reyes. Do NOT issue a two-party version without confirming Karen Lai's approval is still required.
- Markup rate of 18% is per `config.yml.material_markup_pct` and is consistent with the escalation rider §4 pass-through language.
- Future tariff-event exposure: If Section 232 rates change again before the June 22 delivery date, a separate Tariff-Event CO (CO-012 or subsequent) would be needed. The CO-011 terms explicitly preserve future claims.
- Recommend attaching: (Attachment A) Eaton re-quote 2026-04-14; (Attachment B) Eaton PO-release confirmation 2026-02-14; (Attachment C) the project escalation rider TE-MOB-2025-118-RIDER for GC/owner's rep reference.
- Cross-reference: `admin/material-tariff-escalation-clause-drafter.md` for the rider language; `customer-service/project-delay-communicator.md` if Karen Lai needs a formal delay notification letter for the 2-week slip in addition to this CO.

---

## Example 3 — AHJ-Driven Code Trigger CO (NEC 2026 Arc-Flash Label Upgrade, Portland OR)

**Input:** Torres Electric, 1422 Maple St, Portland OR, service upgrade project. Original contract TE-2026-0142, dated 2026-03-10, $38,650. Prior COs: 7 COs, $5,828.30 ($4,820 prior + CO-007 $1,008.30). This CO: at the final inspection on 2026-04-24, City of Portland BDS Inspector Kowalski cited a correction: the two arc-flash warning labels on SWBD-1 (service equipment) and MCC-B (feeder-supplied) do not include the date of assessment as required by NEC 2026 §110.16(B) and (C), which Portland adopted 2026-04-01. The original labels were printed before the 2026 adoption and are §110.16 (2023 cycle) compliant but not §110.16 (2026 cycle) compliant. Torres Electric will obtain the assessment date from the firm that did the arc-flash study (Northgate Engineering), re-label both pieces of equipment, and schedule a re-inspection. Labor: 2 journeyman hours. Materials: 2 × Brady B-534 5×7 labels @ $4.50 ea (reprinting cost). Config: Torres Electric, license EC-00842; Journeyman $82/hr; material markup 22%; residential owner-direct routing (homeowner: Sarah Chen). No OT. NEC 2026 applies. Assessment date to be confirmed with Northgate Engineering before labels are printed.

---

**CHANGE ORDER #CO-008**
Date: 2026-04-25
Project: 1422 Maple St — Service Upgrade & Interior Electrical
Original Contract: TE-2026-0142, dated 2026-03-10, $38,650
Prior COs to date: 7 COs, $5,828.30
This CO (ref): City of Portland BDS Final Inspection Correction — Inspector Kowalski, 2026-04-24 (Permit 2026-04-EL-1142)

**1. REASON FOR CHANGE**
At the April 24, 2026 final inspection, Inspector Kowalski cited that both arc-flash labels on SWBD-1 and MCC-B do not include the date of assessment as required by NEC 2026 §110.16(B) and §110.16(C). The original labels were produced under the NEC 2023 cycle before Portland adopted NEC 2026 on April 1, 2026. This CO covers the labor to obtain the assessment date from Northgate Engineering, re-print compliant Brady B-534 labels, and re-label both pieces of equipment before the re-inspection.

**2. ORIGINAL SCOPE (relevant portion)**
Service upgrade scope included arc-flash labeling on SWBD-1 (service equipment, 200 A, 120/240V) and MCC-B (feeder-supplied motor control, 240V) per the arc-flash study performed by Northgate Engineering dated 2026-01-15, per NEC 2023 §110.16 as adopted at time of permit issuance.

**3. REVISED SCOPE**
Re-label SWBD-1 and MCC-B with NEC 2026 §110.16(B)/(C)-compliant labels including the assessment date (2026-01-15 per Northgate Engineering study NG-2026-0115). No change to the assessment values (incident energy, PPE category, boundary) — the assessment itself is current; only the label format is being corrected.

**4. CODE / NEC TRIGGER**
NEC 2026 §110.16(B): Service equipment in non-dwelling occupancies likely to require examination or servicing while energized shall be field marked with a label that includes the date the arc flash risk assessment was completed.
NEC 2026 §110.16(C): Same requirement applies to feeder-supplied equipment (panelboards, switchboards, industrial control panels, motor control centers, meter sockets) in non-dwelling occupancies.
City of Portland adopted NEC 2026 effective April 1, 2026; this project's final inspection falls under the 2026 cycle.

**5. PRICING (Lump Sum)**

**a. Labor**

| Classification | Hours | Rate    | Subtotal |
|---|---|---|---|
| Journeyman     | 2     | $82.00  | $164.00  |
| **Labor subtotal** |   |         | **$164.00** |

**b. Materials**

| Item | Qty | Unit Cost | Extended | Markup @ 22% | Total   |
|---|---|---|---|---|---|
| Brady B-534 5×7 label (reprint, NEC 2026 compliant) | 2 | $4.50 | $9.00 | $1.98 | $10.98 |
| **Material subtotal** | | | | | **$10.98** |

**TOTAL THIS CO:** $174.98
**REVISED CONTRACT VALUE:** $44,653.28 (original $38,650 + prior COs $5,828.30 + this CO $174.98)

**6. SCHEDULE IMPACT**
Re-inspection scheduled with BDS upon execution. Assessment date confirmation with Northgate Engineering expected within 1 business day. Re-labeling: 2 hours on site. Re-inspection: next available BDS window (typically 3–5 business days from request). No impact to any other project milestone.

**7. TERMS**
- This CO is valid for signature for 30 days from the date above.
- Re-labeling work will not commence until (a) assessment date is confirmed in writing by Northgate Engineering and (b) a signed copy of this CO is returned.
- All other terms of the original contract dated 2026-03-10 remain in effect.

**8. APPROVED & ACCEPTED**

OWNER — 1422 Maple St:
Signature: _______________________  Date: _________
Printed name / title: Sarah Chen

ELECTRICAL CONTRACTOR — Torres Electric, LLC, License #EC-00842:
Signature: _______________________  Date: _________
Printed name / title: Mike Torres, Owner

---

**Internal Notes**
- Two-party residential owner-direct CO (config routing: `co.owner_direct_templates.residential`). No GC in the chain.
- Assessment date (2026-01-15) must be confirmed in writing from Northgate Engineering before the label is printed. Do not assume the date from the study report without written confirmation — the label is a legal document.
- Cross-reference: `skills/operations/arc-flash-label-generator.md v1.1` to produce the corrected label text with the assessment-date line before printing. The Labeling Preferences block will pull the correct Brady B-534 5×7 spec from config automatically.
- Portland BDS cycle: NEC 2026 adopted 2026-04-01 (KB verified). Inspector Kowalski's citation is valid.
- This CO is de minimis ($174.98) but creates the documented paper trail showing the correction was made and billed separately — not absorbed into the original contract — so the firm can reference it if a warranty dispute arises later.

---

## Example 4 — T&M CO with Not-to-Exceed Cap (Unforeseen K&T Wiring, Kitchen Remodel)

**Input:** Lone Star Power Systems, Keisha and Marcus Webb residence, 1804 Creekview Ct, Denver CO 80231. Original contract LS-2026-0214, dated 2026-04-01, $14,800 (kitchen remodel electrical scope: new 20A circuits for island, hood vent, dishwasher, refrigerator; new GFCI circuits per NEC 2026). Prior COs: 2 COs, $1,200. This CO: during rough-in, the crew opened the west wall and found original knob-and-tube (K&T) wiring behind lath-and-plaster, mixed with a 1960s renovation splice — not identifiable from the permit drawings or the initial walkthrough. The K&T must be removed and replaced before the new circuits can be safely terminated. The scope of K&T is unknown until the lath is fully opened; T&M with a not-to-exceed (NTE) cap is the appropriate format. Estimated hours: Foreman 2 hrs + Journeyman 8 hrs; NTE total labor $960. Material estimate: 75 ft 14/2 NM-B @ $0.55/ft, 1 wire nut bag $12, electrical tape lot $8. Config: Lone Star Power Systems, license CO-EL-00788; Foreman $95/hr, Journeyman $82/hr; material markup 22%; residential owner-direct routing. No OT assumed. NEC 2026 adopted Denver CO 2026-01-01.

---

**CHANGE ORDER #CO-003**
Date: 2026-05-02
Project: 1804 Creekview Ct, Denver CO — Kitchen Remodel Electrical
Original Contract: LS-2026-0214, dated 2026-04-01, $14,800
Prior COs to date: 2 COs, $1,200
This CO (ref): Unforeseen Field Condition — K&T Wiring Behind West Wall (found during rough-in 2026-04-30)

**1. REASON FOR CHANGE**
During rough-in on April 30, 2026, the crew opened the west kitchen wall and discovered original knob-and-tube (K&T) wiring mixed with a 1960s junction-box splice behind lath-and-plaster. This condition was not visible during the pre-job walkthrough and is not shown on any permit drawings. The K&T must be removed and the affected circuit section replaced with NM-B before the new kitchen circuits can be safely terminated. The full extent of the K&T run is unknown until the lath is fully opened; this CO is therefore T&M with a not-to-exceed cap. Work will halt and a supplemental CO will be issued if the NTE cap is approached.

**2. ORIGINAL SCOPE (relevant portion)**
Kitchen remodel scope: new 20A circuits for island, hood vent, dishwasher, and refrigerator. Assumed all existing wiring in the walls was NM-B or armored cable per the 1990s-era renovation disclosed at contract signing. No K&T removal was included.

**3. REVISED SCOPE**
Remove discovered K&T wiring in the west kitchen wall (and any extension found behind the adjacent north wall during the same opening); replace with 14/2 NM-B; safely cap any live K&T ends not being replaced in this scope. Existing circuits affected: circuits 6 and 8 on Panel P-1 (verified by trace on 2026-04-30). Work to coordinate with drywall contractor's close-in date of 2026-05-08.

**4. CODE / NEC TRIGGER**
NEC 2026 §394.10: Knob-and-tube wiring shall not be used in commercial garages, theaters, motion-picture studios, or hazardous locations. The existing K&T is in a dwelling and not strictly prohibited from remaining in place; however, NEC 2026 §110.12 requires electrical installations to be made in a neat and workmanlike manner, and the mixed K&T-plus-1960s-splice condition creates a fire and continuity risk that the qualified electrician has documented on site. Removal and replacement is required by the contractor's professional judgment and disclosed in writing to the homeowner. No code citation commands removal; removal is the appropriate corrective action given the site condition.

**5. PRICING — T&M Not-to-Exceed $1,550**

**a. Labor (T&M actual hours × rate — NTE $960)**

| Classification | Est. Hours | Rate    | Est. Subtotal |
|---|---|---|---|
| Foreman        | 2          | $95.00  | $190.00       |
| Journeyman     | 8          | $82.00  | $656.00       |
| **Labor NTE**  |            |         | **$960.00**   |

Final billing reflects actual hours × rate. Hours will not exceed the NTE above without a further signed CO.

**b. Materials (T&M actual cost + markup — NTE $590)**

| Item | Est. Qty | Unit Cost | Extended | Markup @ 22% | Total   |
|---|---|---|---|---|---|
| 14/2 NM-B cable | 75 ft | $0.55 | $41.25 | $9.08 | $50.33 |
| Wire nuts / lot | 1 | $12.00 | $12.00 | $2.64 | $14.64 |
| Electrical tape / lot | 1 | $8.00 | $8.00 | $1.76 | $9.76 |
| **Material NTE** | | | | | **$74.73 (estimate)** |

Actual material cost billed at cost + 22% markup; NTE on materials is $590 for the K&T scope (estimate includes contingency for additional wire if the K&T run extends into the north wall).

**TOTAL THIS CO (NTE): $1,550.00**
**REVISED CONTRACT VALUE:** $17,550.00 (original $14,800 + prior COs $1,200 + this CO NTE $1,550)

Actual CO value may be less than the NTE if the K&T scope is smaller than estimated.

**6. SCHEDULE IMPACT**
No schedule impact to the kitchen close-in date of 2026-05-08. The K&T removal is scoped to fit within the existing rough-in window. If the K&T extends significantly beyond the west wall, a schedule notification will be issued before close-in.

**7. TERMS**
- This CO is valid for signature for 30 days from the date above.
- Work covered by this CO shall not commence until a signed copy is returned.
- Final billing reflects actual hours × rate + actual materials at cost + markup, not to exceed the NTE above. Any overage requires a further signed CO before additional work proceeds.
- All other terms of the original contract dated 2026-04-01 remain in effect.

**8. APPROVED & ACCEPTED**

OWNER — 1804 Creekview Ct:
Signature: _______________________  Date: _________
Printed name / title: Keisha and Marcus Webb

ELECTRICAL CONTRACTOR — Lone Star Power Systems, License #CO-EL-00788:
Signature: _______________________  Date: _________
Printed name / title: Jordan Reyes, Owner

---

**Internal Notes**
- T&M NTE format selected because the K&T scope is genuinely unknown until the lath is fully opened. The NTE cap ($1,550) is protective for the homeowner; the contractor stops at the cap and issues a supplemental CO for any additional scope.
- K&T removal is not strictly code-mandated in this dwelling context (NEC 2026 §394.10 does not require dwelling K&T removal). CO §4 is transparent about this — the removal is a professional judgment call, not a mandatory code trigger. This is the correct framing; do not over-cite the code.
- Recommend attaching: (a) photo of west-wall K&T condition (foreman-documented 2026-04-30); (b) brief sketch showing the K&T run location relative to the new kitchen circuits.
- Close-in date 2026-05-08 is an existing contract milestone. Flag to foreman if the K&T run extends into the north wall and threatens the 5/8 date.
- Config routing: `co.owner_direct_templates.residential` — two-party CO (Keisha and Marcus Webb + Lone Star Power Systems). Both homeowners sign because both are on title.
