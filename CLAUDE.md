# Claude Instructions — Training OS

You maintain a structured training knowledge base (KB).
- /inbox = raw dumps (messy, never reformatted)
- /kb = canonical structured knowledge (single source of truth)
- /exports/notebooklm = bundled KB exports for NotebookLM upload
- /kb/schema = schema registry that may evolve

## Core workflow: "process inbox"
When the user says "process inbox", do the following:

1) Read all files in /inbox
2) Classify content into entity types:
   exercise | protocol | progression_model | program | routine | rule | glossary | other
3) Extract entities and update /kb:
   - Create new entities when needed
   - Update existing entities when same entity exists (including aliases)
   - Deduplicate aggressively (merge duplicates)
4) If program examples are provided:
   - Store the program spec under /kb/programs/<program_name>/
   - Extract: weekly split, day structure, recovery days, progression models used
   - Extract: exercises referenced and ensure they exist in /kb/exercises (create/update)
   - Extract: progression models and store them in /kb/progressions (create/update)
5) Write a run report to /logs/YYYY-MM-DD_run.md:
   - Created files
   - Updated files
   - Merges performed
   - Ambiguities (kept conservative)
   - Schema changes (if any)
6) Generate NotebookLM exports in /exports/notebooklm:
   01_exercises.md
   02_protocols.md
   03_progressions.md
   04_rules.md
   05_programs.md
7) Move processed inbox files to /inbox/processed

## Dedupe rules
- Use /kb/glossary/aliases.md to resolve naming differences.
- If a new name seems like an alias, add it to aliases.md and update the canonical entity.
- Never create two files for the same exercise/protocol.

## Schema evolution (IMPORTANT)
The schema may evolve when new kinds of info appear (e.g., nutrition, sleep metrics).
However, schema changes must be controlled:

A schema change is allowed ONLY if the new information cannot be stored cleanly
without ad-hoc notes.

When a schema change is needed:
1) Update /kb/schema/SCHEMA.md
2) Update /kb/schema/CONTROLLED_VOCAB.md if new controlled values are introduced
3) Append an entry in /kb/schema/CHANGELOG.md:
   - What field was added/changed
   - Why
   - What entities are affected
   - Backfill/default strategy
4) Apply the backfill across all impacted KB files to keep them consistent.

Prefer minimal schema changes. Do not add fields prematurely.

## Controlled vocab
movement_pattern: squat | hinge | push | pull | carry | rotation | locomotion | jump | sprint | core | mobility
fatigue.systemic: low | moderate | high
fatigue.local: low | moderate | high
spinal_load: low | moderate | high

## Output formatting
- Canonical KB files are Markdown with YAML frontmatter when appropriate.
- Keep exports readable.
- Never include raw inbox text in exports.

## Entity file structure examples

### Exercise file structure
```md
---
name: Back Squat
aliases: [squat, barbell squat]
movement_pattern: squat
primary_muscles: [quads, glutes]
secondary_muscles: [core, hamstrings]
fatigue:
  systemic: high
  local: high
spinal_load: high
recommended: [strength, hypertrophy]
---

# Back Squat

## Description
[exercise description]

## Key Cues
- [cue 1]
- [cue 2]

## Notes
[any additional notes]
```

### Protocol file structure
```md
---
name: Rest-Pause
goals: [strength, intensity]
frequency: optional
---

# Rest-Pause Protocol

## Steps
1. [step 1]
2. [step 2]

## Cues
- [cue]

## Avoid
- [common mistake]
```

## Processing principles
1. Be conservative with merging - only merge when certain
2. Preserve all meaningful information
3. Use structured frontmatter for searchable fields
4. Keep free-text notes section for context that doesn't fit schema
5. Cross-reference related entities when relevant