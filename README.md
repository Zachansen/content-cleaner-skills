# Content Cleaner Skills for Claude Code

A set of four Claude Code skills that turn raw transcripts, PDFs, training materials, and other knowledge sources into structured, reusable pattern libraries for AI-powered content generation.

## What's Included

### Skills (the 4-step workflow)

1. **Source Preprocessor** - Classifies and cleans your raw files (transcripts, PDFs, scripts, training docs) before extraction
2. **Knowledge Extractor** - Pulls structured knowledge from each file: frameworks, beliefs, language patterns, stories, tactics, definitions, and objection handling
3. **Pattern Library Builder** - Merges multiple extraction docs into one master reference, deduplicates, and ranks by frequency
4. **Pattern-Based Generator** - Generates new content (scripts, emails, sales copy, training) rooted in YOUR pattern library instead of generic AI output

### Agents (parallel processing helpers)

Five sub-agents that handle specialized tasks in parallel:
- `file-cleaner` - Cleans individual files based on their classification
- `transcript-extractor` - Extracts knowledge from cleaned transcripts
- `script-analyzer` - Reverse-engineers the structure of finished scripts
- `training-extractor` - Extracts frameworks and curriculum from training materials
- `quality-checker` - Validates extraction quality before it hits the pattern library

## Installation

### Option 1: Paste this into Claude Code

```
Clone the content cleaner skills repo and install them globally in Claude Code:

gh repo clone zachhansen/content-cleaner-skills ~/.claude/skills/content-cleaner-skills

Then add all four skills to my global Claude Code settings by updating ~/.claude/settings.json. Add these paths to the "skills" array:

- ~/.claude/skills/content-cleaner-skills/source-preprocessor
- ~/.claude/skills/content-cleaner-skills/knowledge-extractor
- ~/.claude/skills/content-cleaner-skills/pattern-library-builder
- ~/.claude/skills/content-cleaner-skills/pattern-based-generator

Verify the install by checking that settings.json was updated correctly.
```

### Option 2: Manual install

```bash
gh repo clone zachhansen/content-cleaner-skills ~/.claude/skills/content-cleaner-skills
```

Then add the skill paths to your `~/.claude/settings.json`:

```json
{
  "skills": [
    "~/.claude/skills/content-cleaner-skills/source-preprocessor",
    "~/.claude/skills/content-cleaner-skills/knowledge-extractor",
    "~/.claude/skills/content-cleaner-skills/pattern-library-builder",
    "~/.claude/skills/content-cleaner-skills/pattern-based-generator"
  ]
}
```

## How to Use

### The Full Workflow

1. Put your raw files (transcripts, PDFs, scripts) in a folder
2. Tell Claude Code: **"Preprocess the files in [folder]"** (triggers source-preprocessor)
3. Tell Claude Code: **"Extract knowledge from the preprocessed files"** (triggers knowledge-extractor)
4. Tell Claude Code: **"Build the pattern library"** (triggers pattern-library-builder)
5. Tell Claude Code: **"Generate a [script/email/sales page] about [topic]"** (triggers pattern-based-generator)

### Quick Start

Drop a folder of transcripts and say:

> Process everything in this folder, extract the knowledge, build the pattern library, then generate a sales email about [topic].

Claude Code will run through all four steps.

## Requirements

- Claude Code (Desktop app, CLI, or Web)
- GitHub CLI (`gh`) for cloning
