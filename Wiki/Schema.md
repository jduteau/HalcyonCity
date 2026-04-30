---
wiki-version: 1.0
last-updated: 2026-04-29
maintained-by: llm-wiki
type: schema
---

# Wiki Schema
Masks: A New Generation campaign world knowledge base. Setting: Halcyon City.
Serves two consumers: the GM (browsing Obsidian) and the world arbiter
agent (querying during play). Write every page to serve both.

## Namespace Conventions

- Top-Level: Wiki/Locations, Wiki/NPCs, Wiki/PCs, Wiki/Factions, Wiki/Sessions, Wiki/Conspiracy, Wiki/World, Wiki/Lore, Wiki/State
- Page Naming: Title Case, hyphens for multi-word (`Wiki/NPCs/Person/NPCName`)
- Max Depth: 3 levels (e.g., `Wiki/NPCs/Person/NPCName`)
- Hub Pages: Every namespace level has a hub page listing its children
- Folder Hierarchy: Namespaces map to folders (e.g., `Wiki/Factions/AEGIS.md`)

### Namespace Definitions

| Namespace | Contains | Page Type |
|-----------|----------|-----------|
| `Wiki/Locations` | Places: neighborhoods, buildings, lairs, landmarks, the city at large | entity |
| `Wiki/NPCs` | Named characters the team has met or heard of | entity |
| `Wiki/PCs` | Player characters (the young heroes) | entity |
| `Wiki/Factions` | Organizations, agencies, gangs, villain groups, institutions | entity |
| `Wiki/Sessions` | Individual play session records | project |
| `Wiki/Conspiracy` | Synthesized analysis of villain scheme threads | knowledge |
| `Wiki/World` | Halcyon City canon: history, superhero context, public knowledge | knowledge |
| `Wiki/Lore` | In-world documents, news articles, rumors, legends | knowledge |
| `Wiki/State` | Current session state block (overwritten each session) | knowledge |


## Page Types and Required Properties

### When to Use Each Type

**entity** — Use for anything with identity and persistence in the
world: a named person, a named place, a named organization. If it
has a name and the team could encounter it, it is an entity.
Subtype is determined by the entity-type field.

**project** — Use for sessions only. A session is a discrete play
event with a date, a location, and outcomes. Not used for anything
else in this wiki.

**knowledge** — Use for synthesized analysis, world reference
material, in-world documents, and the current state block. If it
is a fact about the world rather than a named thing in the world,
it is knowledge. Subtype is determined by the domain field.

**feedback** — Not used in this wiki.

**hub** — One per namespace. Structural index page only.

### Entity

An entity is a named, persistent thing in the world: a person,
place, or organization. Every named NPC, location, faction, and
player character gets its own entity page.

**How to classify during ingest:**

- Named player character (controlled by a player, not the GM) → entity-type: pc
- Named person or character (GM-controlled) → entity-type: npc
- Named place (neighborhood, building, landmark, lair, region) → entity-type: location
- Named group, agency, gang, corporation, villain org → entity-type: faction

**Required YAML frontmatter (all entities):**

```yaml
type: entity
entity-type: pc | npc | location | faction
name: [canonical name]
status: active | inactive | destroyed | unknown
source: [emergent | world-canon | player-created]
confidence: canon | established | inferred | emergent
created: YYYY-MM-DD
updated: YYYY-MM-DD
```

---

#### entity-type: pc

Player characters — the young heroes. One page per character.
Covers both player-voiced and GM-voiced PCs; all are full members
of the team with complete pages. These pages are player-facing by
default; use `gm_only: true` only for content the character's
controller does not know (rare — see guidance below).

**Required YAML frontmatter (additional fields):**

