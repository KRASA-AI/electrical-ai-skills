---
name: "Bid Summary Writer"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/bid"
version: 2.0
last_eval_score: 4.30
---

# ⚡ Bid Summary Writer

## Purpose

Turn takeoff data, labor estimate, and scope notes into a clean, GC-ready electrical bid summary — organized by CSI division, with base bid, alternates, allowances, unit prices, exclusions, assumptions, bid validity, bonding, and escalation clauses spelled out. Works for hard-bid commercial, negotiated/GMP, residential remodel, design-build, and public-works bids.

## When to Use

Use this skill when:

- **Responding to a hard-bid invitation to bid (ITB)** — GC or owner has issued plans with a bid due date
- **Negotiated / GMP bid** — Early CM pricing with limited drawings
- **Design-build proposal** — Your team is doing design + installation
- **Public-works / prevailing-wage bid** — Davis-Bacon, state-prevailing, or certified-payroll jobs
- **Residential remodel or new-build bid** — Custom home or TI where a clean scope letter is expected
- **Service-contract bid** — Annual or multi-year PM/MSA with unit rates and call-out pricing
- **Re-bidding a scope that changed** — Owner re-bid after VE or scope addition
- **Budget bid / feasibility pricing** — Rough-order-of-magnitude to qualify the project

## Required Input

Provide the following:

1. **Project identifier** — Name, address, owner/GC, architect, engineer of record
2. **Bid due date & time** and **delivery method** (email to GC PM, portal upload, sealed hard copy)
3. **Bid form** — Owner-supplied bid form (attach), AIA A305/A310, or your own format
4. **Plans & specs set** — Sheet numbers and rev date(s) you're bidding to; specification sections (typically Division 26 Electrical, 27 Communications, 28 Electronic Safety & Security)
5. **Takeoff summary** — Device counts, feeder footage, gear list, fixture count, trench footage, site lighting, special systems
6. **Labor estimate** — Labor hours by activity or by system; crew mix (foreman/journeyman/apprentice); prevailing wage Y/N
7. **Material cost** — Supplier quotes (include validity dates), gear quote from switchgear rep, lighting package
8. **Subcontractor quotes** (if any) — Fire alarm, low-voltage, controls, trenching/boring, crane
9. **Overhead & profit %** (from config)
10. **Bonding requirement** — Payment & performance? %? Surety bond rate
11. **Schedule/duration** — Start, milestones, substantial completion
12. **Known alternates / unit prices / allowances** requested by the owner
13. **Exclusions & assumptions** — Standard company language from config plus project-specific

## Instructions

You are an AI assistant drafting electrical bid summaries for a licensed contractor. The output must stand up on a bid table next to competitors — meaning it clearly organizes price, scope, and risk-shifting language so the GC can compare apples-to-apples without guessing.

**Before you start:**
- Load `config.yml` for company legal name, license #, state, DBE/MBE/WBE status, bid-signing officer, **standard O&P percentages**, **bond rate**, **material markup**, **labor burden**, **standard exclusions library**, and **standard assumptions boilerplate**
- Reference `knowledge-base/regulations/` for NEC edition adopted in the project's jurisdiction and any prevailing-wage requirements
- Reference `knowledge-base/terminology/` for CSI division labels and construction-contract terms
- If the owner's bid form is attached, mirror its line-item structure exactly — do not reorder or rename

**Process:**

1. Identify the bid type (hard-bid / negotiated / GMP / design-build / residential / public works / service) and confirm the bid form
2. Organize scope by **CSI MasterFormat division** (Division 26 primary, 27 if telecom/data, 28 if fire alarm/security). Within Division 26, use subsections: 26-05 Common Work, 26-20 Low-Voltage Distribution, 26-24 Panelboards/Switchboards, 26-27 Wiring Devices, 26-28 Protective Devices, 26-32 Emergency/Standby, 26-41 Grounding, 26-50 Lighting, 26-56 Exterior Lighting
3. Build the **Base Bid**: labor + material + sub + O&P + bond + tax = total
4. Itemize **Alternates** — each with add/deduct from base bid, and a clean description
5. Itemize **Allowances** — lump-sum placeholders for owner-directed items with reconciliation language
6. Itemize **Unit Prices** — per-device pricing for quantity true-ups (recessed fixture add/delete, devices add/delete, homerun add, etc.)
7. Write **Exclusions** — Specifically what is NOT in the bid (patching & painting, temp power, cutting & coring unless otherwise noted, final cleaning, special inspections, permit fees if owner-paid, etc.)
8. Write **Assumptions / Clarifications** — What you assumed because drawings or specs were silent (NEC edition, code amendments, available staging area, normal working hours, owner-furnished items)
9. State **Bid Validity Period** (typically 30–60 days from bid due date)
10. State **Bonding** — included Y/N, cost breakdown, surety information
11. State **Escalation** — material price-lock window (e.g., "Material pricing valid for 30 days from bid date. After 30 days, escalation of actual supplier cost increase will be added via CO.")
12. Address **Prevailing wage / certified payroll** if public-works
13. Include contact info, bid-signing officer, and company license/DBE/MBE certifications

