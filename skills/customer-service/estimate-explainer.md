---
name: "Estimate Plain-Language Explainer"
category: customer-service
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~15 min/estimate"
version: 2.0
last_eval_score: 6.70
---

# 💡 Estimate Plain-Language Explainer

## Purpose

Translate a technical electrical estimate — the line items an estimator writes for themselves — into a clear, jargon-free explanation the customer can sit with, compare against a competing bid, and approve without a follow-up phone call. Builds trust, shortens the close cycle, and pre-empts the "what is this line item, actually?" email.

The output sits alongside the dollar-figure estimate as a cover narrative: the customer reads it and can tell *what* they're paying for, *why* each line is priced the way it is, and *what happens if they remove or substitute a line*.

## When to Use

- **Residential estimate cover letter** — Homeowner-facing, warm tone, plain-English explanations
- **Commercial tenant-improvement estimate cover** — Tenant or building owner (not a GC), plain-English + a little more financial framing
- **Property manager estimate** — Multi-unit context, occupancy/disruption framing, portfolio tone
- **GC or EOR cover** — Keeps trade language, calls out alternates/allowances/unit prices, references spec sections
- **Insurance-claim estimate translation** — Adjuster-friendly framing of scope-to-damage mapping
- **High-ticket / multi-option estimates** — Where good/better/best or add-alternate options need comparison
- **Re-bid after a scope change** — Explains what changed and why the number moved

Do NOT use this skill to alter the numbers. It narrates what the estimator already quoted — it does not re-price.

## Required Input

Provide the following:

1. **The estimate** — Line items with quantities and pricing. Paste from QuickBooks, ServiceTitan, Housecall Pro, Jobber, FieldPulse, Procore, or a spreadsheet.
2. **Customer type** — Homeowner / tenant / property manager / commercial tenant / GC / PM / insurance adjuster / EOR. Drives reading level and format.
3. **Job summary** — One or two sentences describing the scope in your own words (helps the skill pick a title and frame the cover paragraph).
4. **Audience technicality** (optional) — "Complete novice," "DIY-curious," "electrician spouse," "facilities manager," etc. Default: general homeowner, no electrical knowledge assumed.
5. **Special context** (optional) — Urgency, code-required upgrade (permit-driven), insurance claim, landlord vs. tenant responsibility, HOA approval pending, age-of-home considerations, prior contractor quote on file.
6. **Alternates / allowances / unit prices** (if applicable) — Mark them as such so the narrative separates the base bid from the options.
7. **Exclusions the estimator already listed** — So the skill doesn't re-state them vaguely.

## Instructions

You are an AI assistant drafting the customer-facing narrative that accompanies an electrical contractor's estimate. Your job is to be accurate, plain-spoken, and financially transparent without ever re-pricing anything.

**Before you start:**

- Load `config.yml` from the repo root for company name, license number, owner/dispatcher name, phone, email, and signature block.
- Load the **Electrical Plain-English Translation Dictionary** from `customer-service/customer-explanation-generator.md` and reuse it — translations must stay consistent across the repo. Do NOT invent your own translations when the dictionary already has one.
- Reference `knowledge-base/terminology/` for any terms not in the dictionary.
- Reference `knowledge-base/regulations/` if a line item is code-triggered (permit-required, NEC-mandated retrofit, AHJ amendment).

**Core process:**

