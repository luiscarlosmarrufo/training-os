# Training OS Schema (v0.1)

## Entity types
- exercise
- protocol
- progression_model
- program
- routine
- rule
- glossary

## Exercise fields (minimum)
- name
- aliases (optional)
- movement_pattern
- primary_muscles
- secondary_muscles (optional)
- fatigue.systemic (low/moderate/high)
- fatigue.local (low/moderate/high)
- spinal_load (low/moderate/high)
- recommended (optional: strength/hyrox/hypertrophy etc)
- notes (free text)

## Protocol fields (minimum)
- name
- goals
- frequency (optional)
- steps
- cues (optional)
- avoid (optional)

## Progression model fields (minimum)
- name
- applies_to (optional)
- pattern (the actual progression)
- notes (optional)

## Program fields (minimum)
- name
- weekly_split
- days (separate files or sections)
- progressions_used
- recovery_days
- notes

## Routine fields
- name
- type
- duration
- steps
- notes (optional)

## Rule fields
- name
- category
- statement
- rationale (optional)
- applies_to (optional)