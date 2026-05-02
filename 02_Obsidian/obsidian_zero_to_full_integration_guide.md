**Obsidian from Zero to Full Integration**

*Beginner setup guide for iPhone now, Windows + WSL later, and Mac mini OpenClaw integration*

| **Best overall design**    | One Obsidian vault as your knowledge/control plane; Claude Code works in real repos; OpenClaw stays live on the Mac mini.             |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| **What this guide covers** | iPhone-first setup, later Windows sync, folder structures, plugin choices, integration patterns, and ready-to-use Markdown templates. |

This version assumes you are new to Obsidian. Start simple, get the foundation working, and add automation only after the vault structure feels natural.

# 1. The architecture at a glance

Think of the system as three layers, not one giant app.

| **Layer** | **Primary home**     | **What belongs there**                                                                                     |
|-----------|----------------------|------------------------------------------------------------------------------------------------------------|
| Knowledge | Obsidian vault       | Dashboards, notes, SOPs, runbooks, prompt libraries, repo summaries, learning notes, troubleshooting logs. |
| Execution | Claude Code repos    | Real code, scripts, tests, CLAUDE.md, repo-local docs, project settings.                                   |
| Runtime   | OpenClaw on Mac mini | Live config, skills, prompts, scripts, connectors, service-specific docs, deployment state.                |

| **Why this matters.** Obsidian should document and coordinate the system. It should not become the live runtime folder for Claude Code or OpenClaw. |
|-----------------------------------------------------------------------------------------------------------------------------------------------------|

# 2. Choose your sync path before you start

For your exact situation—iPhone now, Windows later—the cleanest choice is Obsidian Sync. The practical free fallback is iCloud Drive now plus iCloud for Windows later.

| **Option**    | **Cost** | **Best for**                              | **Pros**                                        | **Tradeoffs**                             |
|---------------|----------|-------------------------------------------|-------------------------------------------------|-------------------------------------------|
| Obsidian Sync | Paid     | Smoothest iPhone → Windows setup          | Official, simple, vault-specific, easiest later | Subscription cost                         |
| iCloud Drive  | Low/Free | Apple-first start with a Windows fallback | Easy to start on iPhone                         | Windows path is workable but less elegant |

Recommendation: use Obsidian Sync if you want the least friction. Use iCloud Drive only if you strongly prefer a lower-cost path and accept extra setup later.

# 3. Phase 1: set up the vault today on your iPhone

1.  Install Obsidian on your iPhone.

2.  Create a new vault named Dara-OS.

3.  If using Obsidian Sync, sign in and create a remote vault. If using iCloud Drive, place the vault in iCloud Drive/Obsidian/Dara-OS.

4.  Create the folder structure shown in Section 4.

5.  Turn on only the core plugins you need first: Daily notes, Templates, Backlinks, Outgoing links, Bookmarks, and File recovery.

6.  Set new notes to land in 00 Inbox and attachments to land in 08 Assets.

7.  Create the starter notes listed in Section 5 and begin capturing everything into 00 Inbox.

# 4. The exact Obsidian vault folder structure

Create this structure exactly first. You can rename later after you have lived in it for a couple of weeks.


```
Dara-OS/
00 Inbox/
01 Home/
02 Personal/
Journal/
Health/
Goals/
Finance/
Relationships/
Admin/
03 Learning/
Books/
Courses/
Research/
Concepts/
Tutorials/
Source Notes/
04 Projects/
Personal Projects/
Business Ideas/
Build Logs/
Experiments/
05 AI Systems/
Claude Code/
Workflows/
Prompt Library/
Repo Notes/
Troubleshooting/
Session Logs/
OpenClaw/
Architecture/
Config Map/
Skills Catalog/
Prompt Library/
Runbooks/
Troubleshooting/
Session Logs/
Integrations/
URI/
REST API/
Automation Backlog/
VS Code/
Setup/
Remote Workflow/
WSL Notes/
06 SOPs/
07 Templates/
08 Assets/
Images/
PDFs/
Screenshots/
99 Archive/
```


# 5. The first notes to create

| **Note**            | **Where to place it**                | **Purpose**                                                        |
|---------------------|--------------------------------------|--------------------------------------------------------------------|
| Home                | 01 Home                              | Top navigation page for the whole vault                            |
| Personal Home       | 01 Home                              | Journal, health, goals, finance, admin overview                    |
| Learning Home       | 01 Home                              | Books, courses, concepts, active learning queue                    |
| AI Systems Home     | 01 Home                              | Landing page for Claude Code, OpenClaw, and VS Code workflow notes |
| Projects Home       | 01 Home                              | Project dashboards and active initiatives                          |
| Claude Code Home    | 05 AI Systems/Claude Code            | Overview of Claude workflows, repos, and session notes             |
| OpenClaw Home       | 05 AI Systems/OpenClaw               | Overview of OpenClaw architecture, runbooks, and issues            |
| WSL Repo Index      | 05 AI Systems/Claude Code/Repo Notes | List of WSL repos and links to repo notes                          |
| Mac Mini Repo Index | 05 AI Systems/OpenClaw/Config Map    | List of OpenClaw repos on the Mac mini                             |

