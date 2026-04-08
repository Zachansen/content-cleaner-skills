---
name: source-preprocessor
description: Classify and prepare source files (transcripts, PDFs, existing scripts, guides, training materials) for knowledge extraction. Use this skill BEFORE running the knowledge-extractor when the user has a mixed collection of file types, drops a folder of various documents, or says things like "process everything in this folder", "I've got a bunch of files to go through", "prepare these for extraction", or "classify these files". Also trigger when the user has PDFs, SRT files, existing scripts, or mixed content that needs different handling before extraction.
---

# Source Preprocessor

Classify, clean, and route source files to the correct extraction approach before running the knowledge-extractor skill.

## Why This Exists

Not all source material is the same. A raw transcript from a 90-minute workshop recording needs heavy extraction. An existing hypnosis script IS the knowledge — it needs structural analysis, not content extraction. A PDF training manual needs different handling than a podcast transcript.

If you run the knowledge-extractor on everything the same way, you get garbage. This skill sorts files first, cleans them up, and routes each one to the right extraction approach.

## How It Works

### Step 1: Scan and Classify

When pointed at a directory, scan all files and classify each one into a category:

| Category | File Types / Indicators | What It Contains |
|---|---|---|
| **RAW_TRANSCRIPT** | .txt, .srt, .vtt files with timestamps, filler words, speaker labels | Spoken content from recordings. Messy, unstructured. |
| **FINISHED_SCRIPT** | Files containing structured scripts (hypnosis, coaching, sales). Look for stage directions, sections, technique markers. | The actual deliverable. Already structured knowledge. |
| **TRAINING_MATERIAL** | PDFs, docs with headers, sections, curriculum structure. Teaching content. | Organized educational content. |
| **REFERENCE_DOC** | Research papers, articles, external references, third-party content. | Supporting material, not original methodology. |
| **CLIENT_MATERIAL** | Intake forms, session notes, client feedback, testimonials. | Context about how the methodology is applied. |
| **UNKNOWN** | Anything that doesn't fit the above. | Needs human review. |

### Step 2: Present Classification

Show the user a table of all files with their assigned categories:

```
FILE CLASSIFICATION REPORT
==========================

📁 Source Directory: [path]
📄 Total Files: [count]

| # | Filename | Category | Size | Notes |
|---|----------|----------|------|-------|
| 1 | workshop-jan-2025.txt | RAW_TRANSCRIPT | 45KB | Contains speaker labels, timestamps |
| 2 | anxiety-relief-v3.pdf | FINISHED_SCRIPT | 12KB | Structured hypnosis script |
| 3 | training-manual.pdf | TRAINING_MATERIAL | 2.1MB | 47 pages, has TOC |
| 4 | research-paper.pdf | REFERENCE_DOC | 890KB | Third-party content |
| 5 | mystery-file.docx | UNKNOWN | 34KB | Could not determine type |

⚠️ UNKNOWN files need your review. What are they?
```

Wait for user confirmation before proceeding.

### Step 3: Clean and Prepare

For each category, apply the appropriate cleaning:

#### RAW_TRANSCRIPT
- Strip timestamps (from .srt/.vtt files)
- Remove filler words (um, uh, like, you know) — but preserve these if they're part of a technique demonstration
- Consolidate speaker labels
- Fix obvious transcription errors where identifiable
- Split into sections if the transcript covers multiple distinct topics
- Save cleaned version as `[filename]-cleaned.txt`

#### FINISHED_SCRIPT
- Extract from PDF if needed (preserve formatting)
- Identify structural elements: sections, techniques, transitions, instructions
- Do NOT reformat — preserve the original structure
- Add metadata header noting what type of script it is
- Save as `[filename]-prepared.md`

#### TRAINING_MATERIAL
- Extract text from PDF
- Preserve heading hierarchy
- Identify and tag: concepts, exercises, examples, theory sections
- Note any images/diagrams that can't be extracted (flag for user)
- Save as `[filename]-prepared.md`