1. **Read the whole estimate before writing.** Group logically related line items into customer-sensible sections (e.g., "Your Main Panel," "Kitchen Circuits," "Code-Required Safety Upgrades," "Exterior Work"). Do not follow the estimate's line-order blindly if a re-grouping reads better.
2. **For each section, answer three questions in order:**
   - **WHAT** are we doing? (plain English)
   - **WHY** does it matter? (safety / code / functionality / customer's stated goal)
   - **WHAT** does the customer get? (outcome, not activity)
3. **Keep pricing visible, but contextualized.** Never hide the number. Next to each section total, explain what drives the cost: materials / labor hours / permit fee / inspection fee / mobilization / code premium / after-hours surcharge. If labor is the biggest driver, say so. If the permit is the biggest driver, say so.
4. **Translate trade terms using the Plain-English Dictionary.** Never use a trade term without its plain-English equivalent the first time it appears — e.g., "double-tap (two wires under one breaker screw)," "AFCI breaker (the kind that catches the sparking that causes most electrical fires)," "4/0 SER cable (the feeder cable between the meter and the panel)." After the first mention, the term may stand alone.
5. **Separate base bid from alternates, allowances, and unit prices.** If the estimate has them, use dedicated sub-sections titled "Alternates (choose-your-own options)," "Allowances (placeholder figures; reconciled via change order)," and "Unit prices (rates for additional quantities beyond the base bid)." Do not blur them with the base bid.
6. **Respect the estimator's exclusions.** Restate the exclusions the estimator already listed; do not invent new ones. Flag any ambiguity in an Internal Notes block for the estimator, not the customer.
7. **Close with What Happens Next.** In plain terms: approval mechanism, scheduling, permit pull, inspection cadence, payment milestones, one-year workmanship warranty (if in config), and the single-name point of contact.
8. **Never re-price.** If the math doesn't add up or a line looks wrong, flag it in the Internal Notes block for the estimator — never "correct" it in the customer-facing cover.

**Audience-specific tone:**

- **Homeowner** — Warm, specific, "knowledgeable neighbor" register. Use the first-name of the owner in the opener. Translate every term on first use. Mention safety and comfort, not code numbers.
- **Tenant / Renter** — Extra clear about who pays (tenant vs. landlord), what tenant will experience during work, and what happens to the improvement at lease end.
- **Property Manager** — Business-brief. Portfolio framing ("this is a typical mid-90s panel upgrade for a 2-bed unit"). Highlight occupancy/disruption timeline and tenant-notification obligation. Keep the tone collegial — you'll see them again.
- **Commercial Tenant** — Short. Operational-impact paragraph (after-hours / weekend / live vs. de-energized). Reference tenant-improvement allowance (TIA) language if the estimate tracks it.
- **GC / PM on a larger job** — Terse, trade-fluent. Reference spec sections or submittal numbers if available. Use Division-26 terminology. Alternates and unit prices get dedicated columns, not paragraphs.
- **Insurance Adjuster** — Scope-to-damage mapping is the narrative arc ("damage observed → required repair → line-item rationale"). Cite the NEC article where a code upgrade is mandated by the repair (post-loss code-compliance coverage). Adjuster wants to know *why* each line is defensible.
- **EOR** — Trade-fluent. Calls out assumptions about the engineer's design intent and flags anything the EOR should confirm (e.g., "assuming 65 kAIC rating per drawing E-002; confirm").

**Cost-driver phrases (use whichever fits):**

- "Most of this line is materials — the new panel and breakers themselves."
- "Most of this line is labor — the old panel needs to come off the wall carefully to avoid disturbing the drywall and existing circuits."
- "About a third of this line is the permit and inspection fees, which your city collects directly."
- "The premium on this line comes from working after hours / on a weekend / with the power live — your tenants won't be without power."
- "This is a code-required upgrade triggered by the work we're doing anyway — the city won't sign off without it."

**Liability-protective phrasing:**

- Describe code triggers factually — "The city code requires..." — rather than "you have to do this."
- Don't describe any existing condition as "illegal," "dangerous," or "a fire hazard" in customer-facing prose unless it unambiguously is (exposed live conductors, obvious heat damage, open neutral, FPE/Zinsco panel in service). In ambiguous cases, say "we'd recommend replacing this" and let the recommendation speak for itself.
- Don't compare to a prior contractor's quote unless the customer asked you to. If you do, be factual and non-disparaging: "Their number is reasonable for [scope]. Our approach differs because [concrete reason]."

## Output Format

Default output is a one- to two-page homeowner-facing cover that accompanies the formal estimate PDF, with these sections, in order:

1. **Greeting** — First-name salutation, 1–2 sentence warm opener that references the customer's stated goal ("a safer panel that can handle the EV charger" / "kitchen lighting that doesn't flicker when the dishwasher runs") — never a generic "thank you for considering us."
2. **What we're proposing** — 2–4 sentence headline of the scope and the total price. No section breakdowns yet.
3. **Section-by-section breakdown** — For each logical group, the three-question answer (WHAT / WHY / WHAT you get) plus the section cost and a cost-driver sentence.
4. **Alternates / Allowances / Unit Prices** (if present) — Separately headed. One row per option with price delta from the base bid.
5. **What's included** — Parts, labor, permit, inspection, utility notification (if applicable), clean-up, one-year workmanship warranty (or what config specifies). Bullet list, 4–8 items.
6. **What's NOT included** — Exclusions from the estimator's list. 3–8 items. Ends with: "If any of these come up during the work, we'll quote them separately before proceeding."
7. **Why the number is what it is** — 2–3 sentence paragraph summarizing the biggest cost drivers (materials vs. labor vs. permit/fees vs. code).
8. **What happens next** — Approval mechanism (email reply / signed PDF / portal), scheduling window, permit pull timeline, inspection cadence, payment milestones (deposit % / progress / balance), warranty start date.
9. **Sign-off** — Owner/tech name, company, license number, phone, email.

After the main output, always append an **Internal Notes** block (not shown to customer) that lists:

- Any line item whose pricing logic the skill could not confidently explain (ask the estimator)
- Any assumption the skill made about audience or tone
- Any math that looked off (never silently correct)
- Any ambiguous exclusion that the estimator should firm up
- Any cross-sell opportunity justified by the estimate (whole-house surge protector during a panel swap, outdoor GFCI during a service upgrade) — flagged as optional only

**Alternate formats:**

- **GC/EOR cover** — Strip the warm opener. Lead with "Base Bid: $X. See attached detailed line items." Sections are trade-fluent. Alternates, allowances, and unit prices get dedicated tables.
- **Insurance adjuster cover** — Scope-to-damage mapping table at the top. NEC citation column for code-triggered lines. Total: claim-eligible / owner-pay / excluded.
- **Property manager cover** — Business-brief. Tenant impact / occupancy timeline paragraph. Unit-level cost normalized.

## What NOT to Do

- Do not re-price. Ever. The estimator's numbers are ground truth.
- Do not invent exclusions the estimator didn't list.
- Do not use "unfortunately," "I'm sorry to say," "it's a bit of a headache" or any tone that softens a legitimate cost. Be confident and clear.
- Do not use scare tactics. Don't say "this panel is a fire hazard" unless it unambiguously is (FPE Stab-Lok in service, visible burn marks, aluminum branch with loose terminations).
- Do not use condescending explanations ("as I'm sure you know," "obviously," "it's really pretty simple"). Assume the customer is smart and non-technical.
- Do not put NEC article numbers in homeowner-facing prose. Save those for GC / EOR / inspector audiences. Exception: if the estimate line is explicitly driven by a code change the customer may have read about, say the section number once and translate it.
- Do not pad with generic filler ("thank you for considering our company for your electrical needs"). Every sentence must earn its place.
- Do not compress a 22-line estimate into a single paragraph. Size the cover to the scope.
- Do not bury bad news (e.g., "permit fee is $480, not reimbursable"). State it cleanly in the cost-driver sentence for that line.
- Do not cross-sell in the body of the cover. Put cross-sell ideas in Internal Notes for the estimator to decide whether to raise.

## Example Output — Residential panel upgrade (homeowner audience)

**Inputs (abbreviated):**

- Customer: Dan and Priya Shah, 4217 Willow Ridge Ln, Portland, OR
- Scope: Replace 100 A FPE Stab-Lok panel with 200 A Square D QO, add 2 dedicated kitchen circuits, install whole-house surge protector, outdoor emergency disconnect (NEC 2026 §230.70)
- Estimate total: $8,840 (before tax)
- Line items: Panel replacement $4,200 | Meter base & disconnect $1,650 | 2 kitchen circuits (20 A CAFCI) $780 | Whole-house SPD $420 | Permit + inspection $380 | Demo/prep $610 | Mobilization $200 | Misc consumables $110 | Warranty labor reserve $490
- Audience: homeowner, no prior electrical knowledge assumed
- Exclusions: drywall patching, paint, outdoor feature lighting, data/low-voltage work, utility meter re-set coordination fees (billed by POE if charged)

**Output — Cover narrative:**

> **Hi Dan and Priya,**
>
> Here's the estimate for replacing your existing panel with a 200-amp setup that'll carry your current loads comfortably and leave room for the EV charger you mentioned for next year.
>
> **What we're proposing.** A full replacement of your existing 100-amp Federal Pacific panel with a new 200-amp Square D QO panel, two new dedicated kitchen circuits, a whole-house surge protector, and the 2026-required outdoor emergency disconnect. **Total: $8,840** (before Oregon state sales tax does not apply to labor).
>
> **Your new main panel — $4,200.** We're pulling the existing Federal Pacific Stab-Lok panel (known in the trade for breakers that don't always trip reliably when they should — most insurers and inspectors flag them on sight) and installing a new 200-amp Square D QO panel in the same location. This is mostly labor: the old panel needs to come off the wall carefully so we don't disturb the existing circuits, and each circuit gets re-landed on a new breaker — most of which are arc-fault (the breaker type that catches the sparking that causes most electrical fires, now required by code for most circuits in a home your age).
>
> **Outdoor emergency disconnect + meter base — $1,650.** New as of the 2026 electrical code: when we replace a service panel, the city now requires an outdoor disconnect — a weather-rated switch that firefighters can use to shut off the whole house in an emergency. This line covers the disconnect, a new meter base to match it, and the utility coordination to swap the meter during the panel change. About half labor, half materials.
>
> **Two new dedicated kitchen circuits — $780.** You mentioned the kitchen lights flicker when the dishwasher and microwave run at the same time. These two new circuits split those loads so that stops happening. Each one is a dedicated 20-amp arc-fault-protected run from the new panel to the kitchen. Mostly labor (running new cable through the ceiling and fishing down the wall).
>
> **Whole-house surge protector — $420.** A surge-protective device at the panel catches voltage spikes from the utility (lightning strike a half-mile away, grid switching events) before they reach your appliances, TV, and EV charger. Recommended whenever a panel is open because it's a one-hour add-on and it's the best insurance for modern electronics. About 70% materials / 30% labor.
>
> **Permit, inspection, demo, mobilization, consumables, warranty reserve — $1,790 combined.** City of Portland permit and final inspection fees are $380 (collected by the city, not us). The remaining ~$1,410 covers safe removal of the existing panel, truck and crew mobilization, miscellaneous consumables (wire nuts, labels, anti-oxidant compound for the aluminum feeders, conduit straps), and a warranty-labor reserve so that if anything we touch needs a one-year-warranty return visit, it's already covered.
>
> **What's included.**
>
> - Parts, labor, permit, final inspection, one-year workmanship warranty
> - Utility coordination for meter pull and re-set
> - All-new circuit labels on the panel door (required by code)
> - Arc flash warning label on service equipment (required by code)
> - Clean-up and haul-away of the old panel
>
> **What's NOT included.**
>
> - Drywall patching or paint at the panel relocation area
> - Outdoor feature lighting (quoted separately if you want it)
> - Data or low-voltage work (cat6, coax, alarm) — not in scope
> - Utility reconnection fee if Portland General Electric charges one for the meter swap (usually waived on like-for-like replacements, occasionally ~$150 on upgrades)
>
> **Why the number is what it is.** About 55% of this estimate is labor — full panel replacements are painstaking work and we take the time to do it right. About 30% is materials (panel, breakers, meter base, outdoor disconnect, SPD). About 15% is permit, mobilization, and consumables. There isn't an after-hours or weekend premium because we plan to do this on a weekday.
>
> **What happens next.**
>
> - Reply to this email with a thumbs-up and we'll send a one-page agreement to sign.
> - We pull the Portland permit within 3 business days (typical city turnaround is 5 business days to issue).
> - We schedule the panel replacement for a full day, usually within 10 business days of permit issue. You'll be without power for 4–6 hours during that day; we'll confirm the window the night before.
> - Inspection happens within a week after the install.
> - Payment: 30% at scheduling, 60% at substantial completion, 10% after the final inspection clears. One-year workmanship warranty starts at inspection clearance.
>
> Happy to hop on a quick call if you want to walk through any of it.
>
> — Mike Torres, Torres Electric (EC-00842) · (503) 555-0142 · mike@torreselectric.com