```yaml
player: [player name or handle, or "GM" if GM-voiced]
playbook: [e.g. "The Bull", "The Nova", "The Doomed"]
hero_name: [cape name]
real_name: [civilian name — gm_only if secret even from other players]

# GM-voiced PCs (omit if player-voiced)
gm_controlled: true   # present only on GM-voiced PCs; omit entirely on player-voiced PCs

# Labels (live — overwrite each session)
label_freak: [integer, typically -2 to +3]
label_danger: [integer, typically -2 to +3]
label_savior: [integer, typically -2 to +3]
label_superior: [integer, typically -2 to +3]
label_mundane: [integer, typically -2 to +3]

# Conditions (live — overwrite each session; true = marked)
condition_afraid: true | false
condition_angry: true | false
condition_guilty: true | false
condition_hopeless: true | false
condition_insecure: true | false

# Advancement
potential_current: [integer 0-5]
advances_taken: [integer]

# Influence (live — overwrite each session)
# List names of PCs and NPCs who currently hold Influence over this PC
influence_held_by:
  - [Name or [[Wiki/NPCs/...]] link]

# Status
pc_status: active | retired | corrupted
last_location: "[[Wiki/Locations/...]]"
first_session: [integer]
```

**Notes on gm_controlled:**

- Apply `gm_controlled: true` to any PC whose moves and decisions
  are made by the GM rather than a dedicated player.
- GM-voiced PCs are still full PCs: they have Playbooks, Labels,
  Conditions, Influence, and advancement exactly like player-voiced PCs.
- The `gm_only` flag on a GM-voiced PC page is still reserved for
  content the character themselves does not know in-fiction (the same
  standard as player-voiced PCs). It does not mean "GM reference only"
  — the page is world-facing, not secret.
- Lint rules treat GM-voiced and player-voiced PCs identically.

**Required sections:**

- **Description** — appearance in costume and out, personality, notable traits
- **Background** — origin, what brought them to the team, what drives them
- **Moves** — all moves this character currently has, in freeform Markdown.
  Organize by source: Playbook Moves, Additional Moves (from advancement
  or other playbooks), Team Moves. For each move, note the move name and
  a brief description of what it does for this character. Update on
  advancement; tag [Session N].
- **Special Features** — playbook-specific mechanics that are not moves.
  Examples: Burn (The Nova) — current Burn value and what it is charged
  with; Moment of Truth (any) — text and whether it has been triggered;
  The Doom (The Doomed) — doom track and current status; The Bull's Conditions
  and drives; etc. Use freeform Markdown. Update each session as state
  changes; tag significant changes [Session N].
- **Influence** — narrative account of who holds Influence over this PC and why;
  who this PC holds Influence over and why. Update each session. The live
  structured data is in YAML; this section adds context and history.
- **Relationships** — how this PC relates to each other PC and to significant
  NPCs; tag changes [Session N]
- **History** — significant events in session order; tag [Session N]
- **Advancement Log** — advances taken, potential milestones, playbook
  end-of-session moves triggered; tag [Session N]
- **Cross-References**

---

#### entity-type: npc

Named characters controlled by the GM. Includes adult heroes,
adult villains, teen NPCs, civilian contacts, and threats with
named identity.

**Required YAML frontmatter (additional fields):**

```yaml
npc_status: active | dead | missing | imprisoned | unknown
npc_scope: adult-hero | adult-villain | teen-npc | civilian | threat
last_location: "[[Wiki/Locations/...]]"
allegiance_known: [what the team believes]
allegiance_true: [GM truth — may differ from above]
faction: "[[Wiki/Factions/...]]"   # omit if none
first_session: [integer]
gm_only: true   # add when allegiance_true differs from allegiance_known

# Influence (live — overwrite each session)
# List PCs this NPC currently holds Influence over
holds_influence_over:
  - [PC name or [[Wiki/PCs/...]] link]
```

**npc_scope definitions:**

- `adult-hero` — established superhero, mentor figure, or legacy hero.
  Mechanically represents authority and the weight of expectation on the
  young heroes. May offer aid, impose conditions, or shift Labels.
- `adult-villain` — active antagonist with power and agenda. Drives threats
  and schemes; typically the face of a Faction or a scheme thread.
