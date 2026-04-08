---
name: knowledge-extractor
description: Extract structured knowledge from raw transcripts, recordings, interviews, workshops, or coaching calls. Use this skill whenever the user wants to process a transcript into a usable knowledge base document, extract methodology from audio/video content, turn raw spoken content into structured reference material, or says things like "process this transcript", "extract the knowledge from this", "turn this into something usable", or "build a knowledge doc from this". Also trigger when the user drops a .txt, .md, or .srt transcript file and asks to organize, structure, or extract from it.
---

# Knowledge Extractor

Turn raw transcripts into structured, reusable knowledge base documents that AI can reference for high-quality output generation.

## Why This Exists

Raw transcripts are messy. They contain filler words, tangents, repeated points, and buried gold. When you feed raw transcripts directly into an AI project or knowledge base, the AI pulls shallow keyword-matched fragments instead of deeply understanding the methodology, frameworks, and language patterns inside.

This skill solves that by extracting and categorizing the actual substance from a transcript into a clean, structured document that AI (or humans) can reference effectively.

## What It Extracts

The extractor pulls seven categories of knowledge from any transcript:

1. **Frameworks & Methodologies** - Named systems, step-by-step processes, models, sequences. The "how they do things" stuff.
2. **Core Beliefs & Principles** - Philosophical positions, convictions, worldview statements. The "why they do things" stuff.
3. **Language Patterns & Signature Phrases** - Specific wording, metaphors, recurring phrases, ways of explaining things that are unique to the speaker.
4. **Stories & Examples** - Case studies, anecdotes, client stories, personal experiences used to illustrate points.
5. **Tactical Advice** - Specific, actionable recommendations. The "do this, not that" stuff.
6. **Definitions & Distinctions** - How the speaker defines key terms, especially where their definition differs from common usage.
7. **Objection Handling & Reframes** - How they address pushback, common questions, or limiting beliefs.

## How To Use

### Single Transcript Processing

When the user provides a transcript file or pastes transcript text:

1. Read the full transcript
2. Identify the speaker(s) and the context (workshop, interview, coaching call, podcast, etc.)
3. Extract content into all seven categories
4. Output a structured markdown document

### Batch Processing

When the user points to a folder of transcripts:

1. List all transcript files found
2. Confirm with the user before processing
3. Process each file individually
4. Save each output as `[original-filename]-extracted.md` in the same directory (or a specified output directory)

## Output Format

ALWAYS use this exact template for the output document:

```markdown
# Knowledge Extract: [Topic or Title]

**Source:** [filename or description]
**Speaker(s):** [name(s) if identifiable]
**Context:** [workshop / coaching call / podcast / interview / training]
**Date Processed:** [YYYY-MM-DD]

---

## Frameworks & Methodologies

### [Framework Name]
- **What it is:** [1-2 sentence description]
- **Steps/Components:**
  - [Step 1]
  - [Step 2]
  - [etc.]
- **When to use it:** [context for application]
- **Direct quote(s):** "[exact words from transcript that capture the essence]"

---

## Core Beliefs & Principles

### [Belief/Principle Name]
- **Statement:** [the principle in clear language]
- **Context:** [why they believe this / what experience led to it]
- **Direct quote(s):** "[exact words]"

---

## Language Patterns & Signature Phrases

| Phrase / Pattern | Context Used | Why It Matters |
|---|---|---|
| "[exact phrase]" | [when they use it] | [what it accomplishes] |

---

## Stories & Examples

### [Story Title / Short Description]
- **Setup:** [the situation or context]
- **Key Point:** [what the story illustrates]
- **Outcome:** [the result or lesson]
- **Quotable moment:** "[exact words]"

---

## Tactical Advice

- **[Topic]:** [specific recommendation] — "[supporting quote if available]"

---

## Definitions & Distinctions

| Term | Their Definition | How It Differs From Common Usage |
|---|---|---|
| [term] | [their definition] | [distinction] |

---

## Objection Handling & Reframes

| Objection / Limiting Belief | Their Reframe | Supporting Reasoning |
|---|---|---|
| "[common objection]" | "[their response]" | [why it works] |

---

## Raw Gold (Unclassified Insights)

Anything valuable that doesn't fit neatly into the above categories:

- [insight]
```

## Important Guidelines

- **Preserve exact language.** When capturing quotes and phrases, use the speaker's actual words. This is critical for maintaining voice and authenticity in downstream generation.
- **Don't invent.** Only extract what's actually in the transcript. If a category has nothing, write "None identified in this transcript." Don't fabricate content to fill sections.
- **Context matters.** Always note the context a phrase or framework was used in. A technique used for sales conversations is different from one used for onboarding.
- **Name unnamed things.** Speakers often describe frameworks without naming them. Give them a descriptive working name in [brackets] to make them referenceable.
- **Flag contradictions.** If the speaker contradicts themselves or gives conflicting advice at different points, note both versions and flag the contradiction.
- **Quality over quantity.** Five deeply extracted insights beat twenty surface-level bullet points. Go deep on what matters.

## File Handling

- Save output files as markdown (.md)
- Default output location: same directory as the source file
- If the user specifies an output directory, use that instead
- Naming convention: `[source-filename]-extracted.md`
- If processing a batch, create a `_processing-log.md` that lists all files processed with status