#### REFERENCE_DOC
- Extract text
- Add a header noting this is EXTERNAL/REFERENCE content (not original methodology)
- This distinction matters for the pattern library — reference material informs but shouldn't be treated as core methodology
- Save as `[filename]-reference.md`

#### CLIENT_MATERIAL
- Extract relevant content
- Anonymize any client-identifying information (names, specific details)
- Flag testimonials and outcomes separately — these are useful for the pattern library's Story Bank
- Save as `[filename]-client.md`

### Step 4: Generate Routing Instructions

Create a `_preprocessing-report.md` that tells the knowledge-extractor how to handle each file:

```markdown
# Preprocessing Report

**Date:** [YYYY-MM-DD]
**Source Directory:** [path]
**Files Processed:** [count]

## Extraction Routing

### Full Extraction (use knowledge-extractor as-is)
These files need the full 7-category extraction:
- [filename]-cleaned.txt (RAW_TRANSCRIPT)

### Structural Analysis (modified extraction)
These files are already structured. Extract PATTERNS, not content:
- [filename]-prepared.md (FINISHED_SCRIPT)

For FINISHED_SCRIPT files, the knowledge-extractor should focus on:
1. Script structure and pacing patterns
2. Specific language and phrases used
3. Technique sequencing (what comes after what)
4. Transition methods between sections
5. Opening and closing patterns
6. Embedded suggestions and language patterns

Skip these extraction categories for scripts:
- Core Beliefs & Principles (scripts demonstrate, they don't state beliefs)
- Tactical Advice (scripts ARE the tactic)

### Educational Content Extraction
These files need extraction focused on frameworks and definitions:
- [filename]-prepared.md (TRAINING_MATERIAL)

### Reference Only (do not extract as primary knowledge)
These files should inform but not be treated as core methodology:
- [filename]-reference.md (REFERENCE_DOC)

### Client Context (supplementary extraction)
These files provide application context and outcomes:
- [filename]-client.md (CLIENT_MATERIAL)

## Domain Tags Detected

Based on content analysis, the following domains were identified:
- [domain 1]: [which files relate]
- [domain 2]: [which files relate]

These tags should be applied during extraction for the pattern library builder.

## Flagged Issues

- [any files that need human review]
- [any content that couldn't be extracted (images, charts)]
- [any contradictions noticed across files]
```

## Domain Detection

While classifying files, also detect what DOMAINS the content covers. This is critical for the pattern library builder's domain tagging system.

Look for:
- **Topic clusters** — Do multiple files cover the same subject? (e.g., "anxiety," "confidence," "weight loss")
- **Application areas** — Are there distinct use cases? (e.g., "1:1 sessions," "group workshops," "self-guided")
- **Technique categories** — Are there named technique families? (e.g., "inductions," "deepeners," "suggestions," "metaphors")

Tag each file with its detected domains. If a file spans multiple domains, tag all of them.

## Important Guidelines

- **Don't over-clean transcripts.** The speaker's natural language, including pauses and repetition, can be meaningful. Only remove clear transcription artifacts.
- **Respect script structure.** Finished scripts are the most valuable source material. Don't reorganize them — analyze them as-is.
- **Flag, don't guess.** If you can't classify a file confidently, mark it UNKNOWN and ask. Wrong classification leads to wrong extraction.
- **Anonymize client data.** Always. No exceptions. Replace names with [Client A], [Client B], etc.
- **Note what's missing.** If a PDF has charts, diagrams, or images that contain important information but can't be extracted as text, flag them explicitly so the user knows to review manually.
- **Preserve file originals.** Always create new files (-cleaned, -prepared, -reference). Never modify the originals.

## File Handling

- All output files go in a `_preprocessed/` subdirectory within the source directory
- The preprocessing report goes in the same `_preprocessed/` directory
- Original files are never modified
- If `_preprocessed/` already exists, confirm with user before overwriting
