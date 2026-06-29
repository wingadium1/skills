---
name: start-course-section
description: Starts a new section for a course, creating the necessary literature note. Use with 'start section {N}: {title} from course {course_slug}'.
---

# Discipline: Course Section Initialization

This skill performs one, critical function: initializing a literature note for a new course section. It is a prerequisite for adding lecture transcripts.

## Workflow

**Goal**: Prepare a literature note to hold all lectures for a new course section.

**Workflow**:
1.  **Trigger**: `start section {N}: {section_title} from course {course_slug}`.
2.  **Context Check**: Verifies the course's Work-in-Progress note exists at `content/wip/{course-slug}.md`. If not, it will fail with a clear error.
3.  **Action**: Creates a new literature note at `content/literature/{course-slug}-section-{N}-{section-slug}.md`. The note is pre-populated with a standard YAML frontmatter and empty `## Summary`, `## Key Ideas`, `## Quotes`, `## My Take`, and `## Links` sections.
4.  **Confirmation**: "Ready for transcripts for Section {N}: {section_title}. You can now use the `course-transcript` skill."

**Completion Criterion**: An empty literature note for the section MUST exist at the correct path with the correct frontmatter.
