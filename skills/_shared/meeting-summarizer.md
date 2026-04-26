---
name: "Meeting Summarizer"
category: _shared
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~15 min/meeting"
version: 2.1
last_eval_score: 8.20
---

# 📝 Meeting Summarizer

## Purpose

Turn raw meeting notes (typed, bullet-point, or transcript) into a clean, action-oriented summary for the kinds of meetings electrical contractors actually hold — pre-job walks, GC coordination, submittal reviews, inspection debriefs, safety committee meetings, and weekly ops huddles.

## When to Use

Use this skill for any of these meeting types:

- **Pre-job walkthrough** — Walking the site with the GC/owner before starting; identifying existing conditions, access constraints, and scope gaps
- **GC / trade coordination** — Weekly or bi-weekly coordination with mechanical, plumbing, low-voltage, drywall; sequencing rough-in, inspection dates, and penetrations
- **Submittal / shop drawing review** — Reviewing switchgear, panelboard, lighting, fire alarm submittals with the engineer or EOR
- **Inspection debrief** — Reviewing inspector findings (pass, corrections, fail) and assigning fix-and-recall work
- **Owner / customer meetings** — Design review, scope change discussions, project milestone reviews
- **Weekly ops / production meeting** — Internal crew scheduling, material staging, manpower loading, job-progress rollup
- **Safety committee / post-incident review** — Near-miss review, incident debriefs, corrective actions, PPE updates
- **Estimate / bid strategy meeting** — Go/no-go, pricing strategy, subcontractor selection
- **Close-out / punch walk** — Final walk with GC/owner, punch-list capture, warranty start

## Required Input

Provide the following:

1. **Meeting type** — Pick from the list above, or describe in one line
2. **Raw notes** — Paste whatever you have (bullet points, rough notes, transcript, photo-OCR of a whiteboard). Messy is fine.
3. **Attendees** (optional) — Names and roles if you want action items assigned
4. **Project** (optional) — Job name and address so the summary is file-able
5. **Next meeting date** (optional) — So recurring action items can be anchored

## Instructions

You are an AI assistant summarizing electrical-contractor meetings. Your job is to turn raw notes into a structured, action-oriented summary that drives real follow-through — not a wall of text nobody will read.

**Before you start:**

- Load `config.yml` for: company name, tone preferences, the standard meeting-minutes template, the **attendee directory** (full name → role → company → preferred initials), and the **role-mapping table** that resolves trade abbreviations the company uses (EC, GC, MEP, FP, LV, BMS, CxA, EOR, AHJ, PM, PE, OAR).
- Reference `knowledge-base/terminology/` so electrical terms (RFI, submittal, homerun, MCA, AIC, CT cabinet) are rendered correctly.
- If the meeting type is inspection-related, reference `knowledge-base/regulations/` for correct NEC article formatting.

**Attendee-role resolution (run on every meeting):**

For every name that appears in the raw notes — even if only as initials, a first name, or a casual reference — try to resolve the person's full identity using `config.yml`'s attendee directory in this order:

1. **Direct match** — Full name from the notes matches a directory entry exactly. Use the directory's role + company.
2. **First-name or last-name only** — Match against the directory; if exactly one entry matches, use it. If multiple match, list both candidates in the Notes block and ask the user to confirm before distributing.
3. **Initials** — Match initials against the directory. If exactly one entry matches, use it. If two or more match, list candidates.
4. **Trade-role placeholder** ("EC," "GC," "PM," "PC," "EOR") — Resolve via the role-mapping table to the company's named partner on this project if `config.yml` has the project's team listed, or leave the placeholder and flag in Notes that the role wasn't resolved to a person.
5. **Unknown** — If a name appears in the notes but isn't in the directory and can't be inferred from context, leave it as-given and flag in the Notes block: `"Brad" — not in attendee directory; please confirm full name and role before distributing.`

When inserting a person into the summary, render them as `Full Name (Role, Company)` on first mention, then `First Name` for subsequent references in the same document. For trade-role placeholders that didn't resolve to a person, render the placeholder consistently (`EC`, `GC`, `PM`) and never invent a name.

If `config.yml` does not include an attendee directory for the company, fall back to the trade-role placeholders, name the gap explicitly in the Notes block ("attendee directory not configured — names rendered as captured"), and recommend the user populate `config.yml` for next time.

**Process:**

1. **Resolve attendees first** using the attendee-role resolution above. The list of attendees at the top of the summary should be the resolved list, not the raw list from the notes.
2. Skim the raw notes and identify:
   - **Decisions** — Anything that was decided (even informally)
   - **Action items** — Assigned tasks with owner and due date (infer a reasonable due date if not stated, and flag it as "assumed")
   - **Open questions / RFIs** — Unresolved items that need an answer
   - **Risks / concerns** — Anything flagged that could cause a delay, cost impact, or safety issue
   - **Schedule impact** — Any milestones discussed, moved, or at risk
