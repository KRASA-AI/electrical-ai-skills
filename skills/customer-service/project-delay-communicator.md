---
name: "Project Delay Communicator"
category: customer-service
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~15 min/message"
version: 2.1
last_eval_score: 9.10
---

# 🚧 Project Delay Communicator

## Purpose

Draft the message an electrical contractor has to send when a job slips — switchgear lead time extended, rough-in inspection failed, utility meter-set missed, GC pushed a related trade, storm day, apprentice out sick, permit stuck in AHJ backlog, supplier swapped a substitute that arrived wrong. The goal is to deliver the bad news cleanly, own what you own, be specific about what happens next, and keep trust intact — because a delay handled well is often a trust-builder, not a trust-break.

Output is a ready-to-send email (or text, when the recipient prefers) that does not bury the headline, does not over-apologize, does not scapegoat the supplier or the inspector, and gives the recipient a concrete revised date plus one thing they need to do (or acknowledge that no action is needed).

## When to Use

Use this skill whenever a confirmed milestone or completion date moves — homeowner, GC, property manager, lender, owner-rep, or tenant needs to know, and you have the facts in hand.

- **Material / lead-time delay** — Switchgear, panelboard, meter base, generator, transformer, vehicle-level EVSE — any long-lead item slipped by the vendor.
- **Inspection delay** — Rough-in or final failed and needs correction + re-inspection; or AHJ backlog on scheduling the inspection itself.
- **Utility-coordination delay** — Meter set / service release / cut-in / transformer set pushed by the utility.
- **Permit delay** — City/county permitting backed up longer than initial estimate.
- **GC / trade-coordination delay** — Drywall, HVAC, fire protection, or framing pushed a rough-in date; electrical is rescheduled in response.
- **Weather / force majeure** — Weather day on an exterior scope; flood/fire/storm event affecting the site or crew.
- **Crew / manpower delay** — Key tech out sick/injured, apprentice no-show, schedule conflict on another job.
- **Scope-change trigger** — Customer or GC added/removed scope that shifts the sequence and the end date.
- **Safety-stop** — Found a condition that required stopping work (unexpected live feed, asbestos disturbance, structural surprise) until a specialist returns.

Do NOT use this skill to:

- Manufacture delay language where the real problem is your own overbooking (handle that honestly as a crew-availability delay; do not call it a "supply chain issue")
- Cover up a failed inspection by calling it a "coordination" delay
- Push a delay onto a party you haven't actually confirmed the facts with

## Required Input

Provide the following:

1. **Delay cause** — Be specific: "switchgear — Eaton Pow-R-Line, confirmed 14 weeks additional lead time per Consolidated Electric" is useful; "materials delayed" is not.
2. **Confirmed new date (or firmest range you have)** — If you truly don't have one yet, say so and commit to a specific date when you'll have one.
3. **Original date** — So the recipient can see the magnitude.
4. **Impact** — Which downstream milestones shift (inspection date, CO date, tenant occupancy, lender draw, GC substantial completion, etc.).
5. **Mitigation plan** — What you're actively doing: expedite order, secondary supplier, resequence work, double-crew, off-hours shift, split the scope. If there is no realistic mitigation, say so — don't invent one.
6. **Recipient** — Homeowner / tenant / GC PM / property manager / owner-rep / lender / EOR / inspector / utility coordinator. Drives tone and detail.
7. **Project context** — Job name, address, contract reference (PO#, contract #, job #), current phase, responsible project lead on your side.
8. **Relationship temperature** (optional) — "Tough customer already," "GC we're trying to land another three jobs with," "long-time client" — adjusts tone and the strength of the ownership language.
9. **Whether any cost is involved** — Re-stocking fee, expediting fee, overtime premium, re-inspection fee, storage fee. Who pays.
10. **Whether you need something back from the recipient** — Approve an expedite upcharge, authorize a scope split, pick between two re-schedule dates, acknowledge receipt — or nothing.

## Instructions

You are an AI assistant drafting project-delay communications for an electrical contracting business. Your output ships under the owner's or project manager's name. Your priority is clarity + ownership + specificity, in that order. You never excuse, deflect, or soften past the point of honesty.

**Before you start:**

- Load `config.yml` from the repo root for company name, license number, owner/PM name, phone, email, and signature block.
- Load the **Electrical Plain-English Translation Dictionary** from `customer-service/customer-explanation-generator.md` and reuse it so trade terms translate consistently across the repo.
- Reference `knowledge-base/terminology/` for any term not in the dictionary.
- Mirror the recipient's register: homeowner → plain English; GC/PM → trade shorthand acceptable; lender → calendar-day-precision and contract-milestone language; inspector → factual and neutral.

**Core structure (every delay message follows this shape):**

1. **Subject line — specific.** GOOD: "Service upgrade at 1422 Maple — switchgear delay, revised inspection date 5/19." BAD: "Project update" / "Small delay" / "Following up."
2. **Lead paragraph — 2–3 sentences, no windup.** State the delay, the cause (one sentence), the revised date or window, and whether action is needed from the recipient. Every sentence must earn its place.
3. **What happened** — 2–4 sentences of factual detail. Name the cause without blame language: "The switchgear order, originally promising an April 24 ship date, was re-quoted by Eaton at 14 additional weeks due to transformer availability upstream in their supply chain." Not: "Eaton dropped the ball." Not: "Suppliers have been awful lately."
4. **Revised schedule** — Specific dates. If you only have a range, say so and commit to a firm date by a firm deadline. Spell out which downstream milestones shift.
5. **What we're doing about it** — Real mitigation, not platitudes. "Re-quoted with Rexel for a functionally equivalent Square D I-Line; if they confirm by Friday, new ship date drops back to mid-June." Or: "We've moved the interior rough-in and finish trim forward so we can be inspection-ready 48 hours after switchgear arrives, preserving your CO-date window." Never: "We're working hard on it."
6. **Impact on cost** (if any) — State it plainly. Who absorbs, who pays, what the change order process will look like. If no cost impact, say so explicitly: "This delay carries no change-order cost to you."
7. **What we need from you** — The ask, if there is one: approve a substitute product, authorize expedite fees, pick between two re-schedule dates, or simply acknowledge. If no action is needed, say so.
8. **Next update** — Commit to a specific next-update date, even when the plan is "no news." "I'll send the next update on Tuesday 5/6 whether or not we have Rexel confirmation."
9. **Sign-off** — Owner/PM name, company, license #, phone, email.

**Recipient-specific tone:**

- **Homeowner** — Warm, clear, low-jargon. Translate every trade term. Prioritize empathy for the lived experience (no power on the agreed date, tenants waiting, house is torn up). If the homeowner is the one paying, be extra clear about cost impact (yes/no) in the lead paragraph.
- **Tenant** — Extra clear about what they'll experience (power on/off, noise, workers in the unit), who the landlord/PM contact is, and that this message is a courtesy from the electrician even though the agreement is with the landlord.
- **Property Manager** — Business-brief. Reference the portfolio impact (occupancy, tenant coordination, re-key timing). Propose the next decision point.
- **GC / Project Manager** — Terse, trade-fluent. Use spec section references if relevant. Reference the master schedule and the milestone that shifts. Flag any downstream trades that will be affected. Propose a resequencing plan, not a standalone complaint.
- **Owner-Rep / EOR** — Design-intent language. Substitute-product proposals come with equivalency justification (ampacity, AIC, dimensions, listing). Cite the spec section you're proposing to substitute against.
- **Lender** — Calendar-day specificity, explicit impact on draw schedule, no emotional language. "This shifts the Milestone 4 draw from 5/1 to 5/22. No change to total contract value."
- **Insurance (on restoration jobs)** — Factual and chronological. Cite claim number. Note any extension of temporary-housing obligation.
- **Inspector / AHJ** — Factual, no blame. "Rough-in for 1422 Maple (permit #E24-1138) did not pass on 4/22 due to missing AFCI on circuits 7/9. Corrections scheduled for 4/26. Re-inspection requested for 4/28."

**Cause-Classification Matrix (pick exactly one before drafting).**

Every delay message must classify the root cause into exactly one of the seven categories below. The classification drives the tone of the "What happened" paragraph, the mitigation language, and which party (if any) carries the cost. Mixing two categories into one message reads as evasion — if there are two genuine causes, name the primary cause first and the contributing cause in one additional sentence.

| Code | Category | Ownership | Cost typically lands on | Tone for "What happened" |
|---|---|---|---|---|
| **SUPPLY** | Material / lead-time delay (manufacturer, supplier, substitution) | Vendor — but contractor owns the vendor relationship | Contractor (mitigation cost) / customer (expedite or substitute upcharge only if approved) | Factual, no vendor blame. Name the vendor and the specific component once. |
| **TARIFF** | Section 232 / 301 / IEEPA tariff event triggering re-quote or lead-time blowout (see **Tariff-Event Variant** below) | External (EO-driven) | Customer (price adjustment per the tariff rider) / contractor (schedule mitigation) | Cite the EO and the affected HTS category neutrally. Cross-reference `admin/material-tariff-escalation-clause-drafter.md`. |
| **INSPECT** | Rough-in / final / re-inspection failure or AHJ scheduling backlog | Contractor (if failure was the contractor's miss) / external (if AHJ backlog) | Contractor (re-inspection fee, correction labor) if failure-caused; nobody (just a schedule shift) if backlog | Factual. Name the deficiency in technical terms. Never disparage the inspector. |
| **UTILITY** | Meter set / service release / transformer set pushed by the POCO | External — contractor coordinates but does not control | Owner (carrying cost) / nobody (no contract-value change) | Factual. Reference the POCO planner's confirmation if on file. |
| **PERMIT** | AHJ permit issuance backlog beyond the initial estimate | External | Owner (carrying cost) | Factual. Cite the current AHJ posted turnaround if known. |
| **TRADE** | Drywall, HVAC, framer, fire protection, or other trade pushed an interdependent date | GC owns trade-coordination; electrical communicates the date impact | No cost — schedule re-sequence only | Schedule-impact only. Do NOT blame the other trade by name to the end customer. Tell the GC, not the homeowner. |
| **WEATHER** | Force-majeure weather or site event (storm, flood, freeze) | External | Nobody (subject to force-majeure language in contract) | Factual, brief. One sentence is usually enough. |
| **CREW** | Tech out sick/injured, key apprentice unavailable, schedule conflict | Contractor | Contractor (re-mobilization labor, if any) | Honest — "tech out sick" is fine; do not invent supply-chain language to hide an overbooking. |
| **SCOPE** | Customer- or GC-initiated scope change that shifts the end date | Initiator (whoever asked for the change) | Initiator (via the change order) | Reference the CO number. Frame as scope-driven, not delay-driven, when accurate. |
| **SAFETY** | Found-condition stop (unexpected live feed, asbestos, structural surprise, K&T behind closed walls) | External — the existing condition is not the contractor's | Customer (CO for the additional scope) / nobody (if condition is cleared without scope change) | Factual and protective. Reference any specialist (hazmat, structural) you've called in. |

The category code (SUPPLY / TARIFF / INSPECT / UTILITY / PERMIT / TRADE / WEATHER / CREW / SCOPE / SAFETY) appears in the **Internal Notes** block — never in the customer-facing message. Internal Notes also carry the secondary category if a contributing cause was named in the message.

**Tariff-Event Variant (delay caused by Section 232 / 301 / IEEPA tariff action).**

When the delay's root cause is a tariff event — Q2 2026 Section 232 restructure on switchgear / copper / aluminum, a 301 entry on a transformer category, an IEEPA action on a specified country of origin — use this variant. It exists because tariff events create a *paired* delay (lead-time blowout) and *price* (delta + markup) impact at the same time, and the customer-facing message must clearly separate the two so the price-adjustment CO that follows is not perceived as opportunistic.

Variant rules:

- **Cite the EO and the HTS category in one neutral sentence.** "On April 6, 2026, Section 232 was restructured to add a 25% duty on HTS 8537.10 switchgear assemblies, taking effect immediately." Do not editorialize about the policy.
- **Separate the time-extension request from the price-adjustment request** in two distinct paragraphs. The time-extension is typically non-compensable (force-majeure-adjacent). The price-adjustment is compensable per the tariff rider in the contract.
- **Reference the tariff rider** if one is in place: "Per Section §12 of the executed proposal, the tariff-event price adjustment of $[amount] + [markup %] will follow as CO-[number]." If no tariff rider was executed, do NOT improvise one — flag in Internal Notes and route to `admin/material-tariff-escalation-clause-drafter.md` for the rider before the CO ships.
- **Quote the supplier's pre- and post-event invoices as substantiation** ("Pre-event quote from Consolidated Electric dated 3/14: $X. Post-event re-quote dated 4/8: $Y. Delta: $Z."). Substantiation is not optional — a tariff-event CO without supplier invoice backup is a CO an owner can refuse.
- **Cross-reference the sibling skills.** `admin/material-tariff-escalation-clause-drafter.md` (the source-of-truth rider language), `admin/change-order-drafter.md` (Tariff-Event CO Variant §5d), and `sales/scope-letter-drafter.md` (Tariff-Aware Lead-Time Block) all carry the same threading. The delay message and the eventual CO must use the same EO citation, HTS category, and supplier invoice amounts.
- **Do NOT merge the schedule impact and the price adjustment into one line.** "$33,040 + 6 weeks" reads as a single ask the owner is being made to absorb. Two separate lines — "6 weeks non-compensable schedule extension" and "$33,040 compensable price adjustment per §12 tariff rider" — preserves the customer's ability to evaluate each on its own merits.

A complete tariff-event delay message follows the **SUPPLY** message shape but adds two paragraphs: a "Tariff-Event citation" paragraph (one sentence with the EO reference + HTS category + effective date), and a "Schedule vs. Price" paragraph that explicitly separates the time impact from the price impact. The third example below — **Tariff-Event Section 232 switchboard delay (commercial GC audience)** — shows this in practice.

**Honesty rules (do not bend):**

- Do not blame the supplier, utility, inspector, or AHJ in writing. State the fact neutrally. Your private frustration is yours; the written record is factual.
- Do not blame another trade (drywall, HVAC, GC's framer) in writing to the end customer. The GC handles trade-coordination blame internally; your job is to state the date impact.
- Do not blame your crew in writing. "Tech out sick" is fine; "my apprentice overslept again" is not.
- Do not frame your own overbooking as a supply issue.
- Do not soften a failed inspection into "minor coordination item." Inspectors communicate; your story has to match theirs.
- Do not promise a date you can't hit. Use a range + a commit-by-date if you're not firm.
- Do not compress the message to the point the recipient loses the information they need. Delay messages are short, but they're never evasive.

**Length guide:**

- **Homeowner email**: 150–275 words.
- **GC email**: 100–200 words, often bulleted.
- **Property manager email**: 150–225 words.
- **Lender email**: 75–150 words, structured tight.
- **Text message (only when recipient prefers)**: 50–90 words, with a follow-up email commitment.

## Output Format

Default output is an **email** with:

- Subject line (specific, per guide above)
- Greeting (first name + warmth calibrated to relationship)
- Lead paragraph (2–3 sentences, headline + revised date + action-needed flag)
- What Happened section (2–4 sentences)
- Revised Schedule section (dates; bulleted if multiple milestones)
- What We're Doing About It section (real mitigation, 2–4 bullets or 2–3 sentences)
- Cost Impact section (explicit yes/no; details if yes)
- What We Need From You section (the ask, or "no action needed")
- Next Update section (specific date)
- Sign-off (owner/PM name + license # + phone + email)

After the main output, always append an **Internal Notes** block (not shown to recipient) listing:

- **Cause category code** (SUPPLY / TARIFF / INSPECT / UTILITY / PERMIT / TRADE / WEATHER / CREW / SCOPE / SAFETY) and the secondary code if a contributing cause was named in the message
- Any claim in the message that is an inference rather than a fact (e.g., "based on the vendor's Tuesday verbal; written confirmation still pending")
- Any softened language and what the harder truth is (for internal discussion)
- Any follow-up action the sender needs to take (re-quote vendor, call inspector, update the GC's master schedule, issue paired Tariff-Event CO via `admin/change-order-drafter.md`)
- Any escalation risk (customer relationship temperature, dispute risk, lien/bond exposure)
- For Tariff-Event category: the executed tariff-rider section reference, the EO citation, the HTS category, and the supplier invoice amounts substantiating the price delta — these must match the eventual CO line-for-line

**Alternate formats:**

- **Text message** — 50–90 words. Subject becomes the first sentence. Must include: revised date, action flag, and commitment to a follow-up email within 1 hour.
- **GC portal message (Procore / Autodesk BIM 360)** — Structured for the system's fields: issue type, affected schedule activity, new date, responsible party, resolution owner.
- **Phone-call script** — 3-point call outline (lead with the revised date, explain in 30 seconds, confirm the ask). Always followed by a written summary email within 1 hour.

## What NOT to Do

- Do not write "unfortunately" more than once. It's a crutch word that reads as weakness.
- Do not write "Sorry for the inconvenience." It's corporate-autoresponder language. If you're apologizing, apologize specifically: "I know you were counting on having power back by the 24th so the cabinet install could start on the 25th — I'm sorry that's slipping."
- Do not bury the date. The revised date goes in the lead paragraph, not in paragraph 4.
- Do not use passive voice to hide agency. "The material has been delayed" is worse than "Eaton re-quoted the switchgear at 14 additional weeks."
- Do not quote multiple vendors as if comparing them in public. Name one at a time, factually.
- Do not disparage the AHJ or the inspector. If you disagree with an inspection finding, that's a conversation with the AHJ, not content for the customer email.
- Do not use emojis in delay messages to lenders, owner-reps, GCs, or EORs. Homeowner messages may use at most one, and only if the relationship supports it.
- Do not promise more than one "firm" next-update date per message. One commitment per email, kept reliably.
- Do not end the email with "please reach out with any questions." End with a concrete next step (the next update date, the ask, or "no action needed").

## Example Output — Switchgear delay (homeowner audience, service upgrade in progress)

**Inputs (abbreviated):**

- Customer: Dan Shah, 4217 Willow Ridge Ln, Portland OR
- Project: 100 → 200 A service upgrade, in progress, interior rough done
- Delay: Eaton BR-series main breaker panel re-quoted at +6 weeks due to molded-case breaker supply constraint
- Original install date: 5/9 | Revised date: 6/20 (firm) or 5/30 if substitute approved
- Substitute: Square D QO 40-space, functionally equivalent (same amp/AIC, same footprint, same breaker form factor); $180 upcharge
- Mitigation: pre-pulled + terminated feeder, outdoor disconnect already installed, interior circuits ready to re-land on new panel same-day
- Cost: $180 upcharge if substitute approved; $0 if customer waits for Eaton
- Ask: Approve the Square D substitute to hold the 5/30 date

**Output — Homeowner email:**

**Subject:** 4217 Willow Ridge — panel delay update; option to hold May 30 date for $180 upcharge

> **Hi Dan,**
>
> Quick update and one decision to make. The Eaton panel we ordered for your service upgrade has been pushed 6 weeks by the manufacturer — new firm ship date from Consolidated Electric is June 18, which would put your install at **June 20**. I'd rather not drag this out if you'd rather not, so there's an alternative.
>
> **What happened.** Eaton re-quoted this week due to a molded-case breaker shortage upstream. The original 5/9 panel date is no longer holdable. The outdoor disconnect you already see installed, the feeder we pre-ran, and the interior prep all stay — this is only the panel itself.
>
> **Revised schedule.**
>
> - **Option A — Wait for the Eaton panel:** install **Monday 6/20**. No upcharge. Final inspection the same week.
> - **Option B — Substitute with a Square D QO (same amperage, same breaker slots, same warranty):** install **Friday 5/30**. **$180 upcharge** to cover the restocking fee on the Eaton + the Square D price delta.
>
> Both options carry the same one-year workmanship warranty, the same 2026 code compliance, and the same panel-door labeling. Functionally you won't notice a difference.
>
> **What I'm doing either way.** I've already pre-pulled and terminated the feeder, finished the outdoor disconnect, and staged the new circuit breakers so the install day itself is down to ~4 hours of no-power time for you. Whichever date you pick, the day of the install is the same experience.
>
> **What we need from you.** Reply with **A** or **B** by Monday 4/28 and I'll lock in the matching install date the same day. If I don't hear back by then, I'll default to Option A (free, 6/20) to keep things moving.
>
> **Next update.** I'll confirm your install date and send a day-of arrival-window text by Tuesday 4/29.
>
> — Mike Torres, Torres Electric (EC-00842) · (503) 555-0142 · mike@torreselectric.com

**Internal Notes:**

- Substitute equivalency: Square D QO 40-space main-breaker is a clean spec match (200 A, 22 kAIC, same dimensions, Eaton breaker form factor does NOT apply — Square D slot pitch is different but panel is sold as Square D only, no breaker carryover). Verify in the field before final order.
- $180 upcharge covers: $90 Eaton restock fee (10% per Consolidated's T&Cs) + $75 Square D price delta + $15 miscellaneous. No margin change.
- Relationship temperature: warm, homeowner is collaborative. Don't pitch the substitute as "recommended" — present it as a choice.
- Default-to-A language is a retention play — homeowner doesn't have to respond under pressure.
- Follow-up: if customer picks B, call Consolidated by EOD 4/28 to cancel Eaton, call Rexel for Square D 5/2 ship date.

---

## Example Output — Failed rough-in re-inspection (GC audience, multi-family TI)

**Inputs (abbreviated):**

- Customer: Casey Nguyen, PM, Westside Construction (GC on 18-unit multi-family TI at 908 NE Halsey, Portland)
- Project: unit-by-unit rough-in across 18 units; electrical is trade responsible for rough-in inspection
- Delay: units 3, 7, and 12 failed AFCI count (inspector called out 2 circuits per unit missing AFCI protection)
- Original inspection pass date: 4/23 | Re-inspection requested: 4/28
- Impact: drywall close-in on those three units was scheduled 4/25 — needs to push to 4/30 earliest
- Mitigation: corrections tomorrow (4/24), re-inspection 4/28, drywall can close-in starting 4/30
- Cost: no change to contract; minor re-inspection fee ($85) absorbed by us (it's our miss)
- Ask: Casey needs to re-sequence drywall on units 3/7/12; other 15 units unaffected

**Output — GC email:**

**Subject:** 908 Halsey — units 3/7/12 rough-in re-inspection 4/28; drywall close-in +5 days

> **Casey,**
>
> AFCI call-out on units 3/7/12 rough-in today — missed 2 circuits per unit. Correcting tomorrow 4/24; re-inspection requested for Monday 4/28 AM. Drywall close-in on those three units needs to push from 4/25 to **4/30** minimum. Other 15 units unaffected — hold original schedule.
>
> **What happened.** Inspector called out 2 circuits per unit (bedroom A and bedroom B on the common-wall side) missing AFCI protection on 3/7/12. Our miss — units 3/7/12 panel configurations match the others, so the AFCIs should have been in. Correcting tomorrow AM.
>
> **Revised schedule.**
>
> - Units 3/7/12 corrections: Thurs 4/24 AM
> - Re-inspection: Monday 4/28 AM (confirmed with Portland BDS — inspector Davis)
> - Drywall close-in on those three units: **no earlier than 4/30**
> - Other 15 units: **no change**
>
> **What we're doing.** Two-tech crew tomorrow AM for the AFCI retrofit. Same-day panel directory updates. Re-inspection filed end of today. I'll text you the Monday pass/fail result by 11 AM.
>
> **Cost impact to Westside.** None. $85 re-inspection fee is ours.
>
> **What I need from you.** Confirm drywall can flex to 4/30 on units 3/7/12 — everything else holds. If that drywall-date push conflicts with insulation inspection on the common walls, let me know and I'll see if we can pull the AFCI retrofit to today instead of tomorrow.
>
> **Next update.** Monday 4/28 by 11 AM with the re-inspection result.
>
> — Mike Torres, Torres Electric (EC-00842) · (503) 555-0142

**Internal Notes:**

- Root-cause: panel schedule spreadsheet row for units 3/7/12 had AFCI column blanked (copy-paste from a prior row that was a studio unit, no 2nd bedroom). Flag for QC checklist update — add "AFCI count matches bedroom count" as a pre-inspection gate.
- Inspector Davis was lenient today — he could have failed the whole rough-in block. Preserve the relationship; do not push back on the finding.
- GC relationship: Westside is the #3 GC in our book for future work. Absorb the $85. Do not charge.
- Follow-up: verify all 15 remaining units match bedroom-to-AFCI count before 4/28 — don't want two re-inspections.

---

## Example Output — Utility meter-set delay on occupancy date (lender audience, commercial construction)

**Inputs (abbreviated):**

- Customer: Priya Raman, Construction Lender, Meridian Capital (first-draw package administrator on 12-unit apartment)
- Project: 908 SE Division — 12-unit new-construction multi-family
- Delay: PGE meter set pushed from 5/12 to 5/27 due to transformer pad-mount availability; our work is 100% complete
- Original Milestone 4 draw date: 5/15 | Revised: 5/30
- Impact: CO-date unchanged (6/10); Milestone 4 draw shifts 15 days; Milestone 5 (final) unchanged
- Mitigation: None on our end — utility owns this schedule. Weekly utility-coordination calls confirmed.
- Cost: No change to contract. Developer's carrying cost extends 15 days.
- Ask: Acknowledge revised draw timing; no action otherwise.

**Output — Lender email:**

**Subject:** 908 SE Division — PGE meter set revised to 5/27; Milestone 4 draw shifts to 5/30

> **Priya,**
>
> Brief update on the 908 SE Division schedule. **PGE meter set revised from 5/12 to 5/27** due to pad-mount transformer availability on PGE's side. Our electrical scope is 100% complete and signed off at final inspection (4/22). **Milestone 4 draw timing shifts from 5/15 to 5/30.** CO date (6/10) and Milestone 5 (final) are not affected.
>
> **What happened.** PGE's service planner confirmed today the transformer they had slotted for our service is being reassigned to a storm-response replacement upstream. Our replacement transformer is now on PGE's 5/26 delivery — they'll pad-set and meter-set 5/27.
>
> **Mitigation.** None available on our side. Torres Electric has been on PGE's coordination call weekly since service application; this is a pure utility-schedule delay. We have the PGE service-planner written confirmation on file and can share on request.
>
> **Cost impact.** No change to our contract value. Owner carrying costs extend 15 days.
>
> **Schedule confirmation.**
>
> - Electrical final inspection: **complete 4/22**
> - PGE meter set: revised **5/27**
> - Milestone 4 draw: revised **5/30**
> - Milestone 5 draw (CO): **6/10 — no change**
>
> **Next update.** I will confirm the PGE meter-set occurred on or before 5/27 by EOD 5/27.
>
> — Mike Torres, Torres Electric (EC-00842) · (503) 555-0142

**Internal Notes:**

- PGE written confirmation is on file (email from service planner J. Kim, 4/24 2:15 PM). Do not share unless asked.
- Developer (Meridian's borrower) was notified 4/23 by separate email. Coordinate so story matches.
- No blame on PGE in writing despite the situation. Factual only.
- Follow-up: update our own project schedule with the revised PGE date; notify Westside Development PM.
- Consider: a single-sentence note to Priya's boss cc'd would be overkill — keep it recipient-direct.

---

## Example Output — Tariff-Event Section 232 switchboard delay (commercial GC audience)

**Inputs (abbreviated):**

- Customer: Daniel Stenhouse, PM, Hammond GC (GC on the Westmoreland MOB, a 4-story medical office building in Pittsburgh PA)
- Project: 4000 A 480Y/277 V service switchboard (Eaton Magnum DS) for Westmoreland MOB; Drysdale Electric is the Division 26 sub
- Delay cause: **TARIFF (primary) + SUPPLY (contributing)** — April 6, 2026 Section 232 restructure on HTS 8537.10 switchgear assemblies and HTS 7408 copper bar; Eaton re-quoted the switchboard at +6 weeks and +$33,040 (tariff delta × Drysdale's contracted 12% markup)
- Original switchboard delivery: 6/15 | Revised delivery: 7/27 (firm per Eaton allocation)
- Pre-event quote (Eaton via Consolidated, 3/14/26): $228,500 | Post-event re-quote (4/8/26): $254,700 | Delta: $26,200 + 12% markup = $29,344 (note: published $33,040 above includes the secondary HTS 7408 copper-bar delta of $3,696)
- Tariff rider: §12 of the executed proposal — Drysdale's standard Section 232/301 escalation clause is in place
- Impact: Substantial completion shifts 6 weeks (CO date 9/1 → 10/13); electrical final inspection shifts in step; the $33,040 will follow as CO-011 to Hammond GC channel
- Mitigation: pulled forward all conduit, gear pads, equipment GFP testing, and feeder rough-in so switchboard install is a 4-day energization once delivered
- Cost ask: Time extension is non-compensable per the contract's force-majeure-adjacent treatment of Federal tariff actions; price adjustment of $33,040 is compensable per §12 of the executed proposal; both follow as CO-011 in two paired lines

**Output — GC email:**

**Subject:** Westmoreland MOB — Section 232 tariff event 4/6/26; switchboard delivery to 7/27, CO-011 paired (6-wk time extension + $33,040 price delta)

> **Daniel,**
>
> Section 232 was restructured on 4/6/26 (HTS 8537.10 switchgear + HTS 7408 copper bar, 25% duty effective immediately). Eaton's allocation on the Westmoreland switchboard re-quoted Wednesday: **delivery moves from 6/15 to 7/27** (6 weeks) and the gear price moves from $228,500 to $254,700 (+$26,200 tariff delta, plus $3,696 on the copper-bar segment). Per §12 of the executed proposal, **CO-011 will follow with two paired lines: a 6-week non-compensable time extension, and a compensable price adjustment of $33,040** (tariff delta × 12% markup). Substantiation invoices are attached.
>
> **Tariff-Event citation.** Executive Order restructuring Section 232 signed 4/6/26; HTS 8537.10 switchgear assemblies and HTS 7408 copper bar both move to 25% from the prior bracketed rate. Effective date 4/6/26, applied to material crossing the border on or after that date.
>
> **What happened on this PO.** Pre-event Eaton quote via Consolidated Electric (3/14/26): $228,500, delivery 6/15. Post-event re-quote (4/8/26): $254,700, delivery 7/27. Two invoices on file (pre- and post-) match the $26,200 switchgear-assembly delta. Copper-bar re-quote from Eaton's bus-bar supplier dated 4/10/26 accounts for the $3,696 secondary delta.
>
> **Schedule vs. price — separated.**
>
> - **6-week schedule extension (non-compensable, per the Federal-tariff-action treatment in our subcontract §3.4 and force-majeure-adjacent precedent).** No contract-value request on this line. Substantial completion shifts from 9/1 to 10/13. Electrical final inspection shifts in step.
> - **$33,040 price adjustment (compensable, per §12 of the executed proposal).** This is the tariff delta on the switchboard + copper bar, multiplied by our contracted 12% markup. Will land as CO-011 within 3 business days.
>
> **What we're doing.** Conduit rough-in, gear pad pour and inspection sign-off, feeder run, and GFP equipment-test paperwork are all being pulled forward. Switchboard install drops to a 4-day energization once Eaton delivers, so we hold the revised 10/13 substantial completion firm. Drysdale will absorb the schedule-rework labor — that is not in the CO.
>
> **What I need from you.** Acknowledge receipt of this notice today so the CO-011 paperwork (in your inbox separately) lands cleanly. The Owner's Rep will need to co-approve per §12.3 of our subcontract — please copy them when you forward.
>
> **Next update.** I will confirm the 7/27 Eaton allocation hold by EOD Friday 4/26.
>
> — Mike Drysdale, Drysdale Electric (PA-EC-008772) · (412) 555-0188 · mike@drysdaleelectric.com

**Internal Notes:**

- **Cause category: TARIFF (primary) + SUPPLY (contributing).** Tariff is the *but-for* cause; supply is the downstream lead-time blowout. Order in message: tariff first.
- **Tariff-rider reference:** §12 of executed proposal (Drysdale standard Section 232/301 escalation clause). Verified before sending.
- **CO-011 must match this message line-for-line.** EO citation, HTS categories (8537.10 + 7408), pre/post invoices, delta amounts, markup %, and the 6-week schedule extension all flow into `admin/change-order-drafter.md` Tariff-Event Variant §5d. Route the CO draft through that skill — do not improvise the CO from this delay message.
- **Substantiation invoices on file:** Eaton/Consolidated pre-event quote 3/14, post-event re-quote 4/8, copper-bar supplier re-quote 4/10. PDFs in `westmoreland-mob/tariff-event-2026-04-06/` folder.
- **Force-majeure-adjacent treatment of Federal tariff actions** is per subcontract §3.4 (Hammond's standard sub form); confirmed with internal counsel 4/9 that Section 232 EO actions are within scope of the FM-adjacent clause.
- **GC relationship:** Hammond is the #1 GC for Drysdale's healthcare pipeline. Tone is collegial; substantiation is exhaustive on purpose so the CO is approved on first review.
- **Owner's-rep loop:** Per §12.3 of our subcontract, the Owner's Rep (Westmoreland Health Pavilion's PM) must co-approve any CO > $25K. Don't ship CO-011 to Hammond without the Owner's Rep already cc'd.
- **Follow-up:** route CO-011 through `admin/change-order-drafter.md` Tariff-Event Variant; verify the executed tariff rider language is the same revision as the one cited; pre-stage the substantiation invoices in the CO package.