- `teen-npc` — peer-age character: classmates, rival teams, teen sidekicks.
  Subject to the same emotional and social dynamics as the PCs.
- `civilian` — non-powered supporting character: family members, school staff,
  journalists, bystanders with recurring presence.
- `threat` — a named villain, monster, or dangerous entity without a full
  social presence; primarily a combat or encounter entity. May be elevated
  to adult-villain if they develop narrative weight.

**Required sections:**

- **Description** — appearance, manner, memorable traits; costume if applicable
- **Role** — function in Halcyon City and in the campaign
- **Party Relationship** — current standing with the team; update each change;
  tag changes [Session N]
- **Influence** — narrative context for any Influence this NPC holds over PCs,
  and any Influence PCs hold over this NPC; update each session
- **What the Team Knows** — information revealed in play; mirrors the player wiki
- **GM Truth** — actual allegiance, hidden motives, connections; mark gm_only
- **History** — significant events in session order; tag [Session N]
- **Cross-References**

---

#### entity-type: location

Named places in or around Halcyon City.

**Required YAML frontmatter (additional fields):**

```yaml
district: [Halcyon City district or neighborhood, e.g. "Emerald District"]
location_status: intact | damaged | destroyed | contested | unknown
layer: city | district | local
```

**layer definitions:**

- `city` — city-wide or iconic: Halcyon City itself, major landmarks
  (City Hall, Halcyon Memorial, the crater district).
- `district` — neighborhood or zone scale: the Emerald District, the Docks,
  the financial district, a school campus.
- `local` — specific building or site: the team's base, a villain's lair,
  a specific address.

**Required sections:**

- **Overview** — what this place is, why it matters to the campaign
- **Description** — physical appearance, atmosphere, access
- **Current Status** — what the team has done here or what has changed;
  update each visit
- **Known Inhabitants** — NPCs and factions present, with links
- **Team History** — chronological visits and events; tag [Session N]
- **GM Notes** — secrets, hooks, what could happen here; mark gm_only
- **Cross-References**

---

#### entity-type: faction

Named organizations with collective agenda and presence.

**Required YAML frontmatter (additional fields):**

```yaml
faction_type: hero-org | villain-org | government | corporate | civilian | gang | other
scale: local | city | national | global
known_to_team: true | partial | false
allegiance: hero-aligned | villain-aligned | neutral | unknown
```

**Required sections:**

- **Overview** — purpose, reach, history in Halcyon City
- **Leadership** — known leaders; distinguish team-known from GM-truth
- **Goals** — stated goals and true goals; true goals gm_only
- **Team Relationship** — current standing; tag changes [Session N]
- **Scheme Connections** — links to Conspiracy pages and other Factions
- **GM Notes** — future plans, hooks; mark gm_only
- **Cross-References**

---

### Project

Used for sessions only.

**Required YAML frontmatter:**

```yaml
type: project
status: completed | active | abandoned
session_number: [integer]
title: [short descriptive title]
date_played: YYYY-MM-DD
primary_location: "[[Wiki/Locations/...]]"
created: YYYY-MM-DD
updated: YYYY-MM-DD
```

**Required sections:**

- **Summary** — 2-3 sentences; what happened, what changed
- **Narrative** — full session blog post from session scribe;
  paste directly; immutable once filed; never edit
- **Key Events** — bullet list of significant moments;
  cross-reference all NPCs, Locations, Factions affected
- **Moves Triggered** — significant move outcomes (hits, misses, 7-9
  complications) that shaped events; reference PC and move by name
- **World State Changes** — what changed as a result of this session;
  feeds the update pass on other pages after ingest
- **Open Threads** — leads, questions, unresolved situations;
  link to Conspiracy or NPC pages where they live
- **Team Status at End** — location, conditions marked on each PC,
  Influence shifts, immediate situation
- **Cross-References**

Sessions are immutable once filed. Never overwrite session content.

---

### Knowledge

Used for conspiracy analysis, world canon, and in-world lore.

**How to classify during ingest:**

