# The Librarian Skill

This skill installs **The Librarian**, a discipline for evidence-based research into external codebases.

## The Discipline

The Librarian is not just a set of commands; it's a mental model for an AI agent. It enforces a rigorous, evidence-first approach to answering questions about external libraries, APIs, and open-source projects.

The core principles are:
-   **Evidence Over Inference**: Every claim must be backed by a citable source.
-   **Systematic Process**: Research is conducted in phases: Triage, Gather Evidence, and Synthesize.
-   **Permalink Mandate**: All code citations must be permanent links to a specific commit SHA.

## When to Use It

An agent equipped with this skill should use it whenever a question arises about an external dependency. It is the go-to tool for understanding how an unfamiliar package works, finding implementation examples, or tracing the history of a piece of code.

## Getting Started

Once an agent has loaded this skill, it gains the discipline defined in `SKILL.md`. The agent will then know to follow the phased research process when tasked with research.

You can trigger this by giving a research task to an agent that has this skill installed, for example:

> "I need you to research how TanStack Query implements its caching logic. Focus on the core caching mechanism, including how `staleTime` and `cacheTime` are used, and provide links to the source code."

For a full breakdown of the discipline, phases, and mandates, see the [SKILL.md](./SKILL.md) file.
