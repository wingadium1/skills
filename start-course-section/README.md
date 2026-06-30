# Start Course Section Skill

This is a simple, focused skill that creates the literature note for a new section in a course.

## Core Workflow

The skill is triggered with a single command and creates a new markdown file in `content/literature/` for the specified section, pre-populated with the necessary headers.

This skill is intended to be used as a prerequisite for other skills like `course-transcript`.

## Concrete Example

To start the sixth section of the "Certified Claude Architect Masterclass", you would run the following command:

> `start section 6: "Tool Design & MCP" from course "certified-claude-architect-masterclass-2026"`

This action creates the file `content/literature/certified-claude-architect-masterclass-2026-section-6-tool-design-mcp.md` and makes it ready to receive lecture transcripts via the `course-transcript` skill.
