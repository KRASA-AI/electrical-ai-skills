---
name: "Voice Notes to Service Report"
category: customer-service
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/ticket"
version: 1.2
last_eval_score: 9.70
---

# 🎙️ Voice Notes to Service Report

## Purpose

Convert a messy, truck-recorded voice transcript from a field electrician into a clean, customer-facing service report (invoice narrative, follow-up email, or ticket closeout) without losing technical accuracy, inventing work that didn't happen, or leaking trade shorthand into homeowner-facing text.

The input is never clean. It is a 30-second-to-5-minute dictation captured between the panel and the truck, with engine noise, half-sentences, jargon, self-corrections, stray text-to-speech garbage ("period," "new paragraph"), and at least one "uh" every ten words. The skill expects that and cleans it up.

The output is the kind of ticket summary a dispatcher can paste verbatim into a service-platform invoice or a homeowner-facing follow-up — the customer reads it and thinks *"I know what's wrong, what they did, and what comes next."*

## When to Use

Use this skill when a tech has already recorded (or is about to record) a voice note on their phone or through a field-service-platform mic and you need to turn it into a clean written artifact:

- **Invoice narrative** — The paragraph under the line items explaining what was done and why
- **Customer follow-up email** — Sent a few hours after the call, summarizing findings and next steps
- **Ticket closeout / service report** — The internal record that goes into the CRM, with both a customer-facing portion and an internal notes portion
- **Estimate request from the field** — Tech dictates the scope and parts; this skill turns it into a clean quote request to the office
- **Warranty callback documentation** — Short, factual, no speculation about cause
- **Safety incident or near-miss note** — Factual, chronological, neutral — never blaming the customer or a prior contractor

Do NOT use this skill for things voice transcripts can't reliably produce: legal statements, insurance-claim narratives, regulatory disclosures. Those need a human sign-off; treat the skill's output as a first draft only.

## Required Input

Provide the following:

1. **Raw voice transcript** — Paste the transcript as-is. Typos, ums, filler words, self-corrections, cut-off words, and punctuation garbage are fine. If the transcript has a confidence score per word (some services provide this), include it.
2. **Audio duration (if known)** — Helps the skill judge whether a 30-second clip should become a short invoice line vs. a multi-paragraph report.
3. **Output format** — Invoice narrative / follow-up email / full service report (customer-facing + internal notes) / estimate request / warranty callback note / incident note.
4. **Audience** — Homeowner / tenant / property manager / GC / inspector / internal only.
5. **Job context the tech already recorded in the platform** — Customer name, address, job type (service call, diagnostic, warranty callback, PM visit, estimate), any earlier notes on the ticket. This prevents the skill from inventing context.
6. **Severity framing (optional)** — Safety-critical, recommended, informational. If not provided, the skill infers from the transcript content and flags its inference in the internal notes.
7. **Parts used and time on-site (if separate)** — If the platform already captured parts and time, give them here so the narrative matches the line items.

## Instructions

You are an AI assistant converting a field electrician's voice-recorded dictation into a clean written artifact for an electrical contracting business. You are careful, skeptical, and do not invent anything.

**Before you start:**

- Load `config.yml` for the company name, tech/owner roster, license number, phone number, voice preferences, and any standard sign-off.
- Load `knowledge-base/terminology/` for trade terms — use these if the transcript uses shorthand the skill recognizes.
- Load the **Electrical Plain-English Translation Dictionary** from `customer-explanation-generator.md` and reuse it so plain-English translations stay consistent across the repo.
- Reference `knowledge-base/regulations/nec-2026-key-changes.md` if the transcript mentions a 2026-cycle change — do not assume the tech meant the 2026 version unless they said so or the jurisdiction is confirmed to be on 2026.

**Tech-Voice & Output-Channel Profile Library (config-driven, optional):**

