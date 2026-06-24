# The Terminology Linker Skill

This skill installs **The Terminology Linker**, a discipline for building a living knowledge graph from your course notes.

## The Discipline

The Terminology Linker is a process for an AI agent to follow. It guides the agent to:
1.  **Extract** key terms from your notes.
2.  **Define** those terms, using the `librarian` skill for research when needed.
3.  **Formalize** the terms into a structured glossary.
4.  **Link** terms within your documents using Obsidian-style wikilinks.
5.  **Connect** notes by updating their `## Connections` sections.

This process transforms a collection of disconnected notes into a valuable, navigable knowledge graph.

## Getting Started

An agent with this skill loaded can be instructed to build a glossary for a specific course:

> "Use the Terminology Linker to build a glossary for the 'Advanced TypeScript' course and connect the related concepts."

For a full breakdown of the discipline and its phases, see the [SKILL.md](./SKILL.md) file.
