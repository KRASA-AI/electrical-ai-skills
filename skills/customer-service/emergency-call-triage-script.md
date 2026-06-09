---
name: "Emergency Call Triage Script"
category: customer-service
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~15 min/after-hours call + avoided nuisance truck-rolls"
version: 1.1
last_eval_score: 9.40
---

# 🚨 Emergency Call Triage Script

## Purpose

Produce a structured after-hours / on-call triage script that a human dispatcher, an AI answering agent, or a voicemail-override system can follow the same way every time — so that a **life-safety event** (sparking panel, burning smell near service equipment, energized water, shock-on-contact, smoke from an outlet, oxygen-dependent occupant in a total outage) produces an immediate dispatch, and a **nuisance event** (one breaker tripping that resets cleanly, one outlet not working, a switch that stopped working, a flickering light) gets scheduled for the next business day with safe-interim guidance for the caller.

The output is a branching script with: a consistent branded greeting, a safety-screening question set, a decision tree that maps symptoms to one of five response tiers, a safe-interim-action block the caller can do while waiting, dispatch-trigger criteria the contractor (or the AI agent) can lean on to justify the after-hours truck roll, and a liability-clean closing that captures contact info, address, and the caller's own assessment of urgency.

This is a **prompt / process skill**, not a dial-in app. It runs equally well as a laminated card at the answering desk, as a system prompt for an AI voice agent, or as a SOP for a dispatcher on call.

## When to Use

Use this skill when an electrical contractor needs a consistent, defensible after-hours call-handling flow. Specific use cases:

- **Answering-service / AI-voice-agent system prompt** — The script that governs the AI or human operator between the branded hello and the dispatch/schedule handoff
- **Dispatcher laminated card or SOP** — A one-pager for the on-call dispatcher that covers the common life-safety triggers, the common nuisance triggers, and the in-between that needs a callback
- **After-hours voicemail script** — A voicemail prompt that surfaces the life-safety triggers first ("If you have sparks, smoke, or a burning smell coming from your panel or an outlet, hang up and call 911 and your utility — then call us back") before it asks for name / number / address
- **On-call tech handoff template** — What the dispatcher sends to the on-call tech when a call is triaged to immediate dispatch: symptom, address, safe-interim guidance given, caller contact, ETA promised
- **Post-call log format** — A short, consistent note the dispatcher or AI agent writes into the CRM so the next person reading the ticket understands what was triaged, why, and what was promised
- **Training script for new dispatchers** — A standardized flow so two different dispatchers produce the same triage on the same symptoms

Do NOT use this skill to substitute for the caller's local emergency services. Sparking equipment, smoke, fire, or a shock event always routes to 911 and the utility first; the electrician follows.

## Required Input

Provide the following:

1. **Company voice and branding** — Company name (as pronounced and spelled), hours-of-operation greeting, on-call line phone number, service-area county list, and the after-hours rate structure the dispatcher is allowed to quote (flat fee / dispatch + hourly / waived with repair).
2. **Channel** — Live human dispatcher / AI voice agent / voicemail-override / SMS chat / web chat. The script shape adjusts: a live human can ask follow-ups flexibly; an AI agent needs the decision tree spelled out; a voicemail surfaces the life-safety triggers first, not the name-and-number ask.
3. **Dispatch posture** — How aggressive the contractor is about after-hours dispatch. Options: **always-dispatch-on-safety**, **dispatch-on-safety-only-when-on-call-tech-available-within-N-minutes**, **dispatch-on-safety-and-billable-nuisance**, **safety-dispatch-and-next-day-for-everything-else**. Default: always-dispatch-on-safety.
4. **On-call tech availability window** — Who's on call, what their response time window is (e.g., 45 minutes inside city, 90 minutes in outlying county), and the cut-off hour past which the on-call tech stops responding.
5. **Utility and 911 policy** — When to tell the caller to call 911 first and the utility second, when to tell them to call the utility first (e.g., downed service drop on the ground, tree on the line), and when the contractor takes the call after the first responders have cleared the scene.
6. **Rate and authorization language** — The specific sentence the dispatcher says to get verbal authorization for the after-hours rate before the truck rolls.
7. **Excluded calls (optional)** — Service types the contractor refuses after hours (e.g., no generator-service calls without a signed service contract, no new-construction rough-in emergencies, no commercial PM work after hours). Keep the list short and explicit.
8. **Jurisdiction notes (optional)** — Any state / AHJ rule that affects after-hours handling (e.g., some municipalities require a permit for any service-equipment replacement, even an emergency one — the dispatcher should flag this so the tech doesn't energize an un-permitted panel swap on a Sunday).

## Instructions

You are an AI assistant drafting a call-triage script for an electrical contractor's after-hours line. Your job is to produce a flow that is safe for the caller, defensible for the contractor, and consistent regardless of who picks up the phone.

**Before you start:**

- Load `config.yml` for the company name, on-call line, service area, after-hours rates, and any jurisdiction-specific after-hours rules.
- Load `knowledge-base/terminology/` so the trade terms used in the script are rendered consistently.
- Reference `knowledge-base/regulations/nec-2026-key-changes.md` if a recurring emergency pattern has a code nexus (e.g., burning smell at the service disconnect — the 2026-cycle §230.70 outdoor-disconnect provisions affect what the tech can safely de-energize from).
- Do not load the Plain-English Translation Dictionary verbatim — this script needs plain spoken English, not the invoice-explanation register. Reach for short sentences a scared caller can understand at 10:30 PM.

### Priority-Classification Pre-amble (run FIRST — lands P1 / P2 / P3 before any free-text)

The #1 clarity failure in an after-hours script is that the person reading it — a tired dispatcher, an AI voice agent, a brand-new hire — has to read several paragraphs of branching dispatch language *before* they know whether this call is an emergency. By the time the answer emerges from the prose, a P1 caller has been on the line answering questions for 40 seconds. Fix this by landing a one-glance **priority classification** at the very top of every script the skill produces, before any greeting, tier table, or dispatch language.

Every script the skill generates opens with a **Priority Pre-amble**: a 3-line scripted decision tree that collapses the five response tiers into **three priority classes**, so the reader knows the disposition before they read a single sentence of free text:

```
PRIORITY PRE-AMBLE  (read top to bottom; stop at the first YES)

  P1 — DISPATCH NOW / 911-GATE
       Sparks · smoke · flames · burning smell at panel/outlet/fixture ·
       shock or tingling after contact · active arc at meter ·
       water running onto live equipment · downed service drop.
       → 911 gate first if fire/shock/injury, then immediate on-call dispatch.
       (maps to Tier 1 + Tier 2)

  P2 — DISPATCH ELEVATED / MEDICAL-WEATHER
       Outage or partial outage WITH oxygen / dialysis / powered medical
       device · extreme-heat or extreme-cold occupant · refrigerated
       medication or food-service freezer at risk.
       → dispatch on elevated priority even if the symptom alone is minor.
       (maps to Tier 3)

  P3 — SCHEDULE NEXT BUSINESS DAY
       One breaker won't reset · one circuit/room dark · GFCI won't hold ·
       flickering light · humming dimmer · single dead outlet · cosmetic.
       → next-business-day callback window + safe-interim guidance.
       (maps to Tier 4 + Tier 5)
```

**Rules for the pre-amble:**

1. The pre-amble is the **first thing in the output**, before the greeting block. It is a routing index, not the spoken script — the dispatcher/agent uses it to know *where they are going* before they start talking.
2. It is read **top to bottom, stop at the first YES** — a P1 flag short-circuits everything below it. This is the scripted-decision-tree shape: the most dangerous disposition is checked first.
3. The P1 / P2 / P3 label is **stamped into the post-call log and the tech handoff** alongside the existing Tier N, so the priority class and the granular tier travel together (P1/Tier-2, P3/Tier-4, etc.). The five tiers are retained in full below the pre-amble — the pre-amble does not replace them, it indexes them.
4. The pre-amble **never invents a fourth class** and never softens a P1 to P2/P3. When the safety screen is inconclusive, the pre-amble routes to the higher priority class (P1 over P2, P2 over P3) — the same err-toward-dispatch posture the anti-liability rules already require.
5. If the company's `config.yml` defines a tighter dispatch posture (e.g., safety-only after a cutoff hour), the P2 class carries the time-of-night clause as a sub-line; the P1 class is **never** gated by hour.

**Core process:**

1. **Open with a consistent branded greeting.** "Thanks for calling [Company] — you've reached our on-call line. My name is [Name]. Before we go any further: **do you see sparks, smoke, or flames, or is anyone hurt? If yes, please hang up, call 911 first, and call us back as soon as the scene is safe.** If no, let's get some information." This greeting does two things at once: it puts the 911 gate in front of everything else, and it lets the caller self-select out of the electrical triage flow if they're in an actual fire event.
2. **Run the safety screen.** A short, ordered set of yes/no questions the dispatcher asks in the same order every time:
   - *Is anyone injured or feeling dizzy, tingling, or numb after touching a wet surface, a metal pipe, or an appliance?* (If yes → 911 gate, then ambulance, then our tech.)
   - *Do you see sparks, smoke, or flames coming from an outlet, a switch, a light fixture, a panel, or any electrical equipment? Is there a burning plastic smell coming from a panel, an outlet, or a fixture?* (If yes → 911 gate, power shut-off at the main if accessible and safe to reach, on-call tech dispatched.)
   - *Is your service drop (the line from the utility pole to your house) on the ground, or is a tree on the line, or do you hear arcing / buzzing at the meter?* (If yes → utility first, then 911 if occupants in structure, then us after the utility clears the scene.)
   - *Is any water running onto electrical equipment (flooded basement with a panel, washer line at an outlet, roof leak at a light fixture)?* (If yes → main breaker off at the panel if safely reachable, 911 if water level is rising, on-call tech dispatched.)
   - *Is anyone in your home on oxygen, dialysis, a powered medical device, or dependent on cooling / heating during an extreme-weather event? Is a food-service freezer or a refrigerated medication at risk?* (If yes → dispatch, elevated priority; recommend contacting the utility for medical-outage registry if not already done.)
3. **Map to a tier.** Based on the safety screen, assign the call to one of five tiers:
   - **Tier 1 — Life safety / active fire / shock event:** 911 first, utility second, dispatch on-call tech for post-clearance inspection. Never promise a tech will arrive before the fire department.
   - **Tier 2 — Imminent hazard / structural electrical event:** Dispatch on-call tech within the response-window commitment. Examples: burning smell at the panel with no visible flame, panel sparking that stopped when caller turned off the main, a persistent arc at the meter, water actively dripping onto a live panel.
   - **Tier 3 — Partial outage with medical/dependent occupant OR full loss of power in extreme-weather conditions:** Dispatch on-call tech on elevated priority even if the symptom itself is not life-threatening (e.g., the whole house is out but neighbors also have power — possible service-side issue the utility owns — still dispatch for the medical occupant).
   - **Tier 4 — Functional outage, contained:** One breaker that won't reset, one circuit out, one outlet out, one light switch not working, one bedroom without power, GFCI that won't reset. Safe to schedule next business day with safe-interim guidance.
   - **Tier 5 — Nuisance / cosmetic:** Flickering light that isn't a fixture issue, a dimmer humming, a switch that "feels warm" but isn't hot (distinguish clearly from a switch that is hot to the touch — that's Tier 2), an outlet that has always worked but now doesn't. Schedule next business day; give the caller one thing to check themselves.
4. **Give the safe-interim guidance every time.** Whether the tech is rolling or the call is being scheduled, the caller always leaves the conversation with one or two things they can safely do while they wait. Examples: "Turn off the main breaker at your panel if you can reach it dry-handed and with dry shoes," "Unplug the appliance and don't plug it back in until we look at it," "Press the reset button on the GFCI outlet once — if it doesn't hold, leave it tripped," "Don't flip breakers back on if one is warm to the touch." Never tell a caller to open a panel cover.
5. **Ask for the seven data fields every call.** In this order: caller name, callback number (confirm by reading back), service address (confirm by reading back), what's plugged into the affected circuit or fixture, what the caller already tried (reset the breaker, flipped the switch, unplugged the appliance), any recent event the caller can tie to the start of the problem (storm, plumber drilling yesterday, new appliance installed last week, breaker subbed out last month), and whether the caller is the property owner or a tenant (affects who signs authorization).
6. **Quote the rate before the truck rolls.** On any dispatch call, the dispatcher (or AI agent) quotes the after-hours rate clearly ("Our after-hours dispatch is a flat $185 for the truck to arrive, plus $165/hour for repair time, minimum one hour, rolled into the repair ticket if you proceed with the fix tonight.") and gets verbal authorization before confirming dispatch. Log the authorization exchange in the ticket notes.
7. **Read back the ETA and the tech's name.** The caller always leaves the call with: (a) the name of the tech rolling, (b) the ETA window (not a single number — "between 10:45 PM and 11:30 PM"), (c) a callback number if the tech hits a delay, and (d) the safe-interim guidance repeated once. For a scheduled next-day call: the callback window ("We'll call you between 7:00 AM and 8:00 AM Monday to schedule a time that works for you"), the expected dispatch day, and the safe-interim guidance.
8. **Log it the same way every time.** The post-call log is one line plus a short block — the line summarizes the triage tier and action; the block captures the seven data fields, the safety-screen answers, the rate authorization, and anything the caller said that matters later (a tenant-owner dispute, an insurance claim already filed, a prior contractor's work).

**Anti-liability rules:**

- Do not tell the caller to open a panel cover, remove a device cover plate, or touch wiring. The safe-interim guidance is always limited to things that don't require tools or exposed conductors.
- Do not promise the tech will arrive "in 30 minutes." Promise a **window** with an upper bound the on-call schedule can actually meet — then call the customer back if the upper bound will slip.
- Do not diagnose the cause over the phone. "It sounds like it could be…" is not diagnosis; "The problem is a loose neutral on your ungrounded half" is diagnosis and belongs to the tech after the work.
- Do not tell the caller to "just flip the main back on and see what happens." If the main was turned off because of a safety concern, it stays off until the tech looks at it.
- Do not promise insurance claim help. The tech files the inspection report; the insurance-claim process belongs to the customer and their carrier.
- Do not quote a fixed repair price over the phone. Quote only the dispatch fee + hourly rate; diagnosis has to happen before a repair price is committed.
- Do not collect a credit-card number on the phone for the dispatch fee unless the company's intake process explicitly authorizes that. Most contractors do post-service billing; verify in `config.yml` before the script asks for a card.
- Do not dispatch on a call where the safety screen is inconclusive and the caller can't stay on the line. Call the caller back within 10 minutes; if they don't answer, err toward dispatch if the symptoms included any Tier 1 or Tier 2 flag.
- Do not tell the caller their prior contractor's work was the cause. Keep the call forward-looking ("We'll look at it and tell you what we see.").
- Do not escalate to a supervisor without first trying the safe-interim guidance. Supervisor escalation is for rate disputes, aggressive callers, and tier-boundary cases — not every late call.

## Output Format

Default output is a **multi-section script package** with these sections, in order:

0. **Priority Pre-amble** (the 3-line P1 / P2 / P3 routing index, read top-to-bottom, stop at first YES) — lands at the very top so the reader knows the disposition before reading any spoken script. Stamps the P-class into the log and tech handoff alongside Tier N.
1. **Greeting block** (4–6 sentences) — The branded hello + 911 gate + the "let's get some information" pivot.
2. **Safety screen question set** (5 ordered questions) — Each question + the yes-branch action + the no-branch action.
3. **Tier mapping table** (5 rows) — Tier number / symptoms / action / ETA promise / rate quote / safe-interim guidance for the caller.
4. **Safe-interim guidance library** (8–12 items) — The exact sentences to speak for the common scenarios; never tells the caller to open a panel or touch wiring.
5. **Seven-field data block** — The fields in the order the dispatcher asks for them, including the read-back step for callback number and service address.
6. **Rate-and-authorization script** — The specific sentence the dispatcher says, the response options (accept / decline / request supervisor), and the dispatcher's action for each response.
7. **Close-out script** — The read-back sentence for the ETA and tech name (dispatch path) or the callback window (schedule path); the safe-interim-guidance repeat.
8. **Post-call log template** — The one-line summary + the structured block for the ticket system.
9. **On-call tech handoff template** (dispatch path only) — The short SMS or call the dispatcher sends to the tech with the address, symptom, safety-screen answers, safe-interim guidance given, and caller contact.
10. **Edge cases** (8–12 scenarios) — Downed utility drop / tree on line / service drop with tensioner down / active fire department on scene / medical occupant in prolonged outage / water on live panel / tenant-without-owner-authorization / tenant with landlord-on-speakerphone / commercial-after-hours-restaurant / commercial-after-hours-grocery-walk-in / EV-charger-smoking / generator-hunting-or-not-transferring.

**Channel adaptations** — If the output is for an AI voice agent, the decision tree is spelled out as a flowchart the agent follows; if the output is for a voicemail override, the greeting surfaces the 911 gate and then requests name/number/address/short-description-of-issue; if the output is for an SMS / web chat, the safety screen is the first five messages the system sends, each requiring a yes/no reply before the conversation continues.

Below the main output, include a short **Internal Notes** block that lists:
- Any rate structure the intake didn't supply (skill flags it rather than inventing)
- Any jurisdiction-specific rule (utility-first rule, medical-outage-registry reference, commercial-food-service emergency hour limits) the user should confirm
- Any excluded-call category the intake did not state but the user may want to consider (no-appointment-history customer, new-move-in, pre-paid-service-plan)
- A pointer to `skills/customer-service/voice-notes-to-service-report.md` for the tech's post-call dictation workflow
- Any recommended refinements (e.g., tighten the tier-3 cutoff in extreme-weather events, add a specific medical-device list to the safety screen)

## What NOT to Do

- Do not instruct the caller to open a panel cover or touch wiring. Safe-interim guidance stops at breakers, plugs, and GFCI reset buttons — nothing requiring tools.
- Do not promise a single-number ETA. Promise a window with an upper bound that the on-call schedule can actually meet.
- Do not diagnose over the phone. "Sounds like a loose neutral" is not a dispatcher's sentence.
- Do not quote a repair price over the phone. Dispatch fee + hourly rate is the only number the script commits.
- Do not tell the caller the prior contractor's work was the cause. The script stays forward-looking.
- Do not route a Tier 1 event (sparks, smoke, shock, fire) anywhere other than 911 first, utility second, electrician after.
- Do not take a credit card on the phone unless `config.yml` explicitly authorizes it.
- Do not mix the greeting with the 911 gate — the 911 gate is a standalone sentence so a caller in a real fire event can exit the script fast.
- Do not skip the safe-interim guidance on a scheduled call. Even a Tier 4 or Tier 5 caller leaves the conversation with one thing to do safely while they wait.
- Do not reference the after-hours rate as a penalty. It is the cost of the truck rolling outside normal hours; frame it as that, not as a consequence.

## Example Output

### Example — AI voice agent system prompt (Tier-mapping script for a residential/light-commercial contractor)

**Input:**
- Company voice: "Ashwood Electric. Licensed Oregon EC-40218. On-call line answered by an AI voice agent, backed by a human dispatcher who can interrupt at any point."
- Service area: Multnomah, Washington, Clackamas counties (OR)
- After-hours rate: $195 dispatch fee + $175/hour repair time, 1-hour minimum, dispatch fee rolled into the ticket if the repair proceeds same visit; customer can decline dispatch after the quote
- On-call window: 6:00 PM–10:30 PM Sun–Thu, 6:00 PM–12:00 AM Fri–Sat; after 10:30 PM/12:00 AM, safety-only dispatch and the agent schedules non-safety calls for 7:30 AM next business day
- Dispatch posture: dispatch-on-safety + dispatch-on-medical-occupant; all other calls scheduled next business day
- Excluded after-hours calls: no commercial PM, no new-construction rough-in, no generator service without an active service contract
- Jurisdiction notes: Portland AHJ on NEC 2026 since Oct 2025 — service-equipment replacements trigger outdoor-disconnect; the tech should not energize a replaced panel on an after-hours call without the AHJ temp-energize procedure

**Output — AI voice agent system prompt (abridged, Tier 1 and Tier 2 branches shown in full; Tier 3–5 summarized):**

> **PRIORITY PRE-AMBLE (read top to bottom; stop at first YES — know the disposition before you speak):**
>
> ```
> P1 — DISPATCH NOW / 911-GATE
>      Sparks · smoke · flames · burning smell at panel/outlet/fixture ·
>      shock/tingling after contact · active arc at meter ·
>      water on live equipment · downed service drop.
>      → 911 gate (if fire/shock/injury) → immediate on-call dispatch.   [Tier 1 / Tier 2]
>
> P2 — DISPATCH ELEVATED / MEDICAL-WEATHER
>      Outage WITH oxygen / dialysis / powered medical device ·
>      extreme heat-cold occupant · refrigerated meds or food-service freezer.
>      → elevated-priority dispatch even if symptom alone is minor.        [Tier 3]
>      (Ashwood posture: after 10:30 PM Sun–Thu / 12:00 AM Fri–Sat, P2 still dispatches.)
>
> P3 — SCHEDULE NEXT BUSINESS DAY
>      One breaker won't reset · one room dark · GFCI won't hold ·
>      flicker · humming dimmer · single dead outlet · cosmetic.
>      → 7:30–8:30 AM callback window + safe-interim guidance.            [Tier 4 / Tier 5]
> ```
>
> *The P-class is stamped into the post-call log and the tech handoff alongside Tier N. The full five-tier scripts below are the spoken language; the pre-amble is the router.*
>
> **Greeting (first sentence, always):**
> *"Thanks for calling Ashwood Electric — you've reached our on-call line. I'm an AI assistant and a human dispatcher is listening. Before we go any further: **do you see sparks, smoke, or flames right now, or is anyone hurt? If yes, please hang up, call 911 first, and call us back as soon as the scene is safe.** If no, let's get some information."*
>
> **Safety screen (run in this order; if any Tier-1 flag yes, jump directly to Tier 1 script):**
>
> - *"Is anyone injured, or has anyone had a shock — tingling, numbness, or a jolt — after touching a wet surface, a metal pipe, or an appliance in the last hour?"* — Yes → Tier 1. No → continue.
> - *"Right now, at your house, do you see sparks, smoke, or flames coming from an outlet, a switch, a light fixture, a panel, or any electrical equipment? Is there a burning plastic or rubber smell coming from a panel, an outlet, or a fixture?"* — Yes → Tier 1 or Tier 2 (see mapping below). No → continue.
> - *"Is the line from the utility pole to your house lying on the ground, or is a tree on that line, or do you hear a buzzing or arcing sound at the meter?"* — Yes → utility first, then Tier 1 if occupants evacuated. No → continue.
> - *"Is any water running onto any electrical equipment — a flooded basement with a panel, a washer line at an outlet, a roof leak at a light fixture?"* — Yes → Tier 2. No → continue.
> - *"Is anyone in your home on oxygen, dialysis, or a powered medical device? Is anyone in extreme heat or cold without power? Is a food-service freezer or a refrigerated medication at risk?"* — Yes → Tier 3 (dispatch even if symptom would otherwise be Tier 4). No → map by symptom to Tier 4 or Tier 5.
>
> **Tier 1 script (active fire, shock event, smoke from panel):**
> *"Please hang up and call 911 right now. If your main breaker is reachable with dry hands and dry shoes, turn it off before you leave the area. Then call Portland General Electric at 503-228-6322 — they'll cut service at the pole. Call us back as soon as the fire department has cleared the scene and we will send a tech to inspect your panel before you re-energize. I'm logging this call as Tier 1, flagged for the on-call tech as priority-one callback. Your callback number is [read back], correct?"*
>
> **Tier 2 script (imminent hazard — burning smell at panel no flame, panel sparked and stopped when main turned off, active arc at meter, water dripping onto live panel):**
> *"Okay, here's what I need you to do. If you can safely reach your main breaker — that's the big switch at the top of your electrical panel — and you're on dry ground with dry hands, turn it off. Don't open the panel cover. Don't touch anything metal. Don't flip any breaker back on tonight.*
>
> *I'm going to dispatch our on-call tech, [Tech Name], to you. Our after-hours dispatch is $195, plus $175 per hour for repair time, one-hour minimum, and the dispatch fee rolls into the repair ticket if you proceed with the fix tonight. Does that work for you? [Wait for verbal yes.]*
>
> *[Tech Name] will be on the way in about 15 minutes and should reach you between [ETA window, 45–75 minutes]. His callback number if he hits a delay is [on-call line]. While you wait: do not flip any breakers back on, don't touch the panel cover, keep kids and pets away from the affected area. If anything changes — smoke, a burning smell coming back, anything that scares you — call 911 first and us second. Your callback number is [read back], service address is [read back], correct?*
>
> *One more thing: Portland is on the 2026 electrical code, which means if we end up replacing your service panel tonight, we'll be installing an outdoor emergency disconnect as part of the work. [Tech Name] will explain the scope before anything gets touched."*
>
> **Tier 3 script (partial outage with medical occupant / extreme-weather / refrigerated medication):** Dispatch at elevated priority even if the symptom alone would be Tier 4; run through the same rate quote and ETA structure as Tier 2 but note the medical context for the tech handoff. Tell the caller to contact PGE and register for the medical-outage list if not already.
>
> **Tier 4 script (functional outage, contained — one breaker won't reset, one circuit out, one room dark, GFCI won't reset):** Schedule next-business-day callback between 7:30 AM and 8:30 AM. Safe-interim: "Press the reset button on the GFCI once; if it doesn't hold, leave it tripped. Don't keep resetting the breaker — if it trips back within a minute, leave it off until we look at it. Unplug anything on that circuit you can identify." No dispatch unless medical-occupant flag from safety screen.
>
> **Tier 5 script (nuisance / cosmetic — flickering, dimmer hum, switch that has worked for years and now doesn't):** Schedule next-business-day callback between 7:30 AM and 8:30 AM. Safe-interim: "Leave that switch off until we look at it. If a dimmer is humming and getting warm, replace the bulb with a dimmable LED or incandescent — non-dimmable LEDs on a dimmer is the most common cause." No dispatch.
>
> **Seven-field data block (every call, in order):** Caller name / callback number (read back) / service address (read back) / what's on the affected circuit or fixture / what you've already tried / any recent event tied to the start of this / are you the property owner or a tenant?
>
> **Rate-and-authorization script (dispatch path only):** *"Our after-hours dispatch is $195, plus $175 per hour for repair time, one-hour minimum, and the dispatch fee rolls into the repair ticket if you proceed tonight. Do you authorize us to roll a truck under those terms?"* — Yes: dispatch. No: "Understood — I'll schedule you for [next-business-day callback window] and keep the safe-interim guidance I just gave you in place."
>
> **Close-out script (dispatch path):** *"[Tech Name] is on the way. ETA is [window]. His callback number if he hits a delay is [on-call line]. Remember: [safe-interim guidance repeated]. We'll see you shortly."*
>
> **Close-out script (schedule path):** *"I've scheduled you for a callback between [next-business-day window] — that's when our office opens and we'll set you a time that works. Until then: [safe-interim guidance repeated]. If anything changes — a burning smell, sparks, a shock — call 911 first and us back second. Your callback number is [read back]."*
>
> **Post-call log template (into CRM):**
>
> *"[P1/P2/P3] / Tier [N] — [symptom summary, 8 words]. [Dispatch OR Scheduled for {window}]. Safety-screen answers: injury N/Y, smoke-sparks N/Y, drop-down N/Y, water-on-panel N/Y, medical-occupant N/Y. Safe-interim given: [short list]. Rate quoted $195/$175/1-hr-min, authorized: Y/N. Tech dispatched: [Name or N/A]. ETA promised: [window or N/A]. Address/callback confirmed. Notes: [anything material — tenant/owner, insurance claim, prior contractor's work mentioned, recent storm, specific appliance/circuit]."*
>
> **On-call tech handoff (SMS):** *"[P1/P2/P3] / [Tier N] — [address] — [8-word symptom] — caller [name, number] — they [turned main off / tried reset / unplugged appliance] — safe-interim guidance: [given]. ETA promised: [window]. Medical flag: [Y/N]. 2026 NEC note if service-equipment touched."*

**Internal Notes:**
- Rate structure used ($195/$175/1-hr-min) matches the intake. If any field changes in `config.yml`, the rate-and-authorization script line needs a one-edit update.
- Portland AHJ 2026 NEC note is included in the Tier 2 script because the intake explicitly stated the jurisdiction is on 2026 and the outdoor-disconnect trigger matters for service-equipment replacements at 11 PM. Remove the sentence for a jurisdiction still on 2023 or 2020.
- No after-hours credit-card collection is in this script — the intake did not authorize it; collection happens post-service. If the company flips to pre-auth charging, a new sentence is needed before "does that work for you?"
- Tier 3 "medical occupant" is set to dispatch even without a smoke/spark flag. If the dispatch posture is tightened (e.g., "safety only after 11 PM"), the Tier 3 branch needs a time-of-night cutoff clause.
- Excluded categories (commercial PM, new-construction rough-in, generator-service-without-contract) are handled earlier in the call when the caller states the issue — add an exclusion branch before the safety screen if the intake requests a hard gate.
- Priority Pre-amble (P1/P2/P3) lands above the greeting as a routing index — it is read by the dispatcher/agent, not spoken to the caller. P1 is never gated by hour; the Ashwood after-cutoff posture attaches only to P2. The five tiers below the pre-amble are unchanged and remain the spoken scripts.
- 911 gate sits in the greeting itself; this is intentional. Don't move it into the safety screen or a caller in a real fire will waste 30 seconds answering AI questions.
- Voice-agent channel adaptation: the script reads as written. For a voicemail-override channel, collapse Tier 1 language to a 15-second recording that opens with "If you see sparks, smoke, or flames, hang up and call 911 first — then leave us a message after the tone."
- Route tech's post-call dictation through `skills/customer-service/voice-notes-to-service-report.md` so the invoice narrative is consistent with the triage notes.
- Follow-up task for the on-call lead: review the safety-screen list quarterly against any OSHA / NFPA / state-L&I bulletins on electrical emergency handling; the list is current as of the 2026-04 cycle.
