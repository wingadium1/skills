---
name: course-note
description: An active note-taking companion for online courses using the Zettelkasten method. It focuses on capturing, organizing, and refining notes. For external research, it delegates to the 'librarian' skill. Use when a user wants to manage course notes ('start course', 'add note', 'finish lesson').
---

# Course Note Taker

Active companion during online course learning. One literature note per course **section**, one permanent note per **atomic** concept. Progress tracked in `content/wip/{course-slug}.md` — the agent reads this to **resume** context across sessions.

When context is ambiguous (user says "add note" without specifying course), read the most recently updated wip note in `content/wip/` to restore context.

## Lifecycle

```
START COURSE  →  START SECTION  →  START LESSON  →  ADD NOTE*  →  FINISH LESSON
                                                                          ↓
                                                                   FINISH SECTION
                                                                          ↑
                                                                   UPDATE (any time)
```

## Actions

### START COURSE

**Trigger**: "start course {title} on {platform} by {instructor}"

Create `content/wip/{course-slug}.md`:

```yaml
---
title: "{Course Title} — Tracker"
type: course-tracker
platform: {platform}
course: "{course-slug}"
instructor: "{Instructor Name}"
tags: [{domain}, course]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# {Course Title}

**Platform**: {platform} | **Instructor**: {Instructor Name} | **Status**: active

## Progress

| # | Section | Status | Lessons |
|---|---------|--------|---------|
**Current**: —
```

**Done when**: wip note created, all metadata confirmed (slug, platform, instructor, domain tags).

### START SECTION

**Trigger**: "start section {N}: {title}"

Create the section literature note at `content/literature/{course-slug}-section-{N}-{slug}.md` with frontmatter filled, Summary and Key Ideas empty. Update wip note: add row to progress table, mark as current.

If the course wip note doesn't exist, ask owner to START COURSE first.

**Done when**: literature note created, wip note updated.

### START LESSON

**Trigger**: "start lesson {N}: {title}"

Update wip note: set current lesson. The agent confirms: "Ready — Section {N}, Lesson {N}: {title}."

If the section literature note doesn't exist (first lesson in section), create it first.

**Done when**: wip note shows current section + lesson, agent confirmation given.

### ADD NOTE

**Trigger**: "add note: {text}" or bare "{text}" during an active lesson

Append to the section literature note under **Key Ideas**, prefixed `L{N}:`. Keep the user's wording exactly — this is raw capture. Do not discuss, synthesize, or extract concepts.

As notes accumulate, mentally flag any that match the **extraction heuristics** — but do not act on them. Save that for FINISH LESSON.

If no lesson is active (no wip note or ambiguous context), ask which course/section/lesson.

**Done when**: note appended under correct lesson prefix. No file writes beyond the literature note.

### REPHRASE NOTE

**Trigger**: "rephrase this", "clarify this note", "let's refine this thought"

This action transforms a raw, quickly-captured note into a clearer, more familiar, and well-understood concept through a relentless interview process.

**Workflow**:

1.  **Identify Target**: Confirm which note the user wants to rephrase. Default to the last note added.
2.  **Initiate Grilling Session**: Begin an interactive interview based on the note's content and the course context. The goal is to deeply understand the user's mental model.
    - Ask questions one at a time, waiting for feedback before proceeding.
    - For each question, you can suggest a recommended answer to guide the conversation.
    - The questions should probe different angles:
        - **Clarification**: "When you say 'X', what exactly do you mean?"
        - **Assumptions**: "What are you assuming to be true for 'Y' to work?"
        - **Connections**: "How does this relate to {previous concept from the course}?"
        - **Familiarity**: "What's a real-world analogy for this? How would you explain this to a 5-year-old / a colleague?"
        - **Implications**: "What are the consequences of this idea? What breaks if this is wrong?"
    - If a question can be answered by exploring existing notes (`content/permanent/` or `content/literature/`), do that first before asking the user.
3.  **Synthesize**: Once the interview feels complete and a shared understanding is reached, summarize the key insights from the conversation.
4.  **Propose Rephrased Note**: Draft a new, clearer version of the note based on the synthesis. Present it to the user for approval.
5.  **Update Note**: Upon confirmation, update the literature note. You can either:
    - Replace the original raw note with the rephrased version.
    - Add the rephrased version as a sub-bullet or a separate "Refined:" block under the original note. Ask the user for their preference.

**Done when**: The user has confirmed the rephrased note, and the literature note file has been updated accordingly.

### FINISH LESSON

**Trigger**: "finish lesson", "done with this lesson"

