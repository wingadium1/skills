---
name: course-transcript
description: An active learning companion that processes lecture transcripts into a single, auto-linked literature note per course section. Use with 'start section' and 'add transcript for lecture'.
---

# Discipline: Transcript-based Zettelkasten (Section-level)

This skill automates processing lecture transcripts into a structured, interconnected literature note for each section of a course.

**Core Principle**: One section, one note. Each lecture's processed transcript is appended to the section note, with key concepts automatically linked to your permanent knowledge graph.

## Workflow

### 1. Start a Section

**Goal**: Prepare a literature note to hold all lectures for a new course section.

**Workflow**:
1.  **Trigger**: `start section {N}: {section_title} from course {course_slug}`.
2.  **Context Check**: Verifies the course WIP note (`content/wip/{course-slug}.md`) exists.
3.  **Action**: Creates a new, empty literature note at `content/literature/{course-slug}-section-{N}-{section-slug}.md`. The note is pre-populated with frontmatter and empty `## Summary`, `## Key Ideas`, `## Quotes`, `## My Take`, and `## Links` sections.
4.  **Confirmation**: "Ready for transcripts for Section {N}: {section_title}".

**Completion Criterion**: An empty literature note for the section MUST exist.

### 2. Process a Lecture Transcript

**Goal**: Distill a raw transcript for a single lecture and append it as structured, auto-linked key ideas to the active section note.

**Workflow**:
1.  **Trigger**: `add transcript for lecture {N}: {file_path_or_pasted_text}`.
2.  **Step 1: Delegate Summarization & Structuring**:
    *   A specialist sub-agent is invoked with the raw transcript.
    *   **Sub-agent Prompt**: "You are an academic note-taking assistant. Read the transcript for Lecture {N}. Distill it into a structured list of key ideas. **Prefix every line of your output with `L{N}: `.** Be concise, accurate, and capture the core concepts, arguments, and examples."
    *   **Input**: Raw transcript text.
    *   **Output**: A clean block of text where every line starts with `L{N}:`.
3.  **Step 2: Auto-linking**:
    *   The skill retrieves the list of existing permanent notes from `/Users/sonht2.gmo/git/athena/content/permanent/`.
    *   It creates a vocabulary of concept slugs (e.g., "tool-error-design").
    *   The structured `L{N}:` text from the sub-agent is scanned. For each concept slug, the corresponding phrase (e.g., "tool error design") is replaced with a wikilink (`[[tool-error-design]]`).
4.  **Step 3: Append to Section Note**:
    *   The skill reads the target section literature note.
    *   The processed, auto-linked `L{N}:` block is appended to the `## Key Ideas` section.
    *   The file is saved.

**Completion Criterion**: The section literature note MUST be updated with the new, processed, and auto-linked key ideas for the specified lecture.

## Reference

### Naming

| Note        | Pattern                                     |
|-------------|---------------------------------------------|
| Wip tracker | `{course-slug}.md`                          |
| Literature  | `{course-slug}-section-{N}-{section-slug}.md` |
| Permanent   | `{concept-slug}.md`                         |

### Wikilinking

-   **Source**: Files in `/Users/sonht2.gmo/git/athena/content/permanent/`.
-   **Mechanism**: The skill identifies phrases matching permanent note filenames and wraps them in `[[filename-without-md]]`.
