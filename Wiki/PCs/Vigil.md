---
type: entity
entity-type: pc
name: Vigil
status: active
hero_name: Vigil
real_name: John
player: Jean
playbook: The Protégé

# Mentor
mentor_primary: Cypher
mentor_primary_real_name: unknown
mentor_primary_embodies: Danger
mentor_primary_denies: Superior
mentor_secondary: Meridian
mentor_secondary_notes: >
  Cypher's former apprentice. Wants John to become something
  better than Cypher — pulls against the primary mentor's standard.

# Mentor resources (team)
mentor_resources:
  - hidden base (Cypher's)
  - supercomputer (Cypher's)
  - surveillance equipment (Cypher's)

# Labels (live — overwrite each session)
label_freak: 0
label_danger: 0
label_savior: +1
label_superior: +2
label_mundane: 0

# Conditions (live — overwrite each session; true = marked)
condition_afraid: false
condition_angry: false
condition_guilty: false
condition_hopeless: false
condition_insecure: false

# Advancement
potential_current: 0
advances_taken: 0

# Influence (live — overwrite each session)
# Cypher always has Influence over Vigil (Protégé rule)
# Cinder and Glitch hold Influence over Vigil per pre-campaign setup
influence_held_by:
  - Cypher
  - Meridian
  - Cinder
  - Glitch

# Status
pc_status: active
last_location: "[[Wiki/Locations/Cyphers-Base]]"
first_session: 1

source: player-created
confidence: established
created: 2026-04-29
updated: 2026-04-29
---

## Description

John is a young hero operating under the call sign Vigil — chosen because
he watches, waits, and strikes only when the moment is right. He moves with
the quiet precision of someone who has drilled every technique thousands of
times, and there is an intensity about him that can read as either focus or
suppressed anxiety depending on the day. Out of costume he looks like an
unremarkable teenager; in it, he carries himself like someone trying very
hard to be worthy of the name he's operating under.

## Background

John discovered Cypher through news coverage — the kind of hero who never
gets the big splashy headlines but whose work quietly holds the city
together. He became obsessed: clippings, case analyses, training regimens
reverse-engineered from grainy footage. When Cypher actually recruited him,
John could barely believe it. He still can't, some days.

He has two mentors now, which is one more than is comfortable. Cypher
embodies Danger — decisive, direct, willing to hit hard when hitting hard
is what the situation needs. Meridian, Cypher's former apprentice, pushes
back on all of that. He wants John to think, adapt, outmaneuver rather than
overpower. John hasn't figured out yet which of them is right, or whether
the question even has an answer. What he does know is that Cypher asked him
to keep an eye on Glitch, and he's not sure if that's a test, a task, or a
warning.

## Moves

**Been reading the files** — When John first encounters an important
superpowered phenomenon, roll +Superior. On a hit, tell the team one
important detail from his studies; GM says what seems different from what
he remembers. On a 10+, ask a follow-up question answered honestly. Miss:
the situation is well outside his knowledge base.

**Captain** — When the team enters battle together, add an extra Team to
the pool and carry +1 forward if John is leading.

**Heroic tradition** — When John gives advice he believes Cypher would give,
roll +Danger (the Label Cypher embodies) to comfort or support, instead of
+Mundane.

*Protégé team moves:*
- **Triumphant celebration:** Ask the teammate if John has been a good leader
  or effective teammate. Yes → mentor loses Influence, mark potential.
  No → mentor gains Influence, take +1 forward using the Label the mentor
  embodies.
- **Vulnerability/weakness:** Tell them a secret about his mentor (including
  his feelings toward them). Give them Influence; add 1 Team.

## Special Features

**Mentor — Cypher**
Cypher always holds Influence over Vigil (Protégé rule). John does not yet
know Cypher's real name. Cypher embodies Danger and denies Superior —
he values decisive action and is skeptical of John's analytical tendencies.

**Mentor — Meridian**
Cypher's former apprentice; holds Influence over Vigil at start of play.
Meridian's agenda is explicitly different from Cypher's: he wants John to
surpass the mentor, not replicate him. The tension between Cypher's standard
and Meridian's vision is a live pressure on John's identity.

**Mentor resources available to the team:**
- Cypher's hidden base — team headquarters
- Supercomputer — research, case files, communications hub
- Surveillance equipment — city monitoring, op planning

**The Label tension:**
Cypher embodies Danger (+0 at start) and denies Superior (+2 at start).
John's Superior is already his highest Label — exactly the quality his
primary mentor dismisses. Fireside chat rolls +Danger; Venting frustration
(if taken later) rolls +Superior. Both moves encode the mentor conflict
mechanically.

## Influence

Cypher holds Influence over John by rule (Protégé mechanic) — permanent
until advancement removes it. Meridian holds Influence at start of play
by virtue of his direct investment in John's development. Cinder and Glitch
also hold Influence over Vigil per pre-campaign setup state.

John begins with Business demeanor and gives Influence to no teammates at start.
John does not yet hold Influence over either teammate.

*[Update each session as Influence shifts.]*

## Relationships

- John and [[Wiki/PCs/Cinder]] teamed up before the group came together.
  That history is the closest thing to an easy relationship John has on
  this team.
- Cypher asked John to keep an eye on [[Wiki/PCs/Glitch]]. John hasn't
  decided how to feel about that yet — surveillance of a teammate sits
  uncomfortably against his own sense of loyalty.

*[Tag changes [Session N].]*

## History

*[Append significant events here; tag [Session N].]*

## Advancement Log

*[Append advances taken and potential milestones here; tag [Session N].]*

## Cross-References

- [[Wiki/NPCs/Cypher]] — primary mentor; always holds Influence
- [[Wiki/NPCs/Meridian]] — secondary mentor; holds Influence at start
- [[Wiki/PCs/Cinder]] — teammate; prior history
- [[Wiki/PCs/Glitch]] — teammate; under Cypher's watch order
- [[Wiki/Locations/Cyphers-Base]] — team headquarters
