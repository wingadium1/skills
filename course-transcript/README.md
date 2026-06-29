# Course Transcript Skill

This skill processes a lecture transcript, turning it into structured and auto-linked Zettelkasten notes.

**This skill does NOT create new notes.** It only appends processed lecture content to an existing literature note.

## Core Workflow

1.  **Prerequisite**: You must first create a section note using the `start-course-section` skill.
    > "Use the `start-course-section` skill to start section 6: 'Tool Design & MCP' from course 'cca-masterclass'."

2.  **Process a Lecture**: Once the section note exists, use this skill to add a lecture transcript to it.
    > "add transcript for lecture 1 of course 'cca-masterclass': {paste transcript here}"

The skill will then automatically:
- Summarize and structure the transcript into key ideas.
- Auto-link concepts to your permanent notes.
- Append the result to the correct section note.

For a full breakdown, see the [SKILL.md](./SKILL.md) file.
