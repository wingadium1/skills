# Course Transcript Skill

This skill provides a discipline for processing full course transcripts using the Zettelkasten method. It creates one literature note per course section, and appends the processed content of each lecture's transcript into that note.

## Core Workflow

1.  **Start a Section**: Use `start section {N}: {title} from course {course_slug}` to create a new note for the entire section.
2.  **Process a Lecture**: Use `add transcript for lecture {N}: {transcript_text}`. The skill will automatically:
    *   Summarize and structure the transcript into key ideas, prefixed with the lecture number (e.g., `L1:`).
    *   Scan the text for concepts that exist in your permanent notes directory.
    *   Automatically convert those concepts into `[[wikilinks]]`.
    *   Append the processed text to the section note.

## Getting Started

> "Use the course-transcript skill to start section 6: 'Tool Design & MCP' from course 'cca-masterclass'."
> "add transcript for lecture 1: {paste transcript here}"

For a full breakdown, see the [SKILL.md](./SKILL.md) file.
