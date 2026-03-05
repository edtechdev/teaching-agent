# Task: create-image

## Purpose

Generates a detailed image prompt for course materials based on a user description, aligned with the visual style guide.
Creates professional, actionable prompts for AI image generators that maintain visual consistency with the course identity.

## Inputs

- User description: what should be visualized (provided as command parameter)
- Image style guidelines from `style-guide.md#image-prompt-style`
- Website color palette from `style-guide.md#website-colors`
- Course context from `outline.md#abstract` (for thematic alignment)

## Output

- A detailed image prompt (displayed as formatted text)
- Optionally saved to `assets/prompts/image-[description-slug].md`

## Steps

1. Receive user description of what should be visualized.
2. Read image style guidelines from `style-guide.md#image-prompt-style`.
3. Read color palette from `style-guide.md#website-colors`.
4. Read course theme from `outline.md#abstract` for context.
5. Analyze user description and extract:
   - Main subject/concept
   - Required elements or details
   - Intended use (diagram, illustration, header, etc.)
6. Combine user description with style guide parameters:
   - Visual style (photorealistic, illustrated, flat, etc.)
   - Color scheme (using palette from style guide)
   - Composition approach
   - Lighting and mood
   - Educational context
7. Generate a detailed, actionable prompt.
8. Include accessibility considerations (alt text suggestion).
9. Present the prompt in a clear format.
10. Optionally save to `assets/prompts/image-[slug].md`.

## Output Format

The image prompt should follow this structure:

```
Image Prompt: [Brief Title]
============================

Description: [User's original description]
Context: [Course theme alignment]
Intended Use: [Diagram/Illustration/Header/etc.]

Visual Parameters:
- Style: [from style guide]
- Color scheme: [specific colors from palette]
- Composition: [layout approach]
- Lighting: [lighting style]
- Mood: [atmosphere]

Complete Prompt:
"[Full detailed prompt ready for image generator]"

Accessibility:
Alt text suggestion: "[Descriptive alt text for the image]"

Technical Specifications:
- Aspect ratio: [16:9/4:3/1:1/custom]
- Format: PNG/JPG/SVG
- Usage: [Slide/Handout/Web/etc.]
```

## Special Features

- Suggests diagram alternatives (Mermaid, ASCII art) if appropriate
- Offers multiple prompt variations for different styles
- Can generate prompts for image series (maintaining consistency)
- Considers educational context and pedagogical goals

## Usage

This task is invoked when:
- Creating images for lecture materials (`/coauthor-materials`)
- Designing diagrams or illustrations
- Generating visual aids for specific concepts
- Creating consistent imagery across sessions
