---
name: "Safety Toolbox Talk"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~10 min/talk"
version: 2.2
last_eval_score: 9.20
---

# 🦺 Safety Toolbox Talk

## Purpose

Generate a job-specific safety toolbox talk for an electrical crew based on the day's tasks, site conditions, and hazards — formatted for a 5-10 minute field briefing that meets OSHA documentation requirements.

## When to Use

Use this skill when you need to:
- Run a morning safety briefing before starting work
- Address a specific hazard (arc flash, confined space, trenching, energized work)
- Prepare for a task with elevated risk (panel changeout, overhead line work, wet locations)
- Fulfill jobsite safety meeting documentation requirements
- Refresh the crew on seasonal hazards (heat stress, cold weather, lightning)

## Required Input

Provide the following:

1. **Today's tasks** — What the crew is doing today (e.g., "200A service changeout, underground conduit trench from meter to panel, reconnect at the weatherhead")
2. **Site conditions** — Indoor/outdoor, weather, wet/dry, occupied building, confined spaces, heights, proximity to other trades
3. **Crew info** (optional) — Number of workers, any apprentices, any new hires unfamiliar with the site
4. **Specific concerns** (optional) — Anything you want emphasized (e.g., "homeowner has small children" or "asbestos abatement in progress on floor above")

## Instructions

You are an AI assistant helping electrical contractors run effective, job-specific safety briefings. Your job is to generate a focused toolbox talk that addresses the ACTUAL hazards for today's work — not a generic safety lecture.

**Before you start:**
- Load `config.yml` from the repo root for company name and preferences
- **Run the Hazard-Prioritization Pre-load Block** (see below). This pulls the firm's crew roster, primary service-area / climate defaults, and the current month from `config.yml.safety_toolbox_talk` so the talk is tuned to today's specific crew, location, and season — not a generic OSHA recap.
- Reference `knowledge-base/regulations/` for applicable safety standards
- **NFPA 70E citation cycle note:** NFPA 70E 2024 is currently in force. NFPA 70E 2027 is on the cycle, with the NFPA Technical Meeting scheduled for late June 2026 and publication expected fall 2026. Until 70E 2027 publishes, cite 70E 2024 article and section numbers (e.g., §130.4, §130.7, §120.5). After 70E 2027 publishes, a v2.3 citation refresh will swap in any moved or renumbered sections — particularly the expanded sub-50V "or where an electrical hazard exists" hazard-recognition language. Do not pre-cite 70E 2027 until it ships.

### Hazard-Prioritization Pre-load Block (config-driven personalization)

Most contractors run their crews in a stable service area with a known mix of skills, license levels, and seasonal exposure patterns. The Pre-load Block pulls these three dimensions from config so the talk lands on the right hazards for *this* crew *this* season — not a one-size-fits-all OSHA reminder.

**Config block** (`config.yml.safety_toolbox_talk`):

```yaml
safety_toolbox_talk:
  # Crew roster — names + license levels. Lets the skill address the apprentice
  # by name and reserve line-side / >50V tasks for qualified persons per
  # NFPA 70E §110.2(A) "qualified person" definition.
  crew_roster:
    - name: "Mike Torres"
      license: "Master / EC-00842"
      qualified_for: ["line-side energized cut-in", "MV up to 600 V", "voltage-rated glove work"]
    - name: "Devon Mills"
      license: "Journeyman / JE-04488"
      qualified_for: ["MV up to 600 V", "voltage-rated glove work"]
    - name: "Ari Vasquez"
      license: "Apprentice / 2nd year"
      qualified_for: ["de-energized work only", "ladder work", "ground rod", "trench-side observation"]
      apprentice_supervision_required: true

  # Primary service area — climate / weather / utility characteristics that
  # affect hazard mix. Used to default heat / cold / lightning / wildfire /
  # ice-storm guidance when the user doesn't override.
  primary_service_area:
    region: "Pacific Northwest"
    typical_weather:
      january: "cold, wet, 28–48 °F, periodic ice"
      april:   "mild, wet, 45–65 °F"
      july:    "warm, dry, 65–95 °F, smoke season possible"
      october: "cool, wet, 45–60 °F"
    seasonal_hazards:
      november_to_march: ["ice on ladders", "cold-weather glove dexterity", "early dark"]
      june_to_september: ["heat stress", "wildfire smoke / AQI > 150", "afternoon thunderstorm"]
    common_utilities: ["Portland General Electric (PGE)", "Pacific Power"]
    utility_emergency_lines:
      - "PGE outage: 503-228-6322"
      - "Pacific Power outage: 1-877-508-5088"
    nearest_hospital_template: "Adventist Health Portland, 10123 SE Market St (default — verify per site)"

  # Hazard topics the firm has decided to surface even when not explicitly
  # named by today's tasks (e.g., the firm had a near-miss last quarter and
  # wants the topic refreshed for a full year).
  recurring_focus_topics:
    - topic: "Live-dead-live verification on line-side cut-ins"
      reason: "Near-miss 2026-02 — apprentice almost touched an unverified conductor"
      refresh_until: "2027-02"
    - topic: "Spade-only zone around marked gas locates"
      reason: "Industry-wide pattern; firm has had 2 nicks in 3 years"
      refresh_until: "indefinite"

  # NFPA 70E version anchor.
  nfpa_70e_edition: "2024"   # bump to 2027 after publication (fall 2026)
```

