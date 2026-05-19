# Stage 3 — Zettelkasten Links + INDEX

## Goal

Make the relationships between atomic skills explicit, forming a navigable network rather than a collection of isolated files.

## Three Types of Relationships

1. **Depends-on**: The use of A requires understanding B first
   - Example: "Checklist Decision" depends on "Multi-Mental Models" (because checklist items come from models)

2. **Contrasts-with**: A and B are two alternative options to choose from based on context
   - Example: "Forward Reasoning" contrasts with "Reverse Thinking"

3. **Composes-with**: A and B are often used together
   - Example: "Circle of Competence" composes with "Margin of Safety"

## Execution Steps

1. List all skills produced in Stage 2
2. Scan pairs to identify if any of the above three types of relationships exist
3. Fill in the `related_skills` field in the frontmatter of each skill:
   ```yaml
   related_skills:
     - slug: multi-mental-models
       relation: depends-on
     - slug: forward-reasoning
       relation: contrasts-with
   ```
4. Append a "Related Skills" section to the end of each skill's SKILL.md, explaining the relationships in natural language
5. Generate `books/<slug>/INDEX.md` (using template `templates/INDEX.md.template`)

## INDEX.md Must Include

- Basic book information (author/year/one-sentence premise)
- List of all skills, grouped by theme
- Reference diagram (mermaid flowchart or graph)
- Recommended learning sequence (derived from dependency relationships)

## Moderation Principle

**Don't force relationships**. If there's no actual dependency/contrast/composition relationship between two skills, don't add related_skills. It's better to be sparse than to create false links.

A rule of thumb: For a book broken down into 10 skills, a reasonable number of relationships is about 8-15. Below 5 suggests the skills are too isolated (possibly wrong unit selection), above 25 suggests forcing relationships.