- Analysis of how factions/NPCs connect, villain scheme threads → domain: conspiracy
- Facts from Masks canon or established Halcyon City world-building
  (city history, superhero context, public institutions) → domain: world
- In-world documents, news articles, rumors, legends → domain: lore
- Current session state block → domain: state

**Required YAML frontmatter (all knowledge pages):**

```yaml
type: knowledge
domain: conspiracy | world | lore | state
confidence: canon | established | inferred | emergent
created: YYYY-MM-DD
updated: YYYY-MM-DD
```

---

#### domain: conspiracy

Synthesized analysis of a villain scheme, ongoing threat thread,
or escalating situation. One page per distinct thread.

**Required YAML frontmatter (additional fields):**

```yaml
scheme_tier: emerging | active | escalating | resolved
party_awareness: unaware | partial | significant | full
```

**Required sections:**

- **The Scheme** — who is behind it, what they want, what they are doing
- **Known Evidence** — facts the team has discovered; session refs
- **Hidden Evidence** — GM-only facts not yet discovered; gm_only
- **Key Actors** — NPCs and Factions with links
- **Connections** — links to other Conspiracy pages
- **Status** — what the team has done to this thread; tag [Session N]
- **Cross-References**

---

#### domain: world

Reference material about Halcyon City, its history, and the
broader superhero context.

**Required YAML frontmatter (additional fields):**

```yaml
world_category: city-history | superhero-context | institution | geography | culture | public-record
canon_source: [e.g. "Masks Core Rulebook" | "emergent" | "campaign-established"]
```

**Required sections:**

- **Overview** — what this world entry covers
- **Relevance to Campaign** — how it connects to active content;
  grows as the campaign develops
- **Cross-References**

---

#### domain: lore

In-world documents, news coverage, rumors, and other textual
artifacts discovered or referenced during play.

**Required YAML frontmatter (additional fields):**

```yaml
lore_type: news-article | rumor | document | legend | social-media | broadcast
reliability: reliable | unreliable | planted | unknown
discovered_session: [integer]
source_location: "[[Wiki/Locations/...]]"   # omit if not location-specific
```

**Required sections:**

- **Content** — the actual text or account, written in in-world voice
- **GM Analysis** — what is true, false, misleading; gm_only
- **Connections** — what this lore points toward
- **Cross-References**

---

#### domain: state

The current session state block. Lives at Wiki/State/Current-Session.md.
This page is OVERWRITTEN (not appended) after each session.
It is the only page in the wiki that is not append-only.

```yaml
domain: state
confidence: established
```

**Content format:**

```
Team:
- [[Wiki/PCs/HeroName]] (Playbook) — Conditions: [list marked conditions or "none"] | Potential: [n]/5
- [[Wiki/PCs/HeroName]] (Playbook) — Conditions: [list marked conditions or "none"] | Potential: [n]/5

Labels (snapshot):
- [[Wiki/PCs/HeroName]]: Freak [n] | Danger [n] | Savior [n] | Superior [n] | Mundane [n]

Active Influence:
- [NPC/PC Name] holds Influence over [[Wiki/PCs/HeroName]]

Location: [[Wiki/Locations/...]]

Situation: [one sentence]

Active Threads:
- [[Wiki/Conspiracy/...]]
- [[Wiki/Conspiracy/...]]

Last Session: [[Wiki/Sessions/Session-NN-Title]]
```

This page feeds the world arbiter at session start. Keep it minimal.

---

### Hub

Standard hub page. One per namespace. No changes from base schema.

```yaml
type: hub
namespace: Wiki/NamespaceName
```

---

## Confidence Levels

| Level | Meaning |
|-------|---------|
| `canon` | Directly stated in the Masks rulebook or published Halcyon City material. Do not contradict without explicit campaign decision. |
| `established` | Confirmed in play, recorded in a Session page. Authoritative for this campaign. |
| `inferred` | Logical conclusion from canon or established facts. Flag when asserting. |
| `emergent` | Created by GM improvisation during play. Treat as established once filed and verified. |

Campaign facts supersede world canon where they conflict.

---

## Visibility Flags

```yaml
gm_only: true       # GM-facing content only. Never exposed in player wiki.
                    # Apply to: allegiance_true, GM Truth, GM Notes,
                    # Hidden Evidence, unrevealed scheme tiers,
                    # real_name fields marked secret between players.
                    # On PC pages: use only for content the player
                    # themselves does not know (e.g., active compulsion,
                    # secret identity exposed without their knowledge).

player_known: true  # Explicitly revealed to characters in play.
                    # Appears in both GM wiki and player wiki.
```

Default (no flag): GM wiki only, not yet in player wiki.

PC pages are player-facing by default; `gm_only` is the exception,
not the rule.

---

## Ingest Source Routing

Identify the source type and apply these rules before extracting.

**Character sheet / PC creation:**

- Create a `Wiki/PCs/` entity page with `entity-type: pc`
- Set `confidence: established`
- Set `source: player-created`
- Do NOT apply `gm_only` to Labels, Conditions, Potential, moves,
  or history — these are player-facing by default
- Exception: apply `gm_only: true` to `real_name` if it is secret
  between players, or to any section containing GM knowledge the
  player does not have
- Set all five Labels to starting values from character creation
- Set all Conditions to false
- Set `potential_current: 0`
- Update the `Wiki/PCs` hub to include the new page
- Update Wiki/State/Current-Session.md to include the new PC

**Session transcript:**

- Create a session (project) page
- Update `party_relationship` on all NPCs encountered
- Update `location_status` on all locations visited
- Overwrite the five Label fields on all active PC pages to current values
- Overwrite all five Condition fields on all active PC pages to end-of-session state
- Overwrite `potential_current` on all active PC pages
- Update `holds_influence_over` on NPC pages where Influence changed
- Update `influence_held_by` on PC pages where Influence changed
- Append to **Influence** narrative section on affected PC and NPC pages; tag [Session N]
- Append to **History** section on PC pages for significant events; tag [Session N]
- Append to **Advancement Log** on PC pages for any advance taken; tag [Session N]
- Update `last_location` on all PC pages to match end-of-session location
- Tag all new content [Session N]
- Overwrite Wiki/State/Current-Session.md with updated state block
- File World State Changes as update candidates for affected pages

**Linking thread documents:**

- Create or update conspiracy (knowledge) pages
- Set `confidence: established`
- Cross-reference all NPCs and Factions involved

**World canon sources (Masks Core Rulebook, published supplements):**

- Create world (knowledge) pages only
- Set `confidence: canon`
- Create NPC entity pages for named heroes and significant figures;
  set `npc_scope: adult-hero`, `adult-villain`, or `civilian` as appropriate
- Do NOT overwrite established campaign facts with world canon
- Campaign layer supersedes world layer where they conflict

**Named villain or threat (improvised or escalating):**

- Named villain with social presence and agenda → NPC entity page
  with `npc_scope: adult-villain` or `teen-npc`
- Named villain group or organization → Faction entity page
- Generic unnamed threat type (robots, drones, mind-controlled civilians)
  belongs in the rules wiki or a session note, not this world wiki

---

## Append and Contradiction Rules

- APPEND ONLY — never delete or overwrite existing content
- Exceptions: Wiki/State/Current-Session.md is overwritten each session;
  live YAML fields on PC pages (Labels, Conditions, Potential, Influence)
  are overwritten each session — these are current-state fields, not history
- Tag all session additions to narrative sections: [Session N]
- If new information contradicts existing content, flag explicitly:

```
> ⚠️ CONTRADICTION [Session N]: [description of conflict]
```

Do not silently resolve contradictions. Surface them for human review.

---

## Cross-Reference Rules

