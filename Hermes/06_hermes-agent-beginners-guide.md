# Hermes Agent: A Beginner's Guide

*Based on the official documentation at [hermes-agent.nousresearch.com/docs](https://hermes-agent.nousresearch.com/docs/) — last reviewed April 2026.*

---

## 1. What Hermes Agent Is

Hermes Agent is an **open-source, self-hosted AI assistant** built by [Nous Research](https://nousresearch.com) — the lab behind the Hermes family of open-weight language models. Released in February 2026, it sits in the same category as OpenClaw: a personal AI agent that runs on your own hardware and connects to your messaging apps.

But Hermes Agent's headline claim is more specific: it's designed to **get smarter the longer you use it**. The docs describe a "closed learning loop" where the agent doesn't just answer questions — it notices patterns, creates reusable skills from experience, refines those skills over time, and builds a persistent model of who you are and what you're working on. Think of it like an assistant who takes their own notes, develops their own SOPs, and remembers every project you've ever mentioned.

**Who it's for:** Developers, power users, and researchers who want an always-available AI assistant that isn't tied to a browser tab or a single device. You can run it on a $5 VPS, a GPU cluster, or even an Android phone via Termux — and talk to it from Telegram, Discord, Slack, WhatsApp, Signal, or a dozen other platforms.

**What problem it solves:** Most AI assistants are stateless — every conversation starts from zero. Hermes Agent makes the assistant *persistent*: it remembers your projects, preferences, environment, and past work across sessions. It also goes beyond chat to actually do things on your machine: run shell commands, browse the web, manage files, execute code, and run scheduled tasks — all unattended if you want.

**Connection to the Hermes model family:** Nous Research is known for training the Hermes series of fine-tuned LLMs (Hermes 3, Hermes 4) which emphasize agentic tool-calling and instruction-following. Hermes Agent is the natural extension — a runtime environment designed to make those models (or any other LLM) actually *do work*. However, Hermes Agent is **model-agnostic**: you can plug in Claude, GPT-4o, DeepSeek, models via OpenRouter, or your own endpoint. Model selection is configuration, not code.

> 📖 **Docs:** [What is Hermes Agent?](https://hermes-agent.nousresearch.com/docs/#what-is-hermes-agent)

---

## 2. Architecture

Hermes Agent is written in **Python** and managed with `uv` (a fast Python package manager). The codebase is substantial — the core agent loop alone is roughly 9,200 lines — but the architecture is cleanly layered.

### The Big Picture

```
┌──────────────────────────────────────────────┐
│              Entry Points                     │
│  CLI · Messaging Gateway · ACP (IDE) · API   │
└──────────────┬───────────────────────────────┘
               │
               ▼
┌──────────────────────────────────────────────┐
│         AIAgent (the core loop)               │
│                                               │
│  Prompt Builder → Provider Resolution → LLM   │
│       ↕                    ↕                  │
│  Tool Dispatch     Context Compression        │
└──────────────┬───────────────────────────────┘
               │
        ┌──────┴──────┐
        ▼             ▼
  Session Storage   Tool Backends
  (SQLite + FTS5)   (Terminal, Browser,
                     Web, MCP, Files...)
```

Here's what each layer does:

### Entry Points — How You Talk to It

Hermes has four ways in:

- **CLI** (`cli.py`) — A full interactive terminal UI with multiline editing, slash-command autocomplete, streaming output, and conversation history. This is the primary developer interface.
- **Messaging Gateway** (`gateway/run.py`) — A long-running process with adapters for 15+ platforms: Telegram, Discord, Slack, WhatsApp, Signal, Matrix, Mattermost, Email, SMS, DingTalk, Feishu, WeCom, BlueBubbles (iMessage), Home Assistant, and webhooks.
- **ACP Adapter** — Integrates Hermes as an agent inside code editors (VS Code, Zed, JetBrains) so you can use it alongside your coding workflow.
- **Batch Runner** — For research use: generates thousands of tool-calling trajectories in parallel for AI training data.

### AIAgent — The Core Loop

When a message arrives (from any entry point), it flows through the same engine:

1. **Prompt Builder** (`prompt_builder.py`) assembles the system prompt from multiple sources: personality (SOUL.md), memory (MEMORY.md, USER.md), skills, context files, tool-use guidance, and model-specific instructions.
2. **Provider Resolution** (`runtime_provider.py`) maps your configured provider and model to the correct API format. Hermes supports three API modes: OpenAI chat completions, OpenAI Codex responses, and Anthropic native messages. It handles 18+ providers, OAuth flows, and credential pools.
3. The assembled prompt goes to **your chosen LLM**, which returns either a text response or tool calls.
4. If tool calls are returned, **Tool Dispatch** (`model_tools.py`) routes each call to the appropriate tool implementation, collects results, and loops back to the LLM. This continues until the LLM produces a final text response.
5. The response is delivered back through whatever entry point the message came from, and the full exchange is saved to **Session Storage**.

### Session Storage — SQLite with Full-Text Search

All conversation history lives in a local SQLite database with FTS5 (Full-Text Search) indexing. This means the agent can search across its own past conversations to recall earlier work. Sessions have lineage tracking — when conversations are compressed to save context window space, the system maintains parent/child relationships so nothing is truly lost.

### Tool System — 47 Built-In Tools Across 20 Toolsets

The tool system is one of Hermes Agent's richest areas. Tools are organized into "toolsets" (groups that can be enabled/disabled together). Key categories:

- **Terminal** — Run shell commands via 6 backends: local, Docker, SSH, Daytona (serverless), Modal (serverless), and Singularity (HPC containers)
- **Browser** — 11 browser automation tools for web interaction
- **File operations** — Read, write, patch, search files
- **Web** — Search, extract, and vision tools
- **Code execution** — Sandboxed Python/script execution
- **Delegate** — Spawn isolated subagents for parallel workstreams
- **MCP** — Connect to any MCP (Model Context Protocol) server to extend capabilities dynamically

### Memory System — Two Bounded Stores

Hermes uses two small, curated Markdown files for persistent memory:

| File | Purpose | Size Limit |
|------|---------|------------|
| MEMORY.md | Agent's own notes — environment facts, conventions, lessons learned | ~2,200 chars |
| USER.md | User profile — your preferences, communication style, role | ~1,375 chars |

These are injected into the system prompt at session start. The agent manages its own memory autonomously — adding, consolidating, and pruning entries as needed. The tight size limits are deliberate: they force the agent to keep only high-value information.

### Skills System — Procedural Memory

Beyond declarative memory, Hermes creates *skills* — reusable procedures it writes after completing complex tasks. Skills are stored as files, follow the open [agentskills.io](https://agentskills.io) standard, and can be shared via the Skills Hub. The agent can also improve skills during subsequent use.

### Cron — Scheduled Agent Tasks

A built-in scheduler runs agent tasks on a schedule. These are not simple shell cron jobs — they're full agent sessions with access to all tools and skills. Jobs are defined in JSON, support natural language scheduling, can attach skills and scripts, and deliver results to any connected messaging platform.

> 📖 **Docs:** [Architecture](https://hermes-agent.nousresearch.com/docs/developer-guide/architecture) · [Agent Loop Internals](https://hermes-agent.nousresearch.com/docs/developer-guide/agent-loop) · [Prompt Assembly](https://hermes-agent.nousresearch.com/docs/developer-guide/prompt-assembly) · [Session Storage](https://hermes-agent.nousresearch.com/docs/developer-guide/session-storage) · [Gateway Internals](https://hermes-agent.nousresearch.com/docs/developer-guide/gateway-internals)

---

## 3. Strengths and Weaknesses

### Strengths

**The learning loop is genuine, not marketing.** The memory system (MEMORY.md + USER.md), the autonomous skill creation, the FTS5 cross-session search, and the skill self-improvement mechanism are all documented in implementation detail. This is a real architectural commitment, not a bolted-on feature. The memory capacity management — including the 80% threshold consolidation heuristic — shows thoughtful design.

**Exceptional documentation.** The docs at hermes-agent.nousresearch.com are among the best in the open-source agent space. The architecture page includes actual ASCII diagrams, line counts per module, a complete directory tree, data flow diagrams for CLI/gateway/cron paths, and a recommended reading order for new contributors. The memory system docs include concrete examples of good vs. bad memory entries. This level of detail is rare.

**Python-native with clean dependency management.** Using `uv` and pip extras means you install exactly what you need. Want just the CLI agent? Core install. Want Telegram gateway? Add `.[messaging]`. Want voice mode? Add `.[voice]`. This à la carte approach keeps the base footprint small while supporting 14+ optional extras.

**Six terminal backends with serverless options.** The ability to run your agent's compute on Daytona or Modal — where the environment hibernates when idle and costs nearly nothing — is a practical innovation. You get persistent server-like behavior without a server bill.

**Research-ready from the start.** Batch trajectory generation, Atropos RL environment integration, and trajectory export for fine-tuning are first-class features. This reflects Nous Research's identity as a model training lab — Hermes Agent is not just a product, it's infrastructure for training better tool-calling models.

**Plugin architecture is well-designed.** Three discovery sources (user, project, pip entry points), specialized plugin types for memory providers and context engines, and a clean context API for registration. The single-select constraint on memory providers and context engines prevents conflicting subsystems.

**Broad platform support including niche channels.** 15+ messaging platforms including DingTalk, Feishu, WeCom, Home Assistant, and SMS — going beyond the usual Telegram/Discord/Slack trio.

### Weaknesses

**No native Windows support.** The docs state plainly that native Windows is not supported — you must use WSL2. For a personal agent tool, this excludes a significant portion of non-developer users.

**The core agent loop is a monolith.** `run_agent.py` is ~9,200 lines. `cli.py` is ~8,500 lines. `gateway/run.py` is ~7,500 lines. These are very large single files. While the architecture is conceptually clean, the implementation has some monolithic tendencies that could affect contributor onboarding and maintainability.

**Memory limits are very tight.** MEMORY.md at 2,200 characters (~800 tokens) and USER.md at 1,375 characters (~500 tokens) means the agent can only retain about 15-25 memory entries total. For users with complex multi-project workflows, this will feel constrained. The docs justify this as keeping system prompts bounded, but it's a real tradeoff.

**Security model requires active user attention.** Like OpenClaw, Hermes has deep system access — terminal, files, browser, code execution. The docs include a security page covering command approval and container isolation, but the default posture gives the agent significant power. Sandboxing is available but opt-in.

**Newer and less battle-tested than OpenClaw.** Released February 2026, Hermes Agent has far less community mileage than OpenClaw (which launched in late 2025 and has 247K+ GitHub stars). The skills ecosystem and community contributions are still nascent.

**Some areas are lightly documented.** While the core docs are excellent, some features listed in the overview — like Honcho dialectic user modeling, the ACP IDE integration, and the RL training pipeline — have less depth in their dedicated pages. The guides section could use more end-to-end tutorials.

> 📖 **Docs:** [Features Overview](https://hermes-agent.nousresearch.com/docs/user-guide/features/overview) · [Security](https://hermes-agent.nousresearch.com/docs/user-guide/security) · [Memory System](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory)

---

## 4. Requirements and Setup

### Prerequisites

Remarkably minimal — the only hard prerequisite is **Git**. The installer handles everything else automatically:

| Dependency | Installed By |
|---|---|
| **uv** (Python package manager) | Installer |
| **Python 3.11** | uv (no sudo needed) |
| **Node.js v22** | Installer (needed for browser automation and WhatsApp) |
| **ripgrep** | Installer (fast file search) |
| **ffmpeg** | Installer (audio conversion for TTS) |

**Supported platforms:** Linux, macOS, WSL2, and Android via Termux. Native Windows is not supported.

**LLM provider:** You need an API key from at least one provider — Nous Portal, OpenRouter, OpenAI, Anthropic, or any compatible endpoint.

### Project Structure

After installation, two key locations matter:

**The repo** (wherever you cloned it):
```
hermes-agent/
├── run_agent.py          # Core agent loop
├── cli.py                # Interactive CLI
├── model_tools.py        # Tool dispatch
├── hermes_state.py       # Session database
├── tools/                # 47 tool implementations
│   ├── registry.py       # Central tool registry
│   ├── terminal_tool.py  # Shell execution
│   ├── browser_tool.py   # Browser automation
│   ├── mcp_tool.py       # MCP integration
│   └── environments/     # 6 terminal backends
├── gateway/              # Messaging gateway
│   ├── run.py            # Gateway runner
│   ├── session.py        # Session persistence
│   └── platforms/        # 15 platform adapters
├── agent/                # Prompt building, compression, memory
├── skills/               # Bundled skills
├── cron/                 # Scheduler
└── tests/                # 3,000+ tests
```

**Your config home** (`~/.hermes/`):
```
~/.hermes/
├── config.yaml           # Main configuration
├── .env                  # API keys
├── memories/
│   ├── MEMORY.md         # Agent's personal notes
│   └── USER.md           # User profile
├── sessions/             # SQLite conversation databases
├── skills/               # Your workspace skills
├── cron/                 # Scheduled job definitions
├── logs/
├── pairing/              # DM authorization tokens
├── hooks/                # Event hooks
└── whatsapp/session/     # WhatsApp session data
```

### Optional Extras

Hermes uses pip extras so you only install what you need:

| Extra | What It Adds |
|---|---|
| `all` | Everything |
| `messaging` | Telegram & Discord gateway |
| `cron` | Scheduled task support |
| `voice` | Microphone input + audio playback |
| `mcp` | Model Context Protocol support |
| `slack` | Slack messaging |
| `honcho` | AI-native memory via Honcho |
| `modal` | Modal cloud execution backend |
| `tts-premium` | ElevenLabs voices |
| `homeassistant` | Home Assistant integration |
| `termux` | Android-compatible bundle |

> 📖 **Docs:** [Installation](https://hermes-agent.nousresearch.com/docs/getting-started/installation) · [Configuration](https://hermes-agent.nousresearch.com/docs/user-guide/configuration)

---

## 5. How to Run It

### Option A: One-Line Install (Recommended)

```bash
# Install everything — handles Python, Node.js, dependencies, config
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash

# Reload your shell
source ~/.bashrc   # or: source ~/.zshrc

# Start chatting
hermes
```

The installer runs an interactive setup wizard that walks you through choosing an LLM provider and entering your API key. By the end, you're in a conversation.

### Option B: Manual Install

```bash
# 1. Clone the repo with submodules
git clone --recurse-submodules https://github.com/NousResearch/hermes-agent.git
cd hermes-agent

# 2. Install uv and create a virtual environment
curl -LsSf https://astral.sh/uv/install.sh | sh
uv venv venv --python 3.11

# 3. Install dependencies (all extras)
export VIRTUAL_ENV="$(pwd)/venv"
uv pip install -e ".[all]"

# 4. Create config directory structure
mkdir -p ~/.hermes/{cron,sessions,logs,memories,skills,pairing,hooks,image_cache,audio_cache,whatsapp/session}
cp cli-config.yaml.example ~/.hermes/config.yaml
touch ~/.hermes/.env

# 5. Add your API key to ~/.hermes/.env
echo 'OPENROUTER_API_KEY=sk-or-...' >> ~/.hermes/.env

# 6. Run
hermes
```

### Post-Install Configuration

```bash
hermes model           # Switch LLM provider/model interactively
hermes tools           # Enable/disable tool groups
hermes gateway setup   # Connect messaging platforms
hermes setup           # Full setup wizard
```

### Setting Up the Messaging Gateway

To talk to Hermes from Telegram, Discord, or other platforms:

```bash
hermes gateway setup   # Interactive platform configuration
hermes gateway start   # Start the long-running gateway process
```

The gateway runs as a persistent process (optionally as a systemd service) and routes messages from all connected platforms through the same agent core.

### Updating

```bash
hermes update
```

This pulls the latest changes and reinstalls dependencies.

> 📖 **Docs:** [Installation](https://hermes-agent.nousresearch.com/docs/getting-started/installation) · [Quickstart Tutorial](https://hermes-agent.nousresearch.com/docs/getting-started/quickstart) · [Messaging Gateway](https://hermes-agent.nousresearch.com/docs/user-guide/messaging) · [Updating](https://hermes-agent.nousresearch.com/docs/getting-started/updating)

---

## 6. Powerful Use Cases

### Self-Improving Project Assistant

**The scenario:** You're building a Rust web service. Over weeks of working with Hermes, it learns your project structure, preferred tools, coding conventions, and common debugging patterns.

**How it works:** The first time you ask Hermes to run your test suite, it discovers the command is `make test`. It saves this to MEMORY.md. The next time, it just knows. When you spend an hour debugging a tricky database migration, Hermes autonomously creates a *skill* documenting the procedure. The next time a migration issue comes up, it consults its own skill file before starting from scratch. Over time, the agent accumulates a growing knowledge base about your specific project — not generic knowledge, but hard-won specifics like "staging server uses SSH port 2222, not 22."

### Multi-Platform Personal Assistant

**The scenario:** You want one AI assistant reachable from Telegram on your phone, Discord on your desktop, and via CLI on your dev machine — with continuous memory across all surfaces.

**How it works:** Start the gateway (`hermes gateway start`) with Telegram and Discord configured. Message from Telegram during your commute: "Remind me to review the PR for the auth module." The agent saves this to memory. Later, from Discord on your laptop: "What was I supposed to review?" The agent searches its memory and past sessions via FTS5 and reminds you. The session isolation means your DMs stay private while group chats get their own context.

### Unattended Scheduled Automation

**The scenario:** You want a daily briefing of your GitHub notifications, competitor news, and server health — delivered to Telegram every morning at 8 AM.

**How it works:** Set up a cron job through the agent (natural language scheduling is supported). Each morning, the scheduler triggers a fresh agent session with full tool access. The agent runs web searches, checks GitHub, queries your server status, compiles the results, and delivers them to your Telegram chat. This is a full agent session, not a shell script — it can reason about what it finds and highlight things that need attention.

### Parallel Workstreams with Delegates

**The scenario:** You need to research three competing API providers simultaneously, compare their pricing, and produce a recommendation.

**How it works:** You ask Hermes to compare providers A, B, and C. The agent spawns three isolated **subagents** (delegates), each with its own conversation context and terminal. Each delegate researches one provider independently — browsing documentation, extracting pricing tables, running test API calls. The results flow back to the main agent, which synthesizes everything into a comparison. This happens in parallel, collapsing what would be a serial multi-hour research session into a single coordinated operation.

### Serverless Development Environment

**The scenario:** You want an always-available coding agent, but you don't want to pay for a VPS 24/7.

**How it works:** Configure Hermes with the **Daytona** or **Modal** terminal backend. Your agent's development environment runs in the cloud — but it hibernates when idle and wakes on demand. You message from Telegram: "Fix the failing tests in the auth module." The serverless environment spins up, the agent clones your repo, runs tests, patches the code, verifies the fix, and reports back. Then the environment goes back to sleep. You pay only for active compute time.

### AI Research and Training Data Generation

**The scenario:** You're a researcher who needs thousands of tool-calling trajectories to fine-tune an LLM.

**How it works:** Hermes includes a **batch runner** and integration with [Atropos](https://github.com/NousResearch/tinker-atropos) (Nous Research's RL training framework). You define task prompts and tool distributions, then run batch generation with configurable workers and automatic checkpointing. Each trajectory captures the full agent loop — prompt, tool calls, tool results, final response — in a format ready for training. This is the research infrastructure that Nous uses internally to train the next generation of Hermes models.

### Extending with MCP Servers

**The scenario:** You want Hermes to interact with your company's internal tools — Jira, Notion, a custom database — without writing custom tool code.

**How it works:** If your tools expose an MCP (Model Context Protocol) server, Hermes can connect to them natively. Configure the MCP server URL in your config, optionally filter which tools are exposed, and the agent gains access to those capabilities immediately. The MCP tool implementation is substantial (~2,200 lines), suggesting this is a well-supported integration path, not an afterthought.

> 📖 **Docs:** [Skills System](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills) · [Memory System](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory) · [Cron](https://hermes-agent.nousresearch.com/docs/user-guide/features/cron) · [MCP Integration](https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp) · [Voice Mode](https://hermes-agent.nousresearch.com/docs/user-guide/features/voice-mode) · [Tips & Best Practices](https://hermes-agent.nousresearch.com/docs/guides/tips)

---

## Quick Reference Links

| Topic | URL |
|---|---|
| Home / Overview | [hermes-agent.nousresearch.com/docs](https://hermes-agent.nousresearch.com/docs/) |
| Installation | [.../getting-started/installation](https://hermes-agent.nousresearch.com/docs/getting-started/installation) |
| Quickstart | [.../getting-started/quickstart](https://hermes-agent.nousresearch.com/docs/getting-started/quickstart) |
| Architecture | [.../developer-guide/architecture](https://hermes-agent.nousresearch.com/docs/developer-guide/architecture) |
| Memory System | [.../user-guide/features/memory](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory) |
| Skills System | [.../user-guide/features/skills](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills) |
| Tools & Toolsets | [.../user-guide/features/tools](https://hermes-agent.nousresearch.com/docs/user-guide/features/tools) |
| Messaging Gateway | [.../user-guide/messaging](https://hermes-agent.nousresearch.com/docs/user-guide/messaging) |
| MCP Integration | [.../user-guide/features/mcp](https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp) |
| Security | [.../user-guide/security](https://hermes-agent.nousresearch.com/docs/user-guide/security) |
| FAQ | [.../reference/faq](https://hermes-agent.nousresearch.com/docs/reference/faq) |
| GitHub | [github.com/NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) |
| Discord | [discord.gg/NousResearch](https://discord.gg/NousResearch) |
| Skills Hub | [agentskills.io](https://agentskills.io) |