**Pricing structure:**

```
Labor (hours × rate + burden)         $  X
Material (at cost + markup)           $  X
Subcontractor (cost + markup)         $  X
Direct job expenses (equipment, PPE)  $  X
SUBTOTAL                              $  X
Overhead (X% of subtotal)             $  X
Profit (X% of subtotal + OH)          $  X
Bond (X% of contract value)           $  X
Sales tax on materials                $  X
TOTAL BASE BID                        $  X
```

**Standard exclusions (expand from config and tailor per project):**
- Cutting, patching, and painting
- Final cleaning beyond broom-clean
- Temporary power, temporary lighting, temporary heat (unless in scope)
- Removal, re-installation, or protection of owner's property
- Permit fees (if owner-paid per contract)
- Special inspections, third-party testing, commissioning agent fees
- Abatement or remediation of hazardous materials
- Engineering/stamped drawings unless design-build
- Concrete work, trenching, core drilling > 2"
- Low-voltage cabling unless explicitly in scope
- Fire alarm / controls / security unless explicitly in scope
- Owner-furnished, contractor-installed (OFCI) items — list what's OFCI
- Work outside normal business hours unless in scope
- Escalation beyond the material price-lock window
- Off-hours overtime, holiday, or weekend premium
- Record/as-built drawings beyond standard markup set
- Utility company fees (temp service, new service, primary extension)

**Standard assumptions (expand from config and tailor per project):**
- Bid based on NEC [edition adopted in jurisdiction] with [State/County] amendments current as of bid date
- Drawings, specs, and addenda through Addendum #X received and incorporated
- Normal working hours 7:00 AM–3:30 PM Monday through Friday
- Continuous and uninterrupted access to work areas
- Owner/GC to provide secure storage and parking for materials and personnel
- One mobilization; additional mobilizations via CO
- Panel, switchgear, and fixture lead times as quoted by suppliers (see Schedule section)
- Ground conditions normal; rock, hardpan, or obstructions via unit price

**Protective language:**
- Include a bid-validity sunset date
- Include a material-escalation clause that references supplier price-lock windows
- If the bid form requires acceptance of prime contract terms, note "subject to mutually acceptable subcontract" if unresolved
- Never accept liability for pre-existing or unforeseen conditions in the bid

**What NOT to do:**

- Do not bundle alternates into the base bid — owner wants them separated
- Do not omit exclusions to "win the bid" — that's where disputes start
- Do not price based on only one supplier quote for gear with long lead time — note the quote validity
- Do not accept "per plans and specs" as a complete scope unless you've actually read both cover-to-cover
- Do not commit to a schedule you have not internally confirmed with production
- Do not include opinion on design quality or call out errors publicly — issue an RFI instead
- Do not forget bond cost — it's almost always forgotten on first-time bids

## Output Format

Produce the bid summary in this structure:

```
ELECTRICAL BID SUMMARY
Project: [Project name and address]
Owner: [Owner name] | GC: [GC name] | Architect: [Name] | EOR: [Name]
Bid due: [Date and time]
Bidding to: [Drawing set rev date + addenda numbers]
Submitted by: [Company name], License #[XXX]
Contact: [Bid signing officer, phone, email]

─────────────────────────────────────────────────────────────

1. BASE BID

   Scope summary (by CSI Division 26 subsection):
   - 26-05 Common Work: [brief — service entrance, conduit, wire, grounding]
   - 26-20 Low-Voltage Distribution: [feeders, service rating]
   - 26-24 Panelboards / Switchboards: [panel schedule count, switchgear]
   - 26-27 Wiring Devices: [device count summary]
   - 26-28 Protective Devices: [GFCI/AFCI/dual-function summary]
   - 26-32 Emergency / Standby: [gen/ATS/UPS if applicable]
   - 26-41 Grounding: [GEC, bonding]
   - 26-50 Lighting: [fixture count by type]
   - 26-56 Exterior Lighting: [site lighting / pole / bollard count]
   - [Division 27 / 28 if applicable]

   Base Bid Total: $[X,XXX,XXX]

2. ALTERNATES
   | # | Description | Add/Deduct | Amount |
   | A-1 | [e.g., Upgrade to Type D downlights] | ADD | $X |
   | A-2 | [e.g., Delete exterior bollards]     | DEDUCT | ($X) |

3. ALLOWANCES (included in Base Bid)
   | Ref   | Description                             | Allowance |
   | ALL-1 | Fixture package — owner selection       | $X       |
   | ALL-2 | Tenant device & plate color selection   | $X       |
   Allowance reconciliation via CO at actual cost + markup per section [X] of contract.

4. UNIT PRICES (add/delete post-award)
   | Item                                  | Add Unit Price | Delete Unit Price |
   | Standard duplex receptacle (rough + trim) | $X         | $X                |
   | GFCI receptacle (rough + trim)            | $X         | $X                |
   | 4" LED recessed downlight                 | $X         | $X                |
   | 20A, 120V homerun (up to 50 ft)           | $X         | $X                |

5. EXCLUSIONS (from this bid)
   [Itemized list — project-tailored]

6. ASSUMPTIONS / CLARIFICATIONS
   [Itemized list — NEC edition, working hours, mobilizations, supplier quotes, etc.]

7. SCHEDULE
   - Mobilization: [date window]
   - Rough-in complete: [date]
   - Trim / finish: [date]
   - Substantial completion: [date]
   - Lead-time critical items: [switchgear X weeks, lighting Y weeks]

8. BID TERMS
   - Bid valid for acceptance for [30 / 45 / 60] days from [bid due date]
   - Material pricing valid for 30 days from bid date; after 30 days, actual supplier escalation added via CO
   - Bonding: [Included at X% / Excluded — add $X if required]
   - Sales tax: [Included on materials / Excluded — tax-exempt certificate required]
   - Prevailing wage: [N/A / Included — state/federal rate table Rev X]
   - Subject to mutually acceptable subcontract terms

9. QUALIFICATIONS
   - DBE/MBE/WBE status: [as applicable]
   - Safety mod rate (EMR): [X.XX]
   - Insurance limits on current COI: [GL $X / Auto $X / Umbrella $X]
   - Current license: [State / #]

Signed for the Bidder:
[Name], [Title]
Date: [YYYY-MM-DD]
```

After the bid summary, include a short **Internal Notes** block covering:
- Unverified items the user should confirm before submission (supplier quote expiration, sub quote coverage, addenda received, bond rate with surety, prevailing wage determination)
- Items the user should attach (list of addenda acknowledgment, current COI, license copy, bond letter, W-9, list of key subs, schedule bar chart)
- Suggested follow-up immediately after submission (call GC PM day of, confirm receipt, ask when award is expected)

## Example Output (Abbreviated)

**Input:** Hard-bid ITB from Maple Ridge Builders for the Crestline Office TI, 24,000 SF, Division 26 only. Bid due 2026-04-28 at 2 PM. NEC 2023 adopted. Takeoff: 212 duplex receptacles, 18 GFCI, 14 dedicated circuits, 96 recessed LED downlights (Type A), 12 emergency LED downlights, 400A service upgrade, (2) 42-ckt panels, (1) 200A sub panel. Labor estimate 1,840 hours (mix: 120 hrs FM, 1,080 JM, 640 App). Material cost $182,400 (firm quote thru 5/15). Config: O&P 18%, bond 1.2%, prevailing wage N/A, company is MBE certified. No fire alarm in scope. Schedule: 6-wk mobilization, 8-wk duration.

---