**How the Pre-load Block is used:**

1. **Address the crew by name and license level.** When generating the SIGN-OFF block, populate the attendee lines with the named crew from `crew_roster`. The Discussion Questions block should direct the apprentice-level question to the named apprentice ("Ari — walk us through…"). Reserve line-side / energized / >50V questions for crew members in `qualified_for`.
2. **Default the climate / weather to the current month against `primary_service_area.typical_weather`.** If today is in October, default to "cool, wet, 45–60 °F" unless the user supplied a different reading. If today is in July, surface heat-stress and wildfire-smoke hazards even if the user didn't name them.
3. **Surface every applicable `seasonal_hazards` entry for the current month range as a candidate hazard.** Do not invent seasonal hazards outside the configured ranges.
4. **Always include every `recurring_focus_topics` entry whose `refresh_until` is in the future** as one of the HAZARDS & CONTROLS rows, regardless of today's primary task — these are the topics the firm has committed to keep refreshed. Mark them with a `(recurring focus topic)` tag in the talk so the crew knows why it's on every talk.
5. **Use `utility_emergency_lines` and `nearest_hospital_template` to default the EMERGENCY PROCEDURES block** — the user can override per site, but the default lands the firm's standard numbers without re-asking.
6. **NFPA 70E citation cycle.** Cite the edition from `nfpa_70e_edition`. When that field is `"2024"`, use §130.4 / §130.7 / §120.5 / Table 130.7(C)(15)(a) etc. as cited in the existing worked examples. When it bumps to `"2027"`, the v2.3 refresh will swap in any renumbered sections.
7. **Never** invent crew members, license levels, or seasonal-hazard entries not in config. If the config block is absent, behave as the skill did in v2.1 (generic crew lines, user-supplied weather, no recurring topics).

**Process:**

1. Analyze the day's tasks and identify the primary electrical and general hazards:
   - **Electrical hazards:** shock, arc flash/blast, stored energy (capacitors), overhead lines, backfeed potential, improper lockout/tagout
   - **General hazards:** falls (ladders, lifts, roofs), trenching/excavation, heat/cold stress, silica dust, asbestos, confined spaces, struck-by (overhead work), caught-between
2. For each identified hazard, specify:
   - The specific risk (what could go wrong)
   - The control measure (what we're doing to prevent it)
   - Required PPE for this task
3. Reference applicable safety standards:
   - **OSHA 29 CFR 1926 Subpart K** — Electrical (construction)
   - **NFPA 70E** — Standard for Electrical Safety in the Workplace (arc flash, energized work permits, PPE categories)
   - **OSHA 1926 Subpart P** — Excavation (if trenching)
   - **OSHA 1926 Subpart M** — Fall protection (if working at heights)
4. Include the NFPA 70E approach boundaries if energized work is involved (Limited, Restricted, Arc Flash Boundary)
5. Reinforce the right to stop work — any crew member can halt work if they see an unsafe condition

**Output format:**

```
═══════════════════════════════════════════
SAFETY TOOLBOX TALK
[Company Name]
Date: [Today's date]    Job Site: [If provided]
═══════════════════════════════════════════

TOPIC: [Primary safety focus based on today's tasks]

TODAY'S TASKS:
- [Task 1]
- [Task 2]

HAZARDS & CONTROLS:

1. [Hazard Name]
   Risk: [What could happen]
   Control: [How we prevent it]
   PPE: [Specific PPE required]
   Standard: [OSHA/NFPA reference]

2. [Hazard Name]
   Risk: [What could happen]
   Control: [How we prevent it]
   PPE: [Specific PPE required]
   Standard: [OSHA/NFPA reference]

[Repeat for each identified hazard]

EMERGENCY PROCEDURES:
- Nearest hospital / urgent care: [Ask crew or note "verify before starting"]
- Fire extinguisher location: [Note or "verify on site"]
- Emergency contact: [From config or "verify"]

DISCUSSION QUESTIONS:
1. [Question to engage crew — e.g., "Has anyone worked with energized 200A equipment before? What's your approach?"]
2. [Question about site-specific conditions]

REMEMBER: Every crew member has the RIGHT and RESPONSIBILITY to stop work if you see an unsafe condition. No task is so urgent that we can't take time to do it safely.

SIGN-OFF:
Presented by: _______________  Date: ___________
Attendees:
☐ _______________ ☐ _______________
☐ _______________ ☐ _______________
☐ _______________ ☐ _______________
```

**Tone:**
- Direct and practical — not preachy or corporate
- Speak to experienced tradespeople, not trainees (unless apprentices are flagged in crew info)
- Focus on the specific job, not general platitudes
- Use real-world examples and "what-if" scenarios relevant to the task

**What NOT to do:**
- Do not produce a generic "be safe out there" talk — every talk must be task-specific
- Do not skip NFPA 70E references when energized work is involved
- Do not downplay hazards — if a task involves serious risk, say so directly
- Do not include company pricing or sales language — this is a safety document

## Worked Example 1 — 200 A Service Changeout (Energized Cut-In)

**Input:**
- Today's tasks: 200 A residential service changeout. Pull existing 100 A meter base + interior panel; install Siemens MC2040B1200ESC outdoor meter-main; new 200 A indoor sub-panel; ground rod refresh; energized cut-in at the line side after utility cuts the drop and clears the work.
- Site conditions: detached single-family home, side yard meter, dry, 62°F, sunny. Customer home with two small kids and a dog. PGE on-site 9:30 AM for cut.
- Crew info: 1 master (lead), 1 journeyman, 1 second-year apprentice.
- Specific concerns: apprentice's first energized cut-in; homeowner kids in and out of yard.

**Output:**

```
═══════════════════════════════════════════════
SAFETY TOOLBOX TALK
Torres Electric                  License # EC-00842
Date: 2026-04-25                 Job Site: 1422 Maple St, Portland OR
═══════════════════════════════════════════════

TOPIC: 200 A service changeout — energized cut-in, line-side termination

TODAY'S TASKS:
- Pull existing 100 A meter base and interior panel
- Set new outdoor meter-main (Siemens MC2040B1200ESC, 200 A)
- Install new indoor 200 A sub-panel; transfer 24 circuits
- Refresh ground rod system (2 rods, 6 ft apart per §250.53)
- Coordinate with PGE on drop cut (~9:30 AM) and reconnect (~end of day)

HAZARDS & CONTROLS:

1. Line-side energized work at the meter base
   Risk: Arc flash / blast on a 25 kA available fault current panel — burn, eye injury, hearing loss, blast injury.
   Control: Confirm PGE has cut the drop at the pole and has signed our LOTO sheet BEFORE any tool touches the meter base. After PGE cut, verify dead with our Fluke 1587 at line side, neutral, and ground. Live-dead-live test. If we cannot verify dead, we stop and call the dispatcher. NO line-side work without confirmed dead state.
   PPE: Cat 2 arc-rated kit (shirt, pants, balaclava, face shield, voltage-rated gloves with leather protectors, hard hat, safety glasses under face shield). Apprentice does not perform line-side termination this run; observation only at safe distance.
   Standard: NFPA 70E §130.4–130.7 (energized work, approach boundaries). OSHA 1926 Subpart K §1926.416 (LOTO for electrical work).

2. Stored / induced energy on conductors after cut
   Risk: Lateral capacitive coupling on long service drops can hold a few hundred volts after the utility cut; backfeed from a homeowner's generator we don't know about.
   Control: Live-dead-live test on every conductor before touch. Ask the homeowner directly if any portable generator is connected to the house — and inspect the receptacle on the dryer plug for any signs of a backfeed cord. Bond all phase conductors to ground after dead verification.
   PPE: Voltage-rated gloves stay on through the dead verification step.
   Standard: NFPA 70E §120.5 (verification of de-energized state).

3. Arc-flash boundary inside the existing meter base
   Risk: Limited approach boundary inside the meter base puts uninsulated conductors close to hands. Available fault current at this service ~10 kA per PGE; estimated incident energy 4–6 cal/cm² at 18 in working distance.
   Control: One person inside the boundary at a time. Apprentice and journeyman observe from outside the AFB. Insulated tools only. No metal watch / ring / chain on the working person.
   PPE: Cat 2.
   Standard: NFPA 70E Table 130.7(C)(15)(a) (residential single-phase service, ≤240V, ≤10 kA available fault current — Cat 2 typical).

4. Mast wrenching / overhead conductor swing
   Risk: When we pull the old service entrance cable down, the unsupported mast can swing into the eave. Falling conductor end could touch person below.
   Control: Lead climbs the ladder to the weatherhead, supports the mast with a strap. Two-person ladder hold. Conductor end controlled with a tag line. No personnel directly below the mast during pull.
   PPE: Hard hat (mandatory for everyone in the side yard for the entire mast pull).
   Standard: OSHA 1926 Subpart M (fall protection) — 6 ft trigger; ladder is below 6 ft today, but hard hat overhead-strike rule applies regardless.

5. Ground-rod driving — eye / ear protection
   Risk: Rotary hammer chips off the rod during driving, pinch points at the chuck, hearing damage at sustained 100+ dBA.
   Control: Eye and hearing protection on for everyone within 15 ft. Drive rods one at a time, never with anyone holding the rod with hands inside the swing radius — use a rod-holder clamp.
   PPE: Class A/B safety glasses with side shields, foam ear plugs (at minimum 25 dB NRR).
   Standard: OSHA 1910.95 (occupational noise exposure), 1926.102 (eye and face protection).

6. Customer / kid intrusion in the work area
   Risk: Two small kids and a dog in the home. Any of them could step into the side yard during the energized cut-in window.
   Control: Brief the homeowner at start of day — yard is closed from 9:00 AM until we say so, kids and dog inside or in the front yard only. Caution tape across the side-yard entrance from the front and back. One crew member assigned to monitor the perimeter during line-side work.
   PPE: N/A — administrative control.
   Standard: General OSHA duty clause.

EMERGENCY PROCEDURES:
- Nearest hospital: Adventist Health Portland, 10123 SE Market St — 11 minutes via I-205 N
- Eyewash station: bottle in the truck (2 each)
- Fire extinguisher: 5-lb ABC in the truck, second one staged near meter base for the cut
- Emergency contact: dispatcher Mike Torres (555) 123-4567; PGE outage hotline 503-228-6322
- 911 first if anyone is hurt; PGE second; dispatcher third

DISCUSSION QUESTIONS:
1. Apprentice — walk us through the live-dead-live test sequence on the line-side conductors. What's the single most common mistake new techs make on this step?
2. Journeyman — what's our backout plan if the existing meter base is corroded and the line-side lugs won't break loose? Do we cancel the cut-in or push through?
3. Lead — how do we verify the homeowner's panel doesn't have a backfeed circuit (generator inlet) before we open the existing panel cover?

REMEMBER: Every crew member has the RIGHT and RESPONSIBILITY to stop work
if you see an unsafe condition. No task is so urgent that we can't take
time to do it safely. Apprentice — you are EXPECTED to call out anything
that doesn't look right today. That's how you learn and that's how we
finish the day with everyone going home.

SIGN-OFF:
Presented by: ______________________   Date: 2026-04-25
Attendees:
☐ ____________________  (Lead — Master)
☐ ____________________  (Journeyman)
☐ ____________________  (Apprentice — 2nd year)
```

## Worked Example 2 — Overhead Service Drop Work, Bucket Truck, Adjacent to Energized Distribution

**Input:**
- Today's tasks: replace damaged service drop from utility pole to weatherhead at a small commercial strip mall. Bucket truck staged in alley. The drop spans ~40 ft from a utility-owned pole carrying 7,200 V primary on top crossarm, 120/240 V secondary on lower position, communications drops below.
- Site conditions: outdoor, 28°F, light wind 8 mph, partly cloudy, dry pavement. Adjacent businesses occupied. Alley shared with delivery trucks.
- Crew info: 1 master (bucket operator), 1 journeyman ground.
- Specific concerns: minimum approach distance to the 7,200 V primary that we are NOT working on; communications drops we have to thread through.

**Output:**

```
═══════════════════════════════════════════════
SAFETY TOOLBOX TALK
[Company from config]            License # [from config]
Date: 2026-04-25                 Job Site: Cedar Plaza, 4480 NE Cedar Ave
═══════════════════════════════════════════════

TOPIC: Overhead service-drop replacement adjacent to energized 7,200 V distribution

TODAY'S TASKS:
- Stage bucket truck in alley; level and outrigger fully extended
- Coordinate with utility for any required clearance / hold-off
- Disconnect existing 120/240 V drop at weatherhead and at pole
- Pull new 1/0 AWG triplex aerial, terminate at weatherhead and pole
- Re-energize after utility verifies tap is hot

HAZARDS & CONTROLS:

1. Energized 7,200 V primary on the upper crossarm — DO NOT WORK ON
   Risk: Even brief incidental contact with the primary at this voltage is fatal. The primary is on the top crossarm directly above our work zone.
   Control: This crew is qualified for ≤ 600 V only. We are NOT working on the primary. Minimum Approach Distance per NFPA 70E §130.4 / OSHA 1910.269 Table for 7.2 kV is 2 ft 1 in. Our actual working clearance is 4 ft 2 in vertical from secondary tap to primary — confirmed with the utility's pole drawing. Any condition that would put any part of the bucket, conductor, tool, or person within 2 ft 1 in of the primary stops the job. We call the utility for a clearance / hold-off.
   PPE: Class 2 voltage-rated gloves (rated 17,000 V, working voltage 7,500 V) with leather protectors, even though we are not contacting the primary. Glove inspection record on file (last test 2026-01-12).
   Standard: OSHA 1910.269 Table R-6 (MAD); NFPA 70E §130.4. Anti-overhead-line rule under OSHA 1926.1408 for cranes/booms also applies to bucket booms.

2. Communications drops below the secondary
   Risk: Cable, fiber, and phone drops thread through the work zone. They are ungrounded and susceptible to lift / drop / tangle.
   Control: Survey the comms drops before going up. Tag and tie back any drops in the swing path. Do not cut a comms drop without the owner's authorization — call out to the comms provider via the alley pole tag if a drop is in conflict.
   PPE: N/A.
   Standard: OSHA 1926 Subpart V (power-line and comms construction).

3. Bucket truck setup, outriggers, and traffic
   Risk: Truck pinch / tip-over if outriggers aren't fully deployed; struck-by hazard from delivery trucks in the alley.
   Control: Outriggers fully extended, pads on solid pavement. Cones at least 25 ft on either side of the truck. One person in the alley with a high-vis vest spotting traffic during boom motion. Boom not extended until outriggers are confirmed locked.
   PPE: Class 2 high-vis vest for the ground person; hard hat for both.
   Standard: ANSI A92.2 (insulated aerial devices); OSHA 1926 Subpart M.

4. Falling conductor / tool / hardware
   Risk: A dropped wrench, a pulled-down service drop end, a loose insulator can fall 25 ft into the alley.
   Control: All tools on lanyards in the bucket. Drop zone below the bucket cleared and coned. Old service drop end is controlled with a tag line and lowered, not dropped.
   PPE: Hard hat for ground person mandatory entire time bucket is up.
   Standard: OSHA 1926.95 (PPE general); 1926.502 (falling-object protection).

5. Cold-weather grip / dexterity
   Risk: 28°F with light wind reduces hand dexterity. Voltage-rated gloves over a thin liner can mask a damaged outer.
   Control: Inspect outer gloves at the start of the shift and again before going aloft. Hand-warmer packets in the bucket so the master can warm up between maneuvers. No bare-hand work — full glove discipline.
   PPE: Glove liners under the voltage-rated outers; chemical hand-warmers.
   Standard: OSHA 1910.137 (rubber-insulating equipment); manufacturer testing date on the gloves.

EMERGENCY PROCEDURES:
- Nearest hospital: [from config or "verify before starting"]
- 911 first for any contact event — even a "near miss" with the primary needs medical evaluation (delayed cardiac effects)
- Utility outage / emergency line: [utility name + 24-hr number from config]
- Spotter on the ground keeps a charged radio / phone — bucket operator is hands-busy with conductor

DISCUSSION QUESTIONS:
1. What is the minimum approach distance to the 7,200 V primary, and how do we measure it from inside the bucket when we're handling 30 ft of triplex?
2. If a wind gust pushes the conductor toward the primary, what's our recovery?
3. Walk through the lower-and-control method for the old drop end — what's wrong with just dropping it?

REMEMBER: STOP-WORK AUTHORITY — bucket operator AND ground person can
stop the job at any time. No primary contact today. We finish the day
with everyone going home.

SIGN-OFF:
Presented by: ______________________   Date: 2026-04-25
Attendees:
☐ ____________________  (Master — bucket operator)
☐ ____________________  (Journeyman — ground spotter)
```

## Worked Example 3 — Trenching for Underground Conduit, 36" Deep Across Driveway

**Input:**
- Today's tasks: dig 90-ft trench for 1-1/4" Sch 40 PVC carrying 4/0 AL service-entrance triplex, from new transformer pad to house meter pedestal. Trench crosses an asphalt driveway and runs 30 ft through landscaped lawn.
- Site conditions: outdoor, 65°F, dry, soil is type B (sandy clay loam per geotech). 811 marked yesterday — gas main 8 ft west of trench line, telecom 4 ft east, no other utilities flagged.
- Crew info: 1 journeyman (operating mini-excavator), 1 first-year apprentice (groundman), 1 helper.
- Specific concerns: gas main proximity, asphalt cut, hand-dig zone within 24" of marked utilities per state law.

**Output:**

```
═══════════════════════════════════════════════
SAFETY TOOLBOX TALK
[Company from config]            License # [from config]
Date: 2026-04-25                 Job Site: 8421 SW Sycamore Ln
═══════════════════════════════════════════════

TOPIC: Excavation / trenching for underground service feed — utility-strike avoidance

TODAY'S TASKS:
- Open 90-ft × 18" wide × 36" deep trench from transformer pad to meter pedestal
- Saw-cut and remove asphalt section (15 ft) over the driveway
- Lay 1-1/4" Sch 40 PVC, install pull tape, mark with red "CAUTION ELECTRIC LINE BURIED BELOW" tape 12" above
- Backfill to grade with sand bedding 6" below and above conduit, then native fill, compact in lifts

HAZARDS & CONTROLS:

1. Underground utility strike — gas main 8 ft west of trench
   Risk: Strike of the gas main causes leak, fire, explosion, and may de-pressurize the neighborhood. Strike of telecom is property damage and a service outage.
   Control: 811 locates verified yesterday — paint marks reviewed AT START of the day before any digging. Mark the trench centerline with marking paint 18" inside the gas-line tolerance zone. Use the mini-excavator only in the open lawn between marked utilities. Hand-dig with a spade-only zone (no metal tools that can puncture the gas line) within 24" of the marked gas locate per state law.
   Control (continued): Stop and call 811 emergency line if any unmarked utility is found, or if a marked utility appears at a different location than the locate.
   PPE: Eye protection for the spade work, leather gloves for hand-digging.
   Standard: OSHA 1926 Subpart P (excavation); state one-call law.

2. Cave-in — 36" depth, type B soil
   Risk: Trench wall collapse traps the apprentice working in the trench. A cave-in event in a 36" trench can be fatal — soil weight in a 4-ft section is ~1,500 lb.
   Control: 36" is below the OSHA 5-ft trigger for protective systems, BUT we still slope the trench walls per the "competent-person" judgment. Type B soil → 1:1 slope per OSHA Appendix B. Wider at the top, narrower at the bottom. No vertical walls. Spoil pile minimum 2 ft from edge. No equipment running within 2 ft of the edge.
   Control (continued): Apprentice working in the trench has clear egress at any point — entry/exit point at each end. No working in the trench alone. Helper stays topside as the safety watch.
   PPE: Hard hat for the apprentice in the trench at all times.
   Standard: OSHA 1926.652 (protective systems); §1926.651(c) (ingress/egress).

3. Asphalt saw-cut — silica dust, kickback
   Risk: Inhalable crystalline silica from the saw cut on asphalt-over-concrete; saw kickback if blade binds.
   Control: Wet-cut with the saw water feed — no dry cutting on this asphalt. PEL for silica dust is 50 µg/m³ over 8 hr; wet-cutting holds us under that. Eye and hearing protection mandatory within 25 ft of the saw. Two hands on the saw at all times; no one between the saw and the operator.
   PPE: N95 respirator while saw is running (even with wet cut, brief spikes occur), Class A safety glasses, foam ear plugs.
   Standard: OSHA 1926.1153 Table 1 (respirable crystalline silica — saw / drill / chip).

4. Equipment operation — mini-excavator swing zone, slewing radius
   Risk: Apprentice or helper struck by the swinging counterweight or bucket.
   Control: Operator (journeyman) calls out swing direction before each swing. Swing-zone clear before motion. Apprentice never approaches the machine from the rear. ONLY the operator's high-vis is visible in the cab mirrors before motion.
   PPE: Class 2 high-vis vests for everyone in the work area.
   Standard: OSHA 1926.1424 (operating crane/derrick zone — applied to mini-excavator counterweight).

5. Sun / heat exposure — full-day outdoor work
   Risk: 65°F today, but the apprentice has been with us 3 weeks and we don't yet know his hydration baseline. Asphalt work radiates heat.
   Control: Water cooler on the truck, mandatory hydration breaks every hour. Cooler with shade canopy if direct sun.
   PPE: Long-sleeve UV-rated work shirt.
   Standard: OSHA general duty clause (no specific federal heat standard yet).

6. Backfill — settling & repaving
   Risk: Conduit damaged by un-bedded backfill; future settlement of the asphalt patch.
   Control: 6" sand bedding below and above the conduit. Native fill in 8" lifts, compacted with a plate compactor (no random dumping). Burial-tape "CAUTION ELECTRIC LINE BURIED BELOW" 12" above conduit is required by §300.5(D)(3) and the AHJ.
   PPE: Steel-toed boots, eye protection during compaction.
   Standard: NEC §300.5; manufacturer's PVC bedding spec.

EMERGENCY PROCEDURES:
- Nearest hospital: [from config or "verify before starting"]
- Gas leak: 911 first, then NW Natural emergency 800-882-3377 (or [utility number from config])
- 811 emergency: 811 (or "1-800-DIG-RITE" for on-site re-mark)
- Trench rescue: 911 — DO NOT enter a collapsed trench to rescue. Wait for trained trench-rescue.

DISCUSSION QUESTIONS:
1. Apprentice — what's the spade-only zone around a marked gas line, and what does that mean for how you dig?
2. What do we do if you hit something in the trench that wasn't on the locate?
3. How wide is the safe-distance-from-edge for spoil and for equipment?

REMEMBER: STRIKE A GAS MAIN AND PEOPLE DIE. We hand-dig the spade-only
zone, we keep equipment off the locate, and if anything looks wrong we
stop and call. Apprentice — you are our eyes in the trench; tell us
immediately if you see anything we didn't mark.

SIGN-OFF:
Presented by: ______________________   Date: 2026-04-25
Attendees:
☐ ____________________  (Journeyman — operator)
☐ ____________________  (Apprentice — 1st year)
☐ ____________________  (Helper — groundman)
```

---

**Skill outputs are training and documentation aids. The competent person on site is responsible for the final hazard assessment and the conduct of the work.**