# 6. Phase 2: what gets added later on Windows

When you reach your Windows machine, keep the vault on Windows and keep your actual Claude Code repos in the WSL filesystem.


```
Windows
C:\Users\YourName\Documents\Obsidian\Dara-OS\
WSL
/home/yourname/projects/
claude-playground/
web-apps/
scripts/
ai-experiments/
Mac mini
/Users/yourname/projects/openclaw/
openclaw-config/
openclaw-skills/
openclaw-sandbox/
```


| **Important WSL rule.** Do not keep your main Linux development repos under C:\Users\\..\Projects if you plan to work from WSL. Keep them under /home/yourname/projects for better compatibility and performance. |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

# 7. Folder structures for each system

## 7.1 Windows Obsidian vault


> C:\Users\YourName\Documents\Obsidian\Dara-OS\
> 
> 00 Inbox\
> 
> 01 Home\
> 
> 02 Personal\
> 
> 03 Learning\
> 
> 04 Projects\
> 
> 05 AI Systems\
> 
> 06 SOPs\
> 
> 07 Templates\
> 
> 08 Assets\
> 
> 99 Archive\


## 7.2 WSL Claude Code repos


```
/home/yourname/projects/
claude-playground/
CLAUDE.md
README.md
docs/
architecture.md
setup.md
troubleshooting.md
prompts/
scripts/
experiments/
web-apps/
parcel-zoning-viewer/
CLAUDE.md
README.md
docs/
src/
public/
scripts/
automation-tools/
CLAUDE.md
README.md
docs/
scripts/
```


## 7.3 Mac mini OpenClaw repos


```
/Users/yourname/projects/openclaw/
openclaw-config/
CLAUDE.md
README.md
docs/
architecture.md
setup.md
troubleshooting.md
deployment.md
config/
prompts/
skills/
scripts/
tests/
openclaw-skills/
CLAUDE.md
docs/
skills/
tests/
openclaw-sandbox/
CLAUDE.md
docs/
experiments/
```


# 8. How the integrations actually work

There is no single official one-click integration that turns Obsidian into Claude Code or OpenClaw. The durable approach is to connect them through three simple layers.

| **Layer** | **Tool**              | **Use it for**                        | **When to add it**             |
|-----------|-----------------------|---------------------------------------|--------------------------------|
| 1         | Shared Markdown       | Repo docs plus Obsidian notes         | Immediately                    |
| 2         | Obsidian URI          | Open or create notes from scripts     | After vault basics feel stable |
| 3         | Local REST API plugin | Read and write notes programmatically | After desktop setup is working |

## 8.1 Integration path A: shared Markdown (start here)

Every real repo should have a small docs folder and a CLAUDE.md file. Obsidian then stores the higher-level explanation, the dashboard, the runbook, and the cross-links.

- Repo keeps operational truth: code, config, CLAUDE.md, tests, scripts, repo-local docs.

- Obsidian keeps system truth: what matters, how parts connect, what broke, and what to do next.

## 8.2 Integration path B: Obsidian URI

Obsidian supports a custom URI scheme that can open a note or create/add to a note. This is a lightweight way to let external scripts jump into the vault.


```
obsidian://open?vault=Dara-OS&file=01%20Home%2FAI%20Systems%20Home
obsidian://new?vault=Dara-OS&file=05%20AI%20Systems%2FClaude%20Code%2FSession%20Logs%2F2026-03-30%20Session%20Log&content=Session%20summary
```


Use URI integration first for simple tasks like opening the correct dashboard note or appending a session summary.

## 8.3 Integration path C: Local REST API plugin

Once your desktop setup is stable, install the Local REST API plugin in Obsidian. This lets trusted local scripts read and write notes in your vault.


```
curl -k https://127.0.0.1:27124/
curl -k -H "Authorization: Bearer <your-api-key>" \
https://127.0.0.1:27124/vault/
curl -k -H "Authorization: Bearer <your-api-key>" \
https://127.0.0.1:27124/vault/05%20AI%20Systems/Claude%20Code/Claude%20Code%20Home.md
```


Use the REST API for higher-value automation: writing a dated session log, updating a troubleshooting page, or reading a runbook before a script makes changes.

# 9. The exact workflow from state 0 to full integration

