---
name: course-transcript
description: Processes a lecture transcript, distills it into key ideas, and appends it to an existing section literature note with auto-generated [[wikilinks]].
---

# Discipline: Transcript Processing and Linking

This skill has one responsibility: to process a raw lecture transcript and append it as structured, auto-linked notes into a pre-existing section literature file.

**Core Principle**: This skill is for processing, not initialization. It assumes the `start-course-section` skill has already been used to create the target literature note.

## Workflow

**Goal**: Distill a raw transcript for a single lecture and append it as structured, auto-linked key ideas to the active section note.

**Workflow**:
1.  **Trigger**: `add transcript for lecture {N} of course {course_slug}: {file_path_or_pasted_text}`.
2.  **Context Check**: The skill will find the most recently modified literature note in `content/literature/` that matches the `{course_slug}`. This will be the target note. If no note is found, it will fail with an error instructing the user to run the `start-course-section` skill first.
3.  **Step 1: Delegate Summarization & Structuring**:
    *   A specialist sub-agent is invoked with the raw transcript.
    *   **Sub-agent Prompt**: "You are an academic note-taking assistant. Read the transcript for Lecture {N}. Distill it into a structured list of key ideas. **Prefix every line of your output with `L{N}: `.** Be concise, accurate, and capture the core concepts, arguments, and examples."
    *   **Input**: Raw transcript text.
    *   **Output**: A clean block of text where every line starts with `L{N}:`.
4.  **Step 2: Auto-linking**:
    *   The skill retrieves the list of existing permanent notes from `/Users/sonht2.gmo/git/athena/content/permanent/`.
    *   It creates a vocabulary of concept slugs (e.g., "tool-error-design").
    *   The structured `L{N}:` text from the sub-agent is scanned. For each concept slug, the corresponding phrase (e.g., "tool error design") is replaced with a wikilink (`[[tool-error-design]]`).
5.  **Step 3: Append to Section Note**:
    *   The skill reads the target section literature note identified in the Context Check.
    *   The processed, auto-linked `L{N}:` block is appended to the `## Key Ideas` section.
    *   The file is saved.

**Completion Criterion**: The section literature note MUST be updated with the new, processed, and auto-linked key ideas for the specified lecture.

## Reference

### Wikilinking

-   **Source**: Files in `/Users/sonht2.gmo/git/athena/content/permanent/`.
-   **Mechanism**: The skill identifies phrases matching permanent note filenames and wraps them in `[[filename-without-md]]`.
