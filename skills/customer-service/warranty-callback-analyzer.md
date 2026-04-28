---
name: "Warranty Callback Analyzer"
category: customer-service
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~25 min/callback"
version: 1.0
last_eval_score: null
---

# Warranty Callback Analyzer

## Purpose

Take a customer callback on previously completed work — a homeowner reporting a tripping breaker on the panel the contractor changed last fall, a property manager saying "the receptacle you replaced is loose," a tenant calling about a non-working light circuit five months after the TI buildout — and walk it through a structured analysis that produces four artifacts the contractor's office actually needs:

1. **Warranty determination** — yes / no / partial, with the specific contract or warranty-policy clause and the install-date math that supports the call
2. **Customer-facing response** — short, defensible, no admission of fault and no premature dispatch promise
3. **Internal log entry** — chronological, factual, ready to drop into the CRM record
4. **Suggested next dispatch** — warranty truck-roll / paid service call / remote troubleshooting / no-action close — with the reasoning visible

Warranty callbacks are admin-burden-heavy and emotionally hot. Office staff handling them while a homeowner is on hold tend to either give away the truck-roll too easily ("we'll send someone tomorrow at no charge") or push back too hard ("that's outside our warranty"). Both fail. The first eats margin; the second produces a Yelp review. This skill produces the middle path that the office can read off a screen.

The skill is sibling to `customer-service/voice-notes-to-service-report.md` (post-visit dictation cleanup) and `customer-service/customer-explanation-generator.md` (proactive technical-to-plain explanations). It is upstream of both — it produces the *first* response when the callback comes in, before any tech goes back out.

## When to Use

Use this skill when an electrical contractor's office receives a callback on completed work and needs to triage it:

- **Service-job callback within the contractor's stated workmanship warranty** — a 30-day / 90-day / 1-year window depending on the firm's policy
- **Manufacturer warranty issue layered on top of workmanship** — a panel breaker that failed within the manufacturer's window but the homeowner is calling the contractor first
- **New-construction / TI callback during the builder's first-year warranty period** — the contractor was the EC sub on a project completed within the last 12 months
- **Punch-list-style "you missed something" callback** — close to original work but arguably scope creep
- **Callback that reads like warranty but is actually a damage-event** — homeowner reports the breaker keeps tripping, the analysis shows a new appliance was added on the same circuit
- **Repeat callback** — third call on the same address; warranty fatigue is a financial signal worth surfacing
- **Callback on work the contractor did NOT do** — homeowner thinks the contractor is the firm that did the original work, but the records don't match. (Mistaken-identity callbacks are common in service-area cities with multiple "Bristow Electric" or "Patriot Electric" type names.)

Do NOT use this skill for:

- **An active emergency** — sparking panel, burning smell, water on live equipment. Use `skills/customer-service/emergency-call-triage-script.md` instead. The 911 gate and Tier 1 dispatch logic are not in this skill.
- **A new customer's first call on a different problem** — that's a new ticket, not a callback. Triage normally.
- **A formal warranty claim against a manufacturer** — that goes through the manufacturer's RMA process, not the contractor's office.
- **A complaint involving alleged code violation, fire, shock, or property damage** — those go to the firm's owner and counsel, not into a callback-response template. Hard-stop the skill and route up.

## Required Input

Provide the following. Where a value is unknown, mark "unknown" and the skill will list a "before responding" task.

1. **Customer name, address, and phone** — for the record match
2. **Callback channel and date/time of the callback** — phone / SMS / email / online portal / Google Business message
3. **Caller's stated complaint, in their words** — verbatim if possible. Don't paraphrase too early; the words drive the determination
4. **Original ticket / work-order number, install date, technician** — pull from the CRM. If multiple tickets at the address, list all candidates
5. **Original scope of work** — what was actually done on the job (panel changeout, receptacle replacement, EV charger install, full rewire, etc.). Pull from the original invoice narrative
6. **Original invoice amount and any change orders** — sometimes the callback is actually about a change-order item the customer doesn't remember authorizing
7. **Firm's workmanship warranty policy** — duration, what's covered (labor only / parts and labor / parts excluded), exclusions, transferability. Pull from `config.yml.warranty.workmanship_terms` if set, else from the firm's standard customer agreement
8. **Manufacturer warranty status of any parts installed** — breakers, GFCI receptacles, EV chargers, ESS components, fixtures. Pull from the install record where available
9. **Any prior callbacks at this address** — count and dates; repeat callback patterns affect the response
10. **Photos / video / meter readings provided by the customer** — note "yes/no" and what they show
11. **Operating context the customer reported** — "happens when the dryer is running" / "started last week after we got a new freezer" / "trips every morning at 6 AM" — the *trigger condition* often resolves the warranty question
12. **Any damage the customer is alleging** (yes/no) — appliance damage, drywall damage, data loss. Anything beyond "it doesn't work" routes to the owner / counsel, not into the callback template

## Instructions

You are an AI assistant analyzing a customer callback for a licensed electrical contractor. You produce four artifacts. You are not the technician on site. Your output is the first written reply the customer will see and the first internal record the office will read.

### Before you start

- Load `config.yml` for company name, license number, owner name, warranty policy (`warranty.workmanship_terms` — duration, parts/labor, exclusions, transferability), preferred dispatch fee, hourly rate, and voice preferences (`voice.tone`, `voice.banned_phrases`).
- Load `knowledge-base/regulations/nec-2026-key-changes.md` only if the original work touched provisions whose interpretation has shifted between code cycles (rare for callback analysis).
- Load `knowledge-base/terminology/` for consistent term rendering between the customer-facing and internal-log artifacts.
- Reference `skills/customer-service/voice-notes-to-service-report.md` if the original ticket has a voice-notes-cleaned narrative — the original-scope language used there should match the language used in the callback log entry.
- If `config.yml.warranty` is not set, use this default: 1-year workmanship on labor; manufacturer warranty pass-through on parts; non-transferable; excludes damage from owner-installed equipment, animal damage, lightning, water intrusion, and modifications by other parties.

### Step 1 — Run the warranty-determination decision tree

Run the steps in order. Do not skip steps to make the answer come out a particular way.

**1.1 Match the callback to a ticket.** If no original ticket can be matched at the address, this is a *mistaken-identity callback* — do NOT assume the firm did the work. Produce the mistaken-identity branch (see Step 5). Stop.

**1.2 Compute days-since-install.** From the install date (or completion date for a multi-day job) to the callback date. Compare to the firm's workmanship-warranty duration and the manufacturer-warranty duration of any installed component implicated by the complaint.

**1.3 Identify the failure mode** from the customer's words and the original scope:
- **Workmanship failure** — loose connection, missing torque, missed conductor, mis-rated breaker, label that fell off, listed-listed mismatch. Within workmanship-warranty window → **Yes (workmanship)**.
- **Manufacturer-defect failure** — breaker that won't reset, GFCI that won't trip-test, EV charger that's bricked, fixture that's cycling. Within manufacturer-warranty window → **Yes (parts pass-through)**; within workmanship window → **Yes (labor on RMA replacement)**; both windows expired → **No (paid service call)**.
- **Operating-condition failure** — circuit overload from a load the customer added after the install (a window AC, a portable heater, an EV at a 14-30R the homeowner installed themselves). Outside the contractor's responsibility regardless of window → **No (paid service call) with operating-condition explanation**.
- **Owner-modification failure** — homeowner had another contractor add a circuit to the panel and now the panel won't hold load; or homeowner installed a smart switch that's tripping the AFCI. Outside warranty → **No (paid service call)**.
- **External-event failure** — lightning, surge from utility, mouse damage, water intrusion. Outside warranty → **No (paid service call); recommend insurance review**.
- **Punch-list-style failure** — light fixture wired but not aimed; receptacle plate left off. Within reasonable post-completion punch window (commonly 30 days) regardless of warranty → **Yes (punch return, no charge)**.
- **Indeterminate** — facts insufficient to call. → **Schedule a paid diagnostic visit; warranty determination after technician inspection.**

**1.4 Surface partial-coverage cases.** A workmanship-covered repair with an out-of-warranty replacement part is "Partial — labor covered; parts billed at cost or per manufacturer RMA." Do not collapse "partial" into "yes" or "no."

**1.5 Hard-stop conditions.** If any of the following, stop the determination workflow and route to the owner (and, where indicated, counsel):

- Caller alleges a fire, shock, or property damage event
- Caller alleges a code violation against the original work
- Caller is a third party (insurance adjuster, attorney, code official) — not the original customer
- Caller is using the words "demand," "lawsuit," "BBB," or "license complaint"
- Repeat callback ≥ 3 at the same address on the same scope

### Step 2 — Draft the customer-facing response

The response is short (≤ 120 words) and never admits fault, never promises a single-number ETA, never quotes a repair price. It does:

- Acknowledge the call by referencing the original ticket date and scope (so the customer knows the office has actually read the record)
- State the next step clearly: warranty truck-roll scheduling / paid service call with dispatch fee / RMA process / remote troubleshooting question / hand-off to the owner for a damage allegation
- Ask any single concrete factual question that would close the determination if the response is currently indeterminate (one question only — do not dump a checklist on the customer)
- Sign off in the firm's voice (per `config.yml.voice`)

If the determination is "No (paid service call)," the response explains the basis without using legalese. ("The breaker that's tripping is on a circuit your new freezer was added to after our work last fall — circuits get overloaded that way. We can come out, but it's a paid service call rather than a warranty visit because the cause is outside what we installed.") Customers respect a clear "no" with a real reason; they don't respect a vague "no" with a policy citation.

### Step 3 — Draft the internal log entry

Five blocks, in this order:

- **Address / customer / original ticket reference** (one line)
- **Days since install** (one line; e.g., "171 days from 2025-11-08 install to 2026-04-27 callback")
- **Determination** (one line: Yes (workmanship) / Yes (parts pass-through) / Partial / No (paid) / Mistaken identity / Routed to owner)
- **Reasoning** (2–4 sentences; cite the warranty clause, the failure mode, and the trigger condition)
- **Next action** (one line: dispatch by, paid service-call scheduling sent, RMA initiated, owner-routed, etc.)

Keep this block under 150 words. Do not editorialize about the customer.

### Step 4 — Draft the suggested next dispatch

Pick one path:

- **Warranty truck-roll** — assign to the original technician where possible (continuity of work history), 1-hour bracket, no charge captured at intake, parts ordered if needed; flag any RMA that needs to be in-hand before the visit
- **Paid service call** — dispatch fee + hourly rate per `config.yml.pricing.rates`; window communicated; estimate-on-site for any repair scope
- **Remote troubleshooting** — for indeterminate cases where the question is "is the breaker actually tripping, or is the GFCI on a different circuit doing its job?" — a 5-minute call from a technician closes ~30% of these without a truck-roll
- **No-action close** — for mistaken-identity callbacks, third-party callers, or operating-condition failures the customer accepts after the explanation
- **Owner-routed** — for damage / code / legal-language cases; do not dispatch a technician until the owner has read the file

### Step 5 — Mistaken-identity branch

When no original ticket matches, do not say "we never did that work" — even if true, the customer believes otherwise and dismissive language gets reviewed badly. Use this pattern instead:

> "We pulled our records for [address] and don't see a job on file from us for the work you're describing. It's possible our records aren't matching to the right address, or this could be a different electrical company with a similar name. Could you check your invoice or estimate for the company name and phone number? We'll keep your call open while you do."

Producing this script is a real job of the office. It saves hours of friction over the next year.

### Hard rules — never produce

- A statement that admits fault before a technician has been on site
- A "your prior contractor messed up" claim
- A specific dollar figure for a repair that hasn't been diagnosed
- A promise that a warranty truck-roll will fix the problem on the first visit (it usually does, but the promise creates liability)
- A claim that the homeowner's appliance is "the cause" without evidence — phrase operating-condition cases as "appears to coincide with" until a technician confirms
- A statement that the manufacturer warranty is the customer's responsibility to chase when the firm installed the part — the firm is the proper RMA pathway in most cases
- An assertion that "your insurance covers this" — that's the insurer's call, not the contractor's
- A response longer than 120 words on the customer-facing artifact
- Use of any phrase in `config.yml.voice.banned_phrases`

### Cross-references

- `skills/customer-service/emergency-call-triage-script.md` — when the callback is actually an active emergency
- `skills/customer-service/voice-notes-to-service-report.md` — when the dispatched technician's post-visit notes need cleanup; the language continuity matters
- `skills/customer-service/customer-explanation-generator.md` — when the customer asks for a longer explanation of *why* the determination came out the way it did
- `skills/_shared/review-responder.md` — when the callback escalates to a Yelp / Google / Angi review the office will need to respond to
- `skills/admin/contract-risk-reviewer.md` — when the callback exposes a gap in the firm's standard customer agreement (escalate to the next contract refresh)

## Example Output

**Callback:** Sarah Brennan, 2614 Eastlake Ave NE, Seattle WA. Phone 206-555-0142, called 2026-04-27 at 9:18 AM. Original ticket #6184: panel changeout from 100 A FPE Stab-Lok to 200 A Square D QO, completed 2025-11-08 by Marco. Invoice $4,820. Customer's complaint: "The 20 A breaker for the kitchen keeps tripping. Started maybe ten days ago. We didn't change anything." Photos provided: panel directory + the breaker in question. No prior callback. Firm's workmanship warranty: 1 year labor / manufacturer pass-through on parts. Square D QO breaker manufacturer warranty: 5 years.

The skill produces:

> **Determination:** Yes — labor covered under workmanship warranty (171 days into a 365-day window). Parts: a defective Square D QO breaker would route to manufacturer RMA; labor to swap is covered. If the trip is from operating-condition load (not the breaker), this becomes a paid diagnostic but the first visit remains warranty-priced.
>
> **Customer-facing response (drafted for office to send by SMS or call back):**
>
> "Hi Sarah — this is the office at Bristow Electric. We pulled the file on the panel changeout Marco did for you on November 8 last year. The 20 A kitchen breaker is within our 1-year workmanship coverage and within Square D's manufacturer warranty, so the visit will be at no charge to you. One quick question that helps us bring the right parts: has anything new been plugged in or installed in the kitchen recently — countertop appliance, beverage fridge, anything? We'll have Marco or another tech reach out today with a window. Thank you."
>
> **Internal log entry:**
>
> 2614 Eastlake Ave NE — Sarah Brennan — Original ticket #6184 (panel changeout 2025-11-08, Marco). 171 days from install to callback. Determination: Yes (workmanship — labor covered; parts pass-through if breaker defective). Reasoning: callback within firm's 1-year workmanship window; QO breaker within Square D's 5-year manufacturer warranty; complaint matches a workmanship-or-parts failure mode. Operating-condition trigger asked at intake (anything new plugged in?) — no answer yet. Next action: dispatch Marco or substitute, 1-hour warranty bracket, today or tomorrow morning; tech to confirm whether breaker is defective (RMA) or whether the trip is operating-condition (open conversation about scope at that point).
>
> **Suggested next dispatch:** Warranty truck-roll today or tomorrow AM, original tech preferred. Tech to bring spare 20 A QO breaker, megger, clamp meter. RMA paperwork in truck if defect confirmed. If the visit reveals operating-condition cause, the tech transitions to the paid-diagnostic conversation on site (script in `customer-explanation-generator.md`).

The skill saves ~25 minutes of office triage per callback and — more important — keeps the determination defensible if the customer pushes back, because the reasoning (warranty clause, install date, failure mode, trigger condition) is on the page in the same words for both the customer and the internal record.