## Stage 0 — Today on iPhone

8.  Create the Dara-OS vault.

9.  Build the folder structure.

10. Turn on only the core plugins listed earlier.

11. Create the starter notes and templates.

12. Capture all raw ideas in 00 Inbox. Do not over-organize on mobile.

## Stage 1 — First week of usage

13. Use Home and your daily note every day.

14. At the end of each day, process 00 Inbox into the correct folders.

15. Start building repo notes, runbooks, and learning notes even before the code repos exist.

16. Do not add community plugins yet unless you truly need them.

## Stage 2 — Add Windows desktop

17. Install Obsidian on Windows and connect the existing vault.

18. Create the same Dara-OS vault locally or sync it down with Obsidian Sync/iCloud.

19. Verify the folder structure, starter notes, and templates.

20. Install VS Code on Windows.

21. Install the WSL extension in VS Code.

## Stage 3 — Add WSL repos for Claude Code

22. Create /home/yourname/projects in WSL.

23. Create your first repo, such as claude-playground.

24. Inside that repo add CLAUDE.md, README.md, and docs/setup.md.

25. Create a matching repo note in Obsidian using the Repo Note Template.

26. Link the repo note from WSL Repo Index and AI Systems Home.

## Stage 4 — Add Mac mini OpenClaw management

27. Use VS Code Remote SSH from Windows to the Mac mini.

28. Create or confirm the OpenClaw repo structure under /Users/yourname/projects/openclaw.

29. Add CLAUDE.md and docs files to each OpenClaw repo.

30. Create matching Obsidian notes: Config Map, Skills Catalog, Runbooks, Troubleshooting.

31. Document every real operational task as a runbook.

## Stage 5 — Add automation

32. Start with Obsidian URI links for easy open/create flows.

33. After that, install Local REST API on desktop.

34. Add one simple automation first: write a Claude session summary to Obsidian.

35. Add one OpenClaw automation second: append a note when a deployment or troubleshooting event happens.

36. Only after these work should you add more advanced automation.

# 10. What a normal day should look like

| **Time**                      | **Where you work**           | **What you do**                                                                       |
|-------------------------------|------------------------------|---------------------------------------------------------------------------------------|
| Morning                       | Obsidian                     | Open Home and today’s daily note. Review top priorities and active projects.          |
| Build work                    | VS Code in WSL or Remote SSH | Do real code/config work in the repo where it belongs.                                |
| After each meaningful session | Repo + Obsidian              | Update repo-local docs if needed, then write a short session note or log in Obsidian. |
| End of day                    | Obsidian                     | Process Inbox, link new notes, update runbooks, and write next actions.               |

# 11. Beginner rules that will save you pain

- One vault first. Do not split into multiple vaults until you have a real reason.

- Obsidian is the brain, not the runtime.

- Keep repos in WSL if you work from WSL.

- Keep OpenClaw live configs on the Mac mini if that is where OpenClaw runs.

- Always pair each real repo with one Obsidian repo note.

- Add automation gradually: shared Markdown first, URI second, REST API third.

- Do not install ten community plugins on day one.

# 12. Community plugins to add later, in order

| **Plugin**     | **Priority**         | **Why it helps**                                   | **When to add**                         |
|----------------|----------------------|----------------------------------------------------|-----------------------------------------|
| Templater      | High                 | More powerful templates and inserts                | After basic vault use feels comfortable |
| QuickAdd       | High                 | Fast capture and note creation workflows           | After you know your main note types     |
| Tasks          | Medium               | Better task handling in Markdown                   | After you have a daily review habit     |
| Dataview       | Medium               | Query notes and properties into dashboards         | After your metadata is consistent       |
| Local REST API | High for integration | Lets local scripts and agents work with your vault | After desktop setup is stable           |

# 13. Markdown templates

Copy these into your 07 Templates folder. Keep them simple first. You can always add more frontmatter later.

## Home.md


```
# Home
## Areas
- [[Personal Home]]
- [[Learning Home]]
- [[AI Systems Home]]
- [[Projects Home]]
## Quick Access
- [[WSL Repo Index]]
- [[Mac Mini Repo Index]]
- [[Claude Code Home]]
- [[OpenClaw Home]]
## Today
- [[{{date:YYYY-MM-DD}}]]
```


## Personal Home.md


```
# Personal Home
## Journal
- [[Journal Dashboard]]
## Health
- [[Health Dashboard]]
## Goals
- [[Goals Dashboard]]
## Admin
- [[Finance Dashboard]]
- [[Admin Dashboard]]
```


## Learning Home.md


```
# Learning Home
## Active Learning
- [[Current Courses]]
- [[Books In Progress]]
## Concepts
- [[Concept Index]]
## Research
- [[Research Queue]]
```


