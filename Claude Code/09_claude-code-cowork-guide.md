# Claude Code & Claude Cowork: A Practitioner's Guide

*Based on official Anthropic documentation and verified sources — April 2026.*

---

## 1. What Each Tool Is

### Claude Code

Claude Code is Anthropic's **agentic coding system** — an AI assistant that reads your entire codebase, edits files across multiple modules, runs shell commands, executes tests, manages git workflows, and delivers committed code. It was released in February 2025 as a CLI tool and has since expanded to VS Code, JetBrains, a standalone desktop app, a web interface (claude.ai/code), and an iOS app.

Claude Code is not an autocomplete tool. Where code-completion tools suggest the next line as you type, Claude Code operates at the *project level*: it reads the full codebase, plans an approach across multiple files, executes changes, runs tests, and iterates on failures. The developer sets the objective and reviews the result; the execution loop runs independently.

**Who it's for:** Software developers, DevOps engineers, and increasingly non-developers who want to build working software by describing outcomes in plain language. At Anthropic, the majority of code is now written by Claude Code. Engineers focus on architecture, product thinking, and orchestrating multiple agent sessions in parallel.

**How it fits the ecosystem:** Claude Code uses Claude models (primarily Opus 4.6 and Sonnet 4.6) as its reasoning engine but is also available with third-party providers. It requires a Claude subscription (Pro, Max, Team, Enterprise) or an Anthropic Console API account. The Terminal CLI and VS Code extension also support third-party providers via API keys.

> 📖 **Docs:** [Claude Code Overview](https://code.claude.com/docs/en/overview) · [Product page](https://www.anthropic.com/product/claude-code)

### Claude Cowork

Claude Cowork is Anthropic's **agentic AI system for knowledge work**. Released as a research preview in January 2026 and made generally available in April 2026, it brings Claude Code's agentic architecture to the Claude Desktop app — without requiring a terminal.

Where Claude Code targets developers and codebases, Cowork targets **non-technical knowledge workers**: operations teams, marketers, finance analysts, legal professionals, researchers — anyone who works with documents, data, and files. Instead of responding to prompts one at a time, Cowork takes an outcome description, plans the steps, and executes them on your machine: reading and writing local files, creating polished deliverables (spreadsheets, presentations, documents), browsing the web, and coordinating parallel sub-agents.

The key distinction from regular Claude chat: in Cowork, you give Claude access to folders on your computer. Claude can then read, edit, and create files in those folders. In regular chat, Claude can only advise — in Cowork, it *does the work*.

**Origin story:** When Claude Code launched, Anthropic expected developers to use it for coding. They did — and then started using it for everything else: sorting files, compiling research, drafting documents. Anthropic saw non-engineering teams at their own company (marketing, data, finance) bypassing the chat interface for Claude Code. Cowork was built to deliver those same capabilities without requiring terminal proficiency. It was reportedly built by Claude Code itself in approximately two weeks.

**Platform status:** Generally available on macOS and Windows via the Claude Desktop app. Available on all paid plans (Pro, Max, Team, Enterprise). Enterprise features include role-based access controls, usage analytics, Analytics API, OpenTelemetry observability, and granular MCP tool permissions.

> 📖 **Docs:** [Get started with Cowork](https://support.claude.com/en/articles/13345190-get-started-with-claude-cowork) · [Product page](https://claude.com/product/cowork) · [Launch blog post](https://claude.com/blog/cowork-research-preview)

---

## 2. Full Capabilities Breakdown

### Claude Code Capabilities

#### Surfaces and Environments

Claude Code runs identically across five surfaces, all connected to the same engine:

| Surface | Key Strengths |
|---|---|
| **Terminal CLI** | Full-featured: file editing, shell commands, piping, scripting, CI integration |
| **VS Code / Cursor** | Inline diffs, @-mentions, plan review, conversation history in editor |
| **JetBrains** | Interactive diff viewing, selection context sharing |
| **Desktop App** | Visual diff review, multiple parallel sessions, scheduled tasks, cloud sessions |
| **Web (claude.ai/code)** | No local setup, long-running tasks, works from mobile, parallel tasks |

#### Core Operations

- **Codebase understanding:** Searches directories, understands module connections, reads and reasons about your entire project
- **Multi-file editing:** Creates and edits files across a codebase — builds features, executes multi-file refactors
- **Command execution:** Runs shell commands, tests, linters, build tools, and development scripts
- **Git integration:** Stages changes, writes commit messages, creates branches, opens PRs
- **CI/CD automation:** GitHub Actions and GitLab CI/CD integration for automated code review and issue triage

#### CLI Commands and Flags

```bash
# Start interactive session in current project
claude

# One-shot execution (non-interactive)
claude -p "write tests for the auth module"

# Resume a previous session
claude --resume

# Teleport a web/iOS session into your terminal
claude --teleport

# Pipe input
tail -200 app.log | claude -p "find anomalies"
git diff main --name-only | claude -p "review for security issues"

# Run with specific worktree (parallel work)
claude -w 2

# Slash commands (inside a session)
/clear          # Clear context
/compact        # Compress context to save tokens
/desktop        # Hand off to Desktop app for visual diff review
/schedule       # Create a cloud scheduled task
/loop           # Repeat a prompt within a session (polling)
/resume         # Resume a previous session
/review-pr      # Custom skill command (user-defined)
/powerup        # Interactive lessons teaching Claude Code features
/team-onboarding # Generate teammate ramp-up guide
/ultraplan      # Remote session planning (auto-creates cloud environment)
```

#### Configuration and Customization

**CLAUDE.md** — A markdown file in your project root that Claude reads at the start of every session. Use it for coding standards, architecture decisions, preferred libraries, review checklists, and project-specific instructions.

**Auto memory** — Claude Code automatically saves learnings (build commands, debugging insights, environment facts) across sessions without you writing anything. Stored in `~/.claude/` and project-level `.claude/` directories.

**Custom skills** — Package repeatable workflows as slash commands your team can share (e.g., `/review-pr`, `/deploy-staging`).

**Hooks** — Shell commands that run before or after Claude Code actions: auto-formatting after file edits, linting before commits, custom validation.

**Permission modes** — Control Claude Code's autonomy from "approve every action" to letting built-in safety classifiers distinguish safe vs. risky actions automatically.

**MCP (Model Context Protocol)** — Connect Claude Code to external data sources: Google Drive, Jira, Slack, custom tooling. Open standard for tool integration.

**Channels** — Push events from Telegram, Discord, iMessage, or custom webhooks into a Claude Code session.

**Plugins** — Extend Claude Code with community or private plugins. Enterprise admins can create private plugin marketplaces.

#### Advanced Features

- **Sub-agents:** Spawn multiple Claude Code agents working on different parts of a task simultaneously. A lead agent coordinates, assigns subtasks, and merges results.
- **Agent SDK:** Build fully custom agents powered by Claude Code's tools and capabilities, with full control over orchestration, tool access, and permissions.
- **Remote Control:** Continue a local session from your phone or any browser.
- **Dispatch:** Start a task from your phone; the Desktop app creates and runs the session.
- **Cloud scheduled tasks:** Run on Anthropic-managed infrastructure — keep running when your computer is off. Morning PR reviews, overnight CI analysis, weekly audits.
- **Desktop scheduled tasks:** Run on your machine with direct local file and tool access.
- **Computer use (preview):** Claude Code can interact directly with your screen.
- **Chrome extension (beta):** Debug live web applications.
- **Slack integration:** Mention `@Claude` in Slack with a bug report, get a pull request back.
- **Focus mode:** Shows only prompt, one-line tool summary with diff stats, and final response.
- **Brief mode:** Concise structured responses.

> 📖 **Docs:** [CLI Reference](https://code.claude.com/docs/en/cli-reference) · [Memory](https://code.claude.com/docs/en/memory) · [Skills](https://code.claude.com/docs/en/skills) · [Hooks](https://code.claude.com/docs/en/hooks) · [Sub-agents](https://code.claude.com/docs/en/sub-agents) · [Agent SDK](https://code.claude.com/docs/en/agent-sdk/overview) · [MCP](https://code.claude.com/docs/en/mcp) · [Permission modes](https://code.claude.com/docs/en/permission-modes) · [Channels](https://code.claude.com/docs/en/channels)

---

### Claude Cowork Capabilities

#### Core Operations

- **Direct local file access:** Reads from and writes to local files without manual uploads/downloads. You choose which folders Claude can access.
- **Sub-agent coordination:** Breaks complex work into smaller tasks, coordinates parallel workstreams.
- **Professional outputs:** Generates polished Excel spreadsheets (with working formulas), PowerPoint presentations, formatted Word documents, PDFs, and more.
- **Long-running tasks:** Works on complex tasks for extended periods without conversation timeouts or context limits interrupting.
- **Scheduled tasks:** Recurring and on-demand — Claude completes work automatically (not possible in regular chat).
- **Projects:** Organize related tasks into persistent, self-contained workspaces with their own files, links, instructions, and memory.

#### Supported File Types

Documents and text: .docx, .doc, .pdf, .txt, .md, .html, .json, .csv, .tsv
Spreadsheets: .xlsx, .xls
Presentations: .pptx
Images: Various formats for analysis and processing

#### Connectors and Integrations

Cowork uses Claude's existing connector ecosystem:

- **Available:** Slack, Zoom (meeting summaries + transcripts), AWS Marketplace, n8n, Honeycomb, Fellow.ai, and growing
- **Coming soon:** Gmail, Google Calendar, Google Drive (currently require Chrome extension workaround)
- **Claude in Chrome:** When paired with Cowork, Claude can navigate your browser — click buttons, fill forms, navigate tabs, complete web-based tasks
- **Computer use (preview):** Direct screen interaction — opens apps, navigates browser, runs tools. Claude reaches for connectors first, falls back to browser, and only uses screen as a last resort.

#### Plugins and Skills

- **Plugin marketplace:** Bundles tools, knowledge, and workflows into a single install. Enterprise admins can create private marketplaces.
- **Built-in skills:** Improve Claude's ability to create documents, presentations, and spreadsheets.
- **Customize section:** Groups skills, plugins, and connectors in one place within Claude Desktop.
- **Global instructions:** Tell Claude your preferred tone, format, or background — applies across every session.
- **Folder instructions:** Set folder-specific instructions that activate when working in that directory.

#### Safety and Isolation

- **Code execution isolation:** Shell commands and code run inside an isolated VM, separate from your main OS.
- **Controlled file access:** Claude can only access folders you've explicitly connected.
- **Network egress controls:** Follows your configured egress permissions (note: web search tool and MCPs bypass these).
- **Action review:** Claude asks permission before taking consequential actions.

#### Enterprise Features (GA as of April 2026)

- **Role-based access controls:** Organize users into groups (manual or SCIM from identity provider), assign custom roles defining which capabilities members can use.
- **Granular MCP permissions:** Allow/restrict specific actions per tool (e.g., allow Gmail read but not send).
- **Usage analytics:** Per-user activity, skill usage, daily/weekly engagement data.
- **Analytics API:** Programmatic access to engagement and adoption data.
- **OpenTelemetry support:** Monitor Cowork activity with existing observability infrastructure.
- **Group spend limits:** Set budgets for teams using the API.

**Important limitation:** Cowork stores conversation history locally. Activity is not captured in Audit Logs, Compliance API, or Data Exports. Anthropic explicitly states: "Do not use Cowork for regulated workloads."

> 📖 **Docs:** [Cowork help center](https://support.claude.com/en/articles/13345190-get-started-with-claude-cowork) · [Cowork product page](https://claude.com/product/cowork) · [Use Cowork safely](https://support.claude.com/en/articles/cowork-safety)

---

## 3. Practical Use Cases by Complexity

### Claude Code

#### Beginner
- **Explore unfamiliar code:** `claude "explain how the authentication flow works in this project"`
- **Fix a bug from an error message:** Paste a stack trace and ask Claude to trace and fix it
- **Write tests:** `claude "write tests for the auth module, run them, and fix any failures"`
- **Generate commit messages:** `claude "commit my changes with a descriptive message"`
- **Update dependencies:** `claude "update all npm packages to their latest major versions and fix breaking changes"`

#### Intermediate
- **Multi-file refactoring:** `claude "refactor the payment module to use the strategy pattern — update all callers"`
- **CI/CD pipeline authoring:** `claude "create a GitHub Actions workflow that runs tests, lints, and deploys to staging on merge to main"`
- **Code review automation:** Configure GitHub Actions to run Claude Code on every PR for automated review
- **Translation pipelines:** `claude -p "translate new strings into French and raise a PR for review"` (CI integration)
- **Log analysis:** `tail -200 app.log | claude -p "Slack me if you see any anomalies"`

#### Advanced
- **Parallel agent teams:** Spawn sub-agents to work on different modules simultaneously — one refactors the API, another updates tests, a third migrates the database schema
- **Custom agent SDK workflows:** Build your own agents with Claude Code's tool primitives for custom orchestration, specialized access controls, and domain-specific logic
- **Scheduled infrastructure maintenance:** Cloud scheduled tasks that run nightly to audit dependencies, check for CVEs, and open issues for anything flagged
- **Cross-platform development:** Start a task on the web from your phone, teleport it to your terminal when back at your desk, hand it off to the Desktop app for visual diff review
- **Slack-driven development:** Team mentions `@Claude` in Slack with a bug report → Claude Code creates a branch, fixes the issue, runs tests, opens a PR — all without a developer touching a terminal

### Claude Cowork

#### Beginner
- **Organize a messy folder:** Point Cowork at your Downloads folder → it renames, sorts, deduplicates, and categorizes files by content
- **Convert file formats:** Batch-convert .docx files to PDF, compress images, merge CSVs
- **Draft from notes:** Give it scattered notes and voice memos → get a polished first draft

#### Intermediate
- **Market research with deliverables:** Describe a market question → Cowork researches via web search, calculates analysis, produces a PowerPoint presentation or Excel workbook
- **Data analysis pipeline:** Point it at a CSV → outlier detection, cross-tabulation, time-series analysis, chart generation
- **Meeting follow-up:** Connect Zoom connector → Cowork pulls meeting transcripts, extracts action items, creates a follow-up document, and populates a tracking spreadsheet
- **Legal document organization:** Turn a folder of lawsuit documents into a chronologically organized exhibit set with descriptive titles and strategic importance assessment

#### Advanced
- **Cross-source product intelligence:** Synthesize feedback from call transcripts, Slack, CRM notes, and issue trackers to identify cross-platform patterns and generate prioritized product ideas
- **Operational bottleneck analysis:** Connect to org database, Slack, and Jira → Cowork generates an interactive dashboard with team-by-team efficiency analyses and a prioritized remediation roadmap
- **Recurring automated reports:** Schedule daily/weekly tasks: financial reconciliation, competitor monitoring, compliance checks — all running unattended with results delivered to your preferred channel
- **Brand voice extraction:** Have Cowork analyze your existing marketing materials, documents, and conversations to distill your brand's voice into enforceable guidelines

---

## 4. How They Compare and Complement Each Other

### When to Use Which

| Scenario | Use Claude Code | Use Cowork |
|---|---|---|
| Writing, debugging, or refactoring code | ✅ | ❌ |
| Building features or fixing bugs | ✅ | ❌ |
| Git operations and PR management | ✅ | ❌ |
| CI/CD integration | ✅ | ❌ |
| Running shell commands and scripts | ✅ | Limited (isolated VM) |
| Organizing files and folders | Both work | ✅ Easier |
| Creating spreadsheets with formulas | ❌ Limited | ✅ Built-in skill |
| Creating PowerPoint presentations | ❌ Limited | ✅ Built-in skill |
| Research and report generation | ❌ Not its focus | ✅ |
| Data analysis and visualization | ❌ Limited | ✅ |
| Non-technical users | ❌ Requires terminal | ✅ |
| Scheduled recurring tasks | ✅ Cloud + Desktop | ✅ |
| IDE integration | ✅ VS Code, JetBrains | ❌ |
| Browser automation | ✅ Chrome extension | ✅ Claude in Chrome |
| Enterprise governance | ✅ | ✅ (RBAC, analytics) |

### How They Complement Each Other

The fundamental distinction: **Claude Code is for building software; Cowork is for knowledge work.** They share the same agentic architecture and model engine but serve different audiences and workflows.

A developer might use Claude Code all day for coding, then switch to Cowork for a non-coding task: preparing a quarterly business review deck, analyzing survey responses, or organizing project documentation. They're not competing tools — they're two interfaces to the same underlying capability, optimized for different problem domains.

For teams, the combination is powerful: engineering uses Claude Code for development; marketing, finance, legal, and operations use Cowork for their domain-specific workflows. Both connect to the same MCP ecosystem, plugin marketplace, and enterprise governance layer.

---

## 5. Advanced Usage Patterns

### Claude Code Power-User Techniques

**CLAUDE.md layering:** You can create CLAUDE.md files at multiple levels — repository root, subdirectories, and `~/.claude/CLAUDE.md` for global instructions. Claude Code reads all applicable files, with more specific ones taking precedence. Use this for team-wide coding standards at the root and module-specific instructions in subdirectories.

**Parallel worktrees:** Use `claude -w <n>` to run multiple Claude Code sessions in isolated git worktrees simultaneously. Each session gets its own branch and working directory. Useful for working on multiple features in parallel.

**Piping as a superpower:** Claude Code follows Unix philosophy. Chain it with other tools:
```bash
# Security audit of changed files
git diff main --name-only | claude -p "review for security issues"

# Analyze production logs
kubectl logs deployment/api --tail=500 | claude -p "summarize errors and suggest fixes"

# Batch processing
find . -name "*.py" -newer checkpoint.txt | claude -p "add type hints to all functions"
```

**MCP for context injection:** Connect Claude Code to your team's internal tools via MCP servers. Design docs in Google Drive, tickets in Jira, messages in Slack — all become part of Claude's available context without copy-pasting.

**Hooks for workflow enforcement:**
```json
// .claude/hooks.json — auto-format after every file edit
{
  "afterWrite": "prettier --write {{file}}"
}
```

**The `/compact` discipline:** In long sessions, use `/compact` proactively to compress context before you hit limits. Don't wait for Claude to run out of context — compress early and keep working.

**Brief mode + Focus mode:** For experienced users who don't need verbose explanations, these modes dramatically reduce output noise. Brief mode gives structured responses; Focus mode (Ctrl+O) shows only the prompt, one-line tool summary with diff stats, and final response.

### Cowork Power-User Techniques

**Folder-specific instructions:** Set up instructions per folder so Cowork automatically applies the right context when you work in different project directories. Financial analysis folder gets one set of rules; marketing folder gets another.

**Plugin stacking:** Combine multiple plugins to create specialized Claude instances. A brand voice plugin + a data analysis plugin + a document formatting plugin turns Cowork into a purpose-built marketing analyst.

**Connector chaining:** The real power is combining connectors: Zoom meeting transcripts → cross-reference with local project files → update Slack channel with action items → create tracking spreadsheet. Cowork handles the orchestration.

**Prompt pattern for complex tasks:** Front-load your instructions with outcome descriptions, not process descriptions:
```
Bad:  "First read the files in /data, then create a spreadsheet, then add charts..."
Good: "Create a quarterly revenue analysis from the data in /data. I need a spreadsheet
       with pivot tables, trend charts, and an executive summary slide deck. Flag any
       anomalies."
```

**Scheduled task patterns:** Set up recurring tasks for work that repeats: daily competitor news briefings, weekly expense reconciliation, monthly compliance checks. Each runs as a full agentic session with access to all connected tools.

---

## 6. Limitations and Workarounds

### Claude Code Limitations

| Limitation | Workaround |
|---|---|
| **Context window fills up in long sessions** | Use `/compact` proactively; use `/clear` and restart for fresh context; break large tasks into focused sessions |
| **Can make incorrect changes confidently** | Use restrictive permission modes; always review diffs before accepting; write tests first |
| **No native Windows support** (historically) | Now supported via native installer, WinGet, and PowerShell; WSL2 also works |
| **Rate limits on subscription plans** | Use API key with Console account for higher limits; use batch/scheduled tasks for non-urgent work |
| **MCP server connection issues** | Check plugin conflicts; ensure connectors aren't duplicated with unauthenticated claude.ai connectors |
| **Large file operations can be slow** | Recent updates improved Write tool diff speed by 60% for large files; still, break very large refactors into smaller scoped tasks |
| **Sub-agents don't always inherit MCP tools** | Fixed in recent releases; update to latest version |

### Cowork Limitations

| Limitation | Workaround |
|---|---|
| **Not captured in Audit Logs or Compliance API** | Do not use for regulated workloads; use regular Claude chat for auditable conversations |
| **Google connectors (Gmail, Calendar, Drive) not yet native** | Use Claude in Chrome extension as a workaround for Google-related workflows |
| **xlsx skill struggles with complex non-columnar spreadsheets** | Simplify data structure before processing; or use Claude Code's file tools directly |
| **Network egress permissions don't apply to web search or MCPs** | Enterprise admins can disable web search in Organization settings; manage MCP tool permissions granularly |
| **Conversation history stored only locally** | Back up `~/.config/Claude/` or equivalent; no cloud sync of Cowork history |
| **Computer use is still preview-quality** | Claude reaches for connectors/APIs first (faster and more reliable); only falls back to screen interaction when necessary |
| **No Linux desktop app** | Use Claude Code CLI instead (same underlying engine); or access via web at claude.ai |

### Shared Limitations

- **Both require paid subscriptions** — No free tier for either tool. Claude Code CLI and VS Code support third-party API keys as an alternative.
- **Both require careful oversight for consequential actions** — These are powerful agentic tools with real system access. Review before accepting, especially with file modifications and command execution.
- **Model quality varies by provider** — Best results come from Opus 4.6 and Sonnet 4.6. Third-party providers may produce lower-quality agentic behavior.

---

## Quick Reference Links

### Claude Code

| Resource | URL |
|---|---|
| Overview & Docs | [code.claude.com/docs/en/overview](https://code.claude.com/docs/en/overview) |
| Quickstart | [code.claude.com/docs/en/quickstart](https://code.claude.com/docs/en/quickstart) |
| CLI Reference | [code.claude.com/docs/en/cli-reference](https://code.claude.com/docs/en/cli-reference) |
| Memory & CLAUDE.md | [code.claude.com/docs/en/memory](https://code.claude.com/docs/en/memory) |
| Skills (Custom Commands) | [code.claude.com/docs/en/skills](https://code.claude.com/docs/en/skills) |
| Hooks | [code.claude.com/docs/en/hooks](https://code.claude.com/docs/en/hooks) |
| Sub-agents | [code.claude.com/docs/en/sub-agents](https://code.claude.com/docs/en/sub-agents) |
| Agent SDK | [code.claude.com/docs/en/agent-sdk/overview](https://code.claude.com/docs/en/agent-sdk/overview) |
| MCP Integration | [code.claude.com/docs/en/mcp](https://code.claude.com/docs/en/mcp) |
| Permission Modes | [code.claude.com/docs/en/permission-modes](https://code.claude.com/docs/en/permission-modes) |
| Settings | [code.claude.com/docs/en/settings](https://code.claude.com/docs/en/settings) |
| Remote Control | [code.claude.com/docs/en/remote-control](https://code.claude.com/docs/en/remote-control) |
| Channels | [code.claude.com/docs/en/channels](https://code.claude.com/docs/en/channels) |
| Scheduled Tasks (Cloud) | [code.claude.com/docs/en/web-scheduled-tasks](https://code.claude.com/docs/en/web-scheduled-tasks) |
| GitHub Actions | [code.claude.com/docs/en/github-actions](https://code.claude.com/docs/en/github-actions) |
| Slack Integration | [code.claude.com/docs/en/slack](https://code.claude.com/docs/en/slack) |
| Best Practices | [code.claude.com/docs/en/best-practices](https://code.claude.com/docs/en/best-practices) |
| Changelog | [code.claude.com/docs/en/changelog](https://code.claude.com/docs/en/changelog) |
| GitHub Releases | [github.com/anthropics/claude-code/releases](https://github.com/anthropics/claude-code/releases) |

### Claude Cowork

| Resource | URL |
|---|---|
| Getting Started | [support.claude.com/en/articles/13345190](https://support.claude.com/en/articles/13345190-get-started-with-claude-cowork) |
| Product Page | [claude.com/product/cowork](https://claude.com/product/cowork) |
| Anthropic Product Page | [anthropic.com/product/claude-cowork](https://www.anthropic.com/product/claude-cowork) |
| Launch Blog Post | [claude.com/blog/cowork-research-preview](https://claude.com/blog/cowork-research-preview) |
| Training Course | [anthropic.skilljar.com/introduction-to-claude-cowork](https://anthropic.skilljar.com/introduction-to-claude-cowork) |
| Desktop Download | [claude.com/download](https://claude.com/download) |
