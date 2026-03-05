# Task: create-outline

## Purpose

Creates the **Lecture Outline** as a starting point for a lecture.
Defines title, target audience, abstract, learning objectives, and optionally a logo.

## Inputs

- Title of the lecture
- Target audience (e.g., students, professionals, beginners)
- Time commitment (e.g., semester hours per week, total hours)
- Abstract (topics, content, benefits)
- Learning objectives (3–5 concrete goals)
- Logo (optional, as a prompt)

## Output

- `docs/course-outline.md` (Markdown file)
- Structure based on `templates/course-outline.yaml`

## Steps

1. Read `docs/course-context.md` to determine course type and conventions.
2. Collect title, target audience, time commitment, and abstract.
3. Define 3–5 concrete learning objectives.
4. Optionally add a logo prompt.
5. Fill the `templates/course-outline.yaml` with the inputs.
6. Save the file as `docs/course-outline.md`.
