# Course Note Researcher Skill

This skill installs the **Course Note Researcher**, a discipline for integrated learning that combines structured note-taking with rigorous, delegated research.

## The Discipline

The Course Note Researcher is a mental model for an AI agent to manage the entire lifecycle of learning from a course. It guides the agent to not only capture and refine information but to actively verify and enrich it by interfacing with a specialized research skill.

The core principles are:
-   **Capture, Refine, Connect**: A structured workflow for turning raw notes into a connected knowledge graph.
-   **Explicit Delegation**: All external research is explicitly and formally delegated to the `librarian` skill. This ensures research is conducted with rigor and that all findings are evidence-based.
-   **Integrated Knowledge**: The findings from the `librarian` are not just appended; they are integrated into the notes, enriching definitions and forming new connections.

## When to Use It

This is a high-level skill for managing a learning project. An agent equipped with this skill can handle the entire process from starting a course to building a comprehensive, well-researched knowledge base. It is the preferred skill when the user wants both note-taking and research to be handled in a structured, integrated manner.

## Getting Started

An agent with this skill loaded can be instructed to manage the learning process for a course:

> "Use the course-note-researcher skill to guide me through the 'Advanced TypeScript' course. I want to take notes and research the core concepts as we go."

The agent will then follow the phased workflow defined in `SKILL.md`, orchestrating the note-taking process and delegating to the `librarian` skill when research is needed.

For a full breakdown of the discipline and its phases, see the [SKILL.md](./SKILL.md) file.
