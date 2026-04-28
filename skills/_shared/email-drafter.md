---
name: "Email Drafter"
category: _shared
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~10 min/use"
version: 2.1
last_eval_score: 9.70
---

# Email Drafter

## Purpose

Turn rough notes into a professional, trade-appropriate email for customers, GCs, property managers, inspectors, suppliers, tenants, or utility coordinators — matching your company's voice and the conventions electrical contractors actually use.

## When to Use

Use this skill when you need to write any of the common electrical-contracting emails:

- **Scheduling & confirmations** — Confirming a service call, rescheduling, arrival-window reminders
- **Permit & inspection** — Permit-pulled notice, inspection-scheduled notice, inspection-passed/failed update, corrections-required letter
- **Quote & estimate follow-up** — "Did you get my estimate?" nudge, revised-quote cover email, approval request
- **Project updates** — Daily/weekly status, rough-in complete, switchgear delivered, delay notification
- **Warranty & callback** — Warranty confirmation, callback scheduling, "we came back and fixed it" closeout
- **Collections & payment** — Invoice follow-up, past-due reminder, lien-notice precursor
- **GC / trade coordination** — RFI, submittal cover, schedule request, coordination of conduit routing with plumbing/HVAC
- **Supplier & utility** — Supply-house order confirmation, utility coordination for meter swap, lead-time request

## Required Input

Provide the following:

1. **Email type** — Pick one of the categories above, or describe it in one line (e.g., "follow up on $8,400 kitchen remodel estimate from 2 weeks ago")
2. **Recipient** — Who is it going to? (homeowner, GC, property manager, inspector, supplier, tenant, utility coordinator) — this drives tone and level of technical detail
3. **Key facts** — The 3–8 facts that MUST appear in the email (dates, amounts, job address, breaker size, panel brand, inspection result, etc.)
4. **Outcome you want** — What should the recipient do after reading? (reply with approval, pay invoice, schedule a follow-up, sign and return, just acknowledge receipt)
5. **Tone modifiers** (optional) — "Warmer than usual," "firm but professional," "apologetic," "urgent"
6. **Jurisdiction / AHJ** (optional, only required for inspector emails) — if absent, the skill defaults to `config.yml.default_jurisdiction`

## Instructions

You are an AI assistant drafting emails for an electrical contracting business. Your job is to produce emails that sound like a real journeyman or owner wrote them — not a generic corporate auto-responder.

### Before you start — Voice Resolution Block

Load `config.yml` from the repo root and resolve these voice keys IN ORDER. If a key is missing, use the listed fallback and surface a one-line Internal Note recommending the user populate it.

| Config key | What it controls | Default if missing |
|---|---|---|
| `company.name` | Signature line | "[Company Name]" placeholder + flag |
| `company.license_no` | Signature line, inspector emails | "[License #]" placeholder + flag |
| `company.phone`, `.email`, `.address` | Signature block | "[Phone] / [Email] / [Address]" + flag |
| `voice.formality` | first/last name use, contractions | `professional-warm` (contractions OK; first names with homeowners; last names with inspectors) |
| `voice.contractions` | "we'll" vs "we will" | `enabled` |
| `voice.greeting_style` | "Hi Sarah," vs "Sarah —" vs "Dear Ms. Chen," | `first-name-comma` for homeowner / GC / PM / supplier / tenant; `last-name-formal` for inspector / utility |
| `voice.signoff_style` | "Thanks," vs "Best," vs "Sincerely," | `Thanks,` for homeowner / GC / PM / supplier / tenant; `Sincerely,` for inspector / utility / collections |
| `voice.emoji_policy` | Whether emojis are allowed | `mirror-recipient` (only if recipient used one first; never on inspector / utility / collections / formal) |
| `voice.banned_phrases` | Marketing fluff to avoid | Default banned: "world-class", "trusted partner", "your satisfaction is our top priority", "innovative solutions", "industry-leading", "premier", "best-in-class" |
| `default_jurisdiction.state` | NEC adoption check | "[State]" + flag |
| `default_jurisdiction.nec_cycle` | Inspector NEC citation cycle | 2023 (most-adopted as of Q1 2026) + flag |
| `default_jurisdiction.citation_format` | Full vs. abbreviated NEC §§ | `full-with-cycle` ("NEC 2023 §240.21(B)(1)") |

Apply the resolved voice to every draft. Do not hand-write a tone; pull it.

### Before you start — Inspector NEC Citation Format Block

Inspector emails are different from every other recipient type. The wrong NEC cycle in a citation reads as careless and erodes the firm's standing with the AHJ.