- Every wiki page MUST have at least one `[[Wiki/...]]` link to another wiki page
- Hub pages MUST list ALL child pages in their namespace
- When a page mentions an entity that has its own page, use `[[Wiki/Entity/Name]]` link syntax
- Tags: use `#tag` for lightweight categorization (e.g., `#villain`, `#halcyon`, `#critical`)
- External links: `[Text](URL)` for URLs outside the wiki

---

## Content Format Rules

- Flat Markdown (no outliner `- ` prefix required on every line)
- Properties: YAML frontmatter between `---` fences at the top of the file
- Sections: standard `## Heading` syntax
- Code blocks: fenced with triple backticks
- NEVER store credentials, passwords, or API tokens in wiki pages

---

## L1/L2 Architecture

- The standard llm-wiki L1 memory path is disabled for this wiki.
- No credentials or sensitive data exist in campaign knowledge.
- All content including session state lives in the git-tracked vault.

### L1 equivalent = Wiki/State/Current-Session.md

- Loaded explicitly at session start by the world arbiter.
- Contains: team status (Conditions, Potential, Labels snapshot, Influence
  snapshot, playbook, links to PC pages), current location, active threads,
  pointer to last session page.

### L2 = Everything else in the wiki

- Queried on demand. Locations, NPCs, PCs, Factions, Sessions,
  Conspiracy, World, Lore.

### Boundary Rules

- If the arbiter needs it at the start of every session to orient itself → it belongs in the state page.
- Everything else → L2, queried when needed.

---

## Ingest Workflow

1. Identify source type (character sheet | session | linking thread | world canon)
2. Apply source routing rules above
3. Extract: named entities, facts, relationships, Influence changes, Label shifts, decisions
4. Classify each item by namespace and page type using definitions above
5. Scan wiki: identify existing pages to update, new pages to create
6. Target: 5-15 page touches per ingest
7. Create new pages with all required properties
8. Existing pages: APPEND only to narrative sections — never overwrite
   (exceptions: Wiki/State/Current-Session.md and live YAML fields on PC pages)
9. Update hub pages to include new child pages
10. Add cross-references between all affected pages
11. Set `updated` on all changed pages
12. Git commit after ingest

---

## Lint Rules

**Standard rules:**

- **Orphan Detection** — pages with 0 incoming links (hubs excluded)
- **Stale Detection** — `updated` > 180 days AND confidence is canon or established
- **Missing Properties** — pages missing type-specific required properties
- **Broken References** — links to non-existent pages
- **Hub Completeness** — hub pages missing children in their namespace
- **Credential Leak** — scan for token/password/secret patterns
- **Empty Pages** — only properties, no content
- **Cross-Ref Minimum** — fewer than 1 outgoing link

**Campaign-specific rules:**

- NPC page missing `allegiance_true` field → flag
- NPC page with `npc_scope: teen-npc` or `civilian` and no session history
  after 5 sessions of play → flag (adult-hero and adult-villain scope exempt)
- NPC page with `holds_influence_over` entries where the referenced PC page
  does not list that NPC in `influence_held_by` → flag (Influence mismatch)
- PC page with `influence_held_by` entries where the referenced NPC/PC page
  does not list a corresponding `holds_influence_over` entry → flag (Influence mismatch)
- Location page missing `location_status` → flag
- Session page missing World State Changes section → flag
- Session page missing Moves Triggered section → flag
- Conspiracy page with fewer than 2 linked NPC or Faction pages → flag
- NPC marked `npc_status: dead` or `imprisoned` appearing in sessions
  after their status-change session → flag
- PC page missing any of the five Label fields → flag
- PC page missing any of the five Condition fields → flag
- PC page missing **Special Features** section → flag
- PC page with `pc_status: active` not updated within last 3 sessions → flag
  (Label, Condition, and Special Features state may be stale)
- PC page with `gm_controlled: true` and `player` field not set to "GM" → flag (inconsistency)
- State block listing a PC name with no corresponding `Wiki/PCs/` page → flag
- Contradiction flags (⚠️) present → surface for human review
- Pages with `confidence: inferred` not updated in 10+ sessions → flag as potentially stale