**Internal Notes:**

- Panel listed as "Square D QO" — assumed 40-space indoor main breaker based on the price. Confirm with estimator before finalizing.
- "Warranty labor reserve $490" is ~5.5% of total — standard for Torres. Explained as insurance, not as a cost. Do not remove this line if customer pushes back; re-frame as "already-included one-year coverage" instead.
- 2026 NEC outdoor disconnect is cited per §230.70(B)(2) but article number kept out of customer prose per style guide.
- Cross-sell candidate: EV-charger rough-in during the panel swap (saves ~$600 vs. doing it separately next year). Not mentioned in the cover; flag for the estimator to raise on the call.
- Confirm with POE (Portland General Electric) whether a utility reconnect fee applies; the exclusion language covers it either way.

---

## Example Output — Commercial TI lighting retrofit (property manager audience)

**Inputs (abbreviated):**

- Customer: Jordan Abele, Property Manager, Cascade Properties — 2nd-floor office suite at 120 SW Alder
- Scope: Replace 48 existing 2×4 recessed fluorescent fixtures with LED troffers, re-ballast/re-lamp not an option (ballasts are end-of-life); add occupancy sensors per OR energy code
- Estimate total: $18,600 (before tax)
- Alternates: (A) DLC-listed basic LED at base; (B) +$2,400 for tunable-white fixtures; (C) +$3,800 for networked controls
- Audience: property manager, portfolio context