## AI Systems Home.md


> # AI Systems Home
> 
> 
> 
> ## Claude Code
> 
> - [[Claude Code Home]]
> 
> - [[WSL Repo Index]]
> 
> - [[Claude Code Prompt Library]]
> 
> - [[Claude Code Troubleshooting]]
> 
> 
> 
> ## OpenClaw
> 
> - [[OpenClaw Home]]
> 
> - [[OpenClaw Config Map]]
> 
> - [[OpenClaw Runbooks]]
> 
> - [[OpenClaw Troubleshooting]]
> 
> 
> 
> ## Dev Environment
> 
> - [[VS Code Setup]]
> 
> - [[WSL Notes]]
> 
> - [[Mac Mini Remote Workflow]]


## Daily Note Template.md


```
# {{date:YYYY-MM-DD}}
## Top 3
-
-
-
## Personal
-
## Learning
-
## AI Systems
-
## Tasks
- [ ]
## Notes
-
```


## Repo Note Template.md


```yaml
---
type: repo
system:
host:
path:
status: active
review:
---
# Repo -
## Purpose
## Key Files
## Commands
## Claude Memory
- CLAUDE.md:
- .claude/settings.json:
- .claude/settings.local.json:
## Related Obsidian Notes
## Recent Changes
## Next Actions
```


## Runbook Template.md


```yaml
---
type: runbook
system:
status: draft
---
# Runbook -
## Purpose
## Preconditions
## Steps
1.
2.
3.
## Expected Result
## Troubleshooting
## Related Links
```


## Troubleshooting Template.md


```yaml
---
type: troubleshooting
system:
status: open
severity:
date:
---
# Issue -
## Symptoms
## Likely Cause
## What I Tried
## Resolution
## Follow-up Prevention
```


## Learning Note Template.md


```yaml
---
type: learning
topic:
status:
source:
---
#
## Summary
## Key Points
## Questions
## Related Notes
```


## Project Template.md


```yaml
---
type: project
domain:
status: active
owner:
review:
---
# Project -
## Outcome
## Scope
## Current Status
## Next Actions
- [ ]
## Related Notes
```


## Claude Session Log Template.md


```yaml
---
type: session-log
system: claude-code
repo:
date:
---
# Claude Session Log
## Goal
## What Changed
## Commands Run
## Issues Hit
## Follow-ups
## Links
```


## OpenClaw Session Log Template.md


```yaml
---
type: session-log
system: openclaw
repo:
date:
---
# OpenClaw Session Log
## Goal
## What Changed
## Skills / Prompts Touched
## Issues Hit
## Follow-ups
## Links
```


## CLAUDE.md starter


```
# Project Instructions
## Purpose
Describe what this repo is for in plain language.
## Constraints
List rules Claude should respect.
## Workflow
Explain how to explore, edit, test, and document changes.
## Important Files
- README.md
- docs/setup.md
- docs/troubleshooting.md
## Output Expectations
- Keep changes focused
- Update docs when behavior changes
- Summarize changes clearly
```


# 14. Setup checklists

## 14.1 iPhone checklist

- Obsidian installed

- Vault named Dara-OS created

- Sync path chosen: Obsidian Sync or iCloud Drive

- Folder structure created

- Core plugins enabled

- Starter notes created

- Templates created

## 14.2 Windows checklist

- Obsidian installed and vault connected

- VS Code installed

- WSL installed

- WSL extension installed in VS Code

- First WSL repo created under /home/yourname/projects

- Repo note created in Obsidian

## 14.3 Mac mini / OpenClaw checklist

- Remote SSH from VS Code working

- OpenClaw repo structure created

- CLAUDE.md added to the repo

- docs/setup.md and docs/troubleshooting.md created

- OpenClaw notes created in Obsidian

- At least one runbook documented

## 14.4 Integration checklist

- Shared Markdown docs in each repo

- Repo note in Obsidian for each major repo

- Obsidian URI test working

- Local REST API plugin installed on desktop

- One automation writing a session note into Obsidian

# 15. Reference points used for this guide

Use these as your current source-of-truth references when you start implementing the desktop integration:

- Obsidian URI help: https://obsidian.md/help/uri

- Obsidian Local REST API plugin: https://github.com/coddingtonbear/obsidian-local-rest-api

- Claude Code memory and project instructions (CLAUDE.md): https://docs.anthropic.com/en/docs/claude-code/memory

- Claude Code skills: https://docs.anthropic.com/en/docs/claude-code/skills

- OpenClaw docs: https://docs.openclaw.ai/

- OpenClaw getting started: https://docs.openclaw.ai/start/getting-started
