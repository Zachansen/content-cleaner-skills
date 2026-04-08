---
name: pattern-library-builder
description: Consolidate multiple knowledge extraction documents into a unified master pattern library. Use this skill when the user has multiple extracted knowledge docs and wants to merge them into one comprehensive reference, build a master knowledge base from multiple sources, consolidate extractions, create a pattern library, or says things like "combine these into one doc", "build my master reference", "merge these knowledge files", or "create the pattern library". Trigger when the user references multiple -extracted.md files or a folder of extracted docs.
---

# Pattern Library Builder

Merge multiple knowledge extraction documents into a single, consolidated master pattern library that serves as the definitive reference for content generation, script writing, or any downstream AI task.

## Why This Exists

After running the knowledge-extractor skill on multiple transcripts, you end up with several individual extraction docs. Each one captures knowledge from a single source. But frameworks get repeated across transcripts. Language patterns show up in slightly different forms. Stories get referenced multiple times.

This skill takes all those individual extractions and builds ONE master document that:

- Deduplicates repeated frameworks and concepts
- Identifies patterns that show up across multiple sources (these are the speaker's core stuff)
- Consolidates language patterns into a comprehensive reference
- Ranks elements by frequency (things mentioned in 5 transcripts matter more than things in 1)
- Creates a single source of truth for downstream generation

## How To Use

### Standard Flow

1. Point the skill at a directory containing extracted knowledge docs (the `-extracted.md` files from the knowledge-extractor skill)
2. The skill reads all files and cross-references content
3. It outputs a single `pattern-library.md` file

### Command

When the user says something like "build the pattern library" or "consolidate these extractions":

1. Scan the specified directory for `-extracted.md` files
2. List all files found and confirm with the user
3. Read all files
4. Cross-reference and consolidate
5. Output the master document

## Consolidation Rules

Follow these rules when merging content across multiple extraction docs:

### Deduplication
- If the same framework appears in multiple extractions, merge into ONE entry with the most complete description
- Note how many sources it appeared in (frequency = importance)
- Keep the best quotes from across all sources

### Frequency Tagging
- Tag every item with a frequency indicator:
  - **[CORE]** = appears in 3+ sources. This is fundamental to their methodology.
  - **[RECURRING]** = appears in 2 sources. Important but not foundational.
  - **[SINGLE]** = appears in 1 source only. May be situational or secondary.

### Domain Tagging
- Every item also gets domain tags based on what area of the methodology it applies to
- Domains are detected during preprocessing (from the `_preprocessing-report.md`) OR can be specified by the user
- Format: `[CORE] [domains: anxiety, confidence]`
- This allows downstream generators to filter the pattern library for only relevant knowledge
- Common domain structures:
  - **By topic:** anxiety, confidence, weight-loss, sleep, performance, etc.
  - **By application:** 1:1-sessions, group-workshops, self-guided, recordings
  - **By technique type:** inductions, deepeners, suggestions, metaphors, reframes
  - **By content type:** scripts, emails, sales-copy, training
- If an item applies to ALL domains, tag it `[domains: universal]`
- If domain is unclear, tag it `[domains: general]` and flag for user review

### Conflict Resolution
- If a framework is described differently across sources, include the most detailed version as primary
- Note variations in a "Variations" subsection
- If advice directly contradicts across sources, flag it explicitly

### Language Pattern Merging
- Combine all language patterns into one master table
- If the same phrase appears in multiple contexts, list all contexts
- Group related phrases together

## Output Format

ALWAYS use this exact template:

```markdown
# Master Pattern Library: [Subject/Person/Brand Name]

**Sources Processed:** [number] documents
**Date Built:** [YYYY-MM-DD]
**Source Files:**
- [list each source file]

---

## Executive Summary

[2-3 paragraphs summarizing the overall methodology, philosophy, and approach. This is the "if you only read one section" overview. Written in plain language, not bullet points.]

---

## Core Frameworks & Methodologies

### [Framework Name] [CORE/RECURRING/SINGLE] [domains: tag1, tag2]
- **What it is:** [consolidated description]
- **Steps/Components:**
  - [Step 1]
  - [Step 2]
- **When to use:** [consolidated contexts]
- **Key quotes:** "[best quote from across sources]"
- **Sources:** [which extraction docs contained this]
- **Variations:** [if described differently across sources, note here]

---

## Belief System & Principles

### [Principle] [CORE/RECURRING/SINGLE]
- **Statement:** [clearest articulation across all sources]
- **Why they hold this:** [consolidated reasoning]
- **Key quote:** "[best quote]"
- **Sources:** [list]

---

## Complete Language Reference

### Signature Phrases [CORE]

| Phrase | Typical Context | Frequency | Effect/Purpose |
|---|---|---|---|
| "[phrase]" | [when used] | [CORE/RECURRING/SINGLE] | [what it does] |

### Metaphors & Analogies

| Metaphor | What It Explains | Sources |
|---|---|---|
| "[metaphor]" | [concept] | [list] |

### Transition Patterns

| Pattern | Used Between | Example |
|---|---|---|
| "[pattern]" | [what sections/topics] | "[example from transcript]" |

---

## Story & Example Bank

### [Story Name] [CORE/RECURRING/SINGLE]
- **Setup:** [situation]
- **Key Point:** [lesson]
- **Outcome:** [result]
- **Best Telling:** "[best version quote across sources]"
- **Sources:** [list]

---

## Tactical Playbook

Organized by topic area, with frequency tags:

### [Topic Area]
- **[Tactic]** [CORE/RECURRING/SINGLE] — [description] — "[supporting quote]"

---

## Definitions & Distinctions Dictionary

| Term | Their Definition | Standard Definition | Frequency |
|---|---|---|---|
| [term] | [their def] | [common def] | [tag] |

---

## Objection & Reframe Library

| Objection | Reframe | Reasoning | Frequency |
|---|---|---|---|
| "[objection]" | "[reframe]" | [why] | [tag] |

---

## Pattern Frequency Summary

Quick reference of what matters most:

### CORE Elements (3+ sources)
- [list of all CORE items across all categories]

### RECURRING Elements (2 sources)
- [list]

### SINGLE Elements (1 source)
- [list]

---

## Domain Index

Quick reference for filtering by domain. Use this when generating content for a specific topic or use case.

### [Domain Name 1] (e.g., "anxiety")
- **Frameworks:** [list of framework names tagged with this domain]
- **Language Patterns:** [count] patterns tagged
- **Stories:** [list of story names tagged]
- **Tactics:** [count] tactics tagged

### [Domain Name 2] (e.g., "inductions")
- **Frameworks:** [list]
- **Language Patterns:** [count]
- **Stories:** [list]
- **Tactics:** [count]

### Universal (applies to all domains)
- **Frameworks:** [list]
- **Language Patterns:** [count]
- **Stories:** [list]
- **Tactics:** [count]

---

## Generation Guidelines

When using this pattern library to generate content (scripts, copy, emails, etc.):

1. **Prioritize CORE elements.** These define the methodology and should be present in most generated content.
2. **Use exact language patterns.** Don't paraphrase signature phrases. Use them as-is.
3. **Reference stories by name.** Pull from the Story Bank rather than making up examples.
4. **Match the belief system.** Generated content should never contradict the principles listed above.
5. **Use their definitions.** When a term is in the Definitions Dictionary, use THEIR definition, not the standard one.
```

## Important Guidelines

- **Read ALL extraction docs before starting.** Don't process them one at a time and append. Read everything first, then synthesize.
- **The Executive Summary is the most important section.** Write it like a briefing doc for someone who needs to understand this person's entire approach in 2 minutes.
- **Frequency tagging is non-negotiable.** Every single item needs a [CORE], [RECURRING], or [SINGLE] tag. This is how downstream skills know what to prioritize.
- **Preserve the quotes.** Always keep the best direct quotes. These are gold for voice matching.
- **The Generation Guidelines section matters.** This is what tells downstream AI tools how to USE the library. Don't skip it. Customize it based on what you've learned about the subject's style.

## File Handling

- Output file: `pattern-library.md`
- Default location: same directory as the source extraction files
- If a pattern-library.md already exists, confirm with user before overwriting
- If the user specifies an output path, use that