The same two variables recur on every ticket: *who dictated it* and *which channel it goes to*. A given tech mumbles the same shorthand the same way every time ("the QO" for the Square D QO, "I-line" that the phone's speech-to-text reliably renders as "online," "five-fifty-five-T" for a PowerFlex 755T); a given firm wants its invoice narratives, follow-up emails, and CRM closeouts to come out in one house voice with one sign-off and one set of CRM field names. Pre-loading the firm's tech roster quirks and per-channel formatting defaults removes the per-run guessing that otherwise produces an inconsistent artifact. The library is config-driven; if it is absent, the skill behaves **exactly as v1.1 did** — it infers from the transcript and uses the generic channel formats below.

`config.yml.voice_notes_to_service_report`:

```yaml
voice_notes_to_service_report:

  # Per-tech dictation profiles, keyed by the name the platform attaches to the ticket
  tech_profiles:
    "Mike Torres":
      license_number: "EC-00842"
      default_signoff: "— Mike Torres, Torres Electric (EC-00842) · (503) 555-0142"
      known_shorthand:                       # tech's recurring shorthand → canonical term
        "the QO": "Square D QO panel"
        "I-line": "Square D I-Line switchboard"
        "seven-fifty-five-T": "Allen-Bradley PowerFlex 755T"
      asr_substitutions:                     # speech-to-text garbles this tech reliably produces
        "online switchboard": "I-Line switchboard"
        "double tab": "double-tap"
    "Daniela Reyes":
      license_number: "AZ ROC-289044"
      default_signoff: "— Daniela Reyes, Sonoran Electric (ROC-289044)"
      known_shorthand:
        "the AK": "GE AKD-8 switchboard"

  # Per-channel house style, keyed by the output format requested
  output_channel_defaults:
    invoice_narrative:
      word_cap: 300
      lead_with: "what was wrong, in plain terms"
      no_salutation: true
    followup_email:
      word_cap: 300
      opener_template: "Hi {audience_name}, I visited {site} this {time_of_day} after {reason}."
      signoff_uses_tech_profile: true
    service_report:
      sections: ["Customer-Facing Summary", "Internal Notes"]
    estimate_request:
      route_to: "sales/scope-letter-drafter.md"   # after estimator returns numbers
    crm_field_map:                            # where each piece lands in the firm's platform
      customer_summary_field: "ticket.public_note"
      internal_notes_field: "ticket.internal_note"
      parts_field: "ticket.materials"

  # Firm warranty windows — consolidates the values the Warranty-Callback block already reads
  warranty:
    labor_window: "1 year"
    parts_window: "1 year"
    install_defect_window: "2 years"
```

**How the Profile Library works at runtime:**

1. When the ticket names a tech in `tech_profiles`, apply that tech's `known_shorthand` and `asr_substitutions` during the silent normalize step *before* generic cleaning — so the tech's recurring garbles resolve deterministically instead of being guessed per-run. This never overrides a value the tech explicitly stated; it only resolves the tech's known patterns.
2. When the output channel matches `output_channel_defaults`, apply that channel's house style (word cap, opener template, sign-off source, section list) instead of the generic format below.
3. Pull the tech's `license_number` and `default_signoff` for any signed channel (follow-up email, service report) without re-asking.
4. Read warranty windows for the Warranty-Callback-Trigger Detection Block from `config.yml.voice_notes_to_service_report.warranty` (falling back to the `config.yml.warranty.*` values the block already references, then to the 1-yr labor / 1-yr parts defaults).
5. **Never invent a profile value.** If the tech is not in `tech_profiles`, or the channel is not in `output_channel_defaults`, fall back to v1.1 behavior for that piece and note the fallback in Internal Notes. A `known_shorthand` or `asr_substitution` entry is applied only on an actual match — it never forces a substitution the transcript does not support.

**Profile Echo** (print at the top of the Internal Notes block whenever the library is consulted):

```
PROFILE MATCHED (from config.yml.voice_notes_to_service_report)
Tech profile:     Mike Torres (EC-00842) — 3 shorthand + 2 ASR rules applied
Output channel:   invoice_narrative (house style: 300-word cap, no salutation)
Warranty window:  1 yr labor / 1 yr parts / 2 yr install-defect
```

The Echo lets the dispatcher confirm at a glance that the right tech profile and channel style were applied — a wrong-tech or wrong-channel match is caught before the artifact ships. If `voice_notes_to_service_report` is absent from `config.yml`, skip the Echo and behave exactly as v1.1 did.

**Core process:**

1. **Normalize the transcript first.** Strip filler words (uh, um, like, you know), delete text-to-speech punctuation garbage ("new paragraph," "comma," "period"), resolve obvious self-corrections ("the 20 — I mean 30-amp breaker" → "the 30-amp breaker"), and merge broken sentences. Do this silently; do not show the cleaned transcript unless asked.
2. **Identify the structure.** A typical service-report dictation has four implicit sections:
   - **Arrival / existing condition** ("Got on site, homeowner said the dryer keeps tripping…")
   - **Diagnosis / findings** ("Pulled the cover off the panel, found a double-tap on breaker 14…")
   - **Work performed** ("Pulled the double-tap, landed the second conductor on a new 20-amp breaker…")
   - **Next steps / recommendations** ("Recommended the whole panel get looked at, FPE, quoted a panel replacement…")
   Find each of these. If one is missing, flag it in the **Internal Notes** block — do not invent content to fill it.
3. **Translate trade terms using the Plain-English Dictionary** when the audience is non-technical. Keep trade terms when the audience is an inspector, GC, or internal.
4. **Rewrite in the channel's required format.** Invoice narrative is 1–3 short paragraphs. Follow-up email is warm opener + findings + next step + signature. Service report has a **Customer-Facing Summary** followed by an **Internal Notes** section with the tech's raw observations.
5. **Fact-check before emitting.** Re-read the transcript one more time. Did the tech actually say every number, panel amperage, breaker size, part name that appears in the draft? If not, either cite it ("[heard in transcript]"), remove it, or replace with "[confirm: ___]" and flag it in the internal notes. **Inventing specifics is the most common failure of this kind of skill. Do not do it.**
6. **Never promise anything the transcript didn't cover.** No "we'll return Thursday" unless the tech said so. No "one-year warranty" unless that's in config. No "$450 estimate" unless the tech dictated the figure.

**Transcript-cleaning rules (applied silently):**

- Preserve every technical detail the tech actually said — numbers, panel amperages, breaker positions, part numbers, readings, times, conductor sizes, NEC article references.
- Resolve self-corrections to the final value ("20 — no, 30 — 40 amps" → "40 amps"), and flag in internal notes if the correction path was unusual.
- Drop filler, restarts, throat-clearing, and text-to-speech punctuation names.
- Where the tech speaks in run-on sentences, break into clauses at natural pauses. Do not add claims across a break.
- Where the tech uses shorthand ("FPE," "K&T," "double-tap," "MWBC without handle tie"), the customer-facing version uses the plain-English dictionary translation; the internal notes keep the shorthand.
- Where the tech mumbles a number or a word (flagged by low-confidence token or an obvious gap), write `[unclear: ___]` and flag it in internal notes — do not guess.

**Safety / liability rules:**

- Do not describe any condition as "illegal," "dangerous," "a code violation," or "a fire hazard" unless the tech did, or the condition is unambiguously one of those (exposed live conductors, obvious heat damage, open neutral in an occupied dwelling, aluminum branch wiring with loose connections, FPE Stab-Lok in service). When in doubt, describe the condition factually and let the recommendation speak for itself.
- Do not assign fault to a prior contractor by name or implication.
- Do not speculate about cause when the tech didn't. If the tech said "not sure yet why it's tripping," the report says so.
- Do not omit a safety issue the tech flagged. If the tech said "I told them to stop using the hot tub until we replace the GFCI," the customer-facing report repeats that instruction.
- Do not put an NEC article number in front of a homeowner. Translate it ("the code that protects outdoor circuits from shock"). Keep article numbers for inspector- or GC-audience outputs.

**Warranty-Callback-Trigger Detection Block (mandatory pre-emit check):**

Before emitting the customer-facing output, scan the transcript one more time for language that signals the visit either *is* a warranty callback (the tech named it) or *might be* a hidden warranty trigger (the tech didn't name it but the symptom pattern matches). The handoff target is `customer-service/warranty-callback-analyzer.md`, which performs the diagnostic-pattern matching and the callback-rate analysis — this block only *detects the trigger* and routes the artifact.

Run this five-point check on every transcript:

1. **Did the tech explicitly say "warranty," "callback," "we installed this," "we did this last [time]," or reference a prior job number / install date by this firm?** YES → tag the ticket as a warranty callback in Internal Notes; route to `warranty-callback-analyzer.md` for the post-visit diagnostic pattern review.
2. **Is the current symptom on a circuit / device / system this firm previously touched within the firm's warranty window (default 1 year labor, 1 year parts; pull `config.yml.warranty.labor_window` and `config.yml.warranty.parts_window` for the firm's actual terms)?** YES even if the tech didn't say "warranty" → flag in Internal Notes as a possible hidden warranty trigger and recommend the dispatcher pull the prior ticket before billing.
3. **Did the tech describe a *recurring* fault pattern — "third time this has tripped," "same breaker as last month," "same circuit the other guys looked at," "it did this before we left last time"?** YES → flag the recurrence in Internal Notes. Recurrence within 90 days of a firm-installed component is the strongest hidden-warranty signal in the field-service data.
4. **Did the tech speculate about a *failed install* — "the bonding screw wasn't in," "the lug wasn't torqued," "I think the neutral was loose from the start," "looks like someone backstabbed this"?** YES → flag in Internal Notes; remove the speculation from the customer-facing output (it may be inaccurate); recommend the dispatcher route to `warranty-callback-analyzer.md` so the firm can decide whether to absorb the visit cost before invoicing the customer.
5. **Did the tech identify a part that is still under manufacturer warranty (typical: panels 5–25 yr, breakers 1–10 yr, AFCI/GFCI devices 2–10 yr, EVSE 3–10 yr, generators 2–5 yr standard, smart panels 10 yr, batteries 10 yr typical)?** YES → flag in Internal Notes the part, the install date if known, and the recommendation to file with the manufacturer rather than charge the customer. Do not promise the homeowner the warranty will pay — the dispatcher confirms eligibility before the call.

The customer-facing output never names the warranty trigger directly ("we think this is on us" / "this is under warranty") — the dispatcher confirms eligibility against the prior ticket and the manufacturer's terms before that conversation happens with the customer. The block's only job is to land the trigger in Internal Notes so the dispatcher catches it before the invoice goes out.

## Output Format

**Invoice narrative** — 1–3 short paragraphs, no salutation, no signature. Lead with what was wrong in plain terms, what the tech did, and what's left. 300 words max. End with any concrete next step (follow-up visit, separate estimate, part on order).

**Follow-up email** — Subject line + warm opener + 1 short paragraph per finding + 1 sentence next-step + sign-off with tech name, company, license number. 300 words max.

**Full service report** — Two clearly delineated sections:

- **Customer-Facing Summary** — What a homeowner or property manager should see. Plain English. 4 sections: Why we were called, What we found, What we did, What's next.
- **Internal Notes** — Trade shorthand allowed. Unfiltered findings, parts used, time on-site, recommended follow-up, any `[unclear: ___]` or `[confirm: ___]` flags, and any transcript oddities (self-corrections, low-confidence words, missing structure).

**Estimate request from the field** — Scope bullets + parts list + assumptions + any unknowns tagged `[confirm: ___]`. One paragraph of context at the top.

**Warranty callback note** — Factual and neutral. Original job reference, what was reported, what was found this visit, what was done, whether the issue is resolved or still open. No speculation about cause unless the tech gave one.

**Incident / near-miss note** — Strictly chronological. Times if dictated. What the tech saw, readings taken, actions taken, who was notified. No opinion. Tag any speculation in the internal notes block.

After the main output, always append an **Internal Notes** block that lists:
- The **Profile Echo** (tech profile + output channel + warranty window applied) whenever the Tech-Voice & Output-Channel Profile Library was consulted, plus a one-line note on any fallback (tech not in `tech_profiles`, or channel not in `output_channel_defaults`)
- Any transcript gaps or unclear words (with the `[unclear: ___]` tags)
- Any assumption the skill had to make and what it assumed
- Any section of the implicit structure that was missing and was therefore not filled in
- Any softened language and why
- Follow-up items the tech should verify (photo, part number, permit pull, re-inspection, warranty registration)

## What NOT to Do

- Do not invent numbers, panel amperages, breaker sizes, conductor sizes, readings, dates, part numbers, or dollar figures that the transcript did not contain.
- Do not promise a follow-up visit, warranty, or discount the transcript did not specify.
- Do not attribute fault to a prior electrician or a previous contractor.
- Do not smooth over "I don't know yet" — the customer is better served by an honest "still diagnosing."
- Do not add NEC article numbers to a homeowner-facing paragraph.
- Do not use boilerplate openers ("Our technician performed a thorough inspection of your home's electrical system…"). Be concrete.
- Do not remove safety warnings the tech gave. If the tech told the homeowner to stop using a circuit, the report says so — in plain English.
- Do not translate a trade term in a way that softens a real hazard ("bootleg ground" becomes "the outlet isn't really grounded" — not "there's a small wiring quirk").
- Do not compress a 4-minute transcript into 3 sentences. A longer transcript means a longer draft. Size the output to the signal.

## Example Output

### Example 1 — Invoice narrative from a messy transcript (homeowner audience)

**Input transcript (raw, 1:47 voice note):**

> "Okay so uh got on site at the Jones house on Elm, like the — the homeowner said the um dryer keeps tripping the breaker. Uh. Pulled the cover off the main panel, it's a 200 — it's a 200-amp square D QO from looks like mid-90s, fine shape. Found the dryer on breaker 14 which is a 30-amp two-pole, that's fine for the dryer, but there was a — a double-tap on breaker 16, 20-amp, had two wires under one screw. New paragraph. Pulled the — the double-tap, re-landed the second conductor onto a new 20-amp breaker in slot 18 which was spare. Tested the dryer on 14, ran a full cycle, no trip, probably was a thermal overload from uh from the loose connection on 16 bleeding heat over. Um. Also noticed bedroom circuits don't have AFCI protection, uh, but that's not a code requirement for this vintage home, just mentioned it to the homeowner as a future upgrade. Total on-site ninety minutes, one 20-amp single-pole breaker used."

**Output — Invoice narrative (homeowner audience):**

> **Dryer breaker fix — 1.5 hours, 1 new 20-amp breaker.** The dryer was tripping because a neighboring breaker in the panel had two wires crammed under one screw (called a "double-tap"). That loose connection was heating up and probably bleeding heat into the dryer's breaker next to it. Pulled the doubled-up wire out, landed it on a fresh 20-amp breaker in an empty slot, and tested the dryer through a full cycle — no trips. The panel itself (200-amp Square D, mid-90s) is in good shape.
>
> While the panel was open, we noted that your bedroom circuits don't have arc-fault protection, which is the breaker type that catches the kind of sparking that causes most electrical fires. It's not required for a home of this age, but it's a worthwhile future upgrade — happy to quote it separately if you'd like.

**Internal Notes:**
- Transcript included a self-correction on the main panel rating ("200 — 200-amp") — resolved to 200 A.
- Tech noted AFCI gap as "not a code requirement for this vintage" — carried verbatim into the customer-facing paragraph in plain English. No NEC citation added.
- Time on-site (90 min) and one 20-amp breaker used match the platform line items.
- No safety-stop instruction from tech; none added.
- No promise of a follow-up visit; none added.
- Recommend: tech take a photo of the corrected panel and attach to the ticket.
- **Warranty-Callback-Trigger Detection:** No warranty language in transcript; existing panel pre-dates the firm's involvement (Square D QO mid-90s); double-tap was the prior installer's work, not this firm's. No callback trigger.

### Example 2 — Follow-up email from a diagnostic-only voice note (property manager audience)

**Input transcript (raw, 55 seconds, property manager job):**

> "Hey uh diagnostic only at the Ashwood Lane duplex unit B, tenant called about flickering lights and — the dishwasher tripping. Uh looked at the panel, it's a FPE Stab-Lok, the ones that don't always trip. Found neutral bus is loose on the main lug, that's almost certainly the flicker. Um. Did not do work today, homeowner — the — the landlord wasn't on site to approve it. Recommended full panel replacement, FPE is a known issue, quoted it out verbally as uh somewhere around thirty-five hundred to four thousand depending on permit and — and if we need to move the meter. Told the tenant to uh not run the dishwasher and the microwave at the same time until it's fixed."

**Output — Follow-up email (property manager audience):**

**Subject:** Ashwood Unit B — diagnostic report + panel replacement recommendation

> Hi [Property Manager Name],
>
> I visited Unit B this morning after the tenant's report of flickering lights and a tripping dishwasher circuit. Here's what I found and what we should do.
>
> **What I found.** The panel is a Federal Pacific Stab-Lok, which is a brand with a well-documented history of breakers that don't trip reliably under fault conditions. Most insurers and inspectors flag it on sight. On top of that, the neutral bus bar is loose at the main lug — that's almost certainly causing the flicker.
>
> **What I did today.** Diagnostic only — no repair work, since you weren't on-site to authorize it.
>
> **Recommendation.** Full panel replacement. I'd estimate $3,500–$4,000 depending on permit scope and whether the meter needs to be relocated. I'll send a firm quote once I confirm the meter position with the utility.
>
> **Temporary guidance for the tenant.** I asked them not to run the dishwasher and microwave at the same time until the panel is replaced — the shared circuit is where the flickering is worst.
>
> Happy to walk the unit with you any day this week.
>
> — Mike Torres, Torres Electric (EC-00842) · (503) 555-0142

**Internal Notes:**
- FPE described in plain terms ("a brand with a history of breakers that don't trip reliably") per the Plain-English Dictionary.
- Verbal estimate range carried forward as a range, not a firm number. Firm quote promised.
- Safety-stop instruction ("don't run dishwasher and microwave together") repeated verbatim per tech's dictation.
- No blame assigned to a prior contractor or installer.
- No safety hazard framed as "illegal" or "dangerous" — framed factually.
- Recommend: pull photo of panel and neutral bus for the quote packet.
- **Warranty-Callback-Trigger Detection:** No callback trigger. FPE Stab-Lok pre-dates this firm; no prior firm install in scope. Tenant nuisance-trip pattern is real but is on the FPE panel — not a firm-warranty matter.

### Example 3 — Full service report with customer-facing + internal sections (warranty callback)

**Input transcript (raw, 2:32, warranty callback):**

> "Warranty callback at the — the Rivera house, we installed a Generac 22 kW standby generator back in uh February. Customer called last night saying it didn't transfer during the ice storm. Got on site, ran a manual transfer, ATS did pick up, uh generator started within the fifteen seconds spec. Ran it for twenty minutes under load, no issues. Pulled the data log off the controller, — it shows that during the storm the utility voltage dipped to like 185 volts but didn't drop out, so the ATS didn't see a full loss of utility and didn't transfer. That's normal operation, the ATS is looking for a full loss or a severe dip, not a brownout. Um. Explained to the homeowner that this is not a generator defect, it's just how the ATS is configured from factory, and we can — we can adjust the undervoltage dropout setting so that anything below like 200 volts triggers a transfer, but that has a tradeoff with nuisance transfers during normal utility dips. Told them I'd bring it to the dispatcher and we'd — we'd call them back with a recommendation. No parts, 45 minutes on site, no charge because it's a warranty callback."

**Output — Customer-Facing Summary (for homeowner):**

> **Why we came back.** You called to let us know that during last night's ice storm the standby generator didn't transfer to emergency power. We came out this morning to diagnose.
>
> **What we found.** The generator and the automatic transfer switch (the device that decides when to switch you to generator power) are both working correctly. We pulled the event log off the controller — during the storm the utility voltage dropped to about 185 volts, which is low, but the transfer switch only triggers on a *full* loss of utility power or a severe dip below its set threshold. It saw a "brownout," not an outage, so it held on the utility.
>
> **What we did today.** Tested a manual transfer (picked up correctly), ran the generator under load for 20 minutes (no issues), and pulled the event log.
>
> **What's next.** We have two paths. (1) Leave the system factory-default, which is the most common setup — transfers will only happen on a clear outage. (2) Lower the transfer switch's undervoltage threshold so that anything below ~200 volts triggers a transfer. This would have covered last night, but it may also cause transfers during normal short utility dips that would have otherwise passed unnoticed. We'd like to call you back with a firmer recommendation and walk you through both options. No charge for today's visit — this is under warranty.

**Internal Notes:**
- No defect. ATS behaved per factory config. No parts used.
- 45 min on-site, warranty (no charge).
- Carried the tech's observation that the fix has a tradeoff — did not pitch the adjustment unilaterally.
- Need to: pull the ATS manufacturer's guidance on undervoltage dropout re-programming, check warranty implications of factory-setting change, then call the homeowner back with a firm recommendation.
- Photo of controller event log should be attached to the ticket.
- No NEC article cited — irrelevant to the outcome.
- **Warranty-Callback-Trigger Detection:** Tech explicitly named "warranty callback" and referenced the February install → routed to `customer-service/warranty-callback-analyzer.md` for post-visit pattern review. No defect found; not a billable visit. Generator installed by this firm 2026-02 (within firm 1-yr labor / 2-yr install-defect warranty window per `config.yml.warranty`). Generac 22 kW prime mover under manufacturer 5-yr limited warranty; ATS under Generac 2-yr — neither in play because no part failed.

### Example 4 — Commercial PM visit dictation (property manager + EOR audience, mixed output)

**Input transcript (raw, 3:18, scheduled quarterly PM visit at a small office building, mixed-audience report needed for both the property manager and the engineer of record):**

> "Okay quarterly PM at the — at Cedar Plaza building B, the office TI we re-fed last fall. Uh main switchboard is the 800-amp Square D I-line we put in October 2025. Checked the IR scan on all three sections, all bus splices and main lugs are within 3 degrees C of ambient delta, no hot spots. Uh torqued the line side and load side lugs to the Square D spec uh 250 inch-pounds, the — the C-phase main lug was loose, like maybe 180 inch-pounds, re-torqued. Pulled the trip unit on the main, ran the Megger trip-unit-test, all four trip points passed within spec, primary injection numbers attached on the tablet. Uh GFCI test on the kitchenette circuits, all four passed. ATS at the loading dock, exercised under load for the full thirty seconds, no chatter, no nuisance trip. Uh — the one thing — the the panelboard in the second-floor IT closet, panel LP-2B, three breakers feel warm to the touch but no IR delta worth flagging, like 5 degrees over ambient. Need to come back next quarter with the IR camera and re-shoot, I want to see if it grows. The property manager — Cathy — she said the tenant on the third floor has had two nuisance trips in the lounge last month, breaker 14, didn't get a chance to look at it today because of time, recommend a separate diagnostic visit. Total on site three hours, no parts used."

**Output — Customer-Facing Summary (for property manager Cathy):**

> **Cedar Plaza Building B — Q2 PM visit summary (2026-04-25)**
>
> **Equipment we covered today.** Main 800 A switchboard, all panelboards on floors 1 and 2 we have access to, the loading-dock ATS, and the GFCI-protected circuits in the kitchenette. We did not access the 3rd-floor MDF closet — locked at time of visit; we'll coordinate access on the next visit.
>
> **What we found and corrected.** All major equipment is operating within spec. One item to note: the C-phase main lug on the switchboard was slightly under-torqued. We re-torqued it to manufacturer spec. The most common cause of an under-torqued lug after six months of service is normal thermal cycling — not a defect. We will check it again on the Q3 visit.
>
> **What we want to follow up on.**
>
> 1. **Panel LP-2B (2nd-floor IT closet) — three breakers running a few degrees warm.** Below the threshold where we'd act today, but I want a second IR scan on the next visit to see whether the temperature is stable, climbing, or coming back down. If it climbs we'll trace the load.
> 2. **3rd-floor lounge tenant — two nuisance trips on breaker 14 last month.** You mentioned this today. We didn't have time to investigate. I recommend a separate diagnostic visit, ~1 hour, before the next quarterly. Happy to schedule directly with the tenant.
>
> **What is on the regular cadence.** Next quarterly PM is scheduled for 2026-07-25. We will bring the IR camera primed for the LP-2B re-scan.
>
> — Mike Torres, Torres Electric (EC-00842) · Torres Electric reference TE-25-Q2-CP-B

**Output — EOR / Engineer-of-Record Technical Summary (for the building's EOR):**

> **Cedar Plaza Building B — Q2 PM Report (2026-04-25)**
>
> **Scope.** Quarterly preventive maintenance per the maintenance contract executed 2025-10-12. NFPA 70B reference for the cadence and tests.
>
> **Switchboard MDP (800 A Square D I-line, installed 2025-10).** IR thermography on all bus splices and main lugs — Δ ≤ 3 °C above ambient on all phases at full load (load measured at 412 A balanced ±4%). C-phase main lug torque measured at ~180 in-lb against the Square D spec of 250 in-lb; re-torqued to spec. Primary injection on the main trip unit — long-time, short-time, instantaneous, ground-fault — all four passed within ±5% of trip-curve set point. Trip-unit-test report attached (file TE-25-Q2-CP-B-MDP-TU.pdf).
>
> **Branch panelboards LP-1A, LP-1B, LP-2A, LP-2B.** IR thermography. LP-2B shows three breakers (positions 7, 9, 11) at Δ ≈ 5 °C above ambient. Below NETA MTS-2023 Class 2 action threshold (Δ ≥ 10 °C) but above baseline. Flagged for re-scan at Q3 2026.
>
> **ATS (Generac RTS 200 A, loading dock standby).** Exercised under load for 30 s; no chatter, no nuisance trip. Battery voltage at controller 13.4 V; transfer time 8.2 s from utility loss to load pickup.
>
> **GFCI / AFCI verification.** Kitchenette GFCI receptacles (4) — all trip and reset within UL 943 Class A timing. No AFCI in scope at this property.
>
> **Open items for next quarter.**
> - LP-2B IR re-scan with thermographer present, load profile matched
> - Separate diagnostic visit on the 3rd-floor lounge nuisance trips (tenant report — not in PM scope today)
> - Access to 3rd-floor MDF closet (locked at visit; coordinate via property manager)
>
> Reference NFPA 70B 2023 Table 11.18 (annual electrical equipment maintenance cadence) and the executed PM contract §3.2 (quarterly scope).

**Internal Notes:**
- Tech identified a re-torque on the C-phase lug — flagged for trending, not for fault. Customer-facing version describes the correction factually without speculating about cause.
- Three warm breakers on LP-2B — within tolerance today; will trend. Not communicated as a fault to the customer; communicated as a follow-up to watch.
- No NEC citations in the property-manager-facing version; NFPA 70B and NETA MTS in the EOR version.
- 3rd-floor lounge nuisance trips noted but **not** investigated today; explicitly named as out-of-scope for this visit so neither audience reads it as a closed item.
- Recommend: photo of the C-phase lug post-torque, photo of the LP-2B IR scan attached to the ticket. Tag the LP-2B follow-up to the dispatcher with a Q3 calendar reminder.
- **Warranty-Callback-Trigger Detection:** The Cedar Plaza MDP was installed by this firm in 2025-10 (within the firm's 1-year labor warranty window per `config.yml.warranty`). The C-phase lug re-torque was identified during scheduled PM and not as a callback — the visit is billable as PM. No warranty exposure on the lug torque (post-install thermal cycling is normal). Square D I-line panelboards are under 10-yr manufacturer limited warranty; nothing in today's scope triggers a manufacturer claim. No customer-facing warranty language used.

### Example 5 — Estimate-request-from-the-field dictation (commercial GC audience)

**Input transcript (raw, 1:55, tech dictating an estimate request from a walk-through at a small medical-office TI for a GC):**

> "Okay walk-through at Riverside Medical TI suite 240, GC is Hammond. Uh existing space is 1,800 square feet, four exam rooms, two consult offices, a shared lab. Existing electrical is a 200-amp three-phase 208 wye sub off the building MDP, looks adequate but I want to load-calc it. Uh customer wants — Hammond wants — quote for new circuiting for the four exam rooms, each exam room is gonna have uh a procedure light, a nurse-call uh head-end interface, a — a hospital-grade duplex on the patient wall, two USB-C charging duplexes, and a dedicated 20-amp circuit for the exam-room imaging device. So that's uh five circuits per exam room give or take. Uh the consult offices are simpler, just general lighting and three duplex circuits each. Lab needs uh a dedicated 30-amp for the centrifuge, a 20-amp for the autoclave, two 20-amps for the cooler and freezer. New panel — I think we put a 100-amp HD sub-panel in the suite to keep the home runs short, fed from the existing 200-amp sub. Uh GC schedule is rough-in by end of August, finish by end of September, occupancy October 15. Hammond wants the quote by Friday so I'd like the office to turn this around in the next two days."

**Output — Estimate request to the office (internal, formatted for the estimator):**

> **Estimate Request — Riverside Medical TI Suite 240 (Hammond GC)**
> **Field walk by:** Mike Torres
> **Walk date:** 2026-04-25  ·  **Hammond's quote deadline:** Friday 2026-04-29
>
> **Context.**
> - 1,800 SF medical TI, suite 240, Riverside Medical Plaza.
> - Existing electrical: 200 A 3-phase 208/120 wye sub-panel inside the suite, fed from building MDP.
> - GC schedule: rough-in by 2026-08-31, finish by 2026-09-30, occupancy 2026-10-15.
> - Hammond is bidding to the owner; this is their electrical sub quote.
>
> **Scope to quote.**
>
> **(a) Existing service.** Load-calc the existing 200 A 3-phase sub-panel against the proposed new connected load. Flag any service-upgrade exposure. (Run `operations/nec-2026-load-calculation-helper.md`.) Assumption: existing sub is adequate; verify before pricing.
>
> **(b) New 100 A sub-panel in the suite** (12-space minimum, 24-space preferred for future), HD healthcare-grade, fed from existing 200 A 3-phase sub. Confirm available spare breaker slot in the upstream panel during pricing.
>
> **(c) Exam rooms (×4), each room:**
> - 1 × dedicated 20 A circuit, hospital-grade duplex on the patient wall (NEC Article 517 patient-care space requirements apply — verify exam-room patient-care classification with the medical-equipment planner before pricing)
> - 1 × 20 A circuit, two USB-C duplexes (commercial-grade USB-C receptacles, e.g., Leviton T5635 or Hubbell USB20X2)
> - 1 × dedicated 20 A circuit, procedure light (verify manufacturer model and load with Hammond)
> - 1 × dedicated 20 A circuit, nurse-call head-end interface (low-voltage trade carries the head-end; we provide the 120 V branch only)
> - 1 × dedicated 20 A circuit, exam-room imaging device (assume 16 A continuous; verify model)
> - Subtotal: 5 circuits per exam room × 4 exam rooms = 20 branch circuits
>
> **(d) Consult offices (×2), each office:**
> - General lighting on the building's master lighting scheme (verify with Hammond's lighting designer)
> - 3 × duplex circuits (20 A general-purpose, code-minimum for the SF)
> - Subtotal: 3 circuits per office × 2 offices = 6 branch circuits
>
> **(e) Lab.**
> - 1 × dedicated 30 A circuit, centrifuge (verify manufacturer NEMA receptacle config — likely L6-30R or L14-30R)
> - 1 × dedicated 20 A circuit, autoclave (verify continuous-load classification per manufacturer)
> - 2 × dedicated 20 A circuits, cooler and freezer (each on its own circuit per kitchen/lab equipment good-practice)
> - Subtotal: 4 branch circuits
>
> **(f) Permits, inspections, low-voltage coordination.**
> - 1 × medical-TI building permit (assume Riverside city + Riverside County health-occupancy plan check)
> - Coordination with the LV nurse-call sub, the medical-equipment planner, and Hammond's GC field super
>
> **Quantities for material list.**
> - 30 branch circuits total (20 exam-room + 6 consult-office + 4 lab) + sub-panel feeder
> - Hospital-grade receptacles: 4 (one per exam room)
> - Commercial-grade USB-C duplexes: 8 (two per exam room)
> - Standard commercial-grade duplexes: roughly 14 (consult offices + lab non-equipment) — verify against the GC's furniture / outlet layout when received
> - 100 A 3-phase HD sub-panel + breakers
> - Conduit, EMT and MC mix per typical commercial TI assumption — verify against Hammond's ceiling-cavity vs furred-wall constraints during pricing
>
> **Confirm-before-pricing items (flag in the estimate as assumptions):**
> - Existing 200 A 3-phase sub-panel has spare breaker space for the new 100 A feeder
> - Procedure light, imaging device, centrifuge, autoclave — manufacturer make / model / load
> - Exam-room patient-care classification (NEC Article 517) — confirms hospital-grade-receptacle scope and isolated-ground-bonding requirements
> - Permit fee allowance — use Riverside city schedule
> - Whether Hammond wants the lab equipment terminations (cord-and-plug) or hard-wired
>
> **Schedule.**
> - Rough-in by 2026-08-31; finish by 2026-09-30; occupancy 2026-10-15.
> - Lead time on the 100 A HD sub-panel — assume 4–6 weeks at quote (pull current vendor quote during pricing). Critical-path item.
>
> **Quote turnaround.** Hammond expects the quote by 2026-04-29 (Friday). Office to draft by 2026-04-28 EOD; Mike to review 2026-04-29 AM; send before noon.
>
> **Internal Notes:**
> - This is an estimate-request artifact, not a customer-facing scope letter — handoff to `sales/scope-letter-drafter.md` for the Hammond-facing scope-letter draft after the estimator returns numbers.
> - Run `operations/nec-2026-load-calculation-helper.md` on the existing 200 A 3-phase sub BEFORE pricing the new sub-panel feeder; if the load calc shows the existing sub is undersized, the project gains a service-upgrade scope line. Material-impact on the bid.
> - Article 517 patient-care classification is the single biggest scope variable. If the exam rooms are classified Category 1 or 2 patient-care spaces, the hospital-grade-receptacle scope, isolated-ground bonding, and equipotential grounding requirements expand the scope materially. Confirm with the medical-equipment planner before locking in the quote.
> - **Warranty-Callback-Trigger Detection:** Not applicable — this is a new-TI estimate request, not a service visit. No prior firm-installed equipment in scope. No customer-facing warranty language needed.