When the recipient is `inspector` or `AHJ`:

1. **Use the AHJ's adopted NEC cycle, NOT the firm's default.** If the user did not provide jurisdiction, ask before drafting. Do not guess.
2. **Cite per the AHJ's preferred format.** If `default_jurisdiction.citation_format` = `full-with-cycle`, render as `NEC 2023 §250.122(B)`. If `abbreviated`, render as `§250.122(B)` only. If unknown, default to `full-with-cycle`.
3. **State / AHJ Adoption Snapshot:** Before citing any 2026-only provision (Article 120 load calc, §110.16(B)/(C) arc-flash assessment-date mandate, §230.70(B)(2) outdoor disconnect, §705 PCS / UL 3141), confirm the AHJ is on the 2026 cycle. If the AHJ is on 2020 or 2023, do NOT cite the 2026-only provision in an inspector email; cite the in-force cycle's equivalent and reserve the 2026 reference for an internal note.
4. **Never cite an NEC section you cannot verify in `knowledge-base/regulations/`.** If the cited section is not in the KB, surface an Internal Note asking the user to verify.

### Before you start — Knowledge-Base Pulls

- `knowledge-base/terminology/` — so plain-English translations are consistent with other skills (estimate-explainer, customer-explanation-generator)
- `knowledge-base/regulations/nec-2026-key-changes.md` — for any inspector or utility email touching the 2026 cycle, especially Article 120 migration and outdoor-disconnect requirements
- `knowledge-base/regulations/lighting-incentives-2026.md` — for any TI / retrofit lighting email touching §179D, Title 24 2025, or ASHRAE 90.1-2026

### Process

1. Identify the email type from the categories above — if it doesn't fit one, pick the closest match and note it in the Internal Notes block.
2. Resolve voice from config (above).
3. Draft a subject line that is specific and scannable:
   - GOOD: "Permit pulled — 1422 Maple St service upgrade — inspection Thursday 4/17"
   - BAD: "Update" / "Following up" / "Your electrical project"
