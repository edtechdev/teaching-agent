# Task: promote-session

## Purpose

Converts a **Session Skeleton** into a detailed **Session Material**.  
**The agent also adopts the professor persona and style from `docs/lecture-didactics.md` into its own persona, so all content is written in this voice.**

## Inputs

- number, type
- skeleton: file from `skeletons/`
- didactics: content from `docs/lecture-didactics.md`
- agenda: content from `docs/lecture-agenda.md`
- **Professor persona from `docs/lecture-didactics.md` (mandatory handoff)**
- **Style & difficulty level from `docs/lecture-didactics.md` (mandatory handoff)**

## Output

- `materials/{number}-{type}.md`
- Structure based on `templates/session-material.yaml`

## Steps

1. Load skeleton.
2. Adopt didactic concept and course type from Didactics.
3. **Agent adopts the professor persona & style from Didactics into its own persona.**

- From this step, the agent writes in the tone of the professor persona.
- All agenda descriptions reflect this style.

4. Insert agenda information.
5. Consider didactic inputs.
6. Generate planned outline.
7. Apply template.
8. Save the file.