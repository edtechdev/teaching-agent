# Task: create-didactics

## Purpose

Creates the document **Lecture Didactics & Style**.  
Defines the didactic concept, professor persona, style, and course type of the lecture.  
Builds on the Lecture Outline to ensure a consistent teaching strategy.

## Inputs

- Abstract from `docs/course-outline.md`
- Target audience from `docs/course-outline.md`
- Learning objectives from `docs/course-outline.md`
- Course type & conventions from `docs/course-context.md`

## Output

- `docs/course-didactics.md` (Markdown file)
- Structure based on `templates/course-didactics.yaml`

## Steps

1. Read `docs/course-context.md` for course type, persona type, and conventions.
2. Read abstract, target audience, time commitment, and learning objectives from the outline.
3. Design a suitable didactic concept (teaching methods, learning phases).
4. Describe the instructor persona (expertise, role, style) aligned with the persona type from course-context.
5. Define style & difficulty level (humorous, scientific, practical, etc.).
6. Set the course type (introductory, advanced, practice-oriented, group work, self-learning).
7. Fill the `templates/course-didactics.yaml` template with the results.
8. Save the file as `docs/course-didactics.md`.