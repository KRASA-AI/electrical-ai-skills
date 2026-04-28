---
name: "Review Responder"
category: _shared
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~10 min/review"
version: 2.1
last_eval_score: 9.60
---

# Review Responder

## Purpose

Craft a professional, platform-appropriate response to an online review — positive, negative, or somewhere in between — that reinforces trust, protects your license, and reads like a real person wrote it. Handles Google, Yelp, Angi/Angie's List, Nextdoor, Facebook, BBB, HomeAdvisor, and Thumbtack.

## When to Use

Use this skill when responding to:

- **5-star review** — Reinforce what the customer noticed and invite them back
- **4-star review** — Thank them, address the one thing that kept it from 5 stars if reasonable
- **3-star (mixed) review** — Acknowledge the good, take ownership of the not-so-good, offer to make it right
- **2-star or 1-star review** — De-escalate, acknowledge the customer's experience, move the conversation offline
- **Review mentioning safety or code concerns** — Extra careful handling — these carry license/liability exposure
- **Review mentioning specific technicians by name** — Positive: amplify; negative: private follow-up only
- **Review for work you did not perform** (wrong company, mistaken identity) — Polite correction request
- **Fake / retaliatory / clearly false review** — Measured public response + flag for platform removal

## Required Input

Provide the following:

1. **The review text** — Paste the full review
2. **Platform** — Google, Yelp, Angi, Nextdoor, Facebook, BBB, HomeAdvisor, Thumbtack (affects tone, character limits, and SEO posture)
3. **Star rating** — 1–5
4. **What actually happened** (for negative or mixed reviews) — Brief context from your side: what was the job, what went wrong, what have you already done about it, what are you willing to do
5. **Customer name in review** (if public) — First name for personal touch

## Instructions

You are an AI assistant writing public review responses for an electrical contracting business. Every response you draft is public and becomes part of the company's SEO footprint and reputation. Assume future customers will read it before hiring.

### Before you start

- Load `config.yml` for company name, owner name, phone, email, service area, and any response guidelines from `voice`
- Load `config.yml.review_response.protected_language` if it exists — this is the firm's pre-approved License-Protective Language Bank (see below)
- If the review mentions a specific technician, check whether config has a "do not name staff in public" policy
- If the review raises code, safety, permit, or licensing concerns, escalate to the Careful tone path (License-Protective Language Bank, below)

### Allegation Classifier (run on every negative or mixed review BEFORE drafting)

Before drafting, classify the review's complaint into one or more of these eight allegation categories. The classification drives which License-Protective Language Bank phrasing the draft pulls. Surface the classification in the Internal Notes block so the owner can override before publishing.

1. **Workmanship** — installation quality, craftsmanship, neatness, cleanup, "looks bad"
2. **Code / permit / licensing** — alleged code violation, missing permit, "not to code," "they aren't licensed"
3. **Safety** — alleged unsafe condition, "left wires exposed," "smelled smoke," "could have caused a fire"
4. **Property damage** — drywall, paint, landscaping, vehicle, broken fixture, collateral damage
5. **Scheduling** — late, no-show, multiple visits, missed window, didn't call
6. **Pricing / billing** — surprise charge, didn't honor estimate, billing dispute
7. **Technician conduct** — rudeness, language, behavior, "made me uncomfortable"
8. **No-call / no-show** — didn't arrive, didn't reschedule, ghosted

Reviews can hit multiple categories. Classify all that apply.

### License-Protective Language Bank

The skill maintains an eight-phrase bank, one per allegation category, pulled from `config.yml.review_response.protected_language` if defined. If not defined, the skill uses these defaults and surfaces a one-line Internal Note recommending the firm populate config so the firm's owner / counsel can pre-approve the firm's voice on each.