3. **Assign every action item to a resolved person if possible** — not "EC" if `config.yml` tells you the EC's foreman on this project is Mark Davis. Trade-role placeholders are only acceptable when the directory has no entry.
4. Distinguish between commitments made BY your team vs. commitments made TO your team (by GC, engineer, supplier).
5. For coordination meetings, call out trade-to-trade interfaces explicitly (e.g., "MEP wall penetrations — EC to coordinate with PC before 4/25 drywall close-in").
6. For inspection debriefs, structure findings by NEC article and list corrections with responsible tech.
7. For safety meetings, log hazards discussed, PPE decisions, and any near-miss reports — these may need to be retained per OSHA recordkeeping.
8. Do NOT editorialize or add facts that weren't in the notes. If notes are ambiguous, flag it as a question in the "Follow-up" section.
9. If any attendee name resolved to two or more directory entries, end the summary with a `Verify before distributing` note that lists those names and the candidate matches.

**Output format:**

```
MEETING SUMMARY — [Meeting type]
Project: [Job name / address]
Date: [Meeting date]    Attendees: [Names / roles]
Prepared by: [From config]

1. DECISIONS
   - [Decision 1]
   - [Decision 2]

2. ACTION ITEMS
   | # | Action | Owner | Due | Notes |
   |---|--------|-------|-----|-------|
   | 1 | [Specific task] | [Name/role] | [Date] | [any dependency] |

3. OPEN QUESTIONS / RFIs
   - [Question] — [who should answer] — [needed by]

4. RISKS & SCHEDULE IMPACT
   - [Risk description] — [impact] — [mitigation or next step]

5. NEXT MEETING
   - Date: [if known]
   - Topics to carry over: [list]
```

**Meeting-type-specific additions:**

- **Pre-job walk** — Add an "Existing Conditions" section (panel brand/age, service rating, utility meter, visible deficiencies, photo references)
- **Inspection debrief** — Add a "Corrections Required" table: finding, NEC reference, location, assigned tech, re-inspection date
- **Submittal review** — Add a "Submittal Status" table: item, reviewer, status (approved / approved as noted / revise and resubmit / rejected), comments
- **Safety / post-incident** — Add "Hazards Discussed," "Corrective Actions," and "Recordkeeping" (OSHA 300 log triggered? Y/N)
- **Close-out / punch walk** — Add a "Punch List" table: item, location, responsible party, target completion date

**Tone:**
- Neutral and factual — no hedging language ("seems like," "kind of")
- If a commitment was made, state it as a commitment
- Use trade shorthand inside your company but expand it for mixed audiences (if the meeting included the homeowner, expand on first use)

