---
name: course-note-researcher
description: An all-in-one note-taking companion for online courses. It captures, organizes, and refines notes, and includes built-in research capabilities to enrich and validate concepts. Use when you want a single skill to handle both note-taking and research for your courses.
---

# Discipline: Integrated Learning & Research with Zettelkasten

This skill enforces a rigorous discipline for structured learning and integrated research, transforming ephemeral course material into a durable, interconnected knowledge base using the Zettelkasten method. It is your unwavering companion, ensuring every insight is captured, refined, integrated, and externally validated.

**Core Principle**: Every piece of knowledge must be atomic, linked, and verifiable. Ambiguity is the enemy of understanding. External validation via research is paramount.

When context is ambiguous (e.g., "add note" without specifying a course), the system will intelligently infer context by reading the most recently updated work-in-progress (WIP) note in `content/wip/`.

## Phases of Integrated Learning & Research

### Phase 1: Course Initialization

**Goal**: Establish the foundational tracking and metadata for a new course, ensuring a single source of truth for progress.

**Workflow**:
1.  **Trigger**: Initiate with "start course {title} on {platform} by {instructor}".
2.  **Action**: A dedicated work-in-progress (WIP) note will be meticulously crafted at `content/wip/{course-slug}.md`. This note serves as the central hub for your course journey.
3.  **Content**: The WIP note will be pre-populated with essential frontmatter (title, type, platform, course slug, instructor, creation/update timestamps, and domain-specific tags). A progress table will be initialized, awaiting your learning milestones.

**Completion Criterion**: The WIP note (`content/wip/{course-slug}.md`) MUST exist, and all metadata (slug, platform, instructor, domain tags) MUST be accurately confirmed. Failure to meet this criterion halts progress.

### Phase 2: Section & Lesson Engagement

**Goal**: Systematically capture raw insights during active learning sessions and iteratively refine them into clear, concise concepts, leveraging external research for validation.

**Workflow**:
1.  **Start Section**: Trigger with "start section {N}: {title}". A literature note will be created at `content/literature/{course-slug}-section-{N}-{slug}.md`, ready for your observations. The WIP note's progress table will be updated, marking the current section. If the course WIP note is absent, you will be directed to Phase 1.
2.  **Start Lesson**: Trigger with "start lesson {N}: {title}". The WIP note will be updated to reflect the active lesson. A confirmation will be issued: "Ready — Section {N}, Lesson {N}: {title}." If the section literature note is missing, it will be created.
3.  **Add Note**: Use "add note: {text}" or simply "{text}" during an active lesson. Your raw observations will be appended directly to the section literature note under **Key Ideas**, prefixed `L{N}:`. **Crucially, no synthesis or interpretation occurs at this stage; capture verbatim.** Mentally flag potential atomic concepts for later extraction. If no lesson is active, the system will demand clarification.
4.  **Rephrase Note**: Trigger with "rephrase this", "clarify this note", or "let's refine this thought". This initiates a rigorous, interactive interview process to transform raw notes into deeply understood concepts.
    *   **Identify Target**: The system will confirm the note to be rephrased (defaulting to the last added).
    *   **Grilling Session**: A relentless interview will commence, probing for clarification, assumptions, connections to prior knowledge, real-world analogies, and implications. Each question demands a response, guiding you towards a precise understanding. **If external validation is required, the system will explicitly delegate to the `librarian` skill to find established terminology, related patterns, or alternative framings. The `librarian`'s findings will be integrated into the refinement process.**
    *   **Synthesize & Propose**: Upon achieving shared understanding, a summary of key insights will be presented, followed by a proposed rephrased note for your approval.
    *   **Update**: Upon your confirmation, the literature note will be updated, either by replacing the original or adding the refined version as a sub-bullet, based on your preference.

**Completion Criterion**: Each raw note MUST be captured under the correct lesson prefix. Rephrased notes MUST be confirmed by the user and accurately reflected in the literature note. All external research needs identified during rephrasing MUST be delegated to and resolved by the `librarian` skill, and their findings integrated.

### Phase 3: Concept Crystallization & Research

**Goal**: Transform raw and refined notes into atomic, interconnected permanent notes, enriching the knowledge graph through targeted external research and validation.

