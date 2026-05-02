# Cowork Setup

> **Source:** [ruben.substack.com/p/claude-cowork](https://ruben.substack.com/p/claude-cowork) | Mar 5, 2026 ✅
> **Section:** [[How to AI - Index#📂 Free Claude Course (Beginner to Pro)]]

---

## The Claude Product Line
1. **Chat** → like ChatGPT, a chatbot
2. **Project** → Chat separated by individual projects
3. **Code** → revolution for developers
4. **Cowork** → like Code but for knowledge workers ← **focus here**
5. **Skills** → teach Claude repeatable workflows
6. **Connectors** → plug Claude into Slack, Gmail, Google Calendar etc.
7. **Plugins** → like Connectors but you build/upload them

## Install Steps
1. [claude.com/download](https://claude.com/download) → download desktop app
2. Pro account ($20/month)
3. Click **Cowork** tab (top, between Chat & Code)
4. Select a folder from your computer
5. Always: **Opus 4.6** + **Extended Thinking**

## Step 1: Build Your Folder Structure
Create a dedicated "CLAUDE COWORK" folder with 4 subfolders:

```
CLAUDE COWORK/
├── ABOUT ME/         ← your identity + writing rules (read-only)
│   ├── about-me.md
│   └── anti-ai-writing-style.md
├── TEMPLATES/        ← proven structures to reuse (read-only)
├── PROJECTS/         ← live work, one subfolder per project (read-only)
└── CLAUDE OUTPUTS/   ← where Claude delivers finished work (write)
```

**Key rule:** Cowork has real read/write access. Keep things tight and contained.

## Step 2: Create Core Files
- **about-me.md** — who you are, what you do, current priorities
- **anti-ai-writing-style.md** — rules for never sounding like AI

Markdown files (.md) are just plain text files with an .md extension. Claude reads them better than .txt.

**One great markdown file > 50 random uploads.**

## Step 3: Set Global Instructions
Settings → Cowork → Edit Global Instructions. Paste:

```markdown
# GLOBAL INSTRUCTIONS

## BEFORE EVERY TASK
1. Read `ABOUT ME/`. No task starts without reading both.
2. If the task relates to a project, read everything in the 
   matching `PROJECTS/` subfolder before proceeding.
3. If the task involves a content type that has a matching 
   pattern in `TEMPLATES/`, study that template's structure first.

## FOLDER PROTOCOL
### Read-only — never create, edit, or delete:
- `ABOUT ME/` → My identity and writing rules.
- `TEMPLATES/` → Proven structures to reuse as patterns.
- `PROJECTS/` → My briefs, references, and finished work.

### Write folder — only place you deliver work:
- `CLAUDE OUTPUTS/` → Everything you create goes here.

## NAMING CONVENTION
`project_content-type_v1.ext`
Examples: How-To-AI_Newsletter_v1.md, EasyGen_Deck_v1.pptx

## OPERATING RULES
- If the brief is unclear, use AskUserQuestion. Don't fill gaps 
  with generic filler.
- Don't over-explain. Deliver the work.
- Never delete files anywhere.
```

## Prompt Pattern
With this setup, prompts can be 10 words long and still produce quality work:

```
I want to write a cold DM on LinkedIn to a Chief of Staff 
who needs an AI workshop.

Start by using AskUserQuestion. Develop 5x completely 
different strategies.
```

Claude will generate a clickable form asking clarifying questions, then execute.

## Usage Warning
If using Cowork daily, the Pro plan ($20/month) burns fast. Consider Max ($100/month).

## Related
- [[Claude - Basics Best Features Skills]]
- [[How to Make Claude Write Like You]]
- [[Before You Use Claude Create This MD File]]
- [[Cowork Projects]]

#cowork #setup #folder-structure #global-instructions
