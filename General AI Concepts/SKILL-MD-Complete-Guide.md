# The Complete Guide to AI Agent Skills (SKILL.md)

**A plain-English reference for understanding, writing, and deploying AI agent skills**

---

## Table of Contents

1. [What Is a Skill?](#1-what-is-a-skill)
2. [Why Skills Exist](#2-why-skills-exist)
3. [What Is YAML? (The Frontmatter Crash Course)](#3-what-is-yaml-the-frontmatter-crash-course)
4. [Anatomy of a Skill Folder](#4-anatomy-of-a-skill-folder)
5. [The SKILL.md File — Line by Line](#5-the-skillmd-file--line-by-line)
6. [How the Three-Level Loading System Works](#6-how-the-three-level-loading-system-works)
7. [Where Skills Live (Platform Paths)](#7-where-skills-live-platform-paths)
8. [How Skills Get Triggered](#8-how-skills-get-triggered)
9. [The Description Field — The Single Most Important Thing](#9-the-description-field--the-single-most-important-thing)
10. [Writing Pattern Best Practices](#10-writing-pattern-best-practices)
11. [Bundled Resources: Scripts, References, and Assets](#11-bundled-resources-scripts-references-and-assets)
12. [Four Example Skills (Easy → Complex)](#12-four-example-skills-easy--complex)
13. [The `allowed-tools` Field](#13-the-allowed-tools-field)
14. [Testing and Iterating on Skills](#14-testing-and-iterating-on-skills)
15. [Description Optimization](#15-description-optimization)
16. [Sharing Skills with a Team](#16-sharing-skills-with-a-team)
17. [Debugging When a Skill Doesn't Trigger](#17-debugging-when-a-skill-doesnt-trigger)
18. [Security Considerations](#18-security-considerations)
19. [Skills vs. Slash Commands](#19-skills-vs-slash-commands)
20. [Cross-Platform Compatibility](#20-cross-platform-compatibility)
21. [Quick Reference Cheat Sheet](#21-quick-reference-cheat-sheet)

---

## 1. What Is a Skill?

A skill is **a set of instructions packaged in a folder** that teaches an AI agent how to perform a specific task consistently. Think of it like writing an onboarding guide for a new team member — instead of re-explaining your workflows and preferences in every conversation, you write them down once and the agent picks them up automatically whenever your request matches.

A skill is **not** a plugin. It's **not** a script you wire up to an API. It's a structured Markdown file (with optional supporting files) that the agent reads and follows when it decides the skill is relevant to what you're asking.

The key insight: **skills are guidance, not deterministic automation.** The AI model still decides whether and how to follow the instructions. But well-written skills dramatically increase consistency — instead of getting a different approach every time you ask the agent to "write a README," you get the same high-quality structure every time.

---

## 2. Why Skills Exist

Without skills, every conversation starts from zero. You have to re-explain:

- Your preferred formatting and structure
- The tools and commands you want used
- The order of operations for multi-step workflows
- Quality checks and edge cases to watch for

Skills solve this by letting you **write those instructions once** and have them automatically loaded whenever the agent detects a matching request. The result is:

- **Consistency** — the agent follows the same process every time
- **Efficiency** — no re-explaining, no copy-pasting old prompts
- **Shareability** — your team can use the same skills via version control
- **Portability** — the SKILL.md format is an open standard (published by Anthropic at agentskills.io in December 2025) that works across Claude Code, OpenAI Codex, and OpenClaw

---

## 3. What Is YAML? (The Frontmatter Crash Course)

Before we get into skills, you need to understand **YAML** because every SKILL.md file starts with a YAML "frontmatter" block. If you've never seen YAML before, here's the quick version.

### YAML in 60 Seconds

YAML (rhymes with "camel") stands for "YAML Ain't Markup Language." It's a simple way to write structured data that's easy for both humans and machines to read. It uses **indentation** and **colons** instead of curly braces or angle brackets.

Here's what YAML looks like:

```yaml
name: readme-writer
description: Creates README files for projects
version: 1.0.0
```

That's it. Each line is a **key: value** pair. The key is on the left of the colon, the value is on the right. No quotes needed for simple values (though you can add them).

### YAML Rules That Matter for Skills

1. **Indentation matters.** Use spaces, never tabs. Two spaces per level is standard.
2. **Colons separate keys from values.** Always put a space after the colon: `name: my-skill` (not `name:my-skill`).
3. **Strings with special characters need quotes.** If your value contains colons, hashtags, or other YAML-meaningful characters, wrap it in quotes.
4. **Multi-line strings** use the `>` (folded) or `|` (literal) characters.

### What Is "Frontmatter"?

Frontmatter is a block of YAML at the very top of a Markdown file, fenced by triple dashes (`---`). It **must** start on line 1 of the file. Here's the pattern:

```markdown
---
name: my-skill-name
description: What this skill does and when to use it.
---

# The Rest of My Markdown Document

Regular content goes here...
```

The `---` lines tell the parser: "everything between these is metadata, not content." The agent reads the frontmatter separately from the body. This separation is critical to how skills work (more on that in the loading system section).

### Common YAML Mistakes That Break Skills

| Mistake | What Happens |
|---|---|
| Frontmatter doesn't start on line 1 | Skill silently fails to load |
| Missing closing `---` | YAML parser chokes, skill won't load |
| Tabs instead of spaces | YAML parse error |
| No space after colon (`name:value`) | Parsed as a single string, not a key-value pair |
| Using `<` or `>` in frontmatter values | Can inject unintended instructions into the system prompt |

---

## 4. Anatomy of a Skill Folder

Every skill is a folder. The only required file is `SKILL.md`. Everything else is optional but becomes important as your skills grow in complexity.

```
your-skill-name/
├── SKILL.md              ← REQUIRED: instructions + YAML metadata
├── scripts/              ← Optional: executable code the agent runs
│   ├── validate.sh
│   └── generate.py
├── references/           ← Optional: docs loaded only when needed
│   ├── style-guide.md
│   └── api-patterns.md
└── assets/               ← Optional: templates, images, fonts
    └── report-template.html
```

### What Each Folder Is For

| Folder | Purpose | When It's Loaded |
|---|---|---|
| `SKILL.md` | The core instructions — tells the agent what to do, step by step | When the skill is triggered (Level 2) |
| `scripts/` | Code that the agent can execute (bash scripts, Python scripts, etc.) | On demand — can run without being read into context |
| `references/` | Supporting documentation (API docs, style guides, criteria checklists) | On demand — only read when the skill body references them |
| `assets/` | Static files used in output (templates, icons, fonts, images) | On demand — used when producing output |

### Naming Rules for the Skill Folder

The folder name becomes the skill's identifier. Follow these rules from the agentskills.io spec:

- Lowercase letters, numbers, and hyphens only
- Maximum 64 characters
- Must not start or end with a hyphen
- No consecutive hyphens (`my--skill` is invalid)

Good: `readme-writer`, `code-reviewer`, `git-commit-writer`
Bad: `README_Writer`, `My Cool Skill`, `--reviewer--`

---

## 5. The SKILL.md File — Line by Line

Here's a minimal but complete SKILL.md to dissect:

```markdown
---
name: readme-writer
description: Creates and writes professional README.md files for software
  projects. Use when user asks to "write a README", "create a readme",
  "document this project", or "generate project documentation".
---

# README Writer

## Overview

Generate a complete, professional README.md file and write it to disk.

## Step 1: Gather project context

Look for context in the codebase before asking the user:

```bash
ls -la
cat package.json 2>/dev/null || echo "No manifest found"
```

## Step 2: Write the README

Use this structure:

```
# Project Name
One clear sentence describing what this project does.

## Installation
Step-by-step setup.

## Usage
Show the most common use case.
```

## Step 3: Quality check

Before finishing, verify:
- No placeholder text remains
- Every command is accurate
- Prerequisites match what the project needs
```

### Breaking It Down

**Lines 1–5: The YAML Frontmatter**

- `name:` — The skill's identifier. Must match the folder name.
- `description:` — The trigger condition. This is what the agent reads to decide whether to activate the skill. More on this in Section 9.

**Lines 7+: The Markdown Body**

This is your "instruction manual." It's written in standard Markdown and can include:

- Headings to organize steps
- Code blocks with commands the agent should run
- Checklists for quality verification
- References to files in the `references/` or `scripts/` folders

The body uses the **imperative form** — "Look for context," "Write the README," "Verify that..." — because you're giving the agent instructions, not explaining a concept.

---

## 6. How the Three-Level Loading System Works

This is the most important architectural concept to understand. It explains why skills are efficient, why some skills don't trigger, and how to structure complex skills.

Skills use **progressive disclosure** — content is pulled into the AI's context window only as needed, in three levels:

### Level 1: Metadata (~100 tokens per skill) — Always Loaded

When a session starts, the agent reads **only** the `name` and `description` from every installed skill's YAML frontmatter. Nothing else. This compact listing goes into the system prompt so the agent knows what skills exist and when to use them.

**Why this matters:** You can install dozens of skills without any performance penalty. At ~100 tokens each, even 50 skills only cost ~5,000 tokens of context — a tiny fraction of the available window.

### Level 2: Instructions (< 5,000 tokens ideal) — Loaded When Triggered

When the agent decides a skill is relevant to your request, it reads the **full body** of SKILL.md into context. Only at this point do your actual instructions get loaded.

**Why this matters:** Keep your SKILL.md body under ~500 lines. If it's getting longer, move detailed content into reference files (Level 3).

### Level 3: Referenced Files — Loaded on Demand

If the skill body references other files (`references/`, `scripts/`, etc.), the agent reads those **only when it needs them.** Scripts can be executed without being read into context at all.

**Why this matters:** This is what makes skills scalable. You can bundle an entire API reference guide, a 500-line style guide, and a dozen scripts — and the token cost at idle is zero.

### The Loading Sequence in Action

Here's what happens when you say "Can you write a README for this project?":

```
1. Session starts
   → Agent loads: name + description from every skill (~100 tokens each)

2. User asks: "Can you write a README for this project?"
   → Agent matches the request to readme-writer's description
   → Agent reads: readme-writer/SKILL.md full body (Level 2)

3. SKILL.md says "see references/style.md for formatting rules"
   → Agent reads: readme-writer/references/style.md (Level 3)

4. SKILL.md says "run scripts/validate.sh to check the output"
   → Agent executes: scripts/validate.sh
   → (Script runs without being read into context)
```

---

## 7. Where Skills Live (Platform Paths)

Each platform loads skills from specific directories. The directory determines the **scope** — who can see and use the skill.

### Claude Code

| Location | Scope |
|---|---|
| `~/.claude/skills/` | Personal — available across all your projects |
| `.claude/skills/` | Project-level — shared with your team via git |

### OpenAI Codex

| Location | Scope |
|---|---|
| `~/.codex/skills/` | User-level — applies to any repo you work in |
| `.codex/skills/` | Repo-level — checked into git |

### OpenClaw

| Location | Scope |
|---|---|
| `~/.openclaw/skills/` | Global — available to all configured agents |
| Per-agent workspace | Scoped to a specific agent only |

### Claude.ai (Web/Mobile Interface)

In Claude.ai, skills live in specific mounted paths:

| Location | Scope |
|---|---|
| `/mnt/skills/public/` | Built-in skills provided by Anthropic (read-only) |
| `/mnt/skills/examples/` | Example skills for learning (read-only) |
| `/mnt/skills/user/` | User-uploaded custom skills (read-only, but available) |

### Precedence Rules

When two skills share the same name, the **higher-precedence location wins.** A project-level skill overrides a personal one. This lets teams define defaults that individuals can override for their own setups.

---

## 8. How Skills Get Triggered

There are two ways a skill gets activated:

### Implicit (Automatic) Triggering

The agent reads your request, scans its list of installed skill descriptions (Level 1 metadata), and decides that one or more skills are relevant. It then reads the full SKILL.md body and follows the instructions. This is the most common and most natural way skills work.

Example: You say "Review my code for any issues" → the agent matches this to a `code-reviewer` skill and activates it automatically.

### Explicit (Manual) Triggering

You directly tell the agent to use a specific skill.

- **Claude Code:** `/readme-writer` or "Use the readme-writer skill"
- **Codex CLI:** `$readme-writer document this project` or `/skills` to browse
- **Claude.ai:** "Use the readme-writer skill to document this project"

### Important: Skills Don't Guarantee Execution

The model still decides whether to follow the instructions. Skills are structured guidance that dramatically increases consistency — not deterministic automation. If the agent goes off-script, the fix is almost always improving the instructions or the description, not debugging runtime behavior.

Also worth knowing: **the agent only consults skills for tasks it can't easily handle on its own.** Simple, one-step queries like "read this file" may not trigger a skill even if the description matches, because the agent can handle them directly. Complex, multi-step, or specialized queries are where skills reliably trigger.

---

## 9. The Description Field — The Single Most Important Thing

> "If your skill does not trigger, it is almost never the instructions. It is the description."

The `description` field in your YAML frontmatter is **not for humans.** It is the trigger condition the agent uses when deciding whether to activate your skill. Think of it as a search query the agent runs against your skill when it's trying to figure out what to do with the user's request.

### The Formula That Works

```
[What the skill does] + [When to use it, with specific trigger phrases]
```

### Bad Descriptions

```yaml
# Too vague — matches everything, triggers on nothing
description: Helps with documents.

# Describes what, but not when — agent has no trigger phrases to match
description: Creates sophisticated multi-page documentation with advanced formatting.
```

### Good Descriptions

```yaml
# Clear what + explicit when with real trigger phrases
description: Creates and writes professional README.md files for software
  projects. Use when user asks to "write a README", "create a readme",
  "document this project", "generate project documentation", or "help me
  write a README.md".
```

```yaml
# From the real docx skill — notice how specific and "pushy" it is
description: "Use this skill whenever the user wants to create, read, edit,
  or manipulate Word documents (.docx files). Triggers include: any mention
  of 'Word doc', 'word document', '.docx', or requests to produce
  professional documents with formatting like tables of contents, headings,
  page numbers, or letterheads. Also use when extracting or reorganizing
  content from .docx files. Do NOT use for PDFs, spreadsheets, Google Docs,
  or general coding tasks unrelated to document generation."
```

### Pro Tips for Descriptions

1. **Be "pushy."** Agents tend to undertrigger — they err on the side of not using skills. Combat this by being explicit and inclusive. Say "Use this skill whenever..." and "even if they don't explicitly ask for..."
2. **Include negative triggers.** Say what the skill is NOT for: "Do NOT use for PDFs or spreadsheets." This helps the agent distinguish between similar skills.
3. **Use the user's language.** Include the casual phrases real people actually type: "help me commit," "check my PR," "look over this function."
4. **Max 1,024 characters.** That's the spec limit. Use them wisely.
5. **Avoid `<` and `>` in descriptions.** They can inject unintended instructions into the system prompt.

---

## 10. Writing Pattern Best Practices

These patterns come from Anthropic's internal skill-creator documentation and from production skills that have been tested at scale.

### Use Imperative Form

Write instructions as commands, not explanations.

```markdown
<!-- Good -->
## Step 1: Gather project context
Look for context in the codebase before asking the user.

<!-- Bad -->
## Step 1: Gathering project context
The first thing you should probably do is look for context...
```

### Define Output Formats Explicitly

Show the agent exactly what structure you want:

```markdown
## Report Structure
ALWAYS use this exact template:

# [Title]
## Executive Summary
## Key Findings
## Recommendations
```

### Include Examples

Examples are one of the most powerful tools in skill writing. Format them clearly:

```markdown
## Commit Message Format

**Example 1:**
Input: Added user authentication with JWT tokens
Output: feat(auth): implement JWT-based authentication

**Example 2:**
Input: Fixed the login page crashing on mobile
Output: fix(ui): resolve mobile crash on login page
```

### Explain the "Why," Not Just the "What"

Modern AI models are smart. They respond much better to reasoning than to rigid rules.

```markdown
<!-- Good — explains why -->
Keep the subject line under 72 characters. Git truncates longer
lines in log views, which makes commit history harder to scan.

<!-- Bad — just a rule -->
ALWAYS keep subject lines under 72 characters. NEVER exceed this.
```

### Use Checklists for Quality Verification

End each skill with a quality check section:

```markdown
## Quality Check

Before finishing, verify:
- [ ] No placeholder text like "[your description here]" remains
- [ ] Every command in Installation is accurate for this project
- [ ] Prerequisites match what the project actually needs
- [ ] License section matches the LICENSE file if one exists
```

### Keep SKILL.md Under 500 Lines

If your instructions are getting long, split content into reference files. The SKILL.md body should be the **process** — the step-by-step workflow. Detailed criteria, API docs, and templates belong in `references/`.

---

## 11. Bundled Resources: Scripts, References, and Assets

When a skill grows beyond a simple instruction set, you split it across multiple files. Here's how each folder works.

### `scripts/` — Executable Code

Scripts contain code the agent runs during execution. They're useful for deterministic or repetitive tasks that are more reliable as code than as LLM reasoning.

```
scripts/
├── validate.sh        # Check output quality
├── generate_chart.py  # Create visualizations
└── setup.sh           # Install dependencies
```

Key behavior: **scripts can execute without being read into context.** The agent runs them and gets the output, but the script's source code doesn't consume context window tokens.

When to use scripts:
- Validation checks (does the output meet criteria?)
- Data processing (parse a file, transform data)
- Repetitive tasks (create the same boilerplate every time)

### `references/` — Supporting Documentation

Reference files contain detailed information that the skill body points to. They're only loaded when the agent needs them.

```
references/
├── criteria.md        # Detailed review criteria
├── api-patterns.md    # API usage patterns and pagination
└── error-handling.md  # What to do when things fail
```

Reference from SKILL.md with clear guidance on when to read:

```markdown
For detailed review criteria by category,
see [references/criteria.md](references/criteria.md).
```

When to use references:
- Detailed criteria too long for the main skill body
- API documentation the agent needs for external integrations
- Error handling guides
- Domain-specific knowledge (e.g., separate files for AWS vs. GCP vs. Azure)

### `assets/` — Static Files

Assets are files used in the skill's output — templates, icons, fonts, images, etc.

```
assets/
├── report-template.html
├── company-logo.png
└── eval_review.html
```

---

## 12. Four Example Skills (Easy → Complex)

These four skills illustrate the progression from simple to complex, showing when to introduce each pattern.

### Skill 1: README Writer (Basic — Single File)

The simplest useful skill. One SKILL.md, no extra files. It gathers context from the codebase, writes a README in a consistent structure, saves it to disk, and runs a quality check.

```
readme-writer/
└── SKILL.md
```

```markdown
---
name: readme-writer
description: Creates and writes professional README.md files for software
  projects. Use when user asks to "write a README", "create a readme",
  "document this project", "generate project documentation", or "help me
  write a README.md". Works from a project description, existing code,
  or both.
---

# README Writer

## Overview
Generate a complete, professional README.md file and write it to disk.

## Step 1: Gather project context
Look for context in the codebase before asking the user:
```bash
ls -la
cat package.json 2>/dev/null || cat pyproject.toml 2>/dev/null || \
  cat go.mod 2>/dev/null || echo "No manifest found"
```

Gather:
- What does this project do? (1-2 sentence summary)
- What language and main frameworks does it use?
- How do you install and run it?

## Step 2: Write the README
Use this structure. Only include sections that are relevant.

## Step 3: Write to disk
```bash
cat > README.md << 'EOF'
[full readme content]
EOF
echo "README.md written: $(wc -l < README.md) lines"
```

## Step 4: Quality check
- [ ] No placeholder text remains
- [ ] Every command is accurate
- [ ] Prerequisites match the project
```

**When to use this pattern:** Simple, single-purpose skills that can fit comfortably in one file under 100 lines.

---

### Skill 2: Git Commit Writer (Trigger Phrase Coverage)

This skill demonstrates how to write trigger phrases that cover the different ways people ask for the same thing.

```
git-commit-writer/
└── SKILL.md
```

Notice the description includes: "write a commit message," "help me commit," "summarize my changes," "what should my commit say," and "draft a commit." These are all different ways a real person might phrase the same request.

**When to use this pattern:** When the same task gets requested in many different phrasings.

---

### Skill 3: Code Reviewer (Multi-File Split)

This skill shows **when to split content across multiple files.** The process stays in SKILL.md (~40 lines). The detailed review criteria live in a reference file loaded only during an actual review.

```
code-reviewer/
├── SKILL.md                    ← The process (under 40 lines)
└── references/
    └── criteria.md             ← Detailed criteria (loaded on demand)
```

The SKILL.md body says:

```markdown
For detailed review criteria by category,
see [references/criteria.md](references/criteria.md).
```

This keeps Level 2 lean (fast to load, minimal context cost) while the agent still has access to everything it needs at Level 3 (only loaded when actually reviewing code).

**When to use this pattern:** When your instructions exceed ~100 lines, or when detailed reference material would bloat the main file.

---

### Skill 4: Linear Sprint Planner (MCP Integration)

This is the most complex pattern — a skill that coordinates with an external API via MCP (Model Context Protocol). The MCP server gives the agent access to Linear's API. The skill gives it the knowledge of **how to use that access** reliably.

```
linear-sprint-planner/
├── SKILL.md
└── references/
    ├── linear-api.md           ← Pagination, rate limits, etc.
    └── error-handling.md       ← What to do when API calls fail
```

The SKILL.md includes a `metadata` field pointing to the required MCP server:

```yaml
metadata:
  mcp-server: linear
  version: 1.0.0
```

The skill checks prerequisites (is the MCP server connected?), fetches live data, proposes a plan, **waits for user approval**, and then executes.

**When to use this pattern:** When your skill needs to coordinate with external services, APIs, or MCP servers.

---

## 13. The `allowed-tools` Field

An optional frontmatter field that restricts which tools the agent can call when a skill is active. Useful for **read-only skills** where you don't want the agent accidentally writing or executing anything.

```yaml
---
name: log-analyzer
description: Analyzes application log files to identify errors and
  patterns. Use when user says "check the logs", "what errors are in
  my logs", or "analyze this log file".
allowed-tools: Read, Grep, Glob
---
```

With `allowed-tools` set, the agent cannot execute shell commands, write files, or make external calls while this skill is active.

**Note:** This field is marked experimental in the Agent Skills spec. It's well-supported in Claude Code today but support varies across platforms.

---

## 14. Testing and Iterating on Skills

Writing a skill is only step one. The real work is testing it against realistic prompts and refining based on results.

### The Core Loop

1. **Write a draft** of the skill
2. **Create 2–3 realistic test prompts** — things a real user would actually say
3. **Run the agent with your skill** on those prompts
4. **Evaluate the results** — does the output match what you wanted?
5. **Revise the skill** based on what went wrong
6. **Repeat** until you're satisfied
7. **Expand the test set** and try again at larger scale

### Writing Good Test Prompts

Bad test prompts are obvious and simple. Good test prompts are realistic, messy, and specific — like what a real person would actually type.

**Bad:** `"Format this data"`
**Good:** `"ok so my boss just sent me this xlsx file (its in my downloads, called something like 'Q4 sales final FINAL v2.xlsx') and she wants me to add a column that shows the profit margin as a percentage"`

### How to Think About Improvements

1. **Generalize from feedback.** Don't overfit to your test cases. You're building something that should work for many different inputs.
2. **Keep the prompt lean.** Remove instructions that aren't pulling their weight.
3. **Explain the why.** If you find yourself writing ALWAYS or NEVER in all caps, reframe it as reasoning the model can understand.
4. **Look for repeated work.** If every test run independently creates the same helper script, bundle that script into `scripts/`.

---

## 15. Description Optimization

After building your skill, you can systematically optimize the description for better triggering accuracy.

### The Process

1. **Create 20 eval queries** — a mix of 8–10 "should trigger" and 8–10 "should not trigger" examples
2. **Make should-trigger queries diverse** — different phrasings, casual and formal, explicit and implicit
3. **Make should-not-trigger queries tricky** — near-misses that share keywords but need a different tool, not obviously irrelevant queries
4. **Run the optimization loop** — test whether the current description correctly activates (or doesn't activate) for each query
5. **Refine the description** based on what failed

### Example Eval Queries

**Should trigger (for a spreadsheet skill):**
```
"ok so my boss just sent me this xlsx file and she wants me to add
a column that shows the profit margin as a percentage"
```

**Should NOT trigger (near-miss):**
```
"can you summarize this PDF report about Q4 sales for me?"
```

The near-miss shares keywords ("Q4 sales") but needs a different skill (PDF reader, not spreadsheet editor). This is a good negative test case.

---

## 16. Sharing Skills with a Team

The cleanest approach is to commit project-level skills to your repo:

```bash
mkdir -p .claude/skills/readme-writer
# add SKILL.md and any supporting files, then:
git add .claude/skills/
git commit -m "Add readme-writer skill for team"
git push
```

When teammates pull the repo, the skill is immediately available — no separate installation step. In Codex the equivalent path is `.codex/skills/`.

Skills can also be packaged as `.skill` files (ZIP archives) for distribution outside of git repos. The `package_skill.py` script in the skill-creator tooling handles this.

---

## 17. Debugging When a Skill Doesn't Trigger

If your skill isn't activating, work through this checklist in order:

### 1. Check Your Description
The most common issue. Add more specific trigger phrases that match how users actually phrase requests. Make it "pushier."

### 2. Check Your File Path
Make sure SKILL.md is in the right location:
```bash
# Claude Code personal skill
ls ~/.claude/skills/your-skill/SKILL.md
# Claude Code project skill
ls .claude/skills/your-skill/SKILL.md
```

### 3. Check YAML Syntax
Invalid YAML silently prevents loading. Verify:
- Frontmatter starts on line 1 with `---`
- Frontmatter closes with another `---`
- No tabs (spaces only)
- Space after every colon

### 4. Restart the Session
Skills are snapshotted at session start. Edits made during a running session require a restart.

### 5. Run in Debug Mode
```bash
claude --debug
```

### 6. Test with Explicit Invocation
```
Use the readme-writer skill to document this project
```
If it works explicitly but not automatically, the description needs more trigger phrases.

### 7. Check Query Complexity
Remember: the agent only consults skills for tasks it can't easily handle on its own. If your test query is too simple ("read this file"), the agent may handle it directly without consulting any skill, regardless of description quality.

---

## 18. Security Considerations

Skills can bundle executable code and instructions that control agent behavior. That same power makes malicious skills dangerous.

### Practical Rules

1. **Never install a skill that asks you to paste secrets into chat**
2. **Always read `scripts/` before installing** any community skill
3. **Be skeptical of skills that make outbound network calls** in setup instructions
4. **Prefer skills from official sources** — Anthropic's or OpenAI's skills repos, or your own team's
5. **Read every file in the folder** before installing, especially anything in `scripts/`

Security researchers have found community skills with credential theft and malware. Manual review is always worthwhile for anything with broad permissions.

---

## 19. Skills vs. Slash Commands

Both Claude Code and Codex support two invocation modes:

| Mode | How It Works | When to Use |
|---|---|---|
| **Implicit** | Agent automatically detects relevance from description | Default — makes skills feel natural |
| **Explicit** | User types `/skill-name` or `$skill-name` | When you want to force a specific skill |

Even though explicit invocation exists, a well-written description is what drives automatic activation — the behavior that makes skills feel like a natural extension of how you already work rather than a command you have to remember.

---

## 20. Cross-Platform Compatibility

The SKILL.md format is an **open standard**. Skills you write today are portable across Claude Code, OpenAI Codex, and OpenClaw.

However, while the format is standardized, each platform implements discovery and tooling slightly differently:

| Feature | Claude Code | OpenAI Codex | OpenClaw |
|---|---|---|---|
| Skill format | SKILL.md ✅ | SKILL.md ✅ | SKILL.md ✅ |
| Discovery | Automatic + `/skill-name` | Automatic + `$skill-name` | Automatic |
| `allowed-tools` | Well-supported | Varies | Varies |
| MCP integration | Full support | Limited | Full support |
| Session snapshotting | Platform-specific | Platform-specific | Platform-specific |

Think of it as a **shared language, not identical behavior.** A skill that works on Claude Code will very likely work on Codex, but runtime behaviors like tool permissions and invocation modes differ.

---

## 21. Quick Reference Cheat Sheet

### Minimum Viable Skill

```markdown
---
name: my-skill
description: Does X. Use when user asks to "do X", "help me X",
  or "create X". Works with Y and Z inputs.
---

# My Skill

## Step 1: [First action]
[Instructions]

## Step 2: [Second action]
[Instructions]

## Quality Check
- [ ] [Verification item]
```

### YAML Frontmatter Fields

| Field | Required? | Purpose |
|---|---|---|
| `name` | Yes | Skill identifier (lowercase, hyphens, max 64 chars) |
| `description` | Yes | Trigger condition (max 1,024 chars) |
| `allowed-tools` | No | Restrict available tools (experimental) |
| `metadata` | No | Additional config (e.g., MCP server requirements) |
| `license` | No | License information |
| `version` | No | Skill version string |

### Description Formula

```
[What it does] + [When to use it with specific trigger phrases] + [When NOT to use it]
```

### File Size Guidelines

| Component | Target | Maximum |
|---|---|---|
| SKILL.md body | Under 200 lines | 500 lines |
| Reference files | Under 300 lines each | Add TOC if longer |
| Description | As specific as possible | 1,024 characters |

### The Loading Pyramid

```
Level 1: name + description     ← Always loaded (~100 tokens)
Level 2: SKILL.md body          ← Loaded on trigger (< 5k tokens)
Level 3: references + scripts   ← Loaded on demand (unlimited)
```

---

## Further Reading

- **Agent Skills Open Standard:** agentskills.io
- **Anthropic Skills Repo:** github.com/anthropics/skills
- **OpenAI Skills Repo:** github.com/openai/skills
- **Claude Code Skills Guide:** code.claude.com/docs/en/skills
- **Anthropic Agent Skills Overview:** platform.claude.com/docs/en/agents-and-tools/agent-skills/overview

---

*Guide compiled from the agentskills.io specification, Anthropic's internal skill-creator documentation, the SKILL.md pattern article by Bibek Poudel, and production skill examples from the Claude platform.*
