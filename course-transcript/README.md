# Course Transcript Skill

This skill provides a discipline for processing full course transcripts using the Zettelkasten method. It is designed to turn long-form text into a structured, interconnected knowledge base in a single, automated step.

## The Discipline

This skill is for processing and structuring knowledge from transcripts. It is not for conducting research itself. When a concept needs validation or deeper exploration, this skill's discipline is to **delegate that task to a research skill like The Librarian**.

## Core Workflow

1.  **Start**: Use `start lecture {lecture_title} from course {course_slug}` to create a new note for the lecture.
2.  **Process**: Use `add transcript: {file_path_or_pasted_text}`. The skill will automatically:
    *   Summarize and structure the transcript into key ideas.
    *   Scan the text for concepts that exist in your permanent notes directory (`/Users/sonht2.gmo/git/athena/content/permanent/`).
    *   Automatically convert those concepts into `[[wikilinks]]`.
    *   Save the final, processed note.

## Getting Started

An agent with this skill loaded can be instructed to manage a course from its transcripts:

> "Use the course-transcript skill to start a new lecture called 'Tool Design Fundamentals' from course 'cca-masterclass'."

For a full breakdown of the discipline and its phases, see the [SKILL.md](./SKILL.md) file.
