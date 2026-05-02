# Claude Code for Non-Coders Part 1 — Setup & Basics

> **Source:** [ruben.substack.com/p/claude-code](https://ruben.substack.com/p/claude-code) | Mar 19, 2026 ✅
> **Section:** [[How to AI - Index#📂 Free Claude Course (Beginner to Pro)]]

---

## What Is Claude Code?
Claude Code is for developers what Cowork is for knowledge workers. But non-coders can use it too — English is the new code.

**Use cases for non-coders:**
- Create professional websites with prompts
- Build personalized training from any content
- Generate interactive dashboards

## Setup Steps

### 1. Create a Free GitHub Account
GitHub = Google Drive for code. Go to [github.com](https://github.com), sign up free.

### 2. Link GitHub to Claude Code
Claude desktop app → Settings → Connectors → Browse → search "GitHub" → Connect.

### 3. Start Coding Without Code
- Open Claude Code tab
- Use **Opus 4.6** + **Auto accept edits**
- Use this prompt template:

```
Create a GitHub repo named "[project-name]".

I do not know how to code and don't want to learn. 
Code everything for me. Do not ask for permissions.

Follow these instructions:
1. I want to [goal] for [success criteria].
2. Here's an example [attached].
3. [Steps to follow].
```

### 4. The Feedback Loop
1. Check the live website ("I need a link now so anyone can access it")
2. Note all problems you see
3. Follow up with a numbered list of issues
4. Claude fixes them one by one
5. Refresh → repeat

## Speed Up: Bypass Permissions
Claude Code asks for permission 20+ times per session. To bypass:
1. Download VS Code ([code.visualstudio.com](https://code.visualstudio.com))
2. Install the Claude Code extension
3. Use `--dangerously-skip-permissions` flag (vibecoding mode)

## CLAUDE.md — The Game Changer
Every time you open Claude Code on a project, it starts from zero. Create a `CLAUDE.md` file in your project root — Claude reads it first every time, knowing your fonts, colors, page structure, and past edits.

## Warning
- One Code session = 20+ regular Claude chats in usage
- You can't review the code if you don't code — test the actual website instead
- Consider Max plan ($100/month) if using Code daily

## Related
- [[Claude Code for Non-Coders Part 2 - Core Concepts]]
- [[Claude Code for Non-Coders Part 3 - Skills]]
- [[Claude Code for Non-Coders Part 4 - CLAUDE.md]]
- [[Claude Code for Non-Coders Part 5 - Projects Memory]]
- [[Vibe Coding for Beginners]]

#claude-code #vibecoding #github #non-coders
