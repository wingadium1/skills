# Course Note Skill

The `course-note` skill is an active companion for taking structured notes during online courses, following the Zettelkasten method. It is designed to be a focused note-taking tool.

## Purpose

This skill helps you:
- Track your progress through a course.
- Capture raw notes during lessons.
- Organize notes into literature and permanent notes.
- Refine your understanding of concepts through an interactive interview process.

This version of the skill **delegates all external research**. When it identifies a concept that needs validation or deeper exploration, it will prompt you to use the `librarian` skill. This keeps the note-taking process clean and separated from research.

## Usage

To use this skill, you typically start a course and then add notes as you go.

```
/course-note start course "Advanced TypeScript" on "Frontend Masters" by "Matt Pocock"
```
```
/course-note add note "Generics are like variables for types."
```
```
/course-note rephrase this
```

For a complete list of commands and workflows, see the [SKILL.md](./SKILL.md) file.