**Workflow**:
1.  **Finish Lesson**: Trigger with "finish lesson" or "done with this lesson".
    *   **Review & Discuss**: All `L{N}:` notes for the lesson will be reviewed. You will be prompted to confirm completeness and reflect on key takeaways.
    *   **Mine & Delegate Research**: The system will rigorously scan raw notes against predefined **extraction heuristics** (see Reference) to identify 2–5 **atomic** concepts. For each concept scoring **High** (cross-cutting, repeated, foundational), a precise research query will be formulated and **delegated to the `librarian` skill**. The `librarian`'s findings (including citations) will be used to sharpen concept slugs, discover cross-links, and provide foundational evidence. Proposed slugs and one-sentence descriptions will be presented for your confirmation.
    *   **Write Permanent Notes**: For each confirmed concept, the system will check `content/permanent-notes.md`.
        *   **New Concepts**: A new permanent note will be created at `content/permanent/{slug}.md` (1–3 paragraphs, English), incorporating insights from the `librarian`'s research. `## Connections` will be populated by scanning existing permanent notes for related concepts, establishing vital links. `## Sources` will link back to this section's literature note and include citations from the `librarian`'s findings.
        *   **Existing Concepts**: If a permanent note already exists, it will be updated. This lesson's literature note will be added to `## Sources`, and any new insights (including those from `librarian` research) will deepen the body. New cross-links will be integrated into `## Connections`.
    *   **Cross-link Back**: The section literature note's `## Links` will be updated with `[[permanent/{slug}]]` for every permanent note touched, ensuring bidirectional traceability.
    *   **Update Catalogs**: `content/permanent-notes.md`, `content/literature-notes.md` (if new section note), and `content/log.md` will be updated to reflect the new knowledge.
    *   **Update WIP**: The WIP note will mark the lesson as complete.
2.  **Finish Section**: Trigger with "finish section" or "done with this section".
    *   **Review Summary**: The section literature note's Summary will be finalized.
    *   **Delegate Cross-concept Research**: All lessons within the section will be scanned for recurring principles. Any concept appearing across 2+ lessons not yet extracted will be flagged. **The `librarian` skill will be delegated to validate terminology, surface connections, and provide evidence for these flagged concepts.** If a concept is sufficiently atomic, a new permanent note will be created and backlinked, incorporating the `librarian`'s findings.
    *   **Map of Content (MOC)**: You will be prompted to create a Map of Content for the course if 5+ permanent notes exist. If approved, `content/maps/map-of-{course-slug}.md` will be created or updated.
    *   **Update WIP**: The WIP note will mark the section as complete.

**Completion Criterion**: Every extracted concept MUST result in a new or updated permanent note. Bidirectional links MUST exist between literature notes and all touched permanent notes. All three catalogs (`permanent-notes.md`, `literature-notes.md`, `log.md`) MUST be updated. The WIP note MUST reflect lesson and section completion. **Every High-signal concept MUST be researched using the `librarian` skill before finalization, and its findings (including citations) integrated into the permanent note.** No duplicate permanent notes are permitted.

### Phase 4: Knowledge Maintenance & Evolution

**Goal**: Ensure the knowledge base remains dynamic, accurate, and reflective of your evolving understanding.

**Workflow**:
1.  **Update**: Trigger with "update {course|section|lesson} with: {text}".
2.  **Action**: Retrospective insights will be precisely appended:
    *   **Course-level**: Appended to the WIP note (a `## Notes` section will be added if absent).
    *   **Section-level**: Appended to the section literature note's My Take or Summary.
    *   **Lesson-level**: Appended to the lesson's Key Ideas within the section literature note.

**Completion Criterion**: The insight MUST be accurately appended at the specified level within the correct file.

### Phase 5: Research Connections (Dedicated Librarian Interface)

**Goal**: Proactively explore and validate connections between concepts across the entire knowledge base, leveraging the `librarian` skill for deep external research.

