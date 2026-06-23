# Course Note Researcher Skill

The `course-note-researcher` skill is an all-in-one active companion for taking structured notes during online courses. It combines the Zettelkasten note-taking method with built-in research capabilities.

## Purpose

This skill helps you:
- Track your progress through a course.
- Capture raw notes during lessons.
- Organize notes into literature and permanent notes.
- Refine your understanding of concepts through an interactive interview process.
- **Perform external research** on concepts directly within the skill to validate, enrich, and cross-link your notes.

This version of the skill is self-contained. It handles both note-taking and the research required to build a robust personal knowledge base from your course material.

## Usage

To use this skill, you typically start a course and then add notes as you go. The skill will automatically perform research when it detects concepts that need further exploration.

```
/course-note-researcher start course "Advanced TypeScript" on "Frontend Masters" by "Matt Pocock"
```
```
/course-note-researcher add note "Generics are like variables for types."
```
```
/course-note-researcher finish lesson
```

For a complete list of commands and workflows, see the [SKILL.md](./SKILL.md) file.