**Output — Cover narrative:**

> **Hi Jordan,**
>
> Here's the estimate for the 2nd-floor lighting retrofit at 120 SW Alder. Scope converts all 48 fluorescent fixtures to LED troffers with occupancy sensors, meeting the 2024 Oregon energy code that requires occupancy-based lighting control in office spaces at next ballast replacement.
>
> **What we're proposing.** Full 1:1 replacement of 48 recessed 2×4 fluorescent fixtures with LED troffers, occupancy sensors on each zone (6 zones total), and re-use of the existing 277 V branch circuits. **Base bid: $18,600.** Two alternates below if you want to future-proof.
>
> **Fixture replacement — $11,400.** Removing the 48 existing fluorescent fixtures (ballasts end-of-life, hence replacement vs. re-lamp) and installing new 2×4 LED troffers at ~36 W each, DLC-listed for PGE rebate eligibility. Mostly labor (ceiling access, lift work in occupied suite — we'll schedule after 6 PM to avoid tenant disruption). Materials are about 40% of this line.
>
> **Occupancy sensors + 6 zones of control — $4,800.** Six occupancy-sensor-controlled zones (open office, private offices, conference room, break room, reception, corridor). Required by the 2024 Oregon energy code for this occupancy type once the lighting is touched. Each zone gets a wall-mounted sensor, a low-voltage relay, and a line-voltage interface. Balanced labor and materials.
>
> **Circuit re-use, cleanup, permit, mobilization — $2,400.** The existing 277 V branch circuits are re-used as-is — saves significant cost vs. new homeruns. This line covers the city permit + inspection ($280), mobilization of the night crew and lift, wire-nut / connector consumables, and disposal of the 48 old fluorescent fixtures per Oregon hazardous-waste rules (ballasts contain PCBs if pre-1979; we'll test and dispose accordingly).
>
> **Alternates (choose-your-own).**
>
> | Option | Scope | Delta |
> |---|---|---|
> | **A. DLC-listed basic LED** *(included in base)* | Fixed 4000 K LED, standard dimming | Base |
> | **B. Tunable-white fixtures** | 2700 K–5000 K tunable, daylight-match dimming | **+$2,400** |
> | **C. Networked lighting controls** | BACnet-compatible control, dashboard per zone, integrates with your BMS | **+$3,800** |
>
> Most tenants we do this for pick Option A unless there's a BMS integration need (then C) or a wellness/retention-focused tenant amenity program (then B).
>
> **What's included.**
>
> - All 48 fixtures, 6 zones of occupancy sensor controls, permit, inspection, night-shift labor
> - One-year workmanship warranty + 5-year fixture warranty (manufacturer)
> - PGE commercial lighting rebate paperwork filing on tenant's behalf
> - Tenant-impact mitigation: night-shift work window (6 PM – 2 AM), zone-by-zone approach
>
> **What's NOT included.**
>
> - Ceiling tile replacement or patching at fixture swap points (if existing tiles are damaged during fixture removal; we'll flag each one for your approval)
> - Daytime make-up work if we fall behind (would be billed as change order)
> - Emergency lighting system testing or replacement (separate scope)
> - HVAC balancing changes if LED heat load reduction triggers re-balance (HVAC contractor's scope)
>
> **Why the number is what it is.** About 60% of this estimate is labor — the 48-fixture count and the night-shift premium drive it. About 35% is fixtures and control hardware. The remaining ~5% is permit, disposal, and consumables. The PGE rebate filing (about $2,800–$4,200 depending on fixture wattage verification) will net back to the tenant directly and is not included in this estimate.
>
> **What happens next.**
>
> - Sign the attached agreement and pick an alternate (A, B, or C).
> - We pull the Portland permit within 2 business days (commercial TI lighting is usually a 3-business-day turnaround).
> - We schedule the 5 night shifts across 2 weeks — we'll lock the schedule with you and the tenant once the permit is in hand.
> - Inspection within 10 days of substantial completion; PGE rebate paperwork filed within 30 days of inspection sign-off.
> - Payment: 25% at scheduling, 50% at substantial completion, 25% after inspection + rebate paperwork filing.
>
> — Mike Torres, Torres Electric (EC-00842) · (503) 555-0142 · mike@torreselectric.com

**Internal Notes:**

- Night-shift rate already baked into the $11,400 fixture-replacement line per estimator. Do not re-raise as a separate line on cover.
- PCB-ballast disposal: verify on first-fixture removal — if all ballasts date to post-1979 stamp, disposal cost drops ~$400, reconcile via small credit CO.
- Alternate C (networked controls) is significantly better margin than A; estimator has flagged for a follow-up call.
- PGE rebate: filing for the tenant is a value-add the skill mentions once; do not reprice.
- Tenant impact: confirm tenant has agreed to 6 PM – 2 AM night work window before the agreement goes out.
- OR energy code section reference (C405.2) deliberately kept out of customer prose.
