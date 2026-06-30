# Course Transcript Skill

This skill processes a lecture transcript, turning it into structured and auto-linked Zettelkasten notes.

**This skill does NOT create new notes.** It only appends processed lecture content to an existing literature note.

## Concrete Workflow Example

Here is how you would use the skills together to process a lecture from start to finish.

**1. Start the Section**

First, use the `start-course-section` skill to create the note for the entire section.

> `start section 6: "Tool Design & MCP" from course "certified-claude-architect-masterclass-2026"`

This prepares the literature note for all the lectures in section 6.

**2. Process Your First Lecture**

Next, use this skill (`course-transcript`) to add the transcript for the first lecture.

> `add transcript for lecture 1 of course "certified-claude-architect-masterclass-2026": {paste the full transcript here}`

The skill will automatically process the text, add `[[wikilinks]]` to known concepts, and append the structured `L1:` notes to the section file.

**3. Process Subsequent Lectures**

For the next lecture in the same section, you simply run the command again with the new lecture number and transcript.

> `add transcript for lecture 2 of course "certified-claude-architect-masterclass-2026": {paste transcript for lecture 2}`

This will append the structured `L2:` notes to the same section file, keeping all your notes for that section organized in one place.
