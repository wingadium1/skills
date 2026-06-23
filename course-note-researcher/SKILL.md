---
name: course-note-researcher
description: "A note-taking companion for online courses that explicitly uses the 'librarian' skill for research. Use this when you want a structured note-taking process combined with powerful, delegated research."
---

# Course Note Researcher

This skill provides a discipline for taking structured notes on online courses, following the Zettelkasten method. It integrates deeply with the `librarian` skill for all external research tasks.

## The Discipline

This skill is for capturing and structuring knowledge. It is not for conducting research. When a concept needs validation or deeper exploration, this skill's discipline is to **delegate that task to The Librarian**.

## Core Workflow

1.  **Capture**: Use `ADD NOTE` to capture raw thoughts during a lesson.
2.  **Refine**: Use `REPHRASE NOTE` to clarify and deepen your understanding of a captured thought through an interactive interview.
3.  **Synthesize & Research**: At the end of a lesson or section (`FINISH LESSON`, `FINISH SECTION`), identify core concepts. For each concept that requires external validation, **you must load the `librarian` skill** and task it with finding canonical definitions, best practices, and real-world examples.
4.  **Integrate**: Use the findings from the `librarian` to create or update your permanent notes, ensuring they are grounded in citable, external evidence.

## Actions

The actions (`START COURSE`, `ADD NOTE`, `REPHRASE NOTE`, etc.) are the same as the `course-note` skill. The key difference is in the research-related steps:

### FINISH LESSON (Researcher Variant)

In the "Mine & Research" step, the workflow is explicit:
-   **Identify high-signal concepts** using the extraction heuristics.
-   For each concept, **formulate a precise research prompt.**
-   **Invoke the `librarian` skill** using `task(subagent_type="librarian", ...)` with the prompt.
-   **Integrate the librarian's findings** (which must include citations) into your proposal for new permanent notes.

### RESEARCH CONNECTIONS (Researcher Variant)

This action is now a dedicated interface to the `librarian` skill.
-   **Trigger**: "research connections", "cross-link these notes"
-   **Action**:
    1.  Mine all notes for recurring principles.
    2.  For each principle, formulate a research task.
    3.  Delegate the research tasks to the `librarian` skill in a parallel batch.
    4.  Use the results to create and cross-link permanent notes.

By explicitly linking to the `librarian` skill, this `course-note-researcher` ensures that all research is performed with the rigor and evidence-based discipline of The Librarian.