**Workflow**:
1.  **Trigger**: Initiate with "research connections", "cross-link these notes", or "deep dive into {concept}".
2.  **Action**:
    *   **Identify Research Scope**: The system will analyze the request to determine the scope of research (e.g., specific concepts, recurring themes across notes, potential gaps).
    *   **Formulate Precise Queries**: For each identified research need, precise queries will be formulated to maximize the effectiveness of external research.
    *   **Delegate to Librarian**: All research tasks will be **explicitly delegated to the `librarian` skill** (e.g., `task(subagent_type="librarian", prompt="...")`). This ensures rigorous, evidence-based findings.
    *   **Synthesize & Integrate**: The `librarian`'s findings (including SHA-pinned citations) will be meticulously synthesized. New connections will be identified, existing permanent notes updated with new insights and sources, and new permanent notes created if a novel, high-signal concept is discovered.
    *   **Update Cross-links**: All relevant `## Connections` and `## Sources` sections in permanent notes, and `## Links` in literature notes, will be updated to reflect the newly discovered or validated relationships.

**Completion Criterion**: All identified research questions MUST be delegated to and resolved by the `librarian` skill. The `librarian`'s findings (including citations) MUST be integrated into the knowledge base, resulting in updated or new permanent notes and strengthened cross-links.

## Reference

### Naming

| Note        | Pattern                                   |
|-------------|-------------------------------------------|
| Wip tracker | `{course-slug}.md`                        |
| Literature  | `{course-slug}-section-{N}-{section-slug}.md` |
| Permanent   | `{concept-slug}.md` (lowercase, hyphens, no dates) |

### Literature Note Sections

**Summary** (filled during FINISH SECTION) → **Key Ideas** (bullets prefixed `L{N}:`) → **Quotes** → **My Take** (Vietnamese) → **Links** (`[[permanent/...]]`)

### Permanent Note Sections

Body (1–3 paragraphs) → `## Connections` (`[[permanent/...]]` to related concepts) → `## Sources` (`[[literature/...]]` to course lit notes, plus `librarian` citations)

### Language

| Content                     | Language |
|-----------------------------|----------|
| Concept body, Summary, Key Ideas | English  |
| My Take                     | Vietnamese |

For non-technical courses, always confirm language preference with the owner.

### Tags

Domain tags + `course`. Example: `tags: [docker, containers, course]`

### Extraction Heuristics

When rigorously scanning raw notes for permanent-note-worthy concepts, prioritize with extreme prejudice:

| Signal              | Weight | Example                                       |
|---------------------|--------|-----------------------------------------------|
| **Cross-cutting**   | High   | "immutable infrastructure" vs "docker run -p" |
| **Repeated**        | High   | Concept that consistently reappears           |
| **Foundational**    | High   | "container vs image" before "docker layers"   |
| **Owner emphasized**| Medium | Flagged with "important", "key", or in discussion |
| **Actionable**      | Medium | A pattern, rule, or principle for practical application |
| **Definitional**    | Low    | Course-specific jargon or new terms           |

De-prioritize ruthlessly: lesson-specific trivia, UI walkthroughs, version-specific details (unless fundamental), "how to install X" steps.

### Cross-linking Rules

When forging new permanent notes or updating existing ones from course material, adhere to these immutable laws:

1.  **Permanent → Sources**: Invariably include `[[literature/{course-slug}-section-{N}-...]]` for the precise course section that informed the concept, AND **explicitly include SHA-pinned citations from `librarian` research.**
2.  **Permanent → Connections**: Systematically scan `content/permanent-notes.md` for genuinely related concepts. Employ the concept slug and domain tags to discover and link relevant knowledge. The richer the graph, the greater the compounding value.
3.  **Literature → Links**: Post-creation of permanent notes, append `[[permanent/{slug}]]` entries to the section literature note's `## Links`. This establishes the essential backlink; every permanent note derived from this course MUST be referenced here.
4.  **Cross-course Linking**: If a permanent note already exists from a disparate course, under no circumstances create a duplicate. Instead, update the existing note: integrate the new course's literature note into `## Sources` and infuse any new depth into the body, **including new `librarian` citations.**

### Atomic Test

Can this idea be articulated in 1–3 paragraphs without fracturing into another distinct idea? Does an existing note already encapsulate this concept? (Verify `permanent-notes.md` with extreme prejudice.) If it bifurcates, it demands two notes. If it overlaps, the existing note MUST be updated.

### Raw Material

Should `raw/courses/{course-slug}/` contain transcripts or slides, these resources MUST be ingested during START LESSON to enrich the ADD NOTE and FINISH LESSON processes.