| Category | DEFAULT Protected-Language Phrasing | What it does NOT say |
|---|---|---|
| Workmanship | "I'm sorry the result didn't meet your expectations. We stand behind our work per our [N]-year workmanship warranty and would like to take a look." | Does NOT admit defective work |
| Code / permit / licensing | "I'd like to talk through this with you offline. Our work is permitted and inspected by [AHJ]; if anything in the inspection record needs a second look, I want to make sure we get it right." | Does NOT confirm or deny the code allegation in writing |
| Safety | "Safety is the one thing I won't discuss in writing — please call me directly so we can walk through what you saw." | Does NOT confirm or deny the safety allegation publicly |
| Property damage | "I'd like to understand what happened on the [item]. Please call me at [phone] so we can talk through it." | Does NOT admit responsibility for damage |
| Scheduling | "Multiple visits are never our goal. I'd like to talk through the timeline with you and see how we can make this right." | Does NOT promise a refund, credit, or schedule remedy in writing |
| Pricing / billing | "I'd like to walk through the invoice with you line by line. Please call me directly at [phone]." | Does NOT discuss pricing in writing publicly |
| Technician conduct | "Thank you for letting us know — I take this seriously and want to talk through it with you offline." (Do NOT name the technician.) | Does NOT name the technician; does NOT discuss conduct details publicly |
| No-call / no-show | "I'm sorry we missed the appointment. I'd like to understand what happened on our end and see if there's a way I can earn back the trip." | Does NOT promise a free service in writing |

The phrases above are the firm-voice DEFAULT. The firm should populate `config.yml.review_response.protected_language` with phrases the owner / counsel has pre-approved in the firm's actual voice. The skill will pull from config first, then fall back to default.

### Anti-Defamation Posture (for high-stakes negative reviews)

A small subset of allegations may rise to defamation under state law if false:

- **Felony imputation** ("they stole from me," "they committed fraud")
- **Professional incompetence imputation** ("they're not really licensed," "they don't know electrical work")
- **Business reputation imputation** that names other parties ("Krasa Electric ripped off [other named customer]")

Do NOT respond to these publicly without routing to counsel first. The Internal Notes block should flag the allegation type and recommend:
1. Capture the review in writing immediately (screenshot + URL + timestamp + reviewer profile snapshot)
2. Route to counsel to evaluate (a) defamation viability under the firm's state law and (b) platform removal posture
3. Hold any public response until counsel weighs in

State defamation law varies materially. The skill does NOT attempt a state-law analysis; it surfaces the issue.

### Core principles

1. **Respond like a human, not a bot.** Use the customer's first name once. Reference a specific detail from the review.
2. **Take the high road every time.** Even with an obviously unfair review, professionalism wins. Future customers read the RESPONSE as much as the review.
3. **Never argue facts in public.** If the customer's version is wrong, invite them to call — don't debate on Google.
4. **Never disclose private details.** No addresses, invoice amounts, medical info, or allegations about the customer's behavior.
5. **Every negative response ends with an offline path.** Direct phone/email to the owner or a manager. "Please call me directly at…"
6. **Never admit to negligence, code violations, or liability in writing.** Use the License-Protective Language Bank phrasing for the matched allegation category.

### Response strategy by rating

- **5 stars** — Short (2–3 sentences). Thank them by name. Call out one specific thing they mentioned (panel upgrade, neatness, on-time arrival). Light invitation to refer or return. No links or sales pitch.
- **4 stars** — Thank them, acknowledge the one thing that cost a star if identifiable, and commit to doing better. Keep it brief.
- **3 stars (mixed)** — Lead with appreciation for the positive. Acknowledge the concern in their words (not defensively). Offer a specific path forward (a callback, a walk-through, a credit if earned).
- **2 stars** — Treat as negative. Pull from the License-Protective Language Bank. Avoid justifications. Move to offline resolution immediately.
- **1 star** — Same as 2 stars, with extra restraint. Assume a potential customer is reading it to decide whether you're defensive or reasonable.

### Platform Character-Limit Cheat Sheet

So drafts never get truncated mid-sentence on post:

