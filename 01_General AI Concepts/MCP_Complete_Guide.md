# The Complete Guide to the Model Context Protocol (MCP)

**From Zero to Working Understanding — For the Technically Curious**

*Last updated: April 2026*

---

## Table of Contents

1. [The Problem  Solves](#1-the-problem-mcp-solves)
2. [What MCP Actually Is](#2-what-mcp-actually-is)
3. [Core Concepts: The Cast of Characters](#3-core-concepts-the-cast-of-characters)
4. [What an MCP Server Is and How It Works](#4-what-an-mcp-server-is-and-how-it-works)
5. [The Communication Layer: JSON-RPC Under the Hood](#5-the-communication-layer-json-rpc-under-the-hood)
6. [Transports: How the Messages Get There](#6-transports-how-the-messages-get-there)
7. [Where MCP Fits in the AI Architecture](#7-where-mcp-fits-in-the-ai-architecture)
8. [MCP Clients and Hosts: The Major Players](#8-mcp-clients-and-hosts-the-major-players)
   - [Claude Desktop](#claude-desktop)
   - [Claude Code](#claude-code)
   - [OpenClaw (formerly Clawdbot)](#openclaw-formerly-clawdbot)
   - [Hermes Agent (Nous Research)](#hermes-agent-nous-research)
   - [Cursor, Windsurf, and IDE-Based Clients](#cursor-windsurf-and-ide-based-clients)
   - [Other Notable Clients](#other-notable-clients)
9. [Practical Configuration Examples](#9-practical-configuration-examples)
10. [Security Considerations](#10-security-considerations)
11. [The MCP Ecosystem in 2026](#11-the-mcp-ecosystem-in-2026)
12. [Where to Go from Here](#12-where-to-go-from-here)

---

## 1. The Problem MCP Solves

Imagine you have a brilliant assistant who can write code, analyze data, draft reports, and reason through complex problems — but they're locked in a windowless room with no phone, no internet, and no filing cabinet. That's what large language models (LLMs) like Claude, GPT, or Gemini have been for most of their existence: incredibly capable thinkers trapped inside a text box.

Every time you wanted your AI to *do* something in the real world — check your calendar, query a database, read a file on your computer, create a GitHub issue — someone had to build a custom integration. A bespoke bridge. And here's the painful part: if you had 10 AI applications and 10 external tools, you potentially needed **100 separate integrations** (10 × 10). Every AI app had to write its own connector for every tool. This is what Anthropic called the **N×M integration problem**.

Before MCP, the industry tried to solve this in fragments. OpenAI introduced function calling in 2023 and a ChatGPT plugin system. Other providers had their own approaches. But each solution was vendor-specific — a plugin built for ChatGPT wouldn't work with Claude, and vice versa. The connectors were not interchangeable.

MCP was created to solve this by establishing **one universal standard** for connecting AI models to external tools and data. Build a connector once, and any MCP-compatible AI application can use it. Build an AI application once, and it can use any MCP-compatible connector. The N×M problem collapses to **N+M**.

---

## 2. What MCP Actually Is

The **Model Context Protocol (MCP)** is an open standard introduced by Anthropic in November 2024 that defines how AI applications communicate with external data sources and tools. Think of it as **USB-C for AI** — a universal port that lets any AI system plug into any external capability, as long as both sides speak the same protocol.

A few key facts about MCP:

- It is an **open specification**, not a proprietary product. The spec is public and anyone can implement it.
- It uses **JSON-RPC 2.0** as its message format — a well-established, lightweight protocol for remote procedure calls.
- It was inspired by the **Language Server Protocol (LSP)**, which standardized how code editors communicate with language-specific intelligence (autocomplete, error checking, etc.). LSP meant that a single "language server" for Python could work in VS Code, Sublime Text, Vim, and any other LSP-compatible editor. MCP does the same thing, but for AI-to-tool communication instead of editor-to-language communication.
- In December 2025, Anthropic donated governance of MCP to the **AgentFile Foundation**, making it a true community-governed standard.
- As of early 2026, MCP has been adopted by **OpenAI, Google DeepMind, Microsoft, and hundreds of tool providers**. It is the de facto standard for AI-tool integration.

The latest specification version is **2025-11-25**, which added asynchronous tasks, improved OAuth authorization, parallel tool calls, and server-side agent loops.

---

## 3. Core Concepts: The Cast of Characters

MCP has a small number of core concepts. Let's meet them using an analogy: a **restaurant kitchen**.

### Hosts

The **host** is the overall application the user interacts with — the restaurant itself. Examples include Claude Desktop, Cursor, Windsurf, or a custom AI application you've built. The host is responsible for managing the user experience, creating MCP client instances, and enforcing security policies. You (the diner) walk into the restaurant (the host) and interact with it directly.

### Clients

The **client** is the waiter. Each MCP client maintains a **1:1 connection** with a single MCP server. The host creates one client for each server it needs to talk to. So if Claude Desktop is connected to both a GitHub server and a Postgres server, it has two client instances running — one waiter dedicated to each kitchen station.

### Servers

The **server** is the specialized kitchen station — the grill, the pastry station, the sushi bar. Each MCP server exposes a specific set of capabilities related to one domain. A GitHub MCP server knows how to create issues, list pull requests, and search code. A filesystem MCP server knows how to read and write files. A database MCP server knows how to run queries. Servers are lightweight, focused programs. They do NOT contain an LLM themselves — they are tools that an LLM can call through.

### Tools

**Tools** are actions the AI can execute — the specific dishes a kitchen station can prepare. A GitHub server might expose tools like `create_issue`, `list_pull_requests`, `merge_branch`, and `search_code`. Each tool has a defined name, a description (so the LLM understands when to use it), and a schema describing what input parameters it accepts and what output it returns.

Tools are the most commonly used MCP primitive. When people say "MCP lets Claude query my database," they mean the database MCP server exposes a tool that accepts a SQL query and returns results.

### Resources

**Resources** are data the AI can read — the menu, the recipe book, the ingredient list. A Notion MCP server might expose your pages and databases as resources. A filesystem server might expose directory listings. Resources are read-only and are identified by URIs (like `file:///home/user/project/README.md` or `notion://page/abc123`). They give the AI contextual information without requiring it to take an action.

### Prompts

**Prompts** are pre-written templates that help users interact with a server's capabilities — like the specials board that says "Try our chef's tasting menu." A database MCP server might offer a prompt template called "Analyze Table" that pre-fills a useful starting query structure. Prompts are user-selectable (the person chooses to use them), unlike tools which the AI model invokes programmatically.

### How They All Fit Together

```
┌─────────────────────────────────────────────┐
│                   HOST                       │
│  (Claude Desktop, Cursor, your app, etc.)   │
│                                              │
│   ┌──────────┐  ┌──────────┐  ┌──────────┐ │
│   │ Client 1 │  │ Client 2 │  │ Client 3 │ │
│   └────┬─────┘  └────┬─────┘  └────┬─────┘ │
└────────┼─────────────┼─────────────┼────────┘
         │             │             │
    ┌────▼─────┐  ┌────▼─────┐  ┌───▼──────┐
    │ GitHub   │  │ Postgres │  │Filesystem│
    │ Server   │  │ Server   │  │ Server   │
    │          │  │          │  │          │
    │ Tools:   │  │ Tools:   │  │ Tools:   │
    │ -create  │  │ -query   │  │ -read    │
    │  _issue  │  │ -describe│  │ -write   │
    │ -list_pr │  │  _table  │  │ -search  │
    └──────────┘  └──────────┘  └──────────┘
```

Each client-server connection is independent. If the GitHub server crashes, the Postgres connection keeps working. The host orchestrates everything and presents a unified experience to the user.

---

## 4. What an MCP Server Is and How It Works

An MCP server is a **program** — usually small, often a single script — that does three things:

1. **Starts up** and waits for a client to connect.
2. **Announces its capabilities** during an initialization handshake (what tools, resources, and prompts it offers).
3. **Responds to requests** from the client: executing tools, returning resource data, or listing available prompts.

### The Lifecycle

When a host application wants to use an MCP server, the following happens:

1. **Launch**: The host starts the server process (for local servers) or connects to a remote URL (for HTTP servers).
2. **Initialize**: The client sends an `initialize` request. The server responds with its name, version, and a list of capabilities it supports.
3. **Discover**: The client calls methods like `tools/list`, `resources/list`, and `prompts/list` to learn exactly what the server offers.
4. **Operate**: During normal use, the LLM decides it needs a tool (e.g., "I should query the database to answer this question"). The client sends a `tools/call` request to the appropriate server. The server executes the action and returns the result.
5. **Shutdown**: When the host closes, it sends a shutdown notification, and the server cleans up.

### What Can MCP Servers Do? Concrete Examples

The range of MCP servers available is enormous. Here are categories with real examples:

**Filesystem access** — Read, write, search, and manage files on your local machine. The reference server (`@modelcontextprotocol/server-filesystem`) is often the first one people try.

**Database queries** — Servers exist for PostgreSQL, MySQL, SQLite, MongoDB, and Redis. They let the AI run queries, explore schemas, and analyze data directly.

**Code repositories** — The GitHub MCP server lets an AI create issues, manage pull requests, search code, and review diffs. GitLab and Bitbucket servers also exist.

**Productivity tools** — Notion, Slack, Jira, Confluence, Linear, Asana, and Google Workspace all have MCP servers. Your AI can read your Notion docs, post to Slack, or create Jira tickets.

**Browser automation** — Playwright MCP lets an AI control a browser: navigate pages, click buttons, fill forms, take screenshots.

**Cloud infrastructure** — AWS, GCP, DigitalOcean, and Cloudflare offer MCP servers for managing cloud resources.

**Monitoring and observability** — Sentry's MCP server lets an AI query production errors and stack traces.

**Custom internal tools** — You can write an MCP server that wraps any internal API, making your proprietary systems accessible to AI agents.

---

## 5. The Communication Layer: JSON-RPC Under the Hood

MCP uses **JSON-RPC 2.0**, a simple protocol where every message is a JSON object with a specific structure. There are three types of messages:

**Requests** — "Please do something and tell me the result."
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "tools/call",
  "params": {
    "name": "create_issue",
    "arguments": {
      "repo": "my-org/my-repo",
      "title": "Fix login bug",
      "body": "Users report 500 errors on the login page."
    }
  }
}
```

**Responses** — "Here's the result of what you asked."
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "content": [
      {
        "type": "text",
        "text": "Created issue #42: Fix login bug"
      }
    ]
  }
}
```

**Notifications** — "FYI, something happened." (No response expected.)
```json
{
  "jsonrpc": "2.0",
  "method": "notifications/tools/list_changed"
}
```

That's it. Every MCP interaction is one of these three shapes. The simplicity is deliberate — it keeps implementations lightweight and interoperable.

---

## 6. Transports: How the Messages Get There

The JSON-RPC messages need a way to travel between client and server. MCP defines several **transport** mechanisms:

### stdio (Standard Input/Output)

The client launches the server as a **child process** on the same machine. Messages flow through the process's stdin and stdout pipes. This is the most common transport for local tools.

**When to use it**: Local development tools, filesystem access, database servers running on your machine. The server starts when you start your AI app and stops when you close it.

### HTTP (Streamable HTTP)

The client connects to the server over **HTTP**. This is the recommended transport for remote/cloud-hosted MCP servers. It supports streaming via Server-Sent Events (SSE) for long-running operations.

**When to use it**: Cloud services, shared team servers, SaaS integrations (like connecting to Notion's hosted MCP endpoint at `https://mcp.notion.com/mcp`).

### SSE (Server-Sent Events) — Deprecated

An older transport that used SSE for server-to-client streaming. It has been superseded by the Streamable HTTP transport but is still supported by many clients for backward compatibility.

### Which Transport Does What?

| Transport | Server Location | Authentication | Use Case |
|-----------|----------------|----------------|----------|
| stdio | Local machine | Not needed (same machine) | Dev tools, filesystems, local DBs |
| HTTP | Remote/cloud | OAuth 2.1, API keys, Bearer tokens | SaaS integrations, team services |
| SSE | Remote (legacy) | Headers | Older remote servers |

---

## 7. Where MCP Fits in the AI Architecture

MCP doesn't exist in isolation. It's one piece of a larger puzzle. Here's how it relates to other concepts you'll encounter:

### MCP vs. Function Calling / Tool Use

**Function calling** (or "tool use") is the mechanism by which an LLM decides it needs to invoke an external function and generates a structured request for it. This is a capability of the model itself — Claude, GPT-4, Gemini all support it natively.

**MCP is the standardized plumbing** that connects function calling to actual tools. Without MCP, every application has to implement its own way of registering tools, routing calls, and returning results. With MCP, there's a single protocol for all of it. Think of function calling as the LLM saying "I need to use a tool." MCP is the system that makes that tool available and handles the communication.

### MCP vs. RAG (Retrieval-Augmented Generation)

**RAG** is a pattern where you retrieve relevant documents from a knowledge base and inject them into the LLM's context before it generates a response. MCP's **resources** primitive can serve as the retrieval layer for RAG — a server can expose documents, database records, or API responses as resources that get pulled into the model's context. MCP doesn't replace RAG; it can be the *delivery mechanism* for it.

### MCP and AI Agents

An **AI agent** is an LLM that can take autonomous actions in a loop — observe, think, act, observe the result, think again, act again. MCP is what gives agents their *hands*. Without MCP (or similar integration), an agent can only think and talk. With MCP, an agent can read your codebase, run tests, check error logs, create tickets, and deploy fixes — all in a single autonomous loop.

The November 2025 spec update made MCP significantly more agent-friendly by adding **asynchronous tasks** (start a long job, check back later), **server-side agent loops** (servers can run multi-step reasoning internally), and **parallel tool calls**.

### MCP and the Broader Trend

The AI industry is moving from "chat with a model" toward "delegate work to an agent." MCP is foundational infrastructure for this shift. It's the reason Claude Code can manage your GitHub repos, query your database, and deploy your app — all from a terminal. It's why Cursor can pull in Figma designs and turn them into code. It's the universal bridge between AI reasoning and real-world action.

---

## 8. MCP Clients and Hosts: The Major Players

### Claude Desktop

**What it is**: Anthropic's official desktop application for interacting with Claude. It was the **first mainstream MCP client** and remains the reference implementation.

**How MCP works in it**: Claude Desktop reads MCP server configurations from a JSON file on your machine. When you start the app, it launches any configured stdio servers and connects to any configured HTTP servers. Tools from all connected servers appear alongside Claude's native capabilities.

**Configuration file location**:
- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`
- Linux: `~/.config/Claude/claude_desktop_config.json`

**Example configuration**:
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/home/user/projects"
      ]
    },
    "github": {
      "command": "docker",
      "args": [
        "run", "-i", "--rm",
        "-e", "GITHUB_PERSONAL_ACCESS_TOKEN",
        "ghcr.io/github/github-mcp-server"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_your_token"
      }
    }
  }
}
```

After saving the file and restarting Claude Desktop, a hammer icon appears in the interface indicating MCP tools are available. Claude can then use these tools during conversation when relevant.

**Best for**: Non-developers or anyone who wants a GUI-based AI chat with tool access. The smoothest onboarding experience with one-click extension installation via Settings → Extensions.

---

### Claude Code

**What it is**: Anthropic's **terminal-based CLI coding agent**. Unlike Claude Desktop's chat interface, Claude Code runs in your terminal and has direct access to your project directory. It can read/write files, run shell commands, and perform multi-step coding tasks autonomously.

**How MCP works in it**: Claude Code acts as an MCP client that can connect to external servers, extending its already considerable built-in capabilities (file editing, bash execution, grep, glob, etc.). It can also act as an MCP *server* itself, exposing its tools to other clients.

**Adding servers via CLI**:
```bash
# Add a remote HTTP server (recommended for cloud services)
claude mcp add --transport http notion https://mcp.notion.com/mcp

# Add a remote server with authentication
claude mcp add --transport http secure-api https://api.example.com/mcp \
  --header "Authorization: Bearer your-token"

# Add a local stdio server
claude mcp add my-db -- npx -y @modelcontextprotocol/server-postgres

# Add from a JSON blob
claude mcp add-json github '{"command":"npx","args":["-y","@modelcontextprotocol/server-github"],"env":{"GITHUB_PERSONAL_ACCESS_TOKEN":"ghp_xxx"}}'
```

**Configuration scopes**:
- **local** (default): Available only to you in the current project directory. Stored in `.claude.json`.
- **project**: Shared with everyone on the project via `.mcp.json` at the project root. Good for team-shared servers.
- **user**: Available to you across all projects. Stored in your global Claude settings.

```bash
# Add to project scope (team-shared)
claude mcp add --scope project filesystem -- npx -y \
  @modelcontextprotocol/server-filesystem /tmp

# Add to user scope (global, personal)
claude mcp add --scope user my-db -- npx -y \
  @my-org/mcp-server-postgres
```

**Managing servers**:
```bash
claude mcp list              # Show all configured servers
claude mcp remove github     # Remove a server
claude mcp get github        # Show details of a specific server
```

Inside Claude Code, use the `/mcp` command to see server status, enable/disable servers, or trigger reconnection.

**Dual nature** — Claude Code can simultaneously:
- Act as an **MCP client**: Consuming tools from GitHub, Postgres, or any other MCP server you've configured.
- Act as an **MCP server**: Exposing its own tools (Bash, Read, Write, Edit, LS, GrepTool, GlobTool) to other clients via `claude mcp serve`.

This means you can have Cursor or another AI tool delegate complex coding tasks to Claude Code through MCP. It's agents orchestrating agents.

**Best for**: Professional developers who want AI coding assistance in the terminal with extensible tool access.

---

### OpenClaw (formerly Clawdbot)

**What it is**: An **open-source personal AI agent platform** that has become one of the fastest-growing agent frameworks. OpenClaw provides a persistent, always-on AI agent that connects to messaging platforms (Telegram, Discord, WhatsApp, Signal, SMS, and more) and can run 24/7 on your own hardware. It started as "Clawdbot," was briefly called "Moltbot," and is now known as OpenClaw.

**Architecture**: OpenClaw consists of several components:
- **Gateway Daemon**: The always-on core service that routes messages, manages sessions, and coordinates agents over WebSockets.
- **Agent Runtime**: Executes the agent's reasoning loop, memory, plugins/skills, and tool calls.
- **Skills / Plugin System**: Extensible tool layer. MCP servers integrate here.
- **Interfaces**: Web Admin UI, CLI (TUI), and messaging platform channels.
- **MCPorter**: A TypeScript runtime and CLI for managing MCP servers within OpenClaw.

**How MCP works in OpenClaw**: OpenClaw has deep, native MCP integration. It can both *consume* MCP servers (connecting to external tools) and *expose itself* as an MCP server (letting other clients like Claude Code interact with OpenClaw conversations).

**Consuming MCP servers** — Configure in `~/.openclaw/config.yaml` (or equivalent):
```yaml
mcp_servers:
  github:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-github"]
    env:
      GITHUB_PERSONAL_ACCESS_TOKEN: "ghp_xxx"
    tools:
      include: [list_issues, create_issue, search_code]
  
  postgres:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-postgres"]
    env:
      POSTGRES_CONNECTION_STRING: "postgresql://user:pass@localhost/mydb"
  
  remote-api:
    url: "https://mcp.example.com"
    headers:
      Authorization: "Bearer sk-xxx"
```

**Managing MCP servers via CLI**:
```bash
openclaw mcp list                           # List saved MCP server definitions
openclaw mcp show context7 --json           # Show config for a specific server
openclaw mcp set context7 '{"command":"uvx","args":["context7-mcp"]}'
openclaw mcp set docs '{"url":"https://mcp.example.com"}'
openclaw mcp unset context7                 # Remove a server definition
```

**Exposing OpenClaw as an MCP server** — This lets external MCP clients (like Claude Code or Claude Desktop) interact with OpenClaw conversations:
```bash
openclaw mcp serve --url wss://gateway-host:18789 --token-file /path/to/token
```

Configuration for a client to connect to OpenClaw's MCP server:
```json
{
  "mcpServers": {
    "openclaw": {
      "command": "openclaw",
      "args": [
        "mcp", "serve",
        "--url", "wss://gateway-host:18789",
        "--token-file", "/path/to/gateway.token"
      ]
    }
  }
}
```

The bridge exposes tools like `conversations_list`, `messages_read`, `messages_send`, `events_poll`, and `events_wait`, giving external clients access to OpenClaw's conversation channels.

**Key MCP-related features**:
- Supports both **stdio** and **HTTP/SSE** transports.
- **Tool filtering**: Include/exclude specific tools per server to limit what the agent can access.
- **ClawHub**: A registry of pre-built skills and MCP server integrations.
- **MCPorter**: Manages MCP server lifecycles, can install servers directly from GitHub repos.
- Works with any LLM backend — Claude, GPT, Gemini, or local models via Ollama.

**Best for**: Self-hosters who want a persistent AI agent running on their own hardware with messaging platform integration and full MCP extensibility.

---

### Hermes Agent (Nous Research)

**What it is**: An open-source AI agent framework from **Nous Research** (the team behind the Hermes family of open-source LLMs). Hermes Agent is a self-improving agent that runs locally, supports multiple messaging platforms, and has deep MCP integration. It's research-oriented but fully functional for production use.

**How MCP works in Hermes**: Hermes has a built-in MCP client that connects to servers at startup, discovers their tools, and registers them as first-class tools the agent can call. MCP tools appear alongside built-in tools — the agent sees no difference.

**Configuration** in `~/.hermes/config.yaml`:
```yaml
mcp_servers:
  filesystem:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-filesystem", "/home/user/projects"]
  
  github:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-github"]
    env:
      GITHUB_PERSONAL_ACCESS_TOKEN: "ghp_xxx"
    tools:
      include: [list_issues, create_issue, search_code]
    prompts: false
    resources: false
  
  stripe:
    url: "https://mcp.stripe.com"
    headers:
      Authorization: "Bearer sk_xxx"
    tools:
      exclude: [delete_customer, refund_payment]
```

**Notable Hermes MCP features**:

- **Per-server tool filtering**: Use `include` or `exclude` lists to control exactly which tools the agent can see. Critical for sensitive integrations like payment processors.
- **Capability toggling**: Disable `prompts` and/or `resources` per server if you only want tools.
- **Security-first environment handling**: Hermes does NOT pass your full shell environment to MCP subprocesses. Only safe baseline variables (`PATH`, `HOME`, `USER`, `LANG`, etc.) are inherited. API keys and secrets are excluded unless explicitly added via the `env` config key. This prevents accidental credential leakage.
- **Credential redaction**: If an MCP tool call fails, credential-like patterns in error messages are automatically redacted before being shown to the LLM.
- **Dynamic tool updates**: Servers can notify Hermes when their tool list changes at runtime, and Hermes automatically re-fetches and updates the registry.
- **Auto-reconnection**: If a server connection drops, Hermes reconnects with exponential backoff (up to 5 retries, 60-second max).

Hermes can also **expose itself as an MCP server**:
```bash
hermes mcp serve          # Normal mode
hermes mcp serve --verbose  # Debug logging
```

This exposes 10 tools for interacting with Hermes conversations, plus a live event bridge for near-real-time awareness of incoming messages.

**Hermes MCP** (separate project): There is also an **Elixir SDK** called `hermes_mcp` (by CloudWalk) for building MCP servers and clients in Elixir/Phoenix applications. It shares the name but is a different project from Hermes Agent.

**Best for**: Researchers and advanced users who want a self-hosted, multi-platform agent with fine-grained MCP control and a focus on security.

---

### Cursor, Windsurf, and IDE-Based Clients

These AI-powered code editors have all adopted MCP as their standard for tool extensibility.

#### Cursor

Cursor is a **VS Code fork** built for AI-native development. It has strong MCP support with high tool execution reliability.

**Configuration**: Create or edit `.cursor/mcp.json` in your project root:
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxx"
      }
    },
    "supabase": {
      "url": "https://mcp.supabase.com/sse",
      "headers": {
        "Authorization": "Bearer your-access-token"
      }
    }
  }
}
```

You can also add servers through Settings → MCP in the Cursor UI. Cursor picks up config changes automatically without requiring a restart.

#### Windsurf

Windsurf (formerly by Codeium, now under Cognition/Devin) uses a similar JSON configuration approach. Its agent-first "Cascade" mode can autonomously use MCP tools across multi-step workflows.

**Configuration**: Uses `mcp_config.json` accessible through the MCP section in Windsurf settings:
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxx"
      }
    }
  }
}
```

#### VS Code (via Extensions)

If you prefer vanilla VS Code, several extensions add MCP support:
- **Continue**: Open-source AI assistant with full MCP support and multi-provider LLM access.
- **Cline**: Open-source AI coding agent with a dedicated MCP management panel in the sidebar.
- **GitHub Copilot**: Now supports MCP through its Chat panel.

All of these use the same `mcpServers` JSON format, making server configs portable across clients.

---

### Other Notable Clients

| Client | Type | Notes |
|--------|------|-------|
| **Gemini CLI** | Terminal | Google's CLI tool with MCP support for automation |
| **OpenCode** | Terminal | Open-source (MIT), supports 75+ LLM providers, MCP-compatible |
| **Zed** | Editor | Experimental MCP support, known for high performance |
| **LibreChat** | Web app | Open-source, self-hostable chat with multi-LLM and MCP |
| **Cline** | VS Code ext. | Autonomous coding agent with MCP sidebar management |
| **Goose** | CLI | Block's open-source AI agent with MCP integration |
| **Lovable** | Web app builder | Browser-based, supports remote HTTP MCP servers |

---

## 9. Practical Configuration Examples

### Example 1: Full-Stack Developer Setup

A developer who works with GitHub, PostgreSQL, and needs browser testing:

```json
{
  "mcpServers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp",
      "headers": {
        "Authorization": "Bearer ghp_your_token"
      }
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "POSTGRES_CONNECTION_STRING": "postgresql://dev:pass@localhost:5432/myapp"
      }
    },
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp-server"]
    },
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/home/dev/projects/myapp"
      ]
    }
  }
}
```

### Example 2: OpenClaw Multi-Server Orchestration

An OpenClaw config that lets the agent monitor GitHub issues, post to Slack, and query a database:

```yaml
mcp_servers:
  github:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-github"]
    env:
      GITHUB_PERSONAL_ACCESS_TOKEN: "ghp_xxx"
    tools:
      include: [list_issues, create_issue, update_issue, search_code]
    prompts: false
    resources: false
  
  slack:
    url: "https://mcp.slack.example.com"
    headers:
      Authorization: "Bearer xoxb-xxx"
    tools:
      exclude: [delete_message]
  
  sqlite:
    command: "uvx"
    args: ["mcp-server-sqlite", "--db-path", "/data/analytics.db"]
```

### Example 3: Hermes Agent with Security-First Config

Hermes config for a production environment with strict tool filtering:

```yaml
mcp_servers:
  internal_api:
    url: "https://mcp.internal.example.com"
    headers:
      Authorization: "Bearer sk-internal-xxx"
    tools:
      include: [list_customers, get_customer, list_invoices]
    resources: false
    prompts: false
  
  docs:
    url: "https://mcp.docs.example.com"
    tools:
    prompts: true
    resources: true
```

### Example 4: Claude Code Team Setup

A shared `.mcp.json` at the project root (committed to Git):
```json
{
  "mcpServers": {
    "project-db": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "POSTGRES_CONNECTION_STRING": "$DATABASE_URL"
      }
    }
  }
}
```

Each developer sets `DATABASE_URL` in their own shell environment. The config is shared; the credentials are not.

---

## 10. Security Considerations

MCP is powerful — and power requires caution. The protocol gives AI agents the ability to read files, execute code, send messages, and modify data. Here are the critical security principles:

**Principle of least privilege**: Only give each MCP server the minimum permissions it needs. If the agent only needs to read from your database, configure a read-only connection. Use tool include/exclude lists to limit exposed capabilities.

**Credential isolation**: Never put secrets directly in shared config files. Use environment variables (`$DATABASE_URL`, `$GITHUB_TOKEN`) that each developer sets locally. Both OpenClaw and Hermes are designed to avoid passing your full shell environment to MCP subprocess — they only forward variables you explicitly declare.

**Tool descriptions are untrusted**: The MCP spec explicitly warns that tool descriptions and annotations should be considered untrusted unless they come from a verified server. A malicious MCP server could describe a tool as "harmless file reader" when it actually exfiltrates data.

**Human-in-the-loop**: Most MCP clients (Claude Code, Claude Desktop, Cursor) display a permission prompt before executing any MCP tool call, showing the tool name and arguments. Don't blindly approve without reviewing.

**Audit logging**: Enable verbose logging for MCP calls so you can review what tools the agent invoked and what data it accessed.

**The OAuth 2.1 model**: The 2025-11-25 spec modernized authorization with OAuth 2.1, treating MCP servers as OAuth Resource Servers with mandatory Resource Indicators to prevent token misuse across services.

---

## 11. The MCP Ecosystem in 2026

The MCP ecosystem has grown dramatically since the protocol's launch in late 2024:

- The **MCP Registry** (launched September 2025) is the central index for discovering MCP servers, with close to 2,000 entries.
- **SDK support** spans multiple languages: TypeScript/JavaScript, Python, Elixir, Go, Rust, Java, and more. SDKs are tiered by compliance level and maintenance quality.
- **Specification Enhancement Proposals (SEPs)** provide a formal governance process for contributing protocol changes.
- Major features in the **2025-11-25 spec**: asynchronous Tasks, server-side agent loops, parallel tool calls, standardized tool name format, and improved OAuth authorization.
- **Protocol extensions** are being documented for industry-specific use cases (healthcare, finance, education) that don't belong in the core spec but benefit from shared patterns.

The trajectory is clear: MCP is moving from an experimental protocol into production-grade infrastructure. Organizations are building on it with intent, and the ecosystem's rapid growth (from a handful of servers in 2024 to thousands in 2026) reflects genuine adoption, not just hype.

---

## 12. Where to Go from Here

**Read the spec**: [modelcontextprotocol.io/specification](https://modelcontextprotocol.io/specification/2025-11-25) — the authoritative source.

**Browse servers**: The MCP Registry at [modelcontextprotocol.io](https://modelcontextprotocol.io) and community lists like `awesome-mcp-servers` on GitHub.

**Try it hands-on**: Install Claude Desktop or Claude Code, add a filesystem MCP server, and ask Claude to explore your project directory. The fastest way to understand MCP is to use it.

**Build a server**: The TypeScript and Python SDKs make it straightforward to wrap any API as an MCP server. Start with a "Hello World" tool, then wrap an internal API you use daily.

**Key resources**:
- MCP specification and docs: `modelcontextprotocol.io`
- Claude Code MCP docs: `code.claude.com/docs/en/mcp`
- OpenClaw MCP docs: `docs.openclaw.ai/cli/mcp`
- Hermes Agent MCP docs: `hermes-agent.nousresearch.com/docs/user-guide/features/mcp/`
- MCP Inspector: A web debugger for testing MCP servers in isolation before connecting them to agents.

---

*This guide reflects the state of the MCP ecosystem as of April 2026. The protocol is evolving rapidly — check the official spec and tool documentation for the latest changes.*
