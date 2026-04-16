---
name: "Change Order Drafter"
category: admin
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~15 min/CO"
version: 2.0
last_eval_score: 4.30
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

**Before you start:**
- Load `config.yml` for company legal name, license #, state, contact, **labor rates by classification**, **material markup %**, **overtime premium**, and **standard CO terms** (payment timing, validity window, waiver language)
- Reference `knowledge-base/regulations/` for NEC code-edition references (including NEC 2026 adoption timing where applicable)
- Reference `knowledge-base/terminology/` for standard construction-contract terms
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