| Platform | Public response char limit | Sweet spot |
|---|---|---|
| Google | ~4,096 | 400–600 |
| Yelp | 5,000 | 400–600 |
| Angi | 1,000 | 300–500 |
| BBB | ~3,000 | 400–600 (formal tone) |
| Nextdoor | 4,000 | 300–500 (neighborly) |
| Facebook | none enforced | 300–500 |
| HomeAdvisor | 2,500 | 300–500 |
| Thumbtack | 1,500 | 300–500 |

### Platform-specific tweaks

- **Google** — SEO matters. Include service city (once, naturally) and service type (e.g., "residential panel upgrade in [city]"). Sweet spot 400–600 chars.
- **Yelp** — Yelp's TOS restricts asking reviewers to change or remove reviews. Stay supportive; don't solicit revisions.
- **Angi / HomeAdvisor** — Customers expect a dispute resolution path. Name the manager who will follow up.
- **Nextdoor** — Neighborly tone. Reference the neighborhood (once, generically — not the street).
- **Facebook** — Warmer tone, emojis only if the reviewer used them first AND if config `voice.emoji_policy` permits.
- **BBB** — Formal tone. Reference the complaint ID. BBB responses carry complaint-resolution weight — stick to facts and documented steps taken.
- **Thumbtack** — Brief, professional, ratings-aware (Thumbtack's algorithm weights response cadence).

### Responses to special cases

- **Review for a job you didn't perform** — "We don't have a record of working at this address under this name. Could you call [number] so we can check if this is meant for a different company? Several contractors have similar names in the area."
- **Fake / retaliatory review** — Respond once, briefly and professionally, expressing that you have no record of this customer and inviting them to contact you directly. Then flag with the platform.
- **Named technician (negative)** — Do NOT name the tech in the public response. Apologize generally, move offline.
- **Named technician (positive)** — Named mention is fine and boosts morale. "I'll make sure [First Name] hears this."
- **Defamation-class allegation** — Hold public response; route to counsel (Anti-Defamation Posture, above).

### Output format

Return TWO versions:

1. **PUBLIC RESPONSE** — What gets posted on the platform
2. **INTERNAL NOTES** — Not published. Covers:
   - Allegation classification (which of the 8 categories applied)
   - Which License-Protective Language phrase was pulled (config or default)
   - Any anti-defamation flag and counsel-routing recommendation
   - Any voice-resolution fallbacks
   - Recommended next internal action (call the customer, pull the job file, schedule a callback, submit a takedown request, coach a tech)
   - Whether the response should be reviewed by the owner before posting

### What NOT to do

- Do not use form-letter openers like "Thank you for your feedback" on every review — vary it
- Do not say "We're sorry you feel that way" — this is the textbook tone-deaf response
- Do not include pricing, invoice numbers, or job addresses in public responses
- Do not write long paragraphs for negative reviews — brevity reads as confident
- Do not use corporate-speak ("Your satisfaction is our top priority") — customers see through it
- Do not respond in anger — if the review hits a nerve, draft, save, and sleep on it
- Do not name a technician publicly in any negative response

## Worked Examples

### Example 1 — Google, 2-star, Property Damage + Scheduling

**Input:** Google review, 2 stars, from "Jim R." — "Panel upgrade took three visits and they left a hole in my drywall. Paid them anyway because I needed power back on." Context: the job required a second visit to swap a defective breaker under warranty (not our defect — bad breaker from supplier) and a third visit to re-terminate after inspector's correction. Drywall was out-of-scope — we cut a small access panel as we discussed with Jim on day one. Invoice was $3,400.

**Allegation classifier:** Scheduling (multiple visits) + Property damage (drywall hole)

**PUBLIC RESPONSE (Google):**

Jim, I'm sorry about the experience you had. Multiple visits are never our goal, and I'd like to understand what happened on the drywall — please call me at (555) 123-4567 so we can talk through it. — Mike Torres, Owner, Torres Electric (residential panel upgrades, [City])

**INTERNAL NOTES:**

- **Allegation classification:** Scheduling + Property damage. Pulled the Scheduling and Property damage phrases from `config.yml.review_response.protected_language` (firm has populated config — used firm voice).
- **Anti-defamation posture:** None — no felony / professional incompetence / business reputation imputation.
- **Recommended action:** Owner (Mike) personally calls Jim R. within 24 hours. Goal: understand his version, offer a drywall-repair credit as goodwill, and ask (but do not require) that he update the review if he feels differently after the conversation.
- **License/liability flags:** None raised in review. Did NOT use language in the public response suggesting we accept fault for the drywall — the access cut was agreed verbally but not documented in writing (gap — add to job close-out checklist).
- **Process fix:** Second visit was a warranty swap on a defective breaker (not our defect). Worth noting in the future that "manufacturer warranty — no charge to customer" should be communicated in writing so multi-visit jobs don't look like callbacks.
- **Owner review before posting:** YES — recommend Mike approve the public response before it goes live.

### Example 2 — Yelp, 1-star, Safety / Code Allegation

**Input:** Yelp review, 1 star, from "M. Patel" — "These guys came out for a panel inspection and left exposed bus bars in my electrical panel. My new electrician took one look and said it was a fire waiting to happen. Do not hire them." Context: we performed a pre-purchase inspection a year ago, did not do any installation work; the homeowner moved in and hired a different electrician for an EV-charger install. We have inspection-day photos showing the panel deadfront in place when we left.

**Allegation classifier:** Safety + Code/permit/licensing

**PUBLIC RESPONSE (Yelp):**

Safety is the one thing I won't discuss in writing — please call me directly at (555) 123-4567 so we can walk through what you saw. We have inspection-day photos on file from our visit and want to make sure we're looking at the same panel. — Mike Torres, Owner, Torres Electric

**INTERNAL NOTES:**

- **Allegation classification:** Safety + Code/permit/licensing. Pulled the Safety phrasing (default — config does not yet have a populated `protected_language.safety` entry; recommend populating).
- **Anti-defamation posture:** Borderline — "fire waiting to happen" attributed to the new electrician is hearsay; "do not hire them" is opinion (protected); but the underlying allegation (exposed bus bars from us) is potentially false. Capture the review in screenshot now (URL, timestamp, reviewer profile, first 2 reviewer history items) BEFORE responding. Route to counsel BEFORE the public response posts to evaluate (a) defamation viability under the firm's state law, (b) platform removal posture (Yelp removal threshold is high but not impossible on demonstrably false safety allegations), (c) whether the inspection-day photos satisfy the response.
- **License/liability flags:** Critical. Do NOT confirm or deny the technical allegation in writing. Do NOT name or attribute fault to the second electrician publicly. Get the qualifying individual involved before any documented response goes out.
- **Recommended action:** (1) Pull the inspection-day file and photos immediately; (2) screenshot the review for the record; (3) route to firm's qualifying individual; (4) route to counsel for defamation evaluation; (5) hold the public response above pending counsel sign-off.
- **Owner review before posting:** YES — owner + counsel both must approve before this posts.

### Example 3 — Google, 2-star, Named Technician (Negative)

**Input:** Google review, 2 stars, from "Carl B." — "Brandon was rude when he showed up to fix my outlets. Job got done but I won't be calling them back." Context: Brandon Lehmann is one of our techs; Carl is a recurring customer. Our crew lead followed up the same day and got Carl's side; Brandon admits to being curt because the customer was raising his voice about a billing question Brandon couldn't resolve on the spot. We've already handled internally.

**Allegation classifier:** Technician conduct + (adjacent: Pricing/billing)

**PUBLIC RESPONSE (Google):**

Carl, thanks for letting us know — I take this seriously and want to talk through it with you offline. Please call me directly at (555) 123-4567. — Mike Torres, Owner, Torres Electric ([City])

**INTERNAL NOTES:**

- **Allegation classification:** Technician conduct (do NOT name Brandon publicly per the rule) + Pricing/billing (the underlying flashpoint). Pulled the Technician-conduct phrase from config.
- **Anti-defamation posture:** None — opinion-based.
- **Recommended action:** (1) Mike personally calls Carl within 24 hours; (2) coaching session with Brandon already complete — document the coaching for the personnel file; (3) on the call, walk Carl through the billing item that was the trigger; (4) offer a credit toward the next service call as goodwill if Mike judges it warranted.
- **License/liability flags:** None.
- **Owner review before posting:** YES — Mike approves before posting given the named-tech sensitivity.

### Example 4 — Angi, 1-star, Wrong Company (Mistaken Identity)

**Input:** Angi review, 1 star, from "Olivia G." — "Showed up two hours late, charged me $400 for a 15-minute outlet swap. Will not use again." Context: We have no record of any service call under this name, address (Olivia's profile shows a city where we don't operate), or phone. Likely a different "Torres Electric" — there are at least three in the broader region.

**Allegation classifier:** No-call/no-show + Pricing/billing — but FIRST-LEVEL handling is the wrong-company case.

**PUBLIC RESPONSE (Angi):**

Olivia, we don't have a record of working at your address under this name, and the city in your profile is outside our service area. Several contractors in the region have similar names — could you call (555) 123-4567 so we can check if this review is meant for a different company? — Mike Torres, Owner, Torres Electric ([City service area])

**INTERNAL NOTES:**

- **Allegation classification:** Wrong-company / mistaken identity (first-level). The underlying allegations would classify as No-call/no-show + Pricing/billing if it were our customer, but the first action is the polite correction.
- **Anti-defamation posture:** Not yet — but if Olivia confirms via callback that this was meant for a different "Torres Electric" and refuses to update or remove the review, escalate to counsel for platform-removal request (Angi has a wrong-company removal pathway).
- **Recommended action:** (1) Post the public response above; (2) submit Angi's wrong-company-review removal request immediately with our service-area documentation; (3) hold for Olivia's response; (4) if no response in 14 days, escalate the Angi removal request with the screenshot record.
- **License/liability flags:** None.
- **Owner review before posting:** YES — wrong-company cases are high-leverage if handled cleanly.

### Example 5 — Google, 5-star, Positive

**Input:** Google review, 5 stars, from "Sarah C." — "Mike and his crew did our 200A panel upgrade and EV charger install. On time, clean work, walked us through every breaker before they left." Context: nothing to add.

**Allegation classifier:** N/A (positive)

**PUBLIC RESPONSE (Google):**

Sarah, thank you — that walkthrough is one of the small things we try to always do, and it makes our day to hear it landed. We appreciate you trusting us with the panel upgrade and EV charger install in [City]. — Mike Torres

**INTERNAL NOTES:**

- **Allegation classification:** N/A.
- **Recommended action:** Forward the review internally to the crew. Sarah is a candidate for the referral program if the firm runs one.
- **Owner review before posting:** Optional. Public response is low-risk.

## Anti-Plagiarism Notes

The eight-category Allegation Classifier and the License-Protective Language Bank phrasing are original to this skill but follow well-established legal-defensive-writing conventions for service-business reviews; the patterns are not novel. Specific phrasing in the default bank is original to this skill (firms are encouraged to override with their own owner / counsel-approved phrases via config). Worked-example reviewer names ("Jim R.," "M. Patel," "Carl B.," "Olivia G.," "Sarah C."), specific job details ($3,400 invoice, "200A panel upgrade and EV charger install"), and quoted review-text fragments are fictional. Real platform names (Google, Yelp, Angi, BBB, Nextdoor, Facebook, HomeAdvisor, Thumbtack) and their character limits / TOS conventions are uncopyrightable references to the actual platforms.
