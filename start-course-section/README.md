# Start Course Section Skill

This is a simple, focused skill that creates the literature note for a new section in a course.

## Core Workflow

1.  **Trigger**: Use `start section {N}: {title} from course {course_slug}`.
2.  **Action**: The skill creates a new markdown file in `content/literature/` for the specified section, pre-populated with the necessary headers.

This skill is intended to be used as a prerequisite for other skills like `course-transcript`.
