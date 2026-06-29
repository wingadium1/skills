---
name: course-transcript
description: An active learning companion for online courses that processes full transcripts. It summarizes lectures, extracts key ideas, and automatically creates [[wikilinks]] to a permanent knowledge base. Use when a user wants to process a course lecture transcript ('start lecture', 'add transcript').
---

# Discipline: Transcript-based Zettelkasten

This skill automates the processing of lecture transcripts into structured, interconnected notes using the Zettelkasten method. It turns raw text into a refined literature note with automated conceptual linking.

**Core Principle**: A transcript is raw material. The goal is a structured note where key concepts are automatically linked to your permanent knowledge graph.

## Workflow

### 1. Start a Lecture

**Goal**: Prepare the environment to process a new lecture transcript.

**Workflow**:
1.  **Trigger**: Initiate with "start lecture {lecture_title} from course {course_slug}".
2.  **Context Check**: The skill will verify that the course's Work-in-Progress note (`content/wip/{course-slug}.md`) exists. If not, it will guide you to create one first.
3.  **Action**: A new, empty literature note is created at `content/literature/{course-slug}-{lecture-slug}.md`. The system will confirm: "Ready for transcript for lecture: {lecture_title}".

**Completion Criterion**: An empty literature note for the specified lecture MUST exist.

### 2. Process a Transcript

**Goal**: Transform a raw transcript into a structured, auto-linked literature note in a single step.

**Workflow**:
1.  **Trigger**: Use "add transcript: {file_path_or_pasted_text}" while a lecture is active.
2.  **Step 1: Delegate Summarization & Structuring**:
    *   A specialist sub-agent is invoked with the raw transcript.
    *   **Sub-agent Prompt**: "You are an academic note-taking assistant. Read the following transcript. Your task is to distill it into a structured list of key ideas. The format should be a series of bullet points, capturing the core concepts, arguments, and examples. Mimic the structure of the provided sample note. Be concise and accurate."
    *   **Input**: Raw transcript text.
    *   **Output**: A clean, structured block of text representing the "Key Ideas".
3.  **Step 2: Auto-linking**:
    *   The skill retrieves the full list of existing permanent notes from the `/Users/sonht2.gmo/git/athena/content/permanent/` directory.
    *   It processes this list into a vocabulary of concept slugs (e.g., "tool-error-design").
    *   The structured "Key Ideas" text is scanned. For each concept slug, the corresponding phrase (e.g., "tool error design") is replaced with a wikilink (`[[tool-error-design]]`). This search is case-insensitive, and spaces are treated as hyphens.
4.  **Step 3: Assemble & Write Note**:
    *   The skill assembles the complete literature note.
    *   It includes the standard YAML frontmatter (title, type, source, author, tags).
    *   It populates the note with a `## Summary` (left "To be synthesized"), the auto-linked `## Key Ideas`, and empty `## Quotes`, `## My Take`, and `## Links` sections.
    *   The final, processed note is written to the literature note file created in Step 1, overwriting the empty file.

**Completion Criterion**: The literature note (`content/literature/{...}.md`) MUST be populated with the structured, auto-linked content.

## Reference

### Naming

| Note        | Pattern                               |
|-------------|---------------------------------------|
| Wip tracker | `{course-slug}.md`                    |
| Literature  | `{course-slug}-{lecture-slug}.md`     |
| Permanent   | `{concept-slug}.md`                   |

### Literature Note Sections

The processed note will have the following structure:
1.  `## Summary`
2.  `## Key Ideas` (Contains the structured, auto-linked text)
3.  `## Quotes`
4.  `## My Take`
5.  `## Links`

### Wikilinking

-   **Source**: The list of files in `/Users/sonht2.gmo/git/athena/content/permanent/`.
-   **Mechanism**: The skill identifies phrases in the note that match the filenames of permanent notes and wraps them in `[[filename-without-md]]`.
