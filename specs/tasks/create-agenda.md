# Task: create-agenda

## Purpose

Creates the **Lecture Agenda** as a structured schedule for the lecture.  
Defines sessions/modules with title, duration, type (lecture/exercise), learning objectives, summary, and the corresponding materials files.
**The agent also adopts the professor persona and style from `docs/lecture-didactics.md` into its own persona, so all content is written in this voice.**

## Inputs

- Learning objectives from `docs/lecture-outline.md#Learning-Objectives`
- Abstract from `docs/lecture-outline.md#Abstract`
- Time commitment from `docs/lecture-outline.md#Time-Commitment`
- Didactic concept from `docs/lecture-didactics.md#Didactic-Concept`
- **Professor persona from `docs/lecture-didactics.md#Professor-Persona` (mandatory handoff)**
- **Style & difficulty level from `docs/lecture-didactics.md` (mandatory handoff)**
- Course type from `docs/lecture-didactics.md`

## Output

- `docs/lecture-agenda.md` (Markdown file)
- Structure based on `templates/lecture-agenda.yaml`

## Steps

1. Read learning objectives from the outline.
2. Adopt didactic concept and course type from Didactics.
3. **Agent adopts the professor persona & style from Didactics into its own persona.**

- From this step, the agent writes in the tone of the professor persona.
- All agenda descriptions reflect this style.

4. Define sessions/modules.
5. Build the agenda in a structured form.
6. Fill the `templates/lecture-agenda.yaml` template with the results.
7. Save the file as `docs/lecture-agenda.md`.
