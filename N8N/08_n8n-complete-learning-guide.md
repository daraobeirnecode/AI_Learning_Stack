# n8n Complete Learning Guide

> **Source**: [TheOwlLogic.com](https://theowllogic.com) by Shajid Shafee
> **Compiled**: April 2026
> **Image note**: Images reference the original hosted URLs. Run the companion `download-images.sh` script to save all images locally into an `assets/` folder, then find-replace the base URL with `assets/` for fully offline Obsidian viewing.

---

# PART ONE — THEORY FOUNDATIONS

These ten chapters build your conceptual foundation with n8n, from installation through advanced patterns like sub-workflows.

---

## Chapter 1 · Installing n8n Locally (Windows + Mac)

**Source**: https://theowllogic.com/installing-n8n-setup-locally

Running n8n on your own machine is free — no execution limits, no workflow caps, and all data stays on your hardware. There are two installation methods: NPM (fastest setup, best for users already comfortable with Node.js) and Docker (more robust, production-like isolation). NPM requires a minimum of 4 GB RAM; Docker recommends 8 GB since the Docker engine itself consumes resources.

![Installing n8n locally with npm and docker](https://api.theowllogic.com/wp-content/uploads/2025/11/Installing-n8n-locally-with-npm-and-docker.png)

### NPM Installation

**Step 0 (Optional) — Install Warp Terminal**

Warp ([warp.dev](https://www.warp.dev/)) is a free, cross-platform terminal that avoids some of the PowerShell security headaches Windows users encounter. This step is entirely optional but recommended if you run into terminal issues.

**Step 1 — Check for Node.js**

Open your terminal (or Warp) and run:

```bash
node -v
npm -v
```

If both commands return version numbers, you already have Node.js. Skip to Step 3.

**Step 2 — Install Node.js**

Go to [nodejs.org](https://nodejs.org), download the LTS version for your operating system, and run the installer.

![Node.js download page](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251005235756-1024x784.png)

**Step 3 — Verify Installation**

Run `node -v` in your terminal. You should see a version number.

**Step 4 — Install n8n**

```bash
npm install -g n8n
```

Installation takes approximately 3–5 minutes. Once complete, type `n8n` to launch the editor.

![n8n npm install terminal output](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006005539-1024x625.png)

![n8n running in terminal](https://api.theowllogic.com/wp-content/uploads/2025/11/image.png)

The editor is now accessible at http://localhost:5678 — you can also press `o` in the terminal to open it directly in your browser. Register an account and verify your email to finish setup.

![n8n registration screen](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006001030-1024x465.png)

**Everyday NPM commands:**

| Action | Command |
|--------|---------|
| Stop n8n | `Ctrl + C` in the terminal |
| Start n8n again | `n8n` |
| Uninstall n8n | `npm uninstall -g n8n` |

![n8n terminal tips](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006095451-1024x597.png)

### Docker Installation

Docker packages n8n and all its dependencies into a single container that runs identically on any machine. You'll need at least 4 GB of RAM for the container, though 8 GB is recommended — Docker typically stabilises around 1.7–2.0 GB of RAM usage.

**Step 1 — Install Docker Desktop**

Download from [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/) (~517 MB). Choose your OS, install, and launch Docker Desktop.

![Docker Desktop download](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006002149-1024x464.png)

**Step 2 — Create a Persistent Volume**

This is essential — without a volume, all your credentials and workflows vanish when the container stops. Think of it as a folder that persists across container restarts.

1. In Docker Desktop go to **Volumes → Create**
2. Name it `n8n-data`
3. Click **Create**

![Docker create volume screen](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006100148-1024x618.png)

![Docker volume naming](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006100448-1024x626.png)

![Docker volume created](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006100540-1024x632.png)

**Step 3 — Pull the n8n Image**

1. In Docker Desktop go to **Images → Search images to run**
2. Search for `n8nio/n8n` (100M+ downloads, official image)
3. Click **Pull**

![Docker search n8n image](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006003958-1024x587.png)

![Docker pulling n8n image](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006004229-1024x616.png)

**Step 4 — Configure and Run the Container**

Click the **Play** button on the image, then expand **Optional Settings**.

![Docker optional settings](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006101415-1024x630.png)

![Docker container config](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006101604-1024x625.png)

Configure these fields:

- **Container name**: anything you like (e.g., `n8n-local`)
- **Ports**: Host Port → `5678`
- **Volumes**: Host path = `n8n_data`, Container path = `/home/node/.n8n`

Now expand **Environment Variables** and add four entries (click the `+` icon to add more rows):

![Docker env vars](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006102134-1024x640.png)

![Docker env vars filled](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006102527-1024x628.png)

| Variable | Value |
|----------|-------|
| `N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS` | `true` |
| `N8N_RUNNERS_ENABLED` | `true` |
| `GENERIC_TIMEZONE` | `America/Los_Angeles` |
| `TZ` | `America/Los_Angeles` |

> **Important**: Replace the timezone with your own. Find your identifier at the [Wikipedia tz database list](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).

Double-check everything, then click **Run**.

![Docker container running](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006103137-1024x638.png)

**Step 5 — Account Setup**

Navigate to `http://localhost:5678`. Enter your name, email, and password.

![n8n account setup](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006103324.png)

![n8n license activation](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006103805-1024x414.png)

n8n will send a license key by email to unlock selected paid features. Go to **Settings → Usage and Plan → Enter Activation Key** and paste it.

![n8n license key entry](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006103911-1024x640.png)

You can confirm activation in the Docker container logs.

![Docker logs license confirmed](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006104341-1024x463.png)

**Step 6 — Stopping the Container**

Click the **Stop** icon in Docker Desktop. Always save your workflows before stopping.

![Docker stop container](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006104626-1024x647.png)

### Essential Docker CLI Commands

```bash
docker ps -a          # List all containers (running + stopped)
docker stop <id>      # Stop a container
docker rm <id>        # Remove a container
```

![Docker CLI example](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006105127-1024x617.png)

---

## Chapter 2 · Your First Hello-World Workflow

**Source**: https://theowllogic.com/first-hello-world-n8n-workflow

This workflow teaches the two core concepts of n8n: **triggers** (the entry point that starts a workflow) and **data flow** (how information passes between nodes).

![First n8n workflow banner](https://api.theowllogic.com/wp-content/uploads/2025/11/create-first-n8n-workflow.png)

### Part A — Static Hello World

![Static hello world workflow](https://api.theowllogic.com/wp-content/uploads/2025/11/2-1024x481.png)

**Step 1** — Open n8n and click **New** to create a blank canvas.

![New workflow button](https://api.theowllogic.com/wp-content/uploads/2025/11/3-1024x436.png)

**Step 2** — Click the **+** icon. The sidebar asks what triggers the workflow. Select **Trigger Manually**.

![Trigger selection sidebar](https://api.theowllogic.com/wp-content/uploads/2025/11/4-1024x481.png)

![Manual trigger selected](https://api.theowllogic.com/wp-content/uploads/2025/11/5-1024x357.png)

The manual trigger appears on the canvas. Leave it with default settings — it fires when you click "Execute Workflow."

**Step 3** — Click the **+** next to the trigger node. Search for **Set** and add it.

In the Set node configuration:
1. Click **Add field**
2. Key = `message`
3. Value = `Hello, World!`
4. Press Escape to return to canvas

![Set node add field](https://api.theowllogic.com/wp-content/uploads/2025/11/6-1024x518.png)

![Set node field name](https://api.theowllogic.com/wp-content/uploads/2025/11/7-1024x425.png)

![Set node value entered](https://api.theowllogic.com/wp-content/uploads/2025/11/8-1024x465.png)

![Set node configured](https://api.theowllogic.com/wp-content/uploads/2025/11/9-1024x435.png)

![Canvas with both nodes](https://api.theowllogic.com/wp-content/uploads/2025/11/10-1024x365.png)

**Step 4** — Click **Execute Workflow**.

![Execute workflow button](https://api.theowllogic.com/wp-content/uploads/2025/11/11-1024x522.png)

Double-click the Set node to inspect the output — you'll see `Hello, World!` in the `message` field.

![Hello World output](https://api.theowllogic.com/wp-content/uploads/2025/11/12-1024x440.png)

### Part B — Dynamic Data Passing (Expressions)

![Dynamic data workflow](https://api.theowllogic.com/wp-content/uploads/2025/11/13-1024x542.png)

**Step 1** — Double-click the Manual Trigger node. Click the pencil icon. Paste this JSON:

```json
{
  "name": "Add Your Name Here",
  "status": "Ready"
}
```

![Editing trigger output](https://api.theowllogic.com/wp-content/uploads/2025/11/14-1024x428.png)

This simulates the data you'd receive from a real source like a form submission or database query. Each key (`name`, `status`) becomes a variable accessible via the `$json` object.

**Step 2** — Save the code block. Go to the Set node. On the left panel you'll see the `name` and `status` values from the trigger.

![Trigger data visible in Set node](https://api.theowllogic.com/wp-content/uploads/2025/11/15-1024x558.png)

![Set node left panel data](https://api.theowllogic.com/wp-content/uploads/2025/11/16-1024x402.png)

Delete the old `Hello, World!` text and replace with:

```
Hello, {{ $json.name }}! Are You {{ $json.status }} to learn n8n?
```

Execute the workflow. The output now reads dynamically: `Hello, Shajid! Are You Ready to learn n8n?` (with whatever name you entered).

**Key takeaway**: The `{{ $json.fieldName }}` syntax pulls live data from the previous node. This is the fundamental mechanism by which every n8n workflow passes data from step to step.

---

## Chapter 3 · Workflows, Nodes, and Data Flow

**Source**: https://theowllogic.com/n8n-workflow-nodes-data-flow

![Workflows nodes and data flow](https://api.theowllogic.com/wp-content/uploads/2025/11/n8n-workflows-nodes-and-data-flow.png)

### What Is a Workflow?

Think of a workflow as a chain of dominoes. The first one falls (the trigger) and sets off a chain reaction where each domino (node) knocks down the next, creating an automated sequence. Every workflow has three components:

- **A trigger** — what starts it (manual click, schedule, webhook, external event)
- **Actions** — what happens (sending emails, updating databases, calling APIs)
- **Logic** — how data flows and decisions get made along the way

![Workflow canvas overview](https://api.theowllogic.com/wp-content/uploads/2025/11/Pasted-image-20251006190528-1024x448.png)

### Nodes — The Building Blocks

Each node performs a single specific function. What makes them powerful is their simplicity — one node fetches data from an API, another filters it, a third sends it somewhere. By connecting focused nodes together, you build complex automations.

Every node has three parts:

1. **Inputs** — data received from the previous node
2. **Configuration** — settings controlling its behaviour
3. **Outputs** — results passed to the next node

You can click any node to configure it, test it independently, and inspect exactly what data it's working with. The same node type can appear multiple times — need to call three APIs? Just add three HTTP Request nodes.

### Node Categories

**Trigger Nodes** start your workflow. Every workflow needs exactly one trigger.

| Node | What Starts It | When to Use |
|------|---------------|-------------|
| Manual Trigger | Button click | Testing and on-demand tasks |
| Webhook | HTTP request to a URL | External service integration, form submissions |
| Schedule Trigger | Time/interval | Daily reports, regular data syncing |
| Gmail Trigger | New email arrives | Inbox monitoring |
| Google Sheets Trigger | Row added/updated | Form responses, sheet-based workflows |
| On Form Submission | Hosted form page | Simple data collection without external forms |
| When Executed by Another Workflow | Parent workflow calls it | Modular, reusable sub-workflows |
| On Chat Message | Telegram/Discord/Slack/WhatsApp message | Chatbots, conversational automation |
| Error Trigger | Another workflow fails | Alerting and error handling |

**Action Nodes** perform the actual work — calling APIs, sending messages, updating databases.

| Node | Purpose | Beginner-Friendly? |
|------|---------|-------------------|
| HTTP Request | Call any REST API (the Swiss Army knife) | Advanced — learn basics first |
| Gmail | Send/read/manage emails | Yes |
| Slack | Post to channels, DM users | Yes |
| Google Sheets | Read/write spreadsheet data | Yes |
| Google Drive | Upload/download/organise files | Yes |
| Airtable | Database-like spreadsheet operations | Yes |
| Postgres/MySQL | SQL database operations | No — requires SQL knowledge |
| Set | Create or modify data fields | Yes — used constantly |
| Code | Custom JavaScript or Python | Intermediate |

**Logic Nodes** control how your workflow runs — making decisions, combining data, filtering items.

| Node | What It Does | When to Use |
|------|-------------|-------------|
| IF | Routes data into true/false branches | Simple yes/no decisions |
| Switch | Routes to multiple paths | Categorising data, complex routing |
| Filter | Keeps only items matching criteria | Removing unwanted data early |
| Merge | Combines data from multiple branches | Bringing sources back together |
| Split In Batches | Processes items in groups | API rate limits, large datasets |
| Wait | Pauses workflow execution | Delays between actions, timed sequences |

### Data Flow

Every node outputs JSON data. The next node receives it as input. You reference data using expressions: `{{ $json.fieldName }}`. Each node processes **all items** it receives, then passes its output downstream.

### Debugging Data Flow

When things go wrong (and they will), these are the most common scenarios:

| Error | Meaning | Fix |
|-------|---------|-----|
| "No Data" | Previous node returned empty results | Check that node's output tab |
| "Field undefined" | Referencing a field that doesn't exist | Inspect data; field names are case-sensitive |
| "Node not found" | Renamed a node but expressions use old name | Update all expressions to match new name |
| "Too many items" / slow | Processing massive datasets | Add Filter node early; use Split In Batches |

### Practice Exercise

Build this to internalise data flow:

1. **Manual Trigger** → click to start
2. **Set Node** → create test data: `{ "name": "your name", "task": "learn n8n" }`
3. **Code Node** → add: `return [{json: {...$input.item.json, message: "Hello " + $input.item.json.name + "!"}}]`
4. Execute and inspect how the data transforms at each step.

---

## Chapter 4 · Webhooks in n8n for Beginners

**Source**: https://theowllogic.com/webhook-in-n8n-explained

![Webhooks banner](https://api.theowllogic.com/wp-content/uploads/2025/11/whats-a-webhook-in-n8n.png)

### The Concept

A webhook is like a doorbell for your workflow. When something happens in an external app (a form submission, a payment, a new message), that app "rings the doorbell" — sends an HTTP request to a URL — and your n8n workflow starts instantly. No polling, no delays, no constant checking.

### Setting Up a Webhook

1. Create a new workflow. Add a **Webhook** node as the first step.
2. Choose the **HTTP Method**: POST for receiving data (most common), GET for simple trigger-only calls.
3. Set the **Path** — n8n auto-generates one, but customise it to something memorable (e.g., `/grab-contacts`).
4. Click **Listen for Test Event** at the top. n8n waits for incoming data.
5. Copy the test URL that appears.

### Test URL vs. Production URL — Critical Distinction

| | Test URL | Production URL |
|---|---------|---------------|
| Path contains | `/webhook-test/` | `/webhook/` |
| Active when | You click "Listen for test event" in editor | Workflow is toggled **Active** |
| Lifespan | Stops after one request | Always listening 24/7 |
| Purpose | Quick debugging during development | Real integrations with external services |

> **Common mistake**: Putting the test URL into an external service configuration. It will stop working the moment you leave the editor or it receives one request.

### Making Local n8n Accessible from the Internet (ngrok)

When n8n runs locally at `http://localhost:5678`, external services like WordPress or Stripe can't reach it. ngrok creates a secure tunnel between the public internet and your local machine.

1. Create a free account at [ngrok.com](https://ngrok.com)
2. Install ngrok and configure: `ngrok config add-authtoken <your-token>`
3. Run: `ngrok http 5678`

![ngrok tunnel output](https://api.theowllogic.com/wp-content/uploads/2025/11/image-3-1024x432.png)

You'll get a forwarding address like `https://xxxx.ngrok-free.dev` — this is your public URL.

![ngrok install page](https://api.theowllogic.com/wp-content/uploads/2025/11/image-4-1024x388.png)

### Webhook URL Structure

![Webhook URL anatomy](https://api.theowllogic.com/wp-content/uploads/2025/11/image-14.png)

Your complete webhook URL has three parts:
- **Base URL**: `https://xxxx.ngrok-free.dev` (or your server address)
- **Webhook prefix**: `/webhook/` (production) or `/webhook-test/` (testing)
- **Custom path**: whatever you set (e.g., `grab-contacts`)

### Practical Walkthrough — WordPress Form → Google Sheets

**Step 1** — Set up the Webhook in n8n: POST method, path `/grab-contacts`.

**Step 2** — Start ngrok: `ngrok http 5678`.

**Step 3** — In WordPress: install "Contact Form 7" and "CF7 to Webhook" plugins. In the CF7 webhook settings, paste:

```
https://your-ngrok-url.ngrok-free.dev/webhook/grab-contacts
```

![WordPress form setup](https://api.theowllogic.com/wp-content/uploads/2025/11/image-7-1024x765.png)

![CF7 webhook plugin config](https://api.theowllogic.com/wp-content/uploads/2025/11/image-5-1024x552.png)

**Step 4** — Add a **Google Sheets** node after the webhook.

![Google Sheets node connected](https://api.theowllogic.com/wp-content/uploads/2025/11/image-6-1024x369.png)

Configure it:
- **Operation**: Append (adds a new row each time)
- Select your spreadsheet and sheet
- Map columns:
  - `{{ $json.body["your-name"] }}`
  - `{{ $json.body["your-email"] }}`
  - `{{ $json.body["your-message"] }}`

![Google Sheets mapping](https://api.theowllogic.com/wp-content/uploads/2025/11/image-8-1024x390.png)

![Expression mapping detail](https://api.theowllogic.com/wp-content/uploads/2025/11/image-9-1024x502.png)

**Step 5** — **Activate your workflow** before testing. This is critical — the production URL only works when the workflow is active.

![Activate workflow toggle](https://api.theowllogic.com/wp-content/uploads/2025/11/image-10-1024x454.png)

![Production URL visible](https://api.theowllogic.com/wp-content/uploads/2025/11/image-11.png)

Submit the form, then check the Executions tab and your Google Sheet.

![Form submission test](https://api.theowllogic.com/wp-content/uploads/2025/11/image-12-1024x545.png)

![Google Sheet with data](https://api.theowllogic.com/wp-content/uploads/2025/11/image-13-1024x352.png)

### Troubleshooting

| Issue | Fix |
|-------|-----|
| "Error trying to send your message" | **Activate your workflow** — not just listening for test events |
| ngrok shows data but n8n doesn't | Check ngrok inspector at `http://localhost:4040`; verify workflow is active; check Executions tab |
| Data in n8n but not in Google Sheets | Verify credentials, correct spreadsheet selected, column mapping matches |

![Common error screenshot](https://api.theowllogic.com/wp-content/uploads/2025/11/image-15-1024x349.png)

---

## Chapter 5 · Handling API Rate Limits (Throttling & Retry)

**Source**: https://theowllogic.com/how-to-handle-api-rate-limits-in-n8n

![Rate limits banner](https://api.theowllogic.com/wp-content/uploads/2025/11/how-to-handle-API-rate-limits-in-n8n.png)

### The Problem

APIs limit how many requests you can make in a given time window to prevent server overload. When you exceed that limit, you get a `429 Too Many Requests` error and your workflow stops mid-execution — half your data processed, half not.

**Common rate limits:**
- Google Sheets: ~100 requests per 100 seconds per user (~1/sec)
- Airtable: 5 requests per second
- Notion: 3 requests per second

### Level 1 — Throttling with Split In Batches

Instead of firing all requests at once, batch them and add delays.

![Throttling workflow overview](https://api.theowllogic.com/wp-content/uploads/2025/11/image-16-1024x409.png)

![Throttling with batches](https://api.theowllogic.com/wp-content/uploads/2025/11/image-17-1024x407.png)

The pattern: break 500 items into batches of 5, process each batch, wait 10 seconds, repeat.

**Loop Over Items** node — set batch size to 5:

![Batch size config](https://api.theowllogic.com/wp-content/uploads/2025/11/image-19.png)

**Wait node** — set to 10 seconds between batches:

![Wait node config](https://api.theowllogic.com/wp-content/uploads/2025/11/image-18.png)

**Workflow node breakdown:**

| Node | Purpose |
|------|---------|
| Manual Trigger | Starts the workflow |
| Data source (JSON/API) | Your dataset (500 customers) |
| Enriching data (optional) | Filter to only needed fields |
| Loop Over Items | Groups items into batches of 5 |
| Google Sheets / API call | Processes each batch |
| Processed data (optional) | Logs batch count |
| Wait 10 seconds | Prevents rate limiting |
| Loop back | Returns to batching until complete |

**Do the math first:** 500 items ÷ 5 per batch × 10 seconds = ~16 minutes total. Know your execution time upfront.

### Level 2 — Retry Logic (Graceful Recovery)

When rate limits DO hit despite throttling, recover automatically instead of failing:

![Retry logic workflow](https://api.theowllogic.com/wp-content/uploads/2025/11/image-20-1024x260.png)

**Configure the API node:**

![Retry on fail settings](https://api.theowllogic.com/wp-content/uploads/2025/11/image-21.png)

1. Open the node → Settings → toggle **Retry On Fail**
2. Max tries: 3
3. Wait between tries: 5000ms (5 seconds)
4. On Error: "Continue (using error output)"

**Connect the error output** to a Wait node (10–60 seconds) that loops back to the API node:

![Error output to wait node](https://api.theowllogic.com/wp-content/uploads/2025/11/image-22.png)

![Retry success visualization](https://api.theowllogic.com/wp-content/uploads/2025/11/image-23-1024x258.png)

The green-highlighted paths show where the error output resolved the rate limit and items were successfully reprocessed.

### Tips

- Always check the API's documentation for specific rate limits before building
- Test with 20 records before running 500
- Monitor execution logs — multiple retries mean your wait time is too short
- Throttling prevents hitting limits; retry logic handles the occasional slip-through

---

## Chapter 6 · Handling Errors Like a Pro

**Source**: https://theowllogic.com/handle-errors-in-n8n

![Error handling banner](https://api.theowllogic.com/wp-content/uploads/2025/11/handle-errors-in-n8n-like-a-pro.png)

### Why Error Handling Matters

APIs go down. Websites change structure. Data arrives in unexpected formats. Network connections drop. The difference between a fragile workflow and a production-ready one isn't avoiding errors — that's impossible. It's building workflows that detect problems, recover gracefully, and keep running.

### Common n8n Error Types

| Error Type | What It Looks Like | Root Cause |
|-----------|-------------------|------------|
| HTTP failures | `ETIMEDOUT`, `ENOTFOUND` | API down, internet dropped, wrong URL |
| Auth errors | `401 Unauthorized`, `403 Forbidden` | Expired API keys, stale OAuth tokens |
| Rate limits | `429 Too Many Requests` | Too many requests too fast |
| Missing data | `Cannot read property 'x' of undefined` | Field doesn't exist; previous node returned empty |
| Timeouts | `Execution timed out` | Too much data or API too slow |
| JSON parse errors | `Unexpected token < in JSON` | API returned HTML error page instead of JSON |
| Config errors | `Column "Name" not found` | Changed sheet headers, deleted channels, typos |

Most errors fall into three buckets: **external failures** (need retry logic), **data problems** (need validation), and **configuration mistakes** (need testing).

### Technique 1 — Error Trigger Workflow (Get Notified)

Create a separate workflow that fires whenever any other workflow encounters an error. This is your safety net.

![Error trigger concept diagram](https://api.theowllogic.com/wp-content/uploads/2025/11/Error-trigger-workflow-setup.png)

**Setup:**

1. Create a new workflow. Add an **Error Trigger** node → connect it to a notification node (Gmail, Slack, Telegram).
2. **Save and activate** this error workflow.
3. Go to your **main workflow** → Settings → Error Workflow → select the error workflow you just created.

![Main workflow error settings](https://api.theowllogic.com/wp-content/uploads/2025/11/workflow-settings-for-error-trigger-1024x538.png)

![Error workflow selection](https://api.theowllogic.com/wp-content/uploads/2025/11/Error-workflow-settings-1024x534.png)

Now whenever the main workflow errors, you'll get an automatic notification with the error details.

![Error notification email example](https://api.theowllogic.com/wp-content/uploads/2025/11/image-32.png)

**Testing tip**: Error triggers don't fire on manual executions. To test: add a Schedule Trigger to your main workflow (set to every 1 minute), intentionally break something (wrong API URL), wait for it to fire, verify the notification arrives, then switch back to your real trigger.

### Technique 2 — Data Validation with IF Nodes

Check if data exists and is valid **before** trying to use it. This prevents crashes from missing or malformed data coming from external sources.

**Example:** Before sending 766 automated emails, validate that every contact has a non-empty status field:

![Data validation workflow](https://api.theowllogic.com/wp-content/uploads/2025/11/image-33-1024x403.png)

- **True path**: Contacts with valid status → proceed to email
- **False path**: Contacts with missing status → log to error sheet or skip

Without validation, one empty field crashes the entire workflow. With it, incomplete data gets handled gracefully.

### Technique 3 — Status Tracking with Continue On Fail

For large batch operations, don't let one failure stop everything. Instead, let each item fail independently and record the result.

![Fake customer database](https://api.theowllogic.com/wp-content/uploads/2025/11/image-34.png)

![Status tracking workflow](https://api.theowllogic.com/wp-content/uploads/2025/11/image-35-1024x398.png)

**The pattern:**

1. Read all contacts from Google Sheets
2. **Loop Over Items** — process one at a time (or in small batches)
3. **Gmail node with Continue On Fail enabled** — creates two output paths:
   - Success path → update sheet with status = `SENT`
   - Error path → update sheet with status = `FAILED`
4. **Wait 5 seconds** between items
5. Loop back for the next batch

**Results**: Out of 766 contacts — 760 sent successfully (marked SENT), 6 failed (marked FAILED). Complete audit trail. No lost data. The failed items can be retried in a separate workflow later.

This pattern scales from 50 to 5,000+ items — just adjust the batch size and wait time.

---

## Chapter 7 · n8n Expressions — Complete Practical Guide

**Source**: https://theowllogic.com/n8n-expressions-complete-guide

![Expressions banner](https://api.theowllogic.com/wp-content/uploads/2026/01/n8n-expression-practical-complete-guide.png)

### What Are Expressions?

Expressions turn static workflow parameters into dynamic ones that adapt to your data. Without expressions, every value is hardcoded — one email address, one name. With expressions, you tell n8n to grab values from the live data flowing through the workflow. The same workflow then handles thousands of different records without modification.

Under the hood, n8n uses a templating engine called Tournament with `{{ }}` syntax that executes JavaScript. n8n extends it with custom variables (`$json`, `$node`, `$input`), transformation functions, and built-in libraries like Luxon for dates and JMESPath for JSON querying.

### Syntax Fundamentals

Click any parameter field in n8n and you'll see a toggle between **Fixed** (static) and **Expression** (dynamic) modes.

![Fixed vs Expression toggle](https://api.theowllogic.com/wp-content/uploads/2026/01/image-2.png)

Everything inside `{{ }}` executes as JavaScript and gets replaced with the result:

```javascript
// Correct
{{ $json.email }}

// Wrong — missing the wrapper
$json.email

// Wrong — single braces
{$json.email}
```

![Expression error example](https://api.theowllogic.com/wp-content/uploads/2026/01/image-3.png)

If you see an "expression not recognized" error, either you forgot the `{{ }}` or you're still in Fixed mode.

![Expression editor](https://api.theowllogic.com/wp-content/uploads/2026/01/image-4-1024x411.png)

The expression editor (click the expansion icon) gives you syntax highlighting and autocomplete. Use it for anything more complex than a simple field access.

### `$json` — Accessing Current Item Data

`$json` represents the current item from the previous node. When a webhook fires, the payload is `$json`. When you read a Google Sheets row, that row is `$json`.

```javascript
// Simple field access
{{ $json.name }}        // Output: "Shajid"
{{ $json.email }}       // Output: "shajid@example.com"

// Nested objects (common with webhooks)
{{ $json.body.user.first_name }}   // "John"

// Fields with spaces or special characters — use brackets
{{ $json["Full Name"] }}
{{ $json["user-id"] }}
```

![Webhook data expression](https://api.theowllogic.com/wp-content/uploads/2026/01/image-5-1024x268.png)

> **Common webhook gotcha**: Webhook data is always nested under `body`. If `{{ $json.email }}` returns undefined, you probably need `{{ $json.body.email }}`. Always check the actual data structure in the node's output tab.

### `$input` — Working with Multiple Items

```javascript
{{ $input.all() }}              // All items from previous node
{{ $input.first().json.email }} // First item only
{{ $input.last().json.name }}   // Last item only
{{ $input.item.json.id }}       // Current item (same as $json)
```

Use case: your workflow fetches 50 records but you need one summary of all 50. `$json` only gives you the current item; `$input.all()` gives you everything.

### `$node["Name"]` — Cross-Node Data Access

Access data from a specific earlier node (not just the immediately previous one):

```javascript
{{ $node["HTTP Request"].json.userId }}
{{ $node["Get User Data"].json.email }}
```

Node names are case-sensitive. If you see "Referenced node is unexecuted," that node didn't run (possibly on a different branch via an IF node).

### `$now` — Timestamps and Dates

n8n includes Luxon for date handling:

```javascript
{{ $now }}                                    // Current timestamp
{{ $now.toFormat('yyyy-MM-dd') }}             // "2026-04-03"
{{ $now.plus({ days: 7 }).toISO() }}          // 7 days from now
{{ $now.minus({ months: 1 }).toFormat('yyyy-MM-dd') }}  // 1 month ago
```

### String Manipulation Patterns

```javascript
// Case conversion
{{ $json.name.toUpperCase() }}       // "JOHN DOE"
{{ $json.email.toLowerCase() }}      // "user@example.com"

// Combining text
{{ $json.firstName + " " + $json.lastName }}

// Template literals (cleaner for mixed text + variables)
{{ `Hello ${$json.name}, order #${$json.orderId} is ready` }}

// Extracting parts
{{ $json.email.split("@")[1] }}      // "example.com" (domain)
{{ $json.name.slice(0, 1) }}         // "J" (first initial)

// Cleaning whitespace
{{ $json.name.trim() }}
```

### Expressions vs. Code Node

**Use expressions** for single-line operations: extracting a value, simple math, string formatting, combining fields.

**Use the Code node** when you need multiple lines, variables, loops, or complex multi-item logic:

```javascript
// This belongs in a Code node, not an expression:
const activeItems = $input.all().filter(item => item.json.active);
const total = activeItems.reduce((sum, item) => sum + item.json.price, 0);
return { json: { count: activeItems.length, total } };
```

Key difference: expressions execute per item (100 items = 100 executions). The Code node can process all items at once using `$input.all()`.

---

## Chapter 8 · Conditional Logic (IF and Switch Nodes)

**Source**: https://theowllogic.com/conditional-logic-in-n8n

![Conditional logic banner](https://api.theowllogic.com/wp-content/uploads/2026/01/building-conditional-logics-in-n8n.png)

### The IF Node

The IF node examines each item and splits it into two paths: **true** and **false**. Both branches can have completely different workflows attached. The data itself doesn't change — the IF node only makes a routing decision.

![IF node introduction](https://api.theowllogic.com/wp-content/uploads/2026/01/IF-Node-decision-controller-1024x576.png)

When multiple items enter, each is evaluated individually. Some may go true, others false — both outputs can have data simultaneously.

### Practical Example — Filter High-Value Customers

Goal: Find customers who spent over $1,000 for a holiday discount campaign.

![Customer spreadsheet data](https://api.theowllogic.com/wp-content/uploads/2026/01/image-6.png)

![IF node data flow sketch](https://api.theowllogic.com/wp-content/uploads/2026/01/image-7.png)

Set the IF node condition: `Total amount` → larger than → `1000`:

![IF node condition setup](https://api.theowllogic.com/wp-content/uploads/2026/01/image-8-1024x537.png)

Result: 9 customers routed to True (eligible for discount), the rest routed to False.

![IF node output results](https://api.theowllogic.com/wp-content/uploads/2026/01/image-9-1024x455.png)

### Data Types and Comparison Operators

Every comparison needs the correct data type selected:

| Data Type | Common Operators | Use For |
|-----------|-----------------|---------|
| String | equals, contains, starts with, ends with | Email domains, country codes, status values |
| Number | larger, smaller, equals, between | Purchase amounts, ages, quantities |
| Boolean | equals (true/false) | Active status, feature flags |
| Date & Time | after, before, between | Last purchase date, deadlines |

**Common mistakes:**

```javascript
// WRONG — "equals" when you need "contains"
{{ $json.email }} equals "gmail.com"    // Never matches full email

// RIGHT
{{ $json.email }} contains "@gmail.com" // Matches "user@gmail.com"

// Numbers stored as strings — convert first
{{ Number($json.purchaseAmount) }} larger than 1000

// Case sensitivity — "US" ≠ "us"
{{ $json.country.toLowerCase() }} equals "us"
```

### AND/OR Logic — The Expression Workaround

The IF node UI doesn't let you mix AND and OR operators — changing one dropdown changes all of them. For complex logic, write a single expression instead:

```javascript
{{ ($json['Total amount'] >= 1000 && $json['US Country'] === true) || $json.VIP === true }}
```

![AND/OR expression in IF node](https://api.theowllogic.com/wp-content/uploads/2026/01/image-16.png)

Set the IF node to check whether this boolean expression equals `true`.

### Adding Boolean Conditions

With a US Country boolean column added to the spreadsheet:

![Updated spreadsheet with boolean](https://api.theowllogic.com/wp-content/uploads/2026/01/image-10-1024x454.png)

Drag the parameter from the left panel directly into the value field — n8n auto-detects the data type:

![Mapping boolean value](https://api.theowllogic.com/wp-content/uploads/2026/01/image-11-1024x394.png)

![Boolean true selection](https://api.theowllogic.com/wp-content/uploads/2026/01/image-12-1024x441.png)

![Combined filter results](https://api.theowllogic.com/wp-content/uploads/2026/01/image-13-1024x465.png)

### The Switch Node — Multiple Paths

The IF node splits into two directions. The Switch node splits into as many as you need.

![Switch node introduction](https://api.theowllogic.com/wp-content/uploads/2026/01/Switch-node-introduction-in-n8n-1024x576.png)

**Use IF when**: Simple yes/no, two paths, binary choices.
**Use Switch when**: Multiple categories (Gold/Silver/Bronze), routing by region, handling different product types.

**Example — Routing by Customer Tier:**

- Bronze: $0–$500
- Silver: $501–$1,000
- Gold: $1,001+

![Switch node connected](https://api.theowllogic.com/wp-content/uploads/2026/01/image-20-1024x375.png)

![Switch node rules config](https://api.theowllogic.com/wp-content/uploads/2026/01/image-21-1024x572.png)

For the Silver tier (range check), use an expression since a single field can't express "between":

```javascript
{{ $json['Total amount'] > 500 && $json['Total amount'] <= 1000 }}
```

![Switch expression for ranges](https://api.theowllogic.com/wp-content/uploads/2026/01/image-24-1024x444.png)

![Switch node final results](https://api.theowllogic.com/wp-content/uploads/2026/01/image-25-1024x354.png)

> **Important**: Switch sends data to the **first matching rule**. Order rules from most specific to least specific to avoid misrouting.

**Download the example workflow JSON**: https://github.com/WayOfTheKatana/n8n-workflows/blob/main/Tutorials/if-switch-workflow.json

---

## Chapter 9 · Loop Over Items (When You Actually Need It)

**Source**: https://theowllogic.com/how-to-use-loop-in-n8n

![Loop over items banner](https://api.theowllogic.com/wp-content/uploads/2026/01/How-to-use-loop-over-items.png)

### The Key Insight: 80% of the Time You Don't Need This Node

n8n automatically processes all items through each node. If you pull 100 rows from Google Sheets and connect a Slack node, n8n sends 100 messages — no loop required. This is different from Make.com (formerly Integromat) where you need an iterator.

### The Three Scenarios Where Loops ARE Required

**Scenario 1 — API Rate Limiting**

External APIs don't care about n8n's architecture. If you send 500 requests at once, you'll get blocked. Use Loop Over Items + Wait to batch and throttle. (See Chapter 5 for the detailed pattern.)

**Scenario 2 — Node Exceptions**

Some nodes only process the first item by design. The RSS Feed Read node, for example, expects a single URL and returns multiple articles. If you give it 5 URLs, it only reads the first one. Wrap it in a Loop Over Items with batch size 1 to process all 5.

Check n8n's documentation for the current list of exception nodes before assuming you need a loop.

**Scenario 3 — Pagination with Unknown Page Counts**

APIs that return paginated data sometimes don't reveal total pages. You need to loop: fetch a page, check if there's a next page token, loop back if yes, stop if no.

**Example — Paginating Reddit posts:**

Pin initial data to the manual trigger:

```json
[
  {
    "subreddit": "n8n",
    "after": null,
    "allPosts": []
  }
]
```

![Pinned data in manual trigger](https://api.theowllogic.com/wp-content/uploads/2026/01/image-27-1024x302.png)

Connect Loop Over Items (batch size 1, Reset option enabled):

![Loop batch config](https://api.theowllogic.com/wp-content/uploads/2026/01/image-28-1024x437.png)

Connect to HTTP Request fetching Reddit's JSON API:

![Loop to HTTP request](https://api.theowllogic.com/wp-content/uploads/2026/01/image-29.png)

URL: `https://www.reddit.com/r/{{ $json.subreddit }}/hot.json` with query parameter `limit=25`.

![HTTP request config](https://api.theowllogic.com/wp-content/uploads/2026/01/image-30-1024x690.png)

Code node accumulates results across pages:

```javascript
const posts = $input.first().json.data.children;
const nextPageToken = $input.first().json.data.after;
const loopData = $('Loop Over Items').first().json;

const previousPosts = loopData.allPosts || [];
const allPosts = [...previousPosts, ...posts];

return {
  allPosts: allPosts,
  after: nextPageToken,
  subreddit: loopData.subreddit,
  hasMorePages: nextPageToken !== null,
  totalPosts: allPosts.length,
  currentPage: (loopData.currentPage || 0) + 1
};
```

![Code node for pagination](https://api.theowllogic.com/wp-content/uploads/2026/01/image-31-1024x464.png)

Each loop iteration fetches 25 posts, accumulates them, and checks for the next-page token. When the token is `null`, pagination is complete.

### Decision Flowchart

![Decision flowchart](https://api.theowllogic.com/wp-content/uploads/2026/01/image-32-1024x572.png)

1. Do you have multiple items? → No → No loop needed
2. Does the next node auto-process all items? → Yes → No loop needed
3. Is it rate limiting, a node exception, or pagination? → Yes → Use Loop Over Items
4. None of the above → Reconsider; test without the loop first

### Common Troubleshooting

**Loop processes same item every time:**

```javascript
// WRONG — .first() always grabs the first item
{{ $('Google Sheets').first().json.email }}

// RIGHT — current item
{{ $json.email }}
```

**Loop never stops:** Missing exit condition. Always include a check (e.g., `hasMorePages === false` or a safety counter).

---

## Chapter 10 · Sub-Workflows (Modular Reusable Automation)

**Source**: https://theowllogic.com/n8n-sub-workflows

![Sub-workflows banner](https://api.theowllogic.com/wp-content/uploads/2026/01/How-to-use-sub-workflows.png)

### What Sub-Workflows Solve

Sub-workflows let you call one workflow from another. Instead of copying the same 6–8 node validation logic into three different workflows, you build it once and reuse it like a function. Update the validation once, all parent workflows get the new logic automatically.

### When to Use Them

- **Reusable logic** — same node group copied into multiple workflows
- **Memory issues** — workflows with 50+ nodes hitting memory limits
- **Modular testing** — testing a 10-node sub-workflow in isolation is faster than running all 50 nodes
- **Human-in-the-loop** — need approval steps that pause and resume

### When NOT to Use Them

- Simple branching that an IF node handles fine
- Batch processing that Loop Over Items covers
- Logic you'll never reuse elsewhere

### How It Works

The parent sends data to the child. The child runs start-to-finish. The child's **final node output** returns to the parent. The parent continues.

> Sub-workflow executions don't count toward your monthly execution quota on n8n Cloud — only the parent workflow counts.

### Building an Address Verification Sub-Workflow

**Customer data sheet:**

![Customer data sheet](https://api.theowllogic.com/wp-content/uploads/2026/01/image-34-1024x408.png)

**Parent workflow structure:**

1. Manual Trigger → Google Sheets (read customers) → Execute Sub-Workflow → Google Sheets (update status)

**Step 1** — Connect the trigger and sheet reader:

![Parent workflow start](https://api.theowllogic.com/wp-content/uploads/2026/01/image-36.png)

**Step 2** — Add an **Execute Workflow** node. Create a new sub-workflow:

![Execute workflow node](https://api.theowllogic.com/wp-content/uploads/2026/01/image-37.png)

![Execute workflow config](https://api.theowllogic.com/wp-content/uploads/2026/01/image-38-1024x485.png)

A new tab opens with the sub-workflow. Set the trigger to **When Executed by Another Workflow** with input mode "Accept all data":

![Sub-workflow trigger settings](https://api.theowllogic.com/wp-content/uploads/2026/01/image-39.png)

![Sub-workflow canvas](https://api.theowllogic.com/wp-content/uploads/2026/01/image-40.png)

> **Critical**: Save and publish the sub-workflow before running the parent — otherwise it errors.

**Step 3** — Add validation logic in the sub-workflow. For example, only validate addresses for new customers (Return Customer = "No"):

![Sub-workflow logic](https://api.theowllogic.com/wp-content/uploads/2026/01/image-41-1024x536.png)

**Step 4** — Back in the parent, connect the final Google Sheets update node to change Verification Status to "Order Approved":

![Parent update node](https://api.theowllogic.com/wp-content/uploads/2026/01/image-43-1024x328.png)

![Sheet update config](https://api.theowllogic.com/wp-content/uploads/2026/01/image-44-1024x830.png)

**Step 5** — Execute the parent workflow:

![Parent executing with sub-workflow](https://api.theowllogic.com/wp-content/uploads/2026/01/image-45-1024x372.png)

The parent halts while the sub-workflow processes. Check sub-workflow executions:

![Sub-workflow execution log](https://api.theowllogic.com/wp-content/uploads/2026/01/image-46-1024x582.png)

Final sheet result:

![Updated customer sheet](https://api.theowllogic.com/wp-content/uploads/2026/01/Screenshot_2-1024x377.png)

### Data Passing Details

**Sending data IN**: The parent passes all items through the Execute Sub-Workflow node. Two modes:
- "Run once with all items" — sub-workflow gets all records in one execution
- "Run once for each item" — sub-workflow runs N times (once per record)

**Getting data OUT**: The sub-workflow's **last node output** becomes the parent's Execute Sub-Workflow output. Not the first node, not a middle node — the final one.

---

# PART TWO — WORKFLOW-BASED (Practical Projects)

These four chapters apply the theory to real-world automation projects.

---

## Chapter 11 · How to Plan an n8n Workflow

**Source**: https://theowllogic.com/plan-n8n-workflow

![Planning banner](https://api.theowllogic.com/wp-content/uploads/2026/02/How-to-plan-n8n-workflow.png)

### The Real Bottleneck Isn't Technical Knowledge

Most beginners freeze when facing a blank canvas — not because they don't know what nodes do, but because they don't know how to decompose a problem into automation-friendly steps. The thinking process that happens before clicking matters more than the clicking itself.

### The Four Planning Questions

Answer these before adding your first node:

**Question 1 — What's the Desired Outcome?**

Write it as: "I want to **[specific result]** when **[trigger condition]** so that **[business value]**."

- Bad: "I want to scrape Reddit."
- Good: "I want a Slack notification when r/n8n has a post mentioning 'workflow automation' with 50+ upvotes, so I can engage with relevant discussions quickly."

If you can't fill in all three parts, the problem isn't well-defined yet.

**Question 2 — What Triggers It?**

Ask: Does this start based on **time**, an **external event**, or a **manual action**?

| Trigger Type | Example | n8n Node |
|-------------|---------|----------|
| Time-based | "Every Monday at 9am, generate a report" | Schedule Trigger |
| Event-based | "When a new email arrives" | Service-specific trigger or Webhook |
| Manual | "Run on-demand when I click" | Manual Trigger |

Start with Manual Trigger for development, but switch to the real trigger type before deploying.

**Question 3 — What Data Transformations Are Needed?**

Every workflow follows: **Input → Transform → Output**.

Example — pulling Reddit posts:
- **Input**: 50 posts with 20+ fields each (raw API response)
- **Transform**: Filter to score > 100, extract title + URL, format as readable message
- **Output**: Clean list ready for a Slack node

This maps directly to n8n nodes: Filter node, Set node, Code node.

**Question 4 — What Could Go Wrong?**

Think through failure modes before building:
- What if the API is down? (Retry logic, alerts)
- What if no data matches your filters? (Handle empty results gracefully)
- What if the data format changes? (Validate critical fields exist)

### Three Mental Models for Automation Patterns

![Three patterns diagram](https://api.theowllogic.com/wp-content/uploads/2026/02/3-mental-model-patterns-1024x576.png)

**Pattern 1 — The Data Pipeline** (Fetch → Transform → Deliver)

Linear flow with no branching. Data enters at the top, gets transformed step by step, exits at the bottom. Example: Schedule → HTTP Request (weather API) → Set node (extract temperature) → Code node (format message) → Slack (send to #general).

**Pattern 2 — The Decision Tree** (If This, Then That)

Different inputs require different actions. Example: Email Trigger → Code (analyse keywords) → IF node branching into High/Medium/Low priority → different notification actions per branch.

**Pattern 3 — The Batch Processor** (Loop Over Items)

Process multiple items individually with rate limiting or per-item logic. Example: Schedule → Google Sheets (fetch attendees) → Set (personalise variables) → Code (build email body) → Send Email → Wait (2 seconds between sends).

### The Planning Template

![Thinking framework](https://api.theowllogic.com/wp-content/uploads/2026/02/The-WOrkflow-thinking-framework-1024x576.png)

Before opening n8n, write down on paper or in Obsidian:

1. **Outcome**: What does success look like?
2. **Trigger**: Time, event, or manual?
3. **Data source**: Where does the input come from?
4. **Transformations**: What changes between input and output?
5. **Destination**: Where do results go?
6. **Error cases**: What could break?

---

## Chapter 12 · 50 Boring Tasks You Can Automate

**Source**: https://theowllogic.com/automate-boring-tasks-n8n

![Boring tasks banner](https://api.theowllogic.com/wp-content/uploads/2026/02/Automate-boring-tasks-with-n8n.png)

### The Principle

If you find yourself doing the same task more than twice a week, it's a candidate for automation. The author reduced 14 hours of repetitive content-publishing tasks to an automated pipeline where the only human role is writing and final review.

### Why n8n Over Alternatives?

- **vs. Python scripts**: No ongoing code maintenance; visual builder; still allows custom code when needed
- **vs. Zapier/Make**: Self-hosted is free with unlimited executions; full code node escape hatch; community nodes for niche integrations

### Automation Ideas by Category

![Automation ideas](https://api.theowllogic.com/wp-content/uploads/2026/02/boring-ideas-automate-1024x576.png)

**Content & Publishing**: Generate article outlines from keywords, run grammar checks on drafts, format and publish WordPress posts from Google Docs, create social media posts when new blog content goes live, scrape communities for content ideas, pull YouTube transcripts to Docs, auto-create featured images, track content performance weekly, send draft review reminders, auto-translate posts, update internal links when new content publishes.

**Email & Communication**: Sort incoming emails by sender/keyword, auto-reply to common questions, send follow-ups after 3 days of no response, forward invoices to your accountant, flag newsletters you never open.

**Data & Spreadsheets**: Sync form submissions to Google Sheets in real time, update CRM when a sheet row changes, merge data from multiple sheets, clean duplicate entries, pull analytics into weekly reports, convert CSV uploads to formatted sheets, validate entries and flag missing fields, sync Stripe payments to a revenue tracker, auto-calculate monthly totals, pull exchange rates daily, cross-reference sheets and highlight mismatches, export database tables on a schedule, track affiliate commissions across platforms.

**Files & Backups**: Back up folders to Google Drive nightly, rename/organise downloaded files by date, convert images to WebP format, archive old files to cold storage after 90 days.

**Notifications & Reminders**: Slack alerts when invoices are paid, birthday and contract renewal reminders, inventory threshold alerts, competitor monitoring.

**Social Media**: Schedule posts across platforms, auto-share new blog content, repurpose long-form content into social snippets.

**Business Operations**: Generate invoices from completed project rows, update project management boards from email, sync calendars across platforms, track time entries and compile weekly timesheets, monitor SSL certificate expiry, track domain renewals, rotate API keys on schedule.

---

## Chapter 13 · Building a Follow-Up Email Sequence

**Source**: https://theowllogic.com/n8n-followup-email

![Email sequence banner](https://api.theowllogic.com/wp-content/uploads/2026/02/build-email-sequence-in-n8n.png)

### What You're Building

A 3-email automated follow-up sequence that checks your contact list daily, sends the right email based on timing, and tracks status per contact. Real use cases: ebook downloaders, lead nurturing, post-demo follow-up.

### Why n8n Instead of Mailchimp/HubSpot?

Free, unlimited contacts, you own the data, no monthly fees that scale with list size.

### Email Sequence Strategy

Three emails is the sweet spot for most follow-up sequences:

- **Email 1**: Initial contact — remind them who you are, deliver value
- **Email 2**: Follow-up — add context, share resources or a case study
- **Email 3**: Final check-in — gentle nudge, easy opt-out

### Workflow Structure (10 Nodes)

![Workflow plan sketch](https://api.theowllogic.com/wp-content/uploads/2026/02/Email-sequence-planning-on-excalidraw-1024x453.png)

1. Manual Trigger (testing) / Schedule Trigger (production)
2. Google Sheets — Read contacts
3. Switch Node — Route by which follow-up is next
4. Set Nodes (×3) — One per route, defining follow-up number, goal, and column to update
5. AI Agent (Gemini) — Generate personalised email content
6. Code Node — Parse AI output into subject + body
7. Gmail — Send the email
8. Google Sheets — Update the contact's status

### Step-by-Step Build

**Step 1** — Add a Manual Trigger for testing.

**Step 2** — Connect Google Sheets (Read operation) to fetch your contact list:

![Google Sheets read config](https://api.theowllogic.com/wp-content/uploads/2026/02/image-2.png)

![Sheets node configuration](https://api.theowllogic.com/wp-content/uploads/2026/02/image-3-1024x612.png)

Your sheet needs columns for name, email, company, and three boolean follow-up tracking columns (Follow Up #1, #2, #3):

![Contact spreadsheet](https://api.theowllogic.com/wp-content/uploads/2026/02/image-4-1024x455.png)

> Check every row for a valid email address — one empty row crashes the Gmail node.

**Step 3** — Add a **Switch Node** with 3 routing rules:

![Switch node connected](https://api.theowllogic.com/wp-content/uploads/2026/02/image-5.png)

Each rule checks if that follow-up column is `false` (meaning it hasn't been sent yet):

![Switch rule mapping](https://api.theowllogic.com/wp-content/uploads/2026/02/image-6-1024x667.png)

![Follow-up column tagging](https://api.theowllogic.com/wp-content/uploads/2026/02/image-7-1024x712.png)

**Step 4** — Add a **Set Node** to each Switch output, defining the follow-up context:

![Set nodes per route](https://api.theowllogic.com/wp-content/uploads/2026/02/image-8.png)

![Set node configuration](https://api.theowllogic.com/wp-content/uploads/2026/02/image-9-1024x397.png)

For each route, set three fields:
- `followUpNumber`: 1, 2, or 3
- `followUpGoal`: One-line description of the email's purpose
- `columnsToUpdate`: Which sheet column to mark as sent

**Step 5** — Add an **AI Agent** node connected to a Gemini (or Claude) Chat Model. Write a system prompt that includes recipient details from the sheet, specifies tone per follow-up number, and sets rules (max 150 words, conversational, no corporate fluff).

The AI generates a personalised email for each contact based on which follow-up step they're at. This replaces static templates with contextually aware messages.

**Step 6** — Parse the AI output with a Code node to separate subject and body, then send via Gmail and update the sheet column to mark that follow-up as sent.

### Production Deployment

Once testing works: switch the Manual Trigger to a Schedule Trigger (daily at 9 AM). The workflow runs automatically, finds contacts due for their next follow-up, generates and sends the email, and updates the tracking sheet.

---

## Chapter 14 · Building an n8n AI Agent (Step-by-Step)

**Source**: https://theowllogic.com/n8n-ai-agent-workflow

![AI agent banner](https://api.theowllogic.com/wp-content/uploads/2026/02/How-to-build-an-n8n-AI-Agent-in-2026.png)

### What Makes an Agent Different from a Regular Workflow?

A normal workflow follows fixed paths: IF email contains "refund" → send template A. Every branch must be anticipated and pre-built.

An AI agent understands a goal and figures out the steps itself. It reads input, decides which tool to use, uses it, reads the result, and keeps going until it has a complete answer. Agents shine when input is unstructured and the right action depends on context.

**The trade-off**: Agents are less predictable. For deterministic, auditable tasks (payroll, database writes), standard workflows are still the right tool.

### The Four Components Every Agent Needs

1. **Trigger** — Chat Trigger (conversational), Webhook (external integration), Schedule, or Form
2. **AI Agent Node** — The orchestrator that receives input, calls the LLM, decides which tools to use, and loops until complete
3. **Sub-Nodes** attached to the Agent:
   - **Chat Model** — The LLM (Gemini, Claude, GPT, etc.)
   - **Memory** — Retains context across messages
   - **Tools** — What the agent can do (APIs, web search, calculations, database queries)
4. **Output** — Where results go (chat reply, Slack, Google Sheets, etc.)

### Building a Support Triage Agent

**Step 1** — Add a **Chat Trigger**:

![Chat trigger added](https://api.theowllogic.com/wp-content/uploads/2026/02/image-25-1024x428.png)

This gives you a built-in chat interface for testing — no external services needed.

![Chat trigger URL](https://api.theowllogic.com/wp-content/uploads/2026/02/image-26-1024x401.png)

**Step 2** — Add an **AI Agent** node and connect it to the trigger:

![AI Agent node connected](https://api.theowllogic.com/wp-content/uploads/2026/02/image-27-1024x388.png)

Write a system prompt defining the agent's role:

![System message config](https://api.theowllogic.com/wp-content/uploads/2026/02/image-28-1024x467.png)

Example system prompt:

```
You are a support agent for a SaaS product. Your job is to:

1. Answer common questions directly if you can (password resets, billing basics, plan info)
2. If the issue requires account-specific info you don't have, let the user know you're escalating
3. Always be concise. Don't pad responses with filler.
4. If the user seems frustrated, acknowledge it briefly before answering.

You do NOT have access to account data unless a tool provides it. Don't make up information.
```

That last instruction is critical — without it, the agent may hallucinate account details.

**Step 3** — Connect a **Chat Model** sub-node:

![Chat model connected](https://api.theowllogic.com/wp-content/uploads/2026/02/image-30.png)

![Gemini model selection](https://api.theowllogic.com/wp-content/uploads/2026/02/image-31-1024x629.png)

**Getting a Gemini API key**: Go to [Google AI Studio](https://aistudio.google.com/) → sidebar → "Get API Key." A Google Cloud project is created automatically.

![API key entry](https://api.theowllogic.com/wp-content/uploads/2026/02/image-32-1024x548.png)

Select your model (e.g., `models/gemini-2.0-flash`):

![Model selection](https://api.theowllogic.com/wp-content/uploads/2026/02/image-33-1024x462.png)

![Chat model fully connected](https://api.theowllogic.com/wp-content/uploads/2026/02/image-34.png)

**Step 4** — Add **Simple Memory** for conversation context:

![Memory sub-node](https://api.theowllogic.com/wp-content/uploads/2026/02/image-35-1024x501.png)

![Simple Memory config](https://api.theowllogic.com/wp-content/uploads/2026/02/image-36-1024x553.png)

Simple Memory keeps conversation history in RAM during the current session. It's fine for testing and simple chatbots, but resets on every new execution. For production agents that need persistent memory, use a database-backed memory node instead.

### Next Steps for Agents

Once the basic agent works, extend it by:
- Adding **Tools** (Google Sheets lookup, HTTP Request, Calculator) so the agent can take action
- Replacing Simple Memory with **Postgres Chat Memory** or **Redis** for persistence
- Adding a **Webhook trigger** instead of Chat Trigger for integration with Telegram, Slack, or your existing OpenClaw bot
- Using **system prompt variables** to pass context like the current user's name or account ID

---

# APPENDIX — Image Download Script

Save this as `download-images.sh` in the same folder as this markdown file. Run it on your Mac Mini to download all screenshots locally:

```bash
#!/bin/bash
# Download all images referenced in the n8n learning guide
# Run from the same directory as the markdown file

mkdir -p assets

BASE="https://api.theowllogic.com/wp-content/uploads"

# Chapter 1 - Installing n8n
curl -sL -o "assets/ch1-banner.png" "$BASE/2025/11/Installing-n8n-locally-with-npm-and-docker.png"
curl -sL -o "assets/ch1-nodejs-download.png" "$BASE/2025/11/Pasted-image-20251005235756-1024x784.png"
curl -sL -o "assets/ch1-npm-install.png" "$BASE/2025/11/Pasted-image-20251006005539-1024x625.png"
curl -sL -o "assets/ch1-n8n-running.png" "$BASE/2025/11/image.png"
curl -sL -o "assets/ch1-n8n-register.png" "$BASE/2025/11/Pasted-image-20251006001030-1024x465.png"
curl -sL -o "assets/ch1-n8n-tips.png" "$BASE/2025/11/Pasted-image-20251006095451-1024x597.png"
curl -sL -o "assets/ch1-docker-download.png" "$BASE/2025/11/Pasted-image-20251006002149-1024x464.png"
curl -sL -o "assets/ch1-docker-volumes.png" "$BASE/2025/11/Pasted-image-20251006100148-1024x618.png"
curl -sL -o "assets/ch1-docker-volume-name.png" "$BASE/2025/11/Pasted-image-20251006100448-1024x626.png"
curl -sL -o "assets/ch1-docker-volume-created.png" "$BASE/2025/11/Pasted-image-20251006100540-1024x632.png"
curl -sL -o "assets/ch1-docker-search.png" "$BASE/2025/11/Pasted-image-20251006003958-1024x587.png"
curl -sL -o "assets/ch1-docker-pull.png" "$BASE/2025/11/Pasted-image-20251006004229-1024x616.png"
curl -sL -o "assets/ch1-docker-settings1.png" "$BASE/2025/11/Pasted-image-20251006101415-1024x630.png"
curl -sL -o "assets/ch1-docker-settings2.png" "$BASE/2025/11/Pasted-image-20251006101604-1024x625.png"
curl -sL -o "assets/ch1-docker-env1.png" "$BASE/2025/11/Pasted-image-20251006102134-1024x640.png"
curl -sL -o "assets/ch1-docker-env2.png" "$BASE/2025/11/Pasted-image-20251006102527-1024x628.png"
curl -sL -o "assets/ch1-docker-running.png" "$BASE/2025/11/Pasted-image-20251006103137-1024x638.png"
curl -sL -o "assets/ch1-account-setup.png" "$BASE/2025/11/Pasted-image-20251006103324.png"
curl -sL -o "assets/ch1-license.png" "$BASE/2025/11/Pasted-image-20251006103805-1024x414.png"
curl -sL -o "assets/ch1-license-key.png" "$BASE/2025/11/Pasted-image-20251006103911-1024x640.png"
curl -sL -o "assets/ch1-docker-logs.png" "$BASE/2025/11/Pasted-image-20251006104341-1024x463.png"
curl -sL -o "assets/ch1-docker-stop.png" "$BASE/2025/11/Pasted-image-20251006104626-1024x647.png"
curl -sL -o "assets/ch1-docker-cli.png" "$BASE/2025/11/Pasted-image-20251006105127-1024x617.png"

# Chapter 2 - Hello World
curl -sL -o "assets/ch2-banner.png" "$BASE/2025/11/create-first-n8n-workflow.png"
curl -sL -o "assets/ch2-overview.png" "$BASE/2025/11/1-1024x462.png"
curl -sL -o "assets/ch2-static.png" "$BASE/2025/11/2-1024x481.png"
curl -sL -o "assets/ch2-new-workflow.png" "$BASE/2025/11/3-1024x436.png"
curl -sL -o "assets/ch2-trigger-sidebar.png" "$BASE/2025/11/4-1024x481.png"
curl -sL -o "assets/ch2-manual-trigger.png" "$BASE/2025/11/5-1024x357.png"
curl -sL -o "assets/ch2-set-node1.png" "$BASE/2025/11/6-1024x518.png"
curl -sL -o "assets/ch2-set-node2.png" "$BASE/2025/11/7-1024x425.png"
curl -sL -o "assets/ch2-set-node3.png" "$BASE/2025/11/8-1024x465.png"
curl -sL -o "assets/ch2-set-node4.png" "$BASE/2025/11/9-1024x435.png"
curl -sL -o "assets/ch2-canvas.png" "$BASE/2025/11/10-1024x365.png"
curl -sL -o "assets/ch2-execute.png" "$BASE/2025/11/11-1024x522.png"
curl -sL -o "assets/ch2-output.png" "$BASE/2025/11/12-1024x440.png"
curl -sL -o "assets/ch2-dynamic.png" "$BASE/2025/11/13-1024x542.png"
curl -sL -o "assets/ch2-edit-trigger.png" "$BASE/2025/11/14-1024x428.png"
curl -sL -o "assets/ch2-data-visible.png" "$BASE/2025/11/15-1024x558.png"
curl -sL -o "assets/ch2-set-data.png" "$BASE/2025/11/16-1024x402.png"

# Chapter 3 - Workflows Nodes Data Flow
curl -sL -o "assets/ch3-banner.png" "$BASE/2025/11/n8n-workflows-nodes-and-data-flow.png"
curl -sL -o "assets/ch3-canvas-overview.png" "$BASE/2025/11/Pasted-image-20251006190528-1024x448.png"

# Chapter 4 - Webhooks
curl -sL -o "assets/ch4-banner.png" "$BASE/2025/11/whats-a-webhook-in-n8n.png"
curl -sL -o "assets/ch4-ngrok-tunnel.png" "$BASE/2025/11/image-3-1024x432.png"
curl -sL -o "assets/ch4-ngrok-install.png" "$BASE/2025/11/image-4-1024x388.png"
curl -sL -o "assets/ch4-url-anatomy.png" "$BASE/2025/11/image-14.png"
curl -sL -o "assets/ch4-wp-form.png" "$BASE/2025/11/image-7-1024x765.png"
curl -sL -o "assets/ch4-cf7-plugin.png" "$BASE/2025/11/image-5-1024x552.png"
curl -sL -o "assets/ch4-sheets-node.png" "$BASE/2025/11/image-6-1024x369.png"
curl -sL -o "assets/ch4-sheets-config.png" "$BASE/2025/11/image-8-1024x390.png"
curl -sL -o "assets/ch4-expression-map.png" "$BASE/2025/11/image-9-1024x502.png"
curl -sL -o "assets/ch4-activate.png" "$BASE/2025/11/image-10-1024x454.png"
curl -sL -o "assets/ch4-prod-url.png" "$BASE/2025/11/image-11.png"
curl -sL -o "assets/ch4-form-test.png" "$BASE/2025/11/image-12-1024x545.png"
curl -sL -o "assets/ch4-sheet-result.png" "$BASE/2025/11/image-13-1024x352.png"
curl -sL -o "assets/ch4-error.png" "$BASE/2025/11/image-15-1024x349.png"

# Chapter 5 - Rate Limits
curl -sL -o "assets/ch5-banner.png" "$BASE/2025/11/how-to-handle-API-rate-limits-in-n8n.png"
curl -sL -o "assets/ch5-throttle1.png" "$BASE/2025/11/image-16-1024x409.png"
curl -sL -o "assets/ch5-throttle2.png" "$BASE/2025/11/image-17-1024x407.png"
curl -sL -o "assets/ch5-batch-size.png" "$BASE/2025/11/image-19.png"
curl -sL -o "assets/ch5-wait.png" "$BASE/2025/11/image-18.png"
curl -sL -o "assets/ch5-retry-workflow.png" "$BASE/2025/11/image-20-1024x260.png"
curl -sL -o "assets/ch5-retry-config.png" "$BASE/2025/11/image-21.png"
curl -sL -o "assets/ch5-error-output.png" "$BASE/2025/11/image-22.png"
curl -sL -o "assets/ch5-retry-success.png" "$BASE/2025/11/image-23-1024x258.png"

# Chapter 6 - Error Handling
curl -sL -o "assets/ch6-banner.png" "$BASE/2025/11/handle-errors-in-n8n-like-a-pro.png"
curl -sL -o "assets/ch6-error-concept.png" "$BASE/2025/11/Error-trigger-workflow-setup.png"
curl -sL -o "assets/ch6-error-settings.png" "$BASE/2025/11/workflow-settings-for-error-trigger-1024x538.png"
curl -sL -o "assets/ch6-error-select.png" "$BASE/2025/11/Error-workflow-settings-1024x534.png"
curl -sL -o "assets/ch6-error-email.png" "$BASE/2025/11/image-32.png"
curl -sL -o "assets/ch6-validation.png" "$BASE/2025/11/image-33-1024x403.png"
curl -sL -o "assets/ch6-fake-db.png" "$BASE/2025/11/image-34.png"
curl -sL -o "assets/ch6-status-tracking.png" "$BASE/2025/11/image-35-1024x398.png"

# Chapter 7 - Expressions
curl -sL -o "assets/ch7-banner.png" "$BASE/2026/01/n8n-expression-practical-complete-guide.png"
curl -sL -o "assets/ch7-toggle.png" "$BASE/2026/01/image-2.png"
curl -sL -o "assets/ch7-error.png" "$BASE/2026/01/image-3.png"
curl -sL -o "assets/ch7-editor.png" "$BASE/2026/01/image-4-1024x411.png"
curl -sL -o "assets/ch7-webhook-data.png" "$BASE/2026/01/image-5-1024x268.png"

# Chapter 8 - Conditional Logic
curl -sL -o "assets/ch8-banner.png" "$BASE/2026/01/building-conditional-logics-in-n8n.png"
curl -sL -o "assets/ch8-if-intro.png" "$BASE/2026/01/IF-Node-decision-controller-1024x576.png"
curl -sL -o "assets/ch8-spreadsheet.png" "$BASE/2026/01/image-6.png"
curl -sL -o "assets/ch8-sketch.png" "$BASE/2026/01/image-7.png"
curl -sL -o "assets/ch8-condition.png" "$BASE/2026/01/image-8-1024x537.png"
curl -sL -o "assets/ch8-results.png" "$BASE/2026/01/image-9-1024x455.png"
curl -sL -o "assets/ch8-updated-sheet.png" "$BASE/2026/01/image-10-1024x454.png"
curl -sL -o "assets/ch8-map-boolean.png" "$BASE/2026/01/image-11-1024x394.png"
curl -sL -o "assets/ch8-boolean-true.png" "$BASE/2026/01/image-12-1024x441.png"
curl -sL -o "assets/ch8-combined-result.png" "$BASE/2026/01/image-13-1024x465.png"
curl -sL -o "assets/ch8-and-or.png" "$BASE/2026/01/image-16.png"
curl -sL -o "assets/ch8-expression-detail.png" "$BASE/2026/01/image-17-1024x321.png"
curl -sL -o "assets/ch8-expression-if.png" "$BASE/2026/01/image-18.png"
curl -sL -o "assets/ch8-expression-result.png" "$BASE/2026/01/image-19.png"
curl -sL -o "assets/ch8-switch-intro.png" "$BASE/2026/01/Switch-node-introduction-in-n8n-1024x576.png"
curl -sL -o "assets/ch8-switch-connected.png" "$BASE/2026/01/image-20-1024x375.png"
curl -sL -o "assets/ch8-switch-rules.png" "$BASE/2026/01/image-21-1024x572.png"
curl -sL -o "assets/ch8-switch-execute.png" "$BASE/2026/01/image-22.png"
curl -sL -o "assets/ch8-switch-ui.png" "$BASE/2026/01/image-23-1024x461.png"
curl -sL -o "assets/ch8-switch-expr.png" "$BASE/2026/01/image-24-1024x444.png"
curl -sL -o "assets/ch8-switch-final.png" "$BASE/2026/01/image-25-1024x354.png"

# Chapter 9 - Loops
curl -sL -o "assets/ch9-banner.png" "$BASE/2026/01/How-to-use-loop-over-items.png"
curl -sL -o "assets/ch9-pinned.png" "$BASE/2026/01/image-27-1024x302.png"
curl -sL -o "assets/ch9-batch.png" "$BASE/2026/01/image-28-1024x437.png"
curl -sL -o "assets/ch9-loop-http.png" "$BASE/2026/01/image-29.png"
curl -sL -o "assets/ch9-http-config.png" "$BASE/2026/01/image-30-1024x690.png"
curl -sL -o "assets/ch9-code.png" "$BASE/2026/01/image-31-1024x464.png"
curl -sL -o "assets/ch9-flowchart.png" "$BASE/2026/01/image-32-1024x572.png"

# Chapter 10 - Sub-Workflows
curl -sL -o "assets/ch10-banner.png" "$BASE/2026/01/How-to-use-sub-workflows.png"
curl -sL -o "assets/ch10-data-sheet.png" "$BASE/2026/01/image-34-1024x408.png"
curl -sL -o "assets/ch10-parent-start.png" "$BASE/2026/01/image-36.png"
curl -sL -o "assets/ch10-execute-node.png" "$BASE/2026/01/image-37.png"
curl -sL -o "assets/ch10-execute-config.png" "$BASE/2026/01/image-38-1024x485.png"
curl -sL -o "assets/ch10-sub-trigger.png" "$BASE/2026/01/image-39.png"
curl -sL -o "assets/ch10-sub-canvas.png" "$BASE/2026/01/image-40.png"
curl -sL -o "assets/ch10-sub-logic.png" "$BASE/2026/01/image-41-1024x536.png"
curl -sL -o "assets/ch10-parent-update.png" "$BASE/2026/01/image-43-1024x328.png"
curl -sL -o "assets/ch10-sheet-update.png" "$BASE/2026/01/image-44-1024x830.png"
curl -sL -o "assets/ch10-executing.png" "$BASE/2026/01/image-45-1024x372.png"
curl -sL -o "assets/ch10-sub-executions.png" "$BASE/2026/01/image-46-1024x582.png"
curl -sL -o "assets/ch10-final-sheet.png" "$BASE/2026/01/Screenshot_2-1024x377.png"

# Chapter 11 - Planning
curl -sL -o "assets/ch11-banner.png" "$BASE/2026/02/How-to-plan-n8n-workflow.png"
curl -sL -o "assets/ch11-framework.png" "$BASE/2026/02/The-WOrkflow-thinking-framework-1024x576.png"
curl -sL -o "assets/ch11-patterns.png" "$BASE/2026/02/3-mental-model-patterns-1024x576.png"

# Chapter 12 - 50 Boring Tasks
curl -sL -o "assets/ch12-banner.png" "$BASE/2026/02/Automate-boring-tasks-with-n8n.png"
curl -sL -o "assets/ch12-ideas.png" "$BASE/2026/02/boring-ideas-automate-1024x576.png"

# Chapter 13 - Email Sequence
curl -sL -o "assets/ch13-banner.png" "$BASE/2026/02/build-email-sequence-in-n8n.png"
curl -sL -o "assets/ch13-plan.png" "$BASE/2026/02/Email-sequence-planning-on-excalidraw-1024x453.png"
curl -sL -o "assets/ch13-sheets-connect.png" "$BASE/2026/02/image-2.png"
curl -sL -o "assets/ch13-sheets-config.png" "$BASE/2026/02/image-3-1024x612.png"
curl -sL -o "assets/ch13-contact-sheet.png" "$BASE/2026/02/image-4-1024x455.png"
curl -sL -o "assets/ch13-switch.png" "$BASE/2026/02/image-5.png"
curl -sL -o "assets/ch13-switch-rules.png" "$BASE/2026/02/image-6-1024x667.png"
curl -sL -o "assets/ch13-switch-columns.png" "$BASE/2026/02/image-7-1024x712.png"
curl -sL -o "assets/ch13-set-nodes.png" "$BASE/2026/02/image-8.png"
curl -sL -o "assets/ch13-set-config.png" "$BASE/2026/02/image-9-1024x397.png"

# Chapter 14 - AI Agents
curl -sL -o "assets/ch14-banner.png" "$BASE/2026/02/How-to-build-an-n8n-AI-Agent-in-2026.png"
curl -sL -o "assets/ch14-chat-trigger.png" "$BASE/2026/02/image-25-1024x428.png"
curl -sL -o "assets/ch14-chat-url.png" "$BASE/2026/02/image-26-1024x401.png"
curl -sL -o "assets/ch14-agent-node.png" "$BASE/2026/02/image-27-1024x388.png"
curl -sL -o "assets/ch14-system-prompt.png" "$BASE/2026/02/image-28-1024x467.png"
curl -sL -o "assets/ch14-chat-model.png" "$BASE/2026/02/image-30.png"
curl -sL -o "assets/ch14-gemini-config.png" "$BASE/2026/02/image-31-1024x629.png"
curl -sL -o "assets/ch14-api-key.png" "$BASE/2026/02/image-32-1024x548.png"
curl -sL -o "assets/ch14-model-select.png" "$BASE/2026/02/image-33-1024x462.png"
curl -sL -o "assets/ch14-model-connected.png" "$BASE/2026/02/image-34.png"
curl -sL -o "assets/ch14-memory-sub.png" "$BASE/2026/02/image-35-1024x501.png"
curl -sL -o "assets/ch14-simple-memory.png" "$BASE/2026/02/image-36-1024x553.png"

echo ""
echo "✅ Downloaded $(ls assets/*.png 2>/dev/null | wc -l) images to assets/"
echo ""
echo "To use local images in Obsidian, run this find-replace in the markdown:"
echo "  Find:    https://api.theowllogic.com/wp-content/uploads/"
echo "  Replace: assets/"
echo ""
echo "Then rename files to match the download names, or update the markdown paths."
```

> **Usage**: `chmod +x download-images.sh && ./download-images.sh`

---

*End of Guide — 14 chapters, ~120 screenshots referenced, covering n8n from installation through AI agents.*