1. **Review**: read all `L{N}:` notes for this lesson. Ask the owner if anything was missed.
2. **Discuss**: ask 1–2 questions — what surprised them? What would they explain to a colleague? If owner pushes back, proceed; flag in My Take.
3. **Mine & Delegate Research**: Scan raw notes against the **extraction heuristics** (see Reference). Identify 2–5 **atomic** concepts. For each concept scored **High** (cross-cutting, repeated, foundational), formulate a research query and delegate to the `librarian` skill to find established terminology, related patterns, and alternative framings. Use the librarian's findings to sharpen the concept slug and discover cross-links. Propose slugs + one-sentence descriptions to owner for confirmation.
4. **Write permanent notes**: for each confirmed concept, check `content/permanent-notes.md` for existence.
   - **New**: create `content/permanent/{slug}.md`. Write body (1–3 paragraphs, English). Populate `## Connections` by scanning existing permanent notes for related concepts — link them. Populate `## Sources` with this section's literature note.
   - **Existing**: read it. If this lesson adds depth → update body, add this literature note to Sources, add any new cross-links to `## Connections`.
   - **Cross-link back**: update the section literature note's `## Links` with `[[permanent/{slug}]]` for every permanent note touched.
5. **Update catalogs**: `content/permanent-notes.md` (new rows), `content/literature-notes.md` (if new section note), `content/log.md` with `## [YYYY-MM-DD] capture | {Course} — S{N} L{N}`.
6. **Update wip**: mark lesson complete in progress table.

**Done when**: every extracted concept is a new or updated permanent note, bidirectional links exist between the literature note and all touched permanent notes, all three catalogs updated, wip note shows lesson complete. Every High-signal concept was researched using the `librarian` skill before writing. No duplicate permanent notes.

### FINISH SECTION

**Trigger**: "finish section", "done with this section"

1. Review the section literature note — ensure Summary captures all lessons.
2. **Delegate Cross-concept research**: Scan all lessons in this section for recurring principles you may have missed. Any concept that appears across 2+ lessons but wasn't extracted → flag it. Delegate to the `librarian` skill to validate terminology and surface connections for these flagged concepts. If a concept is substantial enough to be atomic, create a permanent note and backlink.
3. Ask: "Create a Map of Content for this course?" If yes and 5+ permanent notes exist → create or update `content/maps/map-of-{course-slug}.md`.
4. Update wip note: mark section complete.

**Done when**: literature note Summary finalized, cross-concept research pass complete, MOC created/updated if requested, wip updated.

### UPDATE

**Trigger**: "update {course|section|lesson} with: {text}"

Add retrospective insight at the specified level:
- **Course** → append to wip note (add a `## Notes` section if needed).
- **Section** → append to section literature note's My Take or Summary.
- **Lesson** → append to the lesson's Key Ideas in the section literature note.

**Done when**: insight appended at correct level.

## Reference

### Naming

| Note | Pattern |
|------|---------|
| Wip tracker | `{course-slug}.md` |
| Literature | `{course-slug}-section-{N}-{section-slug}.md` |
| Permanent | `{concept-slug}.md` (lowercase, hyphens, no dates) |

### Literature note sections

**Summary** (fill during FINISH SECTION) → **Key Ideas** (bullets prefixed `L{N}:`) → **Quotes** → **My Take** (Vietnamese) → **Links** (`[[permanent/...]]`)

### Permanent note sections

Body (1–3 paragraphs) → `## Connections` (`[[permanent/...]]` to related concepts) → `## Sources` (`[[literature/...]]` to course lit notes)

### Language

| Content | Language |
|---------|----------|
| Concept body, Summary, Key Ideas | English |
| My Take | Vietnamese |

For non-technical courses, ask owner.

### Tags

Domain tags + `course`. Example: `tags: [docker, containers, course]`

### Extraction heuristics

When scanning raw notes for permanent-note-worthy concepts, prioritize:

| Signal | Weight | Example |
|--------|--------|---------|
| **Cross-cutting** — applies beyond this lesson or course | High | "immutable infrastructure" vs "docker run -p" |
| **Repeated** — mentioned across multiple lessons or sections | High | concept that keeps coming back |
| **Foundational** — prerequisite for understanding later material | High | "container vs image" before "docker layers" |
| **Owner emphasized** — flagged with "important", "key", or in discuss step | Medium | what surprised them |
| **Actionable** — a pattern, rule, or principle you'd apply at work | Medium | "always pin base image versions" |
| **Definitional** — a term the course invents or redefines | Low | course-specific jargon |

De-prioritize: lesson-specific trivia, UI walkthroughs, version-specific details (unless fundamental), "how to install X" steps.

### Cross-linking rules

When creating or updating a permanent note from a course:

1. **Permanent → Sources**: always include `[[literature/{course-slug}-section-{N}-...]]` for the course section that informed it.
2. **Permanent → Connections**: scan `content/permanent-notes.md` for related concepts. Search for the concept slug and domain tags. Link any that are genuinely related — the richer the graph, the more value compounds.
3. **Literature → Links**: after writing permanent notes, add `[[permanent/{slug}]]` entries to the section literature note's `## Links`. This is the backlink — every permanent note created from this course must appear here.
4. **Cross-course linking**: if a permanent note already exists from a different course, do not create a duplicate. Update it — add the new course's literature note to `## Sources`, and add any new depth to the body.

### Atomic test

Can this idea be explained in 1–3 paragraphs without branching into another idea? Does an existing note already cover it? (Check `permanent-notes.md` first.) If it splits → two notes. If it overlaps → update existing.

### Raw material

If `raw/courses/{course-slug}/` contains transcripts or slides, read them during START LESSON. This enriches the ADD NOTE and FINISH LESSON steps.
