---
name: course-zettel
description: Active note-taking companion for online courses. Tracks progress across sessions, captures raw notes during lessons, and extracts atomic zettelkasten concepts on finish. Researches recurring principles across notes to surface cross-links you'd miss. Use when user mentions 'course', 'lesson', 'lecture', 'udemy', 'start course', 'start section', 'start lesson', 'add note', 'finish lesson', 'finish section', 'research connections', 'cross-link', or any course learning activity.
---

# Course Zettel

Active companion during online course learning. One literature note per course **section**, one permanent note per **atomic** concept. Progress tracked in `content/wip/{course-slug}.md` — the agent reads this to **resume** context across sessions.

When context is ambiguous (user says "add note" without specifying course), read the most recently updated wip note in `content/wip/` to restore context.

## Lifecycle

```
START COURSE  →  START SECTION  →  START LESSON  →  ADD NOTE*  →  FINISH LESSON
                                                                          ↓
                                                                   FINISH SECTION
                                                                          ↓
                                                                   RESEARCH CONNECTIONS
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

### FINISH LESSON

**Trigger**: "finish lesson", "done with this lesson"

1. **Review**: read all `L{N}:` notes for this lesson. Ask the owner if anything was missed.
2. **Discuss**: ask 1–2 questions — what surprised them? What would they explain to a colleague? If owner pushes back, proceed; flag in My Take.
3. **Mine & Research**: scan raw notes against the **extraction heuristics** (see Reference). Identify 2–5 **atomic** concepts — favour cross-cutting ones that apply beyond this lesson. **For each concept scored High on the extraction table** (cross-cutting, repeated, foundational), research externally before proposing: search the web or documentation for established terminology, related patterns, and alternative framings. This sharpens the concept name and surfaces cross-links you'd miss from notes alone. Propose slugs + one-sentence descriptions to owner for confirmation, informed by the research.
4. **Write permanent notes**: for each confirmed concept, check `content/permanent-notes.md` for existence.
   - **New**: create `content/permanent/{slug}.md`. Write body (1–3 paragraphs, English). Populate `## Connections` by scanning existing permanent notes for related concepts — link them. Populate `## Sources` with this section's literature note.
   - **Existing**: read it. If this lesson adds depth → update body, add this literature note to Sources, add any new cross-links to `## Connections`.
   - **Cross-link back**: update the section literature note's `## Links` with `[[permanent/{slug}]]` for every permanent note touched.
5. **Update catalogs**: `content/permanent-notes.md` (new rows), `content/literature-notes.md` (if new section note), `content/log.md` with `## [YYYY-MM-DD] capture | {Course} — S{N} L{N}`.
6. **Update wip**: mark lesson complete in progress table.

**Done when**: every extracted concept is a new or updated permanent note, bidirectional links exist between the literature note and all touched permanent notes, all three catalogs updated, wip note shows lesson complete. Every High-signal concept was researched externally before writing. No duplicate permanent notes.

### FINISH SECTION

**Trigger**: "finish section", "done with this section"

1. Review the section literature note — ensure Summary captures all lessons.
2. **Cross-concept research**: scan all lessons in this section for recurring principles you may have missed lesson-by-lesson. Any concept that appears across 2+ lessons but wasn't extracted → flag it. Run external research on flagged concepts to validate terminology and surface connections. If the concept is substantial enough to be atomic, create a permanent note and backlink.
3. Ask: "Create a Map of Content for this course?" If yes and 5+ permanent notes exist → create or update `content/maps/map-of-{course-slug}.md`.
4. Update wip note: mark section complete.

**Done when**: literature note Summary finalized, cross-concept research pass complete (every recurring concept that spans 2+ lessons has been either extracted as a permanent note or documented as a deliberate skip in the literature note), MOC created/updated if requested, wip updated.

### RESEARCH CONNECTIONS

**Trigger**: "research connections", "cross-link", "find recurring principles"

Run a research pass over accumulated notes to surface cross-links and validate concepts against external sources. Follows the recurring-principle pipeline:

1. **Mine**: scan all literature notes from the active course. Flag concepts that repeat across 2+ lessons or sections — these are **recurring principles**. Also flag standalone concepts scored High on the extraction table.
2. **Check existing**: for each flagged concept, search `content/permanent-notes.md` and existing permanent notes for overlap or related ideas. Skip concepts already covered by an existing permanent note (update it instead).
3. **Research**: for each remaining concept, research externally — web search, documentation, Context7 — to find established terminology, canonical definitions, related patterns, and industry-standard names. Use this to sharpen the concept slug and discover cross-links the notes alone don't reveal.
4. **Create permanent note**: one atomic note per validated concept. Body in 1–3 paragraphs, informed by research. Populate `## Connections` with related permanent notes found in step 2. Populate `## Sources` with the originating literature notes.
5. **Backlink**: update each source literature note's `## Links` with `[[permanent/{slug}]]`.
6. **Cross-link**: for every related permanent note identified in step 2, add `[[permanent/{slug}]]` back to the new note in its `## Connections`.
7. **Update catalogs + log**: `permanent-notes.md`, `literature-notes.md` (if new), `log.md` with `## [YYYY-MM-DD] research | {Course}`.

**Done when**: every recurring principle is a new or updated permanent note with external research in its body, bidirectional links exist between every touched literature note and permanent note, all catalogs and log updated.

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

### Research rules

Research is the external legwork that validates concepts and surfaces cross-links you'd miss from notes alone. Trigger it when a concept meets the **recurring-principle threshold** or scores High on the extraction table.

**When to research**:
- A concept appears across 2+ lessons or sections (recurring principle)
- A concept scores High on the extraction heuristics (cross-cutting, repeated, foundational)
- A concept has a vague or colloquial name that would benefit from established terminology
- An existing permanent note needs external validation to sharpen its body

**How to research**:
- **Search the web** for `"{concept} in {domain}"` or `"{concept} best practice"`. Look for industry-standard definitions, canonical names, and related patterns.
- **Search documentation** (Context7, official docs) when the concept ties to a specific tool or framework.
- **Offline**: before external research, always check `content/permanent-notes.md` and existing permanent notes for related concepts — offline cross-links are the cheapest kind.
- **Record findings** in the permanent note body — cite the source (URL, doc section). The research enriches the note but the note remains atomic: 1–3 paragraphs, not a literature review.

**Done when**: every concept that meets the research threshold has been searched externally, findings reflected in the permanent note body or `## Connections`, and any cross-links discovered through research have been added.

### Atomic test

Can this idea be explained in 1–3 paragraphs without branching into another idea? Does an existing note already cover it? (Check `permanent-notes.md` first.) If it splits → two notes. If it overlaps → update existing.

### Raw material

If `raw/courses/{course-slug}/` contains transcripts or slides, read them during START LESSON. This enriches the ADD NOTE and FINISH LESSON steps.
