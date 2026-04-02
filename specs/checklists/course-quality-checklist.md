# Checklist: Course Quality

> **Usage note:** Read `docs/context.md` first. Skip any check marked `[condition]` if the condition does not apply to this course type.

## Context

- [ ] `docs/context.md` exists
- [ ] Course type defined
- [ ] Terminology set (sessions-called, lectures-called)
- [ ] Language & tone conventions set
- [ ] Agenda flag correct (yes / no / optional)
- [ ] Person (Sie / Du / you) set

## Outline

- [ ] Title present
- [ ] Target audience clearly defined
- [ ] Time commitment specified `[lecture-series, workshop]`
- [ ] Time commitment present or estimated `[self-paced]`
- [ ] Abstract complete (topics, benefits, application)
- [ ] 3–5 learning objectives formulated, measurable (verb + context)
- [ ] Optional: Logo prompt

## Didactics

- [ ] Refers to outline
- [ ] Didactic concept clear
- [ ] Instructor persona defined (background, role, style)
- [ ] Style & difficulty level specified
- [ ] Course type consistent with `docs/context.md`

## Agenda `[if agenda flag = yes in docs/context.md]`

- [ ] All sessions have: title, duration, type, learning objective, summary
- [ ] Session learning objectives align with `docs/outline.md` learning objectives
- [ ] Materials file reference present per session

## Session Progress (docs/sessions.md)

- [ ] `docs/sessions.md` exists `[not single-lesson]`
- [ ] All expected sessions have a row
- [ ] No session marked ✅ Skeleton without a file in `skeletons/`
- [ ] No session marked ✅ Material without a file in `materials/`
- [ ] All sessions marked ✅ Fertig before publishing

## Session Skeletons

- [ ] Exist for all sessions
- [ ] All mandatory sections present (title, summary, content, activities, references)

## Session Materials

- [ ] All skeletons promoted to materials
- [ ] Outline with subchapters present
- [ ] References included per section where claims are made
- [ ] Didactic inputs from `docs/didactics.md` reflected (methods, learning phases)
- [ ] Learning objectives from `docs/agenda.md` addressed in content

## LiaScript Syntax (per material file)

- [ ] Exactly one `#` heading per file (course title)
- [ ] `###` and deeper headings only inside HTML blocks (`<div>`), lists, or blockquotes
- [ ] All code blocks properly closed (triple backticks)
- [ ] Animation counters (`--{{n}}--`, `{{n}}`) reset to 0 after each `##` heading
- [ ] Quiz syntax correct: `[(X)]` single choice, `[[X]]` multiple choice, `[[answer]]` text
- [ ] All media elements (`![]`, `?[]`, `!?[]`) have meaningful alt text
- [ ] No unclosed HTML blocks (`<div>` without `</div>`)
- [ ] Course header metadata present (author, version, language, narrator) `[lecture-series, self-paced, workshop]`

## Overall Consistency

- [ ] Terminology from `docs/context.md` used consistently throughout all docs
- [ ] Instructor persona tone consistent across all materials
- [ ] Learning objectives from `docs/outline.md` traceable into `docs/agenda.md` and materials
- [ ] Context ↔ Outline ↔ Didactics ↔ Agenda ↔ Sessions consistent
- [ ] Numbering correct, no gaps
- [ ] No sessions without materials