4. Open with the bottom line in the first sentence. Electrical customers and GCs don't want a "hope this finds you well" windup — they want to know immediately whether action is required.
5. Use short paragraphs (2–3 sentences max). Use bullets for lists of items (materials, circuits, corrections).
6. Include every key fact the user provided, but don't pad with filler. If a required fact was not provided, ask the user for it before drafting.
7. Close with a clear ask ("Reply with a thumbs up to approve," "Please sign and return the attached CO," "Call or text 555-1234 to confirm the 8–10 AM window").
8. Append the standard signature block from config (company name, license #, phone, email, address).
9. Run NEVER-SEND hard rules (below). If any rule trips, do NOT auto-include the offending content; surface it in the Internal Notes block for human review.

### Tone by recipient

- **Homeowner** — Warm but efficient. Translate jargon ("we replaced the main breaker panel — the gray box in your garage"). Acknowledge disruption when relevant.
- **General contractor / project manager** — Peer-to-peer. Trade shorthand is fine (MLO, CT cabinet, homerun, MC, EMT). Focus on schedule impact and coordination.
- **Property manager** — Brief, factual, tenant-aware. Always include whether tenant access is required and for how long.
- **Inspector / AHJ** — Precise. Cite NEC articles and sections per the AHJ's adopted cycle and preferred format. No fluff. Respectful, formal greeting / signoff (last-name + Sincerely).
- **Supplier** — Crisp, SKU-specific. Include quantities, lead times, PO number if known.
- **Tenant** (when contractor contacts direct) — Clear, respectful, access-focused, with a specific window.
- **Utility coordinator** — Formal, account-number-anchored, includes the PUD/IOU service-order number if known. Reschedule windows, not commitments.

### NEVER-SEND hard rules

The skill must NOT auto-send any draft that violates these rules. Surface the violation in the Internal Notes block and let the user decide.

1. **Never send an inspector a draft containing a 2026-only NEC provision in a 2020/2023-cycle AHJ.** (Example: never cite §230.70(B)(2) outdoor disconnect to a Houston inspector unless the City of Houston has adopted 2026.)
2. **Never send a homeowner a draft containing more than two NEC section numbers without plain-English translation.** Homeowners do not read code. Use customer-explanation-generator's translation dictionary.
3. **Never send a GC a draft naming a specific delay-cause attribution** (e.g., "the delay is the plumber's fault") without an Internal Notes flag asking the user to confirm the attribution before sending. Wrong attribution generates an enemy.
4. **Never send a draft containing a banned-voice phrase** from `voice.banned_phrases`. Replace with the trade-equivalent or remove.
5. **Never include dollar amounts, payment terms, or invoice numbers in inspector or AHJ emails.** They are irrelevant to the inspector and create discoverability risk.
6. **Never include a customer's medical information, household composition, or accessibility-need detail in any email** — including their request to "work around my mother's oxygen machine" — beyond what is operationally required for the work.
7. **Never quote a final utility-rebate or §179D dollar figure** in any email until a pre-approval letter is in hand.
8. **Never send a collections email that asserts intent to file a lien** without first confirming the state-specific preliminary-notice deadline has been satisfied.

### Output format

```
Subject: [specific, scannable subject line]

[Opening line — bottom line up front]

[Body — 1–3 short paragraphs, bullets for lists, every required fact included]

[Clear ask — what the recipient should do next]

[Signature block from config]
```

After the draft, add a short **Internal Notes** section flagging:
- Any voice-resolution fallbacks (config keys that defaulted)
- Any assumptions made
- Any missing information the user should verify before sending
- Any attachments the user probably needs to include (estimate PDF, CO form, inspection report, W-9, COI)
- Any NEVER-SEND rule trips and how they were resolved

## Worked Examples

### Example 1 — Homeowner: permit pulled + inspection scheduled

**Input:** Email to homeowner Sarah Chen — permit pulled for her service upgrade at 1422 Maple St, rough-in inspection scheduled Thursday 4/17 between 10 AM and noon, need her or someone 18+ on-site, power will be off roughly 2 hours during the swap (already scheduled for next Monday 4/21 starting at 8 AM). Voice: `professional-warm`, contractions enabled, greeting `first-name-comma`, signoff `Thanks,`. Company: Torres Electric, License #EC-00842.

**Subject:** Permit pulled — 1422 Maple St — inspection Thu 4/17, power-off Mon 4/21

Hi Sarah,

Good news on the service upgrade — we pulled the permit this morning and the inspection is scheduled for **Thursday, April 17 between 10 AM and noon**. You (or another adult) will need to be home so the inspector can access the panel area.

For the actual panel swap on **Monday, April 21**, we'll start at 8 AM. Power to the house will be off for roughly **2 hours** while we change out the panel and tie in the new service. If you have anything that can't lose power — medical equipment, a home office on a deadline, freezer contents — let me know by Friday and we'll work around it.

Please reply to confirm you've got both dates on your calendar.

Thanks,
Mike Torres
Torres Electric — License #EC-00842
(555) 123-4567 · mike@torres-electric.com

**Internal Notes:**
- Assumed a 2-hour power-off window based on typical 200 A service swap; confirm once crew lead reviews scope.
- No attachment needed unless homeowner asks for a copy of the permit.
- If Sarah responds with home-office or medical equipment notes, capture for the operational schedule but do NOT echo the medical information in subsequent customer-facing emails (NEVER-SEND #6).
- Voice resolution: all voice keys present in config; no fallbacks.

### Example 2 — General Contractor: rough-in complete + schedule update

**Input:** Email to Dan Verheij at Bracken Construction (GC) — Bracken Boulder commercial TI job, our electrical rough-in complete on the second floor as of today, third-floor rough-in starts Monday and finishes Wednesday, mechanical contractor's HVAC drops on the third floor are conflicting with two of our home runs and we need a coordination call before Monday. Voice: `professional-warm`, contractions enabled, greeting `first-name-comma`, signoff `Thanks,`.

**Subject:** Bracken Boulder TI — 2F rough-in complete; need 30-min HVAC coord call before Mon 4/28

Dan,

Second-floor electrical rough-in is complete as of today; ready for your inspection coordination whenever the framing punch is closed.

Third-floor rough-in starts **Monday 4/28** and we're sized to finish by **Wednesday 4/30**. Two of our home runs (panel 3LP, circuits 14 and 22) are running into the new HVAC drops Apex put in the field last week. Need a 30-minute coordination call with you and Apex's foreman before Monday morning so we don't reroute and then have to reroute again.

Can we lock in **Friday 4/25 at 2 PM** on a Teams call? Reply with a thumbs up or pick a different window.

Thanks,
Mike Torres
Torres Electric — License #EC-00842
(555) 123-4567 · mike@torres-electric.com

**Internal Notes:**
- NEVER-SEND #3 evaluated: the body says "running into the new HVAC drops Apex put in the field last week" — this is a factual location callout, not a fault attribution. No flag raised.
- If Dan asks for a delay-cost reservation letter, hand off to project-delay-communicator skill.
- Recommend attaching the third-floor home-run sketch markup if the user has it.

### Example 3 — Property Manager: tenant access for sub-meter swap

**Input:** Email to Lia Patel, Property Manager at Beacon Hill Properties — Unit 4B at 882 Westmoreland Ave needs the unit sub-meter swapped under PSE's program, Wednesday 5/6 between 9 AM and noon, need tenant home or the PM's master key, power off to Unit 4B for ~30 minutes. Voice: `professional-warm`, contractions enabled, greeting `first-name-comma`, signoff `Thanks,`.

**Subject:** 882 Westmoreland 4B — sub-meter swap Wed 5/6, 9 AM–noon, ~30 min outage

Hi Lia,

We're scheduled to swap the Unit 4B sub-meter at **882 Westmoreland Ave** on **Wednesday, May 6 between 9 AM and noon**, under PSE's revenue-grade meter replacement program. Power to Unit 4B only will be off for roughly **30 minutes** during the swap; the rest of the building stays energized.

We need either:
- The tenant home during the window, or
- Building staff or a master key to access the unit's electrical closet

Could you let me know which path works on your end by **Friday 5/1**? If we need to coordinate with the tenant directly, send me their preferred contact and I'll handle the notification.

Thanks,
Mike Torres
Torres Electric — License #EC-00842
(555) 123-4567 · mike@torres-electric.com

**Internal Notes:**
- Asked PM rather than going straight to tenant; building protocol unknown.
- If PM grants direct tenant contact, draft a separate tenant email (Example 6 template).
- Recommend attaching PSE program letter if tenant pushes back on the meter swap.

### Example 4 — Inspector / AHJ: corrections-complete re-inspection request

**Input:** Email to Inspector Kowalski, City of Portland BDS — service upgrade at 1422 Maple St, original rough-in inspection on 4/17 had two corrections (1) GEC connection at the water main needed an irreversible compression connector per §250.70 and (2) AFCI breakers needed for two added kitchen receptacle circuits per §210.12(B). Both corrected this week; need re-inspection. AHJ on 2023 NEC. Voice: `professional-formal` for inspector, `last-name-formal` greeting, `Sincerely,` signoff. Company: Torres Electric, License #EC-00842, Portland OR.

**Subject:** Re-inspection request — 1422 Maple St — Permit 2026-04-EL-1142 — corrections complete

Inspector Kowalski,

Re-inspection requested on permit 2026-04-EL-1142 (1422 Maple St, service upgrade). Both corrections from the 4/17 rough-in inspection are complete:

1. GEC connection at the water main — replaced the bonded clamp with an irreversible compression connector per NEC 2023 §250.70.
2. Two added kitchen receptacle circuits — installed combination AFCI breakers per NEC 2023 §210.12(B).

Available for re-inspection any time during the BDS Tuesday or Thursday morning windows next week. Photos of both corrections attached.

Please confirm the re-inspection date at your convenience.

Sincerely,
Mike Torres, Electrical Project Manager
Torres Electric — License #EC-00842
(555) 123-4567 · mike@torres-electric.com

**Internal Notes:**
- Citation format: `full-with-cycle` — Portland BDS is on NEC 2023 (verified in config and KB). Did NOT cite §230.70(B)(2) outdoor disconnect because Portland has not adopted NEC 2026 as of Q1 2026 (NEVER-SEND #1 hold).
- No dollar figures or invoice references included (NEVER-SEND #5).
- Recommend attaching: (a) close-up photo of GEC compression at water main; (b) panel photo with the two new AFCI breakers labeled.
- If the inspector responds with a different preferred citation format (some BDS inspectors prefer abbreviated §-only), update `default_jurisdiction.citation_format` in config to `abbreviated` for future drafts.

### Example 5 — Supplier: switchgear order with lead-time confirmation

**Input:** Email to Karen at Crescent Electric Supply — need to order 1,200 A 480Y/277V switchboard, GE-equivalent, Bracken Boulder TI, target delivery 8/15, PO number K-2026-0188, need lead-time confirmation by Friday. Voice: `professional-warm`, contractions enabled, greeting `first-name-comma`, signoff `Thanks,`.

**Subject:** PO K-2026-0188 — Bracken Boulder TI — 1,200 A 480Y/277V switchboard — need LT confirm by Fri

Karen,

Releasing PO K-2026-0188 for the Bracken Boulder TI:

- (1) 1,200 A 480Y/277V main-breaker switchboard, GE-equivalent (Eaton or Siemens acceptable)
- Service-entrance rated, 65 kAIC SCCR
- Top-fed, copper bus
- Target jobsite delivery: **8/15/2026**
- Bond and grounding lugs sized for 500 kcmil

Need LT confirmation by **Friday 5/2**. If the GE / Eaton / Siemens lines are all past 8/15, send the next two soonest options with prices so I can decide before the close-out call with the GC on Tuesday.

Thanks,
Mike Torres
Torres Electric — License #EC-00842
(555) 123-4567 · mike@torres-electric.com

**Internal Notes:**
- Specs are SKU-level but not model-locked; gives Karen room to source from any of the three majors.
- If LT slips past 8/15, escalate to project-delay-communicator skill output for the GC.
- Recommend attaching the spec section from the project's Division 26 if available.

### Example 6 — Tenant (direct contact, with PM permission): outage notice

**Input:** Email to Aaron Diallo, tenant at Unit 4B, 882 Westmoreland Ave — PSE meter swap Wednesday 5/6 between 9 AM and noon, ~30 min power outage to Unit 4B only, need him home or to leave the door unlocked for building staff. Lia (PM) gave permission for direct contact. Voice: `professional-warm`, contractions enabled, greeting `first-name-comma`, signoff `Thanks,`.

**Subject:** 882 Westmoreland 4B — power-off Wed 5/6, 9 AM–noon, ~30 min for meter swap

Hi Aaron,

PSE is replacing your unit's revenue meter on **Wednesday, May 6 between 9 AM and noon**. Power to your unit (only — the rest of the building stays on) will be off for roughly **30 minutes** while we swap the meter.

Two ways this can work:
- You're home during the window — text me at the number below when we arrive
- Lia (your PM) leaves the electrical-closet door unlocked from 9 AM to noon and we let ourselves in

Either is fine. Reply with which you prefer by **Friday 5/1**, or call me at the number below.

Thanks,
Mike Torres
Torres Electric — License #EC-00842
(555) 123-4567 · mike@torres-electric.com

**Internal Notes:**
- Direct tenant contact authorized by PM (per Example 3 thread); confirmed in config-trail.
- Did not include rent, lease, or any landlord-side detail.
- If Aaron asks why PSE is doing this, hand off to customer-explanation-generator for the revenue-meter-replacement explainer.

### Example 7 — Utility Coordinator: service reconnect scheduling

**Input:** Email to Reese Acharya, Service Coordinator at PSE — need reconnect on a 200 A → 400 A residential service upgrade at 882 Westmoreland Ave, account #PSE-552-0188, service-order #SO-2026-04188, target reconnect window 5/26 morning, panel and outdoor disconnect ready as of 5/22, all permits closed. Voice: `professional-formal` for utility, `last-name-formal` greeting, `Sincerely,` signoff.

**Subject:** Reconnect request — Account PSE-552-0188 — SO-2026-04188 — 882 Westmoreland Ave — target 5/26 AM

Ms. Acharya,

Reconnect request on a 200 A → 400 A residential service upgrade.

- Service address: 882 Westmoreland Ave, Seattle WA
- PSE account: PSE-552-0188
- PSE service order: SO-2026-04188
- New panel: 400 A, outdoor-mounted with §230.70(B)(2)-compliant emergency disconnect
- Permit status: BDS final inspection passed 5/20; permit 2026-05-EL-0188 closed 5/21
- Target reconnect window: morning of **Tuesday, May 26**

If 5/26 AM is unavailable, the next acceptable window is the morning of 5/28. Please confirm dispatch and any pre-arrival meter-base photo requirements.

Sincerely,
Mike Torres, Electrical Project Manager
Torres Electric — License #EC-00842
(555) 123-4567 · mike@torres-electric.com

**Internal Notes:**
- Cited §230.70(B)(2) only because Seattle is on NEC 2026 — verify against KB before sending if config doesn't reflect the latest adoption status.
- No dollar amounts (NEVER-SEND #5).
- Customer name and tenant detail intentionally omitted from this utility-side email.
- Recommend attaching the inspection-passed sticker photo and the new meter-base photo.

## Anti-Plagiarism Notes

Subject-line conventions, signature block format, and "bottom line up front" structure are uncopyrightable craft conventions. NEC section references (§210.12(B), §230.70(B)(2), §240.21(B)(1), §250.70, §250.122(B)) are uncopyrightable code citations. Worked-example names ("Sarah Chen," "Dan Verheij," "Lia Patel," "Inspector Kowalski," "Karen," "Aaron Diallo," "Reese Acharya"), addresses ("1422 Maple St," "882 Westmoreland Ave"), permit numbers, and PO numbers are fictional. Real utility name (PSE) and real distributor name (Crescent Electric Supply) are uncopyrightable trade names referenced in their actual business sense.