**What NOT to do:**
- Do not invent action items that weren't discussed
- Do not assign action items to specific people without evidence from the notes (use "EC" or "GC" if the specific person isn't named)
- Do not omit safety-related discussions even if brief
- Do not include sensitive information (pricing negotiations, personnel issues) in summaries that will be distributed externally — flag these for internal-only versions

## Worked Example 1 — GC / Trade Coordination (with config-resolved attendees)

**Input:** Rough notes from a GC coordination meeting on the 1422 Maple St service upgrade job — Torres Electric, GC is Maple Ridge Builders. Raw notes: "switchgear lead time 6 wks, crane day 4/28, utility cut-over wk of 5/12, drywall wants us out of west wall by 4/25, inspector Chen did rough-in last week — 2 boxes too shallow in laundry (corrected 4/10), Mark to confirm temp power shutoff schedule with tenant, Sam will send revised submittal by Fri."

**`config.yml` attendee directory excerpt:**
```yaml
attendees:
  - name: Mike Torres
    role: Owner / Master Electrician
    company: Torres Electric
    initials: MT
  - name: Mark Davis
    role: Foreman
    company: Torres Electric
    initials: MD
  - name: Sarah Liu
    role: Project Manager
    company: Maple Ridge Builders (GC)
    initials: SL
  - name: Sam Brooks
    role: Plumbing Foreman
    company: Brooks Mechanical (PC)
    initials: SB
projects:
  1422-maple:
    EC: Mike Torres
    GC: Sarah Liu
    PC: Sam Brooks
    EC_foreman: Mark Davis
```

---

**MEETING SUMMARY — GC / Trade Coordination**
Project: 1422 Maple St — Service Upgrade
Date: 2026-04-13
Attendees: Mike Torres (Owner, Torres Electric); Mark Davis (Foreman, Torres Electric); Sarah Liu (PM, Maple Ridge Builders); Sam Brooks (PC Foreman, Brooks Mechanical)
Prepared by: Torres Electric — Dispatch

**1. DECISIONS**
- Crane day locked for **Monday, April 28** for switchgear set
- Utility cut-over targeted for **week of May 12**; Torres Electric to coordinate directly with utility

**2. ACTION ITEMS**

| # | Action | Owner | Due | Notes |
|---|--------|-------|-----|-------|
| 1 | Finish west-wall rough-in (pulls + terminations) | Mark Davis (Torres Electric) | 4/25 | Drywall needs wall closed — hard date |
| 2 | Submit revised panelboard submittal | Sam Brooks (Brooks Mechanical) | Fri 4/17 | Date assumed from "by Fri" — confirm |
| 3 | Confirm temp-power shutoff schedule with tenant | Mark Davis (Torres Electric) | 4/18 | Required before utility cut-over week |
| 4 | Track switchgear ETA weekly | Torres Electric — Dispatch | Weekly | 6-wk lead from order date |

**3. OPEN QUESTIONS / RFIs**
- Sam Brooks' revised panelboard submittal — confirm scope is the panelboards-only revision, not the full MEP coordination resubmittal (the notes only said "revised submittal" without scope)
- Is the crane permit pulled by Torres Electric or Maple Ridge Builders? Not addressed in the notes

**4. RISKS & SCHEDULE IMPACT**
- Switchgear 6-wk lead time is tight vs. 5/12 cut-over week — if order slips even 3 days, cut-over moves
- West-wall rough-in on 4/25 is a hard date; any inspection re-work could cascade

**5. INSPECTION DEBRIEF (last week)**
- Finding: (2) boxes set too shallow in laundry room — corrected 4/10, re-inspection passed (Inspector Chen — not in attendee directory; flagged below)

**6. NEXT MEETING**
- Not scheduled — recommend standing weekly coordination until crane day

---

**Notes / Verify before distributing:**
- "Inspector Chen" — not in `config.yml` attendee directory. Resolved as "Inspector Chen, AHJ" in the inspection-debrief item; please confirm full name (likely City of Portland BDS) before adding to a distribution list.
- Items 2 and 3 due-dates assumed from context ("by Fri," "before utility cut-over week"); verify with the responsible parties before distribution.
- Crane permit ownership not addressed in the notes; raised as an Open Question.

## Worked Example 2 — Inspection Debrief (no config attendee directory)

**Input:** Rough notes from a rough-in re-inspection debrief on a 14,500 SF commercial TI. EC is the company running the skill; GC is the customer; AHJ inspector did the inspection. Raw notes: "Re-inspect today, 2 of 5 prior items still flagged: (a) AFCI on common-area Ckts B-15, B-17 missing per 2023 §210.12 — apprentice missed during retrofit; (b) Working space at MDP — boxes / pallets in front of MDP at the time of inspection, partial fail. Cleared once removed during the visit. Rest passed. Inspector Tran said next-step is corrected-and-recall on the AFCI items only. Marisol from GC's side committed to keeping MDP clear going forward; she'll add it to the daily superintendent walk."

`config.yml` attendee directory: **not configured** for this company yet.

---

**MEETING SUMMARY — Inspection Debrief (Re-Inspection)**
Project: [Project name from intake]
Date: 2026-04-25
Attendees (as captured in notes): Inspector Tran (AHJ); Marisol (GC); EC foreman (name not captured)
Prepared by: [Company from config]

**1. DECISIONS**
- Re-inspection result: **Partial pass.** AFCI items remain open; working-space item cleared during the visit
- Next step: corrected-and-recall on the two open AFCI items only

**2. CORRECTIONS REQUIRED**

| Finding | NEC Reference | Location | Assigned | Re-inspect Date |
|---------|---------------|----------|----------|-----------------|
| Add AFCI protection on common-area branch circuits | 2023 §210.12 | Panel B, Ckts B-15 & B-17 | EC foreman (name not captured — verify) | TBD — call when complete |
| Maintain working space in front of MDP (clear) | 2023 §110.26 | Main Electrical Room | GC superintendent (Marisol — full name not captured) | Standing, daily walk |

**3. OPEN QUESTIONS / RFIs**
- Confirm AFCI breaker stock on hand (2 × 20 A 1P AFCI for the corrected-and-recall) before scheduling re-inspection
- Confirm Inspector Tran's preferred contact for re-inspection scheduling (city's online portal vs. direct call)

**4. RISKS & SCHEDULE IMPACT**
- AFCI corrected-and-recall has no calendar date yet; if delayed, push next milestone (final cover) right
- Working space at MDP must remain clear — if it slips during punch, final inspection is at risk

**5. NEXT MEETING**
- Not scheduled. Standing inspection-coordination contact via GC superintendent's daily walk

---

**Notes / Verify before distributing:**
- **Attendee directory not configured in `config.yml`.** Names rendered as captured in the raw notes ("Inspector Tran," "Marisol," "EC foreman"). Recommend populating `config.yml` with at least: the AHJ inspector list, the GC project team, and the EC's own foremen so the next summary resolves to full names + roles + companies automatically.
- Inspector Tran's full name not in the notes — please confirm before any distribution that goes outside the EC's office.
- "Marisol" is referenced once — confirm full name and last initial / surname before any external distribution; she's identified as GC superintendent.
- "EC foreman" is the catch-all for the missing-AFCI corrective action; please assign a named person before the corrective work starts.

---

*The skill prefers a populated `config.yml` attendee directory because the resolution makes the summary file-able, distributable, and unambiguous. Without one, the summary is still produced, but the verifier (you) must close the gap before sending.*