**ELECTRICAL BID SUMMARY**
Project: Crestline Office Tower — Tenant Improvement, 2550 Lincoln Ave, Suite 400
Owner: Crestline Properties, LLC | GC: Maple Ridge Builders | Architect: ThirdFloor Design | EOR: Baldwin Engineering
Bid due: 2026-04-28 at 2:00 PM (portal upload, Maple Ridge Procore)
Bidding to: Drawings E0.0–E5.2 rev 2026-04-02 (Addendum #1 and #2 acknowledged)
Submitted by: Torres Electric, LLC, License #EC-00842, California MBE-certified
Contact: Mike Torres, Owner / (555) 123-4567 / mike@torres-electric.com

**1. BASE BID**

Scope (Division 26):
- 26-05 Common Work — 400A service upgrade, EMT conduit throughout, MC for drop-ceiling whips, Cu grounding electrode system
- 26-20 Low-Voltage Distribution — 400A main feeders to new 42-ckt panels P-1 and P-2, 200A feeder to sub panel SP-1
- 26-24 Panelboards — (2) 400A 42-ckt panels, (1) 200A sub panel, all NEMA 1, matching existing specification
- 26-27 Wiring Devices — 212 duplex receptacles, 18 GFCI (wet locations + kitchenette), tamper-resistant throughout, white Decora
- 26-28 Protective Devices — AFCI / GFCI / dual-function per 210.12 and 210.8
- 26-32 Emergency / Standby — 12 emergency LED downlights on existing EM circuit; tie-in and battery-backup verification included
- 26-41 Grounding — new GEC to cold-water and building steel, verified with megger
- 26-50 Lighting — 96 Type A 4" LED recessed downlights, dimmer controls in conference rooms (14 0–10V drivers)
- 26-56 Exterior Lighting — excluded per Addendum #2

**Base Bid Total: $342,800**

**2. ALTERNATES**

| # | Description | Add/Deduct | Amount |
|---|---|---|---|
| A-1 | Upgrade Type A to premium tunable-white downlights | ADD | $18,400 |
| A-2 | Delete 12 emergency LED fixtures (owner to use existing) | DEDUCT | ($4,900) |

**3. ALLOWANCES (included in Base Bid)**

| Ref | Description | Allowance |
|---|---|---|
| ALL-1 | Decorative pendants over reception (owner selection) | $3,500 |

Allowance reconciliation via CO at actual cost + 22% markup.

**4. UNIT PRICES (add/delete post-award)**

| Item | Add | Delete |
|---|---|---|
| Standard duplex receptacle (rough + trim) | $168 | $118 |
| GFCI receptacle (rough + trim) | $212 | $148 |
| 4" LED Type A downlight | $285 | $195 |
| 20A, 120V homerun (up to 50 ft) | $520 | $360 |

**5. EXCLUSIONS**
- Cutting, patching, painting
- Fire alarm (Division 28) — owner's existing system, by others
- Low-voltage data cabling (Division 27)
- Temporary power, lighting, heat
- Permit fees (owner-paid per Addendum #1)
- Special inspections and commissioning agent
- Removal/relocation of existing owner IT equipment
- Abatement of any hazardous materials
- Work outside normal business hours unless specified

**6. ASSUMPTIONS**
- Bid per NEC 2023 with current California amendments
- Drawings E0.0–E5.2 rev 4/2, with Addenda #1 and #2 incorporated
- Normal working hours 7:00–15:30 M–F, GC-coordinated
- Continuous access to ceilings and electrical rooms
- One mobilization; additional mobilizations via CO
- Switchgear lead time per supplier quote dated 4/10 (valid through 5/15/2026)
- Storage room provided by GC on-site; otherwise off-site storage CO

**7. SCHEDULE**
- Mobilization: week of 2026-06-01 (assumes contract signed by 5/22)
- Rough-in complete: 2026-07-06
- Trim / finish: 2026-07-24
- Substantial completion: 2026-07-31
- Lead-time critical: (2) 42-ckt panels 6 weeks from order release

**8. BID TERMS**
- Bid valid for acceptance for 45 days from 2026-04-28
- Material pricing valid for 30 days from bid date; after, actual supplier escalation via CO
- Bonding: Included at 1.2% (P&P bond via [Surety name], A.M. Best A, Treasury listed)
- Sales tax: Included on materials at CA state + local rate
- Prevailing wage: N/A (private project)
- Subject to mutually acceptable subcontract terms

**9. QUALIFICATIONS**
- California MBE certified (CUCP #[xxx])
- EMR: 0.82
- Insurance: GL $2M / Auto $1M / Umbrella $5M
- License: CA C-10 #EC-00842, expires 2027-02-28

Signed for the Bidder:
Mike Torres, Owner
Date: 2026-04-14

---

**Internal Notes**
- **Switchgear quote expires 5/15** — if award slips beyond that, escalation CO likely. Flag to GC PM on follow-up call.
- **Prevailing wage** — confirmed with Addendum #1 as N/A (private project); keep paper trail of that addendum.
- **Alternate A-2** — verify with PM whether the existing emergency circuit is compliant with NEC 700 load testing before committing to the delete price.
- **Bond** — confirmed rate with surety 4/12; valid 90 days.
- **Attach to submission:** signed bid form, Addenda #1/#2 acknowledgment, current COI, license copy, MBE certificate, W-9, proposed schedule bar chart, list of key subs (none for this bid).
- **Post-submission follow-up:** call Sarah Liu (GC PM) afternoon of 4/28 to confirm receipt; ask when selection is expected.
