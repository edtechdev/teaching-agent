# Task: create-didactics

## Purpose

Creates the document **Lecture Didactics & Style**.  
Defines the didactic concept, professor persona, style, and course type of the lecture.  
Builds on the Lecture Outline to ensure a consistent teaching strategy.

## Inputs

- Abstract from `docs/lecture-outline.md`
- Target audience from `docs/lecture-outline.md`
- Learning objectives from `docs/lecture-outline.md`

## Output

- `docs/lecture-didactics.md` (Markdown file)
- Structure based on `templates/lecture-didactics.yaml`

## Steps

1. Read abstract, target audience, time commitment, and learning objectives from the outline.
2. Design a suitable didactic concept (teaching methods, learning phases).
3. Describe the professor persona (expertise, role, style).
4. Define style & difficulty level (humorous, scientific, practical, etc.).
5. Set the course type (introductory, advanced, practice-oriented, group work, self-learning).
6. Fill the `templates/lecture-didactics.yaml` template with the results.
7. Save the file as `docs/lecture-didactics.md`.