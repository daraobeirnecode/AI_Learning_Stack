**AI MASTERY FIELD GUIDE**

*Complete Step-by-Step Edition*

10 Concepts • Expert Definitions • Detailed Project Instructions

*Every project explained as if you've never done it before*

March 2026

# Table of Contents

[Table of Contents](#table-of-contents)

[How to Use This Guide](#how-to-use-this-guide)

[What It Is](#what-it-is)

[Core Mechanics](#core-mechanics)

[Common Misconceptions](#common-misconceptions)

[Tools](#tools)

[What It Is](#what-it-is-1)

[Core Mechanics](#core-mechanics-1)

[Tools](#tools-1)

[What It Is](#what-it-is-2)

[Core Mechanics](#core-mechanics-2)

[Tools](#tools-2)

[What It Is](#what-it-is-3)

[Core Mechanics](#core-mechanics-3)

[Tools](#tools-3)

[What It Is](#what-it-is-4)

[Tools](#tools-4)

[What It Is](#what-it-is-5)

[PEFT: Fine-Tuning Without a GPU Farm](#peft-fine-tuning-without-a-gpu-farm)

[Tools](#tools-5)

[What It Is](#what-it-is-6)

[Tools](#tools-6)

[What It Is](#what-it-is-7)

[Tools](#tools-7)

[What It Is](#what-it-is-8)

[Tools](#tools-8)

[What It Is](#what-it-is-9)

[Tools](#tools-9)

[How the 10 Concepts Connect](#how-the-10-concepts-connect)

[Done When](#done-when)

[Done When](#done-when-1)

[Done When](#done-when-2)

[Done When](#done-when-3)

[Done When](#done-when-4)

[Done When](#done-when-5)

[Done When](#done-when-6)

[Done When](#done-when-7)

[Done When](#done-when-8)

[Done When](#done-when-9)

[Done When](#done-when-10)

[The Challenge](#the-challenge)

[Architecture Requirements (One per Concept)](#architecture-requirements-one-per-concept)

[1. Prompt Engineering](#prompt-engineering)

[2. Workflow Automation](#workflow-automation)

[3. AI Agents](#ai-agents)

[4. RAG](#rag)

[5. Multimodal AI](#multimodal-ai)

[6. Custom Assistant](#custom-assistant)

[7. Voice Interface](#voice-interface)

[8. Tool Stack](#tool-stack)

[9. Video Output](#video-output)

[10. Deployed Product](#deployed-product)

[Required Deliverables](#required-deliverables)

[Retrospective Prompts](#retrospective-prompts)

# How to Use This Guide

This guide is divided into two parts. Part One gives you expert-level definitions of each AI concept — the theory, the mechanics, how the tools work, and how everything connects. Part Two is a step-by-step hands-on learning plan with instructions written for someone who has never done any of it before.

Every project in Part Two follows the same structure: a plain-English explanation of what you are building and why, then numbered steps that tell you exactly what to click, type, or install — including exact commands, what you should see on screen, and how to know if something went wrong.

Icon key:

- ℹ️ Blue note — context, explanation, or expected result

- ⚠️ Warning — something that commonly goes wrong here

- ✅ Done-when checkpoint — how you know a step is complete

- ▶ ACTION box — marks the start of a distinct hands-on sub-project

**PART ONE**

*Expert-Level Concept Definitions*


> 1. Prompt Engineering
>
> Tools: ChatGPT, Gemini, Claude, Perplexity
>
>
> Resources: OpenAI Learn, DeepLearning.AI Prompt Engineering, Anthropic Docs


## What It Is

Prompt engineering is the discipline of crafting and iterating on natural language instructions to reliably elicit specific, high-quality responses from large language models. At expert level it is a rigorous engineering practice rooted in how transformer architectures process token sequences. LLMs are next-token prediction engines — prompt engineering shapes the input context so the most likely completions are also the most useful ones.

### Core Mechanics

**Context Window**

Every LLM has a fixed context window — the total tokens it can process at once (input + output). Models like Claude 3.5 and GPT-4o support 128K-200K+ token windows. Your prompt, conversation history, and response all consume this budget. Expert prompt engineers always account for token budget.

**System vs. User Roles**

API calls use a message structure with roles. The System message sets the model's persistent persona and constraints — it is the highest-authority instruction. The User message is the specific query. A well-crafted system prompt eliminates entire categories of failure modes before the user asks anything.

**Key Techniques**

- Zero-shot: ask the model to perform a task with no examples. Works for tasks clearly within the model's training distribution.

- Few-shot: provide 2-10 input/output example pairs before the request. Shifts the model's distribution toward your desired format and style.

- Chain-of-thought (CoT): instruct the model to reason step by step before giving a final answer. Dramatically improves accuracy on multi-step problems.

- Role prompting: assign a specific expert persona. Activates relevant prior context in the model's weights.

- Structured output: constrain the model to produce JSON or other parseable formats. Essential for production systems.

### Common Misconceptions

- Longer is not better. Unfocused prompts dilute signal and waste token budget.

- Prompts are not universal. GPT-4o, Claude, and Gemini produce different results from identical prompts.

### Tools

- ChatGPT: System/user/assistant message structure exposed via API. Custom GPTs are system prompts with attached knowledge and tools.

- Gemini: Extreme context windows (2M tokens in Gemini 1.5). Enables whole-document analysis without RAG.

- Claude: Particularly responsive to clear role definitions and XML-structured instructions. Anthropic's documentation is the most detailed publicly available resource on prompting best practices.

- Perplexity: Search-augmented LLM. Best for research tasks requiring current, cited information.


> 2. AI Workflow Automation
>
> Tools: Zapier, Make, n8n, Bardeen
>
>
> Resources: Zapier University, Make Academy, Bardeen Playbooks


## What It Is

AI workflow automation embeds AI models as active decision-making components in software pipelines. The foundational pattern: Trigger (something happens) → Process (data moves) → AI Node (model reasons or generates) → Action (something happens in the real world). The AI node is what distinguishes this from a standard automation.

### Core Mechanics

**Triggers and Webhooks**

A trigger is the event that starts a workflow: a new email, a form submission, a scheduled time, or an inbound webhook call. A webhook is a URL that accepts HTTP POST requests — when another system sends data to that URL, the workflow fires. This is how most production AI workflows receive inputs.

**Self-Hosted vs. SaaS**

Zapier and Make are SaaS: fast to start, no infrastructure required, but costs scale with usage and your data leaves your network. n8n is self-hosted: requires a server to run on, but gives unlimited executions, full data control, and the ability to write custom JavaScript or Python nodes.

### Tools

- Zapier: 6,000+ app integrations, built-in AI nodes for OpenAI and Anthropic. Per-task pricing makes it expensive at scale.

- Make: More expressive branching, iterators, and aggregators than Zapier. Better for complex data transformation.

- n8n: Open-source, Docker-deployable. Supports custom code nodes, LangChain integration. The expert tool for production systems.

- Bardeen: Browser extension-based automation for web scraping and CRM data entry.


> 3. AI Agents
>
> Tools: CrewAI, AutoGen, LangChain, LangGraph
>
>
> Resources: LangChain Docs, AutoGen Cookbook, CrewAI Docs


## What It Is

An AI agent is a software system in which an LLM autonomously decides what to do next, executes actions via tools, observes results, and iterates until a goal is achieved. The critical distinction from a chatbot: an agent runs Perceive → Plan → Act → Observe → repeat, until the goal is reached. Agents are given goals, not prompts.

### Core Mechanics

**Tool Use / Function Calling**

The LLM outputs a structured JSON payload naming a function and its arguments. The host application executes that function and returns the result. The agent reasons about the result and decides the next step. Tools can be anything: web search, file system, database, email, or custom functions.

**Memory Types**

- In-context: conversation history in the current window. Fast but non-persistent.

- External / vector store: semantic search over past episodes. Enables memory across sessions.

- Structured state: explicit key-value stores the agent reads and writes. Reliable for tracking progress.

### Tools

- CrewAI: Python framework for agent teams with roles, goals, backstories, and tools. Designed for rapid multi-agent prototyping.

- AutoGen (Microsoft): Multi-agent framework emphasizing conversational collaboration. Supports human-in-the-loop patterns.

- LangChain: Comprehensive LLM framework. Provides agent, chain, memory, and tool primitives.

- LangGraph: Extension for stateful, cyclical agent workflows as directed graphs. The production choice for complex agents.


> 4. Retrieval-Augmented Generation (RAG)
>
> Tools: LlamaIndex, LangChain, Vectara, Haystack
>
>
> Resources: LlamaIndex Docs, Haystack Tutorials, Vectara Academy


## What It Is

RAG solves LLMs' core limitation: knowledge frozen at training time. It retrieves relevant documents from an external store at query time, injects them into the prompt, and has the model generate a response grounded in that retrieved evidence. This enables accurate answers about private data and dramatically reduces hallucination.

### Core Mechanics

**Indexing Phase**

Documents are split into chunks (256-1024 tokens), embedded using an embedding model into high-dimensional vectors where semantically similar text produces similar vectors, and stored in a vector database. Quality of embedding model and chunk strategy directly determines retrieval quality.

**Query Phase**

The user's question is embedded using the same model. The vector database finds the chunks with the most similar vectors (approximate nearest neighbor search). Top-k chunks are inserted into the prompt as context and the LLM generates a grounded answer.

### Tools

- LlamaIndex: Leading RAG framework. 160+ data source connectors, multiple index types, advanced patterns like sub-question decomposition.

- LangChain: General-purpose framework with strong RAG primitives and agent integration.

- Vectara: Managed RAG-as-a-service. Upload documents via API; Vectara handles everything. Includes MMR re-ranking.

- Haystack: Production NLP framework. Strong evaluation tooling and enterprise support.


> 5. Multimodal AI
>
> Tools: ChatGPT (Vision), Gemini, Claude, Grok
>
>
> Resources: OpenAI Guides


## What It Is

Multimodal AI refers to models that process and reason across multiple input types — text, images, audio, video — within a single unified architecture. A true multimodal model uses joint embeddings enabling cross-modal reasoning: analyzing charts, reading diagrams, generating code from screenshots. This is distinct from a pipeline that does OCR and passes text to an LLM.

### Tools

- ChatGPT / GPT-4o: Processes text, images, and audio natively. GPT-4o was designed for native multimodal processing from the ground up.

- Gemini: Natively multimodal from inception. 2M token window enables analysis of hour-long videos in a single request.

- Claude: Accepts images and documents alongside text. Strong at reading charts, tables, and diagrams within PDFs.

- Grok (xAI): Multimodal model with image understanding. Note: the source images label this 'GROK' — this is xAI's model integrated into the X platform.


> 6. Fine-Tuning & AI Assistants
>
> Tools: OpenAI GPT Builder, Hugging Face, Cohere, NVIDIA
>
>
> Resources: Hugging Face Hub, Cohere Academy, NVIDIA AI Foundations


## What It Is

Fine-tuning continues training a pre-trained model on domain-specific data — actually updating the model's weights. AI Assistants (GPT Builder) configure model behavior through system prompts, knowledge bases, and tools without changing weights. For most business use cases, a well-designed assistant with RAG outperforms fine-tuning and requires far less investment. Fine-tune when the model needs to learn reasoning patterns, not just new information.

### PEFT: Fine-Tuning Without a GPU Farm

LoRA (Low-Rank Adaptation) inserts small trainable adapter layers while freezing the rest of the model. This achieves 90%+ of full fine-tuning benefit at 1-10% of the compute cost and is the standard practical approach.

### Tools

- OpenAI GPT Builder: No-code Custom GPT creation. Configure instructions, upload knowledge files, connect API actions.

- Hugging Face: Central hub for open-source AI. AutoTrain provides no-code fine-tuning. PEFT library implements LoRA.

- Cohere: Enterprise API with Command R+ fine-tuning, reranking models, and RAG-optimized embedding.

- NVIDIA: H100 GPU hardware and NeMo framework for large-scale fine-tuning and TensorRT-LLM inference optimization.


> 7. Voice AI & Avatars
>
> Tools: ElevenLabs, HeyGen, Synthesia, Vapi
>
>
> Resources: ElevenLabs Knowledge Base


## What It Is

Voice AI covers Text-to-Speech (TTS), Speech-to-Text (STT/ASR), and conversational voice pipelines combining both with an LLM in a real-time loop. AI Avatars add video: generating realistic human presenters synchronized to generated audio. A real-time voice agent runs: STT (70-200ms) → LLM (200-800ms) → TTS (100-300ms). Total must be under 800ms for natural conversation, requiring streaming at every stage.

### Tools

- ElevenLabs: Leading neural TTS. Voice cloning from 1 minute of audio. 29+ languages. Conversational AI SDK for complete voice agent pipelines.

- HeyGen: Instant Avatar creation from a 2-minute video. Realistic lip sync and expression. Interactive Avatar API for real-time conversations.

- Synthesia: Enterprise AI video. 230+ pre-built avatars, 130+ languages. No camera or studio required.

- Vapi: Developer platform for production voice agents. Sub-500ms latency. Native phone call integration. Function calling enables agents to take real actions during calls.


> 8. AI Tool Stacking
>
> Tools: Notion, ClickUp, Asana, Zapier
>
>
> Resources: Notion Academy, ClickUp University


## What It Is

AI Tool Stacking is the systems design practice of assembling AI-enabled tools into a coherent architecture where each tool's outputs feed the next tool's inputs. Three layers: Intelligence (LLMs and AI APIs), Automation (n8n, Zapier — the connective tissue), and Operational (Notion, ClickUp, CRM — where work is tracked). Well-designed stacks have a single source of truth, event-driven triggers, idempotent operations, and observable logging.

### Tools

- Notion: Flexible workspace with databases, docs, and wikis. Notion AI adds LLM-powered writing and Q&A. Excellent operational hub for AI-generated outputs.

- ClickUp: Project management with AI task summarization and sprint planning. Strong Zapier/n8n integration.

- Asana: Enterprise work management. Asana Intelligence provides AI project status summaries. Strong API for programmatic task creation.

- Zapier: The connective tissue connecting operational tools to the intelligence layer.


> 9. AI Video Content Generation
>
> Tools: Runway, VEED, Opus Clip, OpenAI Sora
>
>
> Resources: Runway Learn, VEED Academy


## What It Is

Two distinct capabilities: generative video (creating new video from text prompts using diffusion architectures) and AI-powered video editing (analyzing, restructuring, captioning, and repurposing existing footage). Confusing them leads to choosing the wrong tool. Runway and Sora create from nothing; VEED and Opus Clip enhance and repurpose what you already have.

### Tools

- Runway: Professional generative video. Gen-3 Alpha produces high-quality short clips from text or image prompts. Also includes video editing: inpainting, background removal, motion brush.

- VEED: Browser-based AI video editing. Auto-subtitles, noise removal, eye contact correction, avatar presenter option.

- Opus Clip: Long-form to short-form repurposing. AI scores clips by virality potential and auto-adds captions and formatting.

- OpenAI Sora: State-of-the-art text-to-video. Up to 1-minute photorealistic videos with coherent physics and camera motion. Available via ChatGPT Pro.


> 10. AI-Powered SaaS Development
>
> Tools: Bubble, Lovable, Cursor, Windsurf
>
>
> Resources: Bubble Academy, Lovable Docs


## What It Is

Building software products where AI is embedded in core product logic from day one. The tooling landscape has bifurcated: no-code/low-code platforms (Bubble, Lovable) abstract the software stack behind visual editors or natural language; AI-augmented IDEs (Cursor, Windsurf) supercharge existing developers by acting as expert pair programmers with full codebase context. The bottleneck shifts from coding ability to product clarity.

### Tools

- Bubble: Visual no-code web app builder. 300,000+ apps built on the platform. Connect to any API via the built-in API Connector.

- Lovable: AI-native app generator. Describe your product in plain English and receive a working React + Supabase application. All code is real, readable, and exportable.

- Cursor: AI code editor built on VS Code. Deeply indexes your codebase. Multi-file agentic edits via Composer mode. The productivity standard among professional developers.

- Windsurf (Codeium): AI code editor with Cascade, an agentic assistant for multi-step development tasks.

# How the 10 Concepts Connect

These concepts form an integrated stack with clear dependencies. Prompt engineering is the foundation — every other concept involves sending prompts to LLMs. Workflow automation and agents are the execution layers, both extending prompting into action. RAG makes agents trustworthy by grounding them in your actual knowledge. Multimodal AI expands every layer's input surface. Fine-tuning specializes the intelligence layer when prompting and RAG are insufficient. Voice and video are output channels. Tool stacking is the architecture connecting everything. AI-powered SaaS is where all concepts ship as real products.

The dependency order for learning: 1 (Prompt Engineering) → 2 (Workflow Automation) → 4 (RAG) → 3 (Agents) → 5 (Multimodal) → 6 (Fine-Tuning) → 7 (Voice) → 8 (Tool Stack) → 9 (Video) → 10 (SaaS). Each step in Part Two follows this sequence, building on what came before.

**PART TWO**

*Hands-On Mastery Plan*

*Step-by-step instructions written as if you've never done any of this before*

| **STEP 1: Prompt Engineering Fundamentals** | **Week 1** |
|---------------------------------------------|------------|

In this step you will set up accounts on three major AI models, learn the core prompting techniques through hands-on exercises, and build a personal Prompt Lab — a tested library of prompts you will use and iterate on throughout this guide.


> **▶ ACTION: 1A — Create Accounts on Three AI Models**


**Claude.ai**

1.  Open your browser and go to https://claude.ai

2.  Click 'Sign up' in the top right.

3.  Enter your email address, click Continue, and verify your email.

4.  Create a password and complete setup. You will land on the Claude chat interface with a text input at the bottom.

> *ℹ️ The free tier gives access to Claude Sonnet. This is sufficient for all exercises in this guide.*

**ChatGPT**

5.  Go to https://chat.openai.com and click 'Sign up.'

6.  Sign up with Google, Microsoft, or an email address.

7.  Complete verification. You will land on the ChatGPT interface. Free tier uses GPT-4o mini.

**Gemini**

8.  Go to https://gemini.google.com and click 'Sign in' with your Google account.

9.  Accept the terms of service. You will land on the Gemini chat interface.


> **▶ ACTION: 1B — Learn the Four Core Techniques**


You will run a structured exercise comparing all three models. The goal is to develop intuition for how different prompting approaches change results — not to get perfect outputs.

**Exercise 1: Zero-Shot vs. Few-Shot**

Choose a classification task relevant to you. This example uses classifying a customer email:

Zero-shot (no examples):

> Classify the following email into one of these categories: billing_question, technical_issue, feature_request, or general_inquiry. Reply with only the category name. Email: 'Hi, I was charged twice this month and need a refund.'

Run this on all three models. Write down each result.

Now add few-shot examples:

> Classify the following email into one of these categories: billing_question, technical_issue, feature_request, or general_inquiry. Reply with only the category name. Examples: Email: 'My invoice shows the wrong amount.' -> billing_question Email: 'The app keeps crashing when I open it.' -> technical_issue Email: 'Can you add dark mode?' -> feature_request Email: 'Just wanted to say your product is great!' -> general_inquiry Email: 'Hi, I was charged twice this month and need a refund.'
>
> *ℹ️ Compare results. Few-shot almost always produces more consistent, correctly-formatted outputs. This is why production systems use examples.*

**Exercise 2: Chain-of-Thought Prompting**

First try without CoT:

> A store buys 200 items at $4 each and sells them for $7 each. Monthly overhead is $150. They sell 80% of inventory this month. What is their profit?

Then add 'Think step by step before giving your final answer.' at the end and compare accuracy.

> *ℹ️ CoT significantly improves accuracy on math, logic, and multi-step reasoning tasks by forcing intermediate reasoning into the context.*

**Exercise 3: Role Prompting**

Compare these two prompts on all three models:

Without role:

> What is a good annual revenue target for a small SaaS product in its first year?

With role:

> You are a SaaS startup advisor who has worked with 50+ B2B software companies from pre-revenue to Series A. A founder asks: 'What is a good annual revenue target for a small SaaS product in its first year?' Give specific benchmarks, caveats for different market types, and a practical framework for thinking about this.
>
> *ℹ️ The role prompt should produce more specific, structured, and useful output. The model activates relevant prior context from its training.*

**Exercise 4: Structured Output (JSON)**

This is essential for any AI output you need to process programmatically:

> Analyze the following customer review and extract key information. Respond ONLY with a valid JSON object. Do not include any text, explanation, or markdown code blocks outside the JSON. Required format: { "sentiment": "positive" \| "negative" \| "neutral", "main_complaint": "string or null", "main_praise": "string or null", "urgency": "high" \| "medium" \| "low" } Review: 'I love the product but delivery took three weeks and my package arrived damaged. I need a replacement ASAP.'
>
> *⚠️ If the model adds text before or after the JSON, add this to your prompt: 'Do not include any explanation, preamble, or text outside the JSON object.'*


> **▶ ACTION: 1C — Build Your Prompt Lab**


Create a new document (in Google Docs, Notion, or Word) called 'My Prompt Lab.' Create five sections: Zero-Shot, Few-Shot, Chain-of-Thought, Role Prompting, and Structured Output.

For each technique, write one prompt using a real task from your own work or life. Test it on all three models. For each, record: (a) what each model returned, (b) which model performed best, (c) what you would change to improve it.

### Done When

✅ You have accounts on Claude, ChatGPT, and Gemini and have sent at least one message on each.

✅ You can explain the difference between zero-shot, few-shot, chain-of-thought, and role prompting with concrete examples.

✅ Your Prompt Lab document has at least 4 tested prompts with notes on model comparison.

| **STEP 2: API Access & Programmatic Prompting** | **Week 1-2** |
|-------------------------------------------------|--------------|

In this step you will connect to the AI APIs directly from Python code. This is the foundation for everything that follows — every automation, agent, RAG system, and deployed app you build will use API calls.


> **▶ ACTION: 2A — Install Python and Set Up Your Project**


**Check if Python is Installed**

Open your terminal. On Mac: press Command+Space, type Terminal, press Enter. On Windows: press the Windows key, type cmd, press Enter.

> python –version
>
> *ℹ️ If you see 'Python 3.10' or higher, you are good. If you see an error or Python 2.x, go to https://python.org/downloads, download Python 3.11, run the installer. On Windows, check 'Add Python to PATH' before clicking Install.*

**Create Your Project Folder**

> mkdir ai-mastery cd ai-mastery

**Create a Virtual Environment**

On Mac/Linux:

> python -m venv venv source venv/bin/activate

On Windows:

> python -m venv venv venv\Scripts\activate
>
> *ℹ️ When active, you will see '(venv)' at the start of your terminal prompt. Always activate this before working on your project.*

**Install the AI Libraries**

> pip install openai anthropic python-dotenv


> **▶ ACTION: 2B — Get Your API Keys**


**OpenAI API Key**

10. Go to https://platform.openai.com and sign in with your ChatGPT account.

11. Click your profile icon in the top right, then 'API keys.'

12. Click 'Create new secret key.' Name it 'ai-mastery.'

13. Copy the key immediately. It starts with 'sk-proj-' and is only shown once.

> *⚠️ Never share your API key, post it on GitHub, or include it in screenshots. It is like a password that bills your credit card.*

**Anthropic API Key**

14. Go to https://console.anthropic.com and sign in.

15. Click 'API Keys' in the left sidebar, then 'Create Key.'

16. Name it 'ai-mastery,' copy the key immediately. It starts with 'sk-ant-'

**Store Keys in a .env File**

In your ai-mastery folder, create a .env file:

Mac/Linux:

> touch .env

Windows:

> type nul > .env

Open .env in a text editor and add:

> OPENAI_API_KEY=sk-proj-your-key-here ANTHROPIC_API_KEY=sk-ant-your-key-here

Create a .gitignore to prevent accidental upload:

> echo '.env' > .gitignore


> **▶ ACTION: 2C — Write Your First API Calls**


Open your ai-mastery folder in a text editor. VS Code is recommended — download at https://code.visualstudio.com if you don't have it. Create a file called test_apis.py and paste this entire script:

> import os from dotenv import load_dotenv from openai import OpenAI import anthropic load_dotenv() openai_client = OpenAI(api_key=os.environ.get('OPENAI_API_KEY')) anthropic_client = anthropic.Anthropic(api_key=os.environ.get('ANTHROPIC_API_KEY')) prompt = 'In exactly two sentences, explain what a large language model is.' print('=== GPT-4o-mini ===') openai_resp = openai_client.chat.completions.create( model='gpt-4o-mini', messages=[ {'role': 'system', 'content': 'You are a helpful assistant.'}, {'role': 'user', 'content': prompt} ] ) print(openai_resp.choices[0].message.content) print('\n=== Claude ===') claude_resp = anthropic_client.messages.create( model='claude-sonnet-4-5', max_tokens=1024, system='You are a helpful assistant.', messages=[{'role': 'user', 'content': prompt}] ) print(claude_resp.content[0].text)

Run it:

> python test_apis.py
>
> *ℹ️ You should see two responses. If you see 'AuthenticationError,' double-check your .env file for typos or extra spaces around the = sign.*


> **▶ ACTION: 2D — Build the Transcript Summarizer**


Create summarizer.py. This tool takes meeting transcript text and produces structured JSON with summary, action items, decisions, and open questions.

> import os from dotenv import load_dotenv import anthropic import json load_dotenv() client = anthropic.Anthropic(api_key=os.environ.get('ANTHROPIC_API_KEY')) def summarize_transcript(transcript_text): system_prompt = """You are an expert meeting facilitator. Analyze meeting transcripts and extract structured information. Always respond with valid JSON only. Do not include any text before or after the JSON object.""" user_prompt = f"""Analyze this meeting transcript. Respond ONLY with a JSON object in this exact format: {{ "summary": "2-3 sentence summary of what was discussed", "action_items": [ {{"owner": "person name or unassigned", "task": "what they need to do", "due": "deadline or not specified"}} ], "decisions": ["list of decisions made"], "open_questions": ["list of unresolved questions"] }} Transcript: {transcript_text}""" response = client.messages.create( model='claude-sonnet-4-5', max_tokens=2048, system=system_prompt, messages=[{'role': 'user', 'content': user_prompt}] ) return json.loads(response.content[0].text) sample = """ Sarah: We need to decide on the Q2 launch date. Mike: I need three more weeks. The backend isn't ready. Sarah: Three weeks is May 15th. Works for everyone? Jen: Works for me. I'll need final copy by May 8th. Mike: I can have backend done by May 10th. I'll update the tracker. Sarah: Jen, can you coordinate with design on the landing page? Jen: Yes, I'll reach out today. Do we have a budget for ads? Sarah: Still with finance. I'll follow up by Thursday. """ result = summarize_transcript(sample) print(json.dumps(result, indent=2))
>
> python summarizer.py
>
> *ℹ️ You should see a formatted JSON object with summary, action items (Mike's backend task, Sarah's finance follow-up, Jen's design coordination), decisions (May 15 launch), and open questions (ad budget).*
>
> *⚠️ If you get a JSON decode error, Claude returned text before the JSON. Strengthen your system prompt by adding: 'You must respond with ONLY the JSON object. Never include markdown code blocks or any explanation.'*

### Done When

✅ python test_apis.py shows responses from both OpenAI and Anthropic in your terminal.

✅ Your summarizer.py produces correctly-formatted JSON from the sample transcript.

✅ You can explain the difference between the system message and user message in an API call.

| **STEP 3: Workflow Automation with AI Nodes** | **Week 2-3** |
|-----------------------------------------------|--------------|

In this step you will install n8n using Docker, then build two real automation workflows: a lead triage system that uses Claude to classify incoming messages, and a daily intelligence briefing that summarizes news for you automatically every morning.


> **▶ ACTION: 3A — Install Docker Desktop**


17. Go to https://www.docker.com/products/docker-desktop

18. Download Docker Desktop for your operating system.

19. Run the installer. On Mac, drag Docker to Applications. On Windows, accept defaults.

20. Launch Docker Desktop. Wait until the bottom left corner says 'Docker Desktop is running.'

> *ℹ️ Docker Desktop may ask you to create a Docker Hub account. This is optional.*


> **▶ ACTION: 3B — Run n8n**


In your terminal, run this command exactly as written:

> docker run -it –rm –name n8n -p 5678:5678 -v ~/.n8n:/home/node/.n8n n8nio/n8n
>
> *ℹ️ The first time, Docker downloads the n8n image (~500MB). This takes 2-5 minutes. You will see download progress bars.*

When you see 'Editor is now accessible via: http://localhost:5678', n8n is running.

21. Open your browser and go to http://localhost:5678

22. Fill in the setup form with your name, email, and a password. Click 'Next.'

23. You will land on the n8n canvas — a blank workflow editor.

> *⚠️ Keep the terminal window open while using n8n. Closing it stops n8n.*


> **▶ ACTION: 3C — Add API Credentials to n8n**


24. In the n8n interface, click the gear icon in the bottom left sidebar.

25. Click 'Credentials,' then 'Add Credential.'

26. Search for 'OpenAI' and click it. Paste your OpenAI API key, click Save.

27. Repeat: 'Add Credential,' search for 'Anthropic,' paste your Anthropic key, click Save.

> *ℹ️ Your keys are now stored in n8n's local database and usable in any workflow.*


> **▶ ACTION: 3D — Build the Lead Triage Automation**


This workflow receives a form submission via webhook, uses Claude to classify the message type, and routes to different response paths.

28. Click '+' to create a new workflow. Click the pencil icon and name it 'Lead Triage.'

29. Click the '+' button or 'Add first step.' Search for 'Webhook' and click it.

30. In the node settings panel, set HTTP Method to 'POST.' Click Save. Copy the webhook URL shown.

Add the AI classification node:

31. Hover over the Webhook node's right side until you see '+', then click it.

32. Search for 'Anthropic' and select 'Anthropic Chat Model.'

33. Under Credential, select your saved Anthropic credential.

34. In Messages, click 'Add Message.' Set Role to 'System.' Paste this as the Message:

> You are a customer service classifier. Given an incoming message, classify it into exactly one of these categories: - SALES: They want to buy something or learn about pricing - SUPPORT: They have a technical problem or need help - PARTNERSHIP: They want to collaborate with us - SPAM: Irrelevant or automated message Respond with ONLY the category name in capitals. No other text.

35. Click 'Add Message' again. Set Role to 'User.' In the Message field, click the expression icon (the gear or {} icon) and type: {{ $json.body.message }}

> *ℹ️ The {{ }} syntax tells n8n to use the value from the incoming webhook data. $json.body.message means: look at the incoming JSON body, find the field called 'message.'*

36. Set Max Tokens to 50. Click Save.

Add a conditional branch:

37. Add an 'IF' node connected to the Anthropic node.

38. Set the condition: Value 1 = {{ $json.text }}, Operator = 'Contains', Value 2 = SALES

39. This creates two paths: 'true' (SALES) and 'false' (everything else).

Add response nodes. For testing, add a 'Set' node on each path to record the result:

40. On the 'true' path, add a Set node. Add a field: name = 'result', value = 'Routed to sales team'

41. On the 'false' path, add a Set node. Add a field: name = 'result', value = 'Routed to general inbox'

Test the workflow:

42. Click 'Test Workflow' (the lightning bolt icon) to start listening.

43. Open a new terminal window and run:

> curl -X POST http://localhost:5678/webhook/YOUR-WEBHOOK-ID \\ -H 'Content-Type: application/json' \\ -d '{"message": "Hi, I am interested in your enterprise pricing. Can we schedule a call?"}'

Replace YOUR-WEBHOOK-ID with the actual ID from your webhook URL (the part after /webhook/).

> *ℹ️ Watch the n8n canvas. Each node should show a green checkmark. Click the Anthropic node to see Claude's classification result.*


> **▶ ACTION: 3E — Build the Daily Intelligence Briefing**


44. Create a new workflow. Name it 'Daily Intelligence Briefing.'

45. Add a Schedule trigger node. Set it to run 'Every Day' at your preferred time (e.g., 7:00 AM).

46. Add an 'RSS Feed Read' node. Set URL to: https://feeds.feedburner.com/TechCrunch (or any news RSS feed). Set Limit to 10.

47. Add a Code node (Language: JavaScript). Paste this code:

> const items = $input.all(); const headlines = items.map((item, i) => { return \`${i+1}. ${item.json.title}: ${item.json.contentSnippet \|\| item.json.summary \|\| ''}\`; }).join('\n'); return [{ json: { headlines } }];

48. Add an Anthropic node. System prompt: 'You are a senior intelligence analyst. Write concise, direct briefings for busy professionals.'

User message:

> Today's headlines: {{ $json.headlines }} Write a 3-paragraph briefing: - Paragraph 1: The most important story and why it matters - Paragraph 2: Two or three other significant developments - Paragraph 3: One thing to watch in the next 24-48 hours

49. Add a Gmail or Send Email node to deliver the briefing to yourself.

50. Set Subject: Daily Briefing - {{ $now.toFormat('MMMM d, yyyy') }}

51. Set Body: {{ $json.text }}

52. Toggle the workflow 'Active' using the switch in the top right.

### Done When

✅ n8n is running at http://localhost:5678 and your API credentials are saved.

✅ Your Lead Triage workflow correctly classifies a test message sent via curl.

✅ Your Daily Intelligence Briefing workflow is active and scheduled.

| **STEP 4: Build Your First RAG System** | **Week 3-4** |
|-----------------------------------------|--------------|

In this step you will build a Document Q&A system: index your own PDFs into a local vector database and ask questions that get answers grounded in the actual document text. This is the same pattern used in enterprise AI knowledge bases.


> **▶ ACTION: 4A — Install Required Libraries**


Make sure your virtual environment is active (you should see '(venv)' in your terminal prompt). Then run:

> pip install llama-index llama-index-vector-stores-chroma llama-index-embeddings-openai chromadb pypdf
>
> *ℹ️ This installs LlamaIndex (the RAG framework), ChromaDB (a local vector database that stores embeddings on your hard drive), and pypdf (for reading PDF files).*


> **▶ ACTION: 4B — Prepare Your Documents**


> mkdir documents

Place 3-5 PDF documents in this folder. Use documents with meaningful text content: work reports, technical manuals, policies, research papers, or any PDFs relevant to your domain. Text-based PDFs work perfectly; scanned images without OCR will produce poor results.


> **▶ ACTION: 4C — Build the RAG System**


Create rag_system.py and paste this entire script:

> import os from dotenv import load_dotenv from llama_index.core import VectorStoreIndex, SimpleDirectoryReader, StorageContext from llama_index.vector_stores.chroma import ChromaVectorStore from llama_index.embeddings.openai import OpenAIEmbedding from llama_index.core import Settings import chromadb load_dotenv() Settings.embed_model = OpenAIEmbedding(model='text-embedding-3-small') chroma_client = chromadb.PersistentClient(path='./chroma_db') chroma_collection = chroma_client.get_or_create_collection('my_documents') vector_store = ChromaVectorStore(chroma_collection=chroma_collection) storage_context = StorageContext.from_defaults(vector_store=vector_store) def build_index(): print('Loading documents...') documents = SimpleDirectoryReader('./documents').load_data() print(f'Loaded {len(documents)} pages.') print('Building vector index (this takes 1-5 minutes the first time)...') index = VectorStoreIndex.from_documents(documents, storage_context=storage_context) print('Index built successfully!') return index def load_index(): return VectorStoreIndex.from_vector_store(vector_store, storage_context=storage_context) def query(question, index): engine = index.as_query_engine(similarity_top_k=3) response = engine.query(question) print(f'\nQuestion: {question}') print(f'\nAnswer: {response}') print('\nSource passages used:') for i, node in enumerate(response.source_nodes): print(f' [{i+1}] Score: {node.score:.3f} \| {node.text[:150]}...') return response if \_\_name\_\_ == '\_\_main\_\_': if not os.path.exists('./chroma_db') or not os.listdir('./chroma_db'): index = build_index() else: print('Loading existing index...') index = load_index() print('\nRAG System ready. Ask questions about your documents (type quit to exit):') while True: q = input('\nYour question: ').strip() if q.lower() in ['quit','exit','q']: break if q: query(q, index)
>
> python rag_system.py
>
> *ℹ️ First run: loads PDFs, splits into chunks, sends each chunk to the OpenAI embeddings API, and stores vectors in ChromaDB. Takes 1-5 minutes. Subsequent runs load from disk instantly.*

Ask a question about your documents. You should see: the answer, the three source passages that produced it, and a relevance score for each passage.


> **▶ ACTION: 4D — Chunk Size Experiment**


Chunk size significantly impacts quality. Run this controlled experiment. Delete your existing index, then test with three chunk sizes:

Mac/Linux:

> rm -rf chroma_db

Windows:

> rmdir /s /q chroma_db

Add this to your build_index function in rag_system.py before building the index:

> from llama_index.core.node_parser import SentenceSplitter # Replace the VectorStoreIndex.from_documents line with: splitter = SentenceSplitter(chunk_size=256, chunk_overlap=50) # Change this number nodes = splitter.get_nodes_from_documents(documents) print(f'Created {len(nodes)} chunks at size 256') index = VectorStoreIndex(nodes, storage_context=storage_context)

Test with chunk_size=256, then 512, then 1024. Ask the same 3 questions each time. Note which size produces the best answers for your documents.

> *ℹ️ Smaller chunks (256) give more precise retrieval but less context per chunk. Larger chunks (1024) give more context but may retrieve irrelevant sections. 512 is a reliable default.*

### Done When

✅ Your RAG system loads documents, builds a persistent index, and answers questions with source citations.

✅ You have run the chunk size experiment and noted how different sizes affect answer quality.

✅ A chroma_db folder exists in your project — the persisted vector index.

| **STEP 5: Build Your First AI Agent** | **Week 4-5** |
|---------------------------------------|--------------|

In this step you will build a Research Agent that autonomously plans and executes multi-step tasks using LangGraph, then build a two-agent team using CrewAI where a Researcher and Writer collaborate. These are the same frameworks used in production AI agent systems.


> **▶ ACTION: 5A — Install Agent Frameworks**


> pip install langchain langgraph langchain-anthropic langchain-openai duckduckgo-search crewai crewai-tools
>
> *ℹ️ This installs LangChain and LangGraph for single-agent development, the Anthropic and OpenAI integrations, a DuckDuckGo web search tool, and CrewAI for multi-agent systems.*


> **▶ ACTION: 5B — Build a Single-Agent Research Assistant**


Create research_agent.py. This agent receives a topic, decides to search the web, reads the results, synthesizes findings, and saves a structured report — all without being told which steps to take.

> import os from dotenv import load_dotenv from langchain_anthropic import ChatAnthropic from langchain_core.messages import HumanMessage, SystemMessage from langchain_core.tools import tool from langgraph.prebuilt import create_react_agent from duckduckgo_search import DDGS load_dotenv() model = ChatAnthropic(model='claude-sonnet-4-5', max_tokens=4096) @tool def web_search(query: str) -> str: """Search the web for current information on a topic. Use this when you need facts, recent news, or data you don't already have.""" try: with DDGS() as ddgs: results = list(ddgs.text(query, max_results=5)) return '\n---\n'.join([f"Title: {r['title']}\nURL: {r['href']}\nSummary: {r['body']}" for r in results]) except Exception as e: return f'Search failed: {str(e)}' @tool def save_report(content: str, filename: str = 'research_report.txt') -> str: """Save the completed research report to a file.""" with open(filename, 'w') as f: f.write(content) return f'Report saved to {filename}' agent = create_react_agent(model, [web_search, save_report]) system_message = """You are a professional research analyst. When given a research topic: 1. Use web_search 2-3 times with different queries to gather comprehensive information 2. Synthesize your findings into a structured report with these sections: - Executive Summary (2-3 sentences) - Key Findings (3-5 bullet points with specific details) - Implications (what this means practically) - Sources (list the URLs you used) 3. Save the report using the save_report tool 4. Tell the user the report is complete and share the single most important finding""" def research(topic): print(f'Researching: {topic}\n' + '='\*50) messages = [ SystemMessage(content=system_message), HumanMessage(content=f'Research this topic thoroughly: {topic}') ] response = agent.invoke({'messages': messages}) for msg in response['messages']: content = str(msg.content) label = type(msg).\_\_name\_\_ print(f'[{label}]: {content[:300]}...' if len(content) > 300 else f'[{label}]: {content}') if \_\_name\_\_ == '\_\_main\_\_': topic = input('Enter research topic: ') research(topic)
>
> python research_agent.py

Enter a topic like 'latest developments in AI voice agents 2025' or 'how small businesses use automation software.'

> *ℹ️ Watch the terminal. You will see the agent's reasoning trace: it calls web_search, reads results, decides what else to search for, then saves the report. A research_report.txt file will appear in your project folder.*


> **▶ ACTION: 5C — Build a Two-Agent Crew with CrewAI**


Create crew_agents.py. A Researcher agent gathers information; a Writer agent turns it into polished content.

> import os from dotenv import load_dotenv from crewai import Agent, Task, Crew, Process from langchain_anthropic import ChatAnthropic load_dotenv() llm = ChatAnthropic(model='claude-sonnet-4-5', temperature=0.3) researcher = Agent( role='Senior Research Analyst', goal='Find comprehensive, accurate, and current information on any assigned topic.', backstory='You are a meticulous researcher who finds relevant information quickly, verifies facts from multiple sources, and separates fact from opinion.', verbose=True, allow_delegation=False, llm=llm ) writer = Agent( role='Content Strategist and Writer', goal='Transform research findings into compelling, clear, well-structured content.', backstory='You are an expert writer who makes complex topics accessible, structures information logically, and always tailors content to the specific audience.', verbose=True, allow_delegation=False, llm=llm ) topic = input('Enter the content topic: ') audience = input('Who is the target audience? ') research_task = Task( description=f"""Research '{topic}' thoroughly. Find: the most important current facts and statistics, key trends or developments in the last 12 months, common misconceptions, and practical implications.""", expected_output='Detailed research notes with facts, statistics, trends, and practical implications. Minimum 400 words.', agent=researcher ) writing_task = Task( description=f"""Using the research notes, write a complete article about '{topic}' for this audience: {audience}. Requirements: engaging headline, hook introduction, 3-4 sections with headings, practical takeaways, conclusion. Target 600-800 words.""", expected_output='A complete polished article with headline, sections, headings, and practical takeaways.', agent=writer, context=[research_task] ) crew = Crew( agents=[researcher, writer], tasks=[research_task, writing_task], process=Process.sequential, verbose=True ) print('\nStarting crew...') result = crew.kickoff() print('\n' + '='\*60 + '\nFINAL ARTICLE:\n' + '='\*60) print(result) with open('crew_article.txt', 'w') as f: f.write(str(result)) print('\nSaved to crew_article.txt')
>
> python crew_agents.py

Enter a topic and audience. Watch both agents work: the researcher gathers information, then the writer transforms it. Both print verbose reasoning traces.

### Done When

✅ research_agent.py executes multiple web searches and saves a structured report file.

✅ crew_agents.py runs two agents sequentially, with the writer using the researcher's output.

✅ You can trace in the terminal output exactly what each agent decided to do and why.

| **STEP 6: Multimodal Workflows** | **Week 5-6** |
|----------------------------------|--------------|

In this step you will build tools that process non-text inputs: a PDF analyzer that reads charts and diagrams, an invoice processor that extracts structured data from scanned documents, and a screenshot-to-code tool. These are among the most immediately practical AI capabilities for business workflows.


> **▶ ACTION: 6A — Install Required Libraries**


> pip install anthropic pypdf pillow pdf2image
>
> *⚠️ pdf2image requires poppler. On Mac, run: brew install poppler (first install Homebrew from https://brew.sh if needed). On Windows: download from https://github.com/oschwartz10612/poppler-windows/releases, unzip it, and add the bin folder to your PATH environment variable.*


> **▶ ACTION: 6B — Build the PDF Visual Analyzer**


This tool converts PDF pages to images and sends them to Claude, enabling analysis of charts, tables, and diagrams — not just the extracted text.

Create pdf_analyzer.py:

> import os, base64, io from pathlib import Path from dotenv import load_dotenv import anthropic from pdf2image import convert_from_path load_dotenv() client = anthropic.Anthropic(api_key=os.environ.get('ANTHROPIC_API_KEY')) def analyze_pdf(pdf_path, question='Provide a comprehensive analysis of this document including all charts and tables.'): images = convert_from_path(pdf_path, dpi=150) print(f'PDF has {len(images)} pages. Sending first 5 to Claude...') content = [{'type':'text','text':f'I am sending you a {len(images)}-page document. Analyze all pages including charts, tables, graphs, and diagrams.'}] for i, img in enumerate(images[:5]): buf = io.BytesIO() img.save(buf, format='JPEG', quality=85) img_b64 = base64.standard_b64encode(buf.getvalue()).decode('utf-8') content.append({'type':'text','text':f'Page {i+1}:'}) content.append({'type':'image','source':{'type':'base64','media_type':'image/jpeg','data':img_b64}}) content.append({'type':'text','text':f'\nAnswer this question about the document:\n{question}'}) response = client.messages.create( model='claude-opus-4-5', max_tokens=4096, system='You are an expert document analyst. Read documents thoroughly, extract key information from text AND visual elements (charts, graphs, tables, diagrams), and provide comprehensive analysis.', messages=[{'role':'user','content':content}] ) return response.content[0].text if \_\_name\_\_ == '\_\_main\_\_': pdfs = list(Path('./documents').glob('\*.pdf')) if pdfs: pdf_path = str(pdfs[0]) print(f'Using: {pdf_path}') else: pdf_path = input('Enter path to PDF: ').strip() question = input('What would you like to know? (Enter for full analysis): ').strip() or 'Provide a comprehensive analysis including all charts and tables.' result = analyze_pdf(pdf_path, question) print('\n' + '='\*60 + '\nANALYSIS RESULT:\n' + '='\*60) print(result)
>
> python pdf_analyzer.py
>
> *ℹ️ Claude can read text, interpret charts, describe diagrams, and extract data from tables — all in a single response. Ask: 'What do the charts show?' or 'Extract all data from the tables in this document.'*


> **▶ ACTION: 6C — Build the Invoice Processor**


This tool extracts structured JSON data from invoice images or PDFs — a real automation that replaces hours of manual data entry. Create invoice_processor.py:

> import os, base64, json, io from pathlib import Path from dotenv import load_dotenv import anthropic from pdf2image import convert_from_path load_dotenv() client = anthropic.Anthropic() def get_image_data(file_path): p = Path(file_path) if p.suffix.lower() == '.pdf': images = convert_from_path(str(p), dpi=200) buf = io.BytesIO() images[0].save(buf, format='JPEG', quality=90) return base64.standard_b64encode(buf.getvalue()).decode('utf-8'), 'image/jpeg' else: with open(file_path,'rb') as f: data = f.read() mt = {'jpg':'image/jpeg','jpeg':'image/jpeg','png':'image/png'}.get(p.suffix.lower()[1:], 'image/jpeg') return base64.standard_b64encode(data).decode('utf-8'), mt def extract_invoice(file_path): img_data, media_type = get_image_data(file_path) prompt = """Extract all invoice data. Respond ONLY with a valid JSON object. No text before or after. { "invoice_number": "string or null", "invoice_date": "YYYY-MM-DD or null", "due_date": "YYYY-MM-DD or null", "vendor": {"name":"string","address":"string or null","email":"string or null"}, "bill_to": {"name":"string","address":"string or null"}, "line_items": [{"description":"string","quantity":0,"unit_price":0,"total":0}], "subtotal": 0, "tax_amount": null, "total_amount": 0, "currency": "USD", "payment_terms": "string or null" }""" response = client.messages.create( model='claude-opus-4-5', max_tokens=2048, messages=[{'role':'user','content':[ {'type':'image','source':{'type':'base64','media_type':media_type,'data':img_data}}, {'type':'text','text':prompt} ]}] ) raw = response.content[0].text.strip() if raw.startswith('\`\`\`'): raw = raw.split('\`\`\`')[1] if raw.startswith('json'): raw = raw[4:] return json.loads(raw) if \_\_name\_\_ == '\_\_main\_\_': path = input('Enter path to invoice (PDF, JPG, PNG): ').strip() print('Processing...') data = extract_invoice(path) print(json.dumps(data, indent=2)) out = Path(path).stem + '\_extracted.json' with open(out,'w') as f: json.dump(data,f,indent=2) print(f'Saved to {out}')
>
> *ℹ️ Download a sample invoice from the web (search 'sample invoice PDF') or use any invoice you have. Run the tool and verify the extracted JSON matches the actual invoice data.*


> **▶ ACTION: 6D — Build the Screenshot-to-Code Tool**


Create screenshot_to_code.py. Take a screenshot of any webpage, pass it to this tool, and get the HTML/CSS to replicate it.

> import os, base64 from pathlib import Path from dotenv import load_dotenv import anthropic load_dotenv() client = anthropic.Anthropic() def screenshot_to_code(image_path): with open(image_path,'rb') as f: data = base64.standard_b64encode(f.read()).decode('utf-8') ext = Path(image_path).suffix.lower() mt = {'png':'image/png','jpg':'image/jpeg','jpeg':'image/jpeg'}.get(ext[1:],'image/png') response = client.messages.create( model='claude-opus-4-5', max_tokens=4096, messages=[{'role':'user','content':[ {'type':'image','source':{'type':'base64','media_type':mt,'data':data}}, {'type':'text','text':'Generate complete working HTML/CSS that replicates this interface. Use a single HTML file with embedded CSS in a <style> tag. Use semantic HTML5 elements. Match the layout, colors, typography, and visual hierarchy as closely as possible. Include all visible text. Use flexbox or CSS grid. Return ONLY the complete HTML file starting with <!DOCTYPE html>. No explanation.'} ]}] ) return response.content[0].text if \_\_name\_\_ == '\_\_main\_\_': path = input('Enter path to screenshot (PNG or JPG): ').strip() html = screenshot_to_code(path) out = Path(path).stem + '\_generated.html' with open(out,'w') as f: f.write(html) print(f'HTML saved to: {out}') print('Open in your browser to see the result.')

Take a screenshot of any webpage: Command+Shift+4 on Mac, or Windows+Shift+S on Windows. Run the tool and open the resulting HTML file in your browser.

### Done When

✅ pdf_analyzer.py describes visual content (charts, tables) in a document, not just text.

✅ invoice_processor.py returns valid JSON with correctly extracted fields from a test invoice.

✅ screenshot_to_code.py generates an HTML file that visually resembles your screenshot.

| **STEP 7: Custom Assistants & Fine-Tuning Basics** | **Week 6-7** |
|----------------------------------------------------|--------------|

In this step you will build a Custom GPT with OpenAI's GPT Builder, run open-source models locally using Hugging Face, and use a decision framework to understand when fine-tuning is actually warranted versus when prompt engineering is sufficient.


> **▶ ACTION: 7A — Build a Custom GPT in GPT Builder**


> *ℹ️ You need a ChatGPT Plus or Team account ($20/month) to create Custom GPTs. The free tier can access existing ones but not create them.*

53. Go to https://chat.openai.com and click 'Explore GPTs' in the left sidebar.

54. Click 'Create' in the top right corner.

55. You will see the GPT Builder: a chat panel on the left for configuration and a preview panel on the right for testing.

In the chat panel, describe what you want to build. Be specific. Example:

> Create an expert assistant for municipal GIS professionals. It should help with ArcGIS Enterprise configuration, Utility Network questions, Python scripting for GIS tasks, and spatial database optimization. It should assume intermediate GIS knowledge, use technical terminology accurately, ask clarifying questions before giving architecture recommendations, and always consider enterprise security and scalability constraints.

56. Review the generated name, description, and instructions. Click 'Configure' at the top to switch to manual configuration mode.

57. Read the auto-generated 'Instructions' field carefully. Edit it to add anything the builder missed.

58. Under 'Knowledge,' click 'Upload files.' Upload 3-5 PDFs relevant to your domain.

59. Under 'Capabilities,' enable 'Web Search' so your GPT can access current information.

Test your GPT in the right preview panel. Test these three things:

60. Ask a question that requires information from your uploaded documents.

61. Ask a question outside your GPT's defined scope to see how it responds.

62. Ask a follow-up question to test whether it maintains context correctly.

Iterate on the instructions in the Configure panel until the behavior matches what you designed.

63. Click Save, then Publish.


> **▶ ACTION: 7B — Run Open-Source Models Locally with Hugging Face**


64. Go to https://huggingface.co and create a free account.

65. Click 'Models' in the top navigation. Filter by Task = 'Text Classification.'

66. Click any popular model. Review the model card, performance metrics, and example code.

67. Click the 'Inference API' widget on the right side. Type a test sentence and click 'Compute.' This runs the model instantly in the browser with no code.

Now run a model locally:

> pip install transformers torch

Create hf_test.py:

> from transformers import pipeline # Downloads model (~265MB) on first run classifier = pipeline('text-classification', model='distilbert-base-uncased-finetuned-sst-2-english') texts = [ 'This product is absolutely fantastic and I love it!', 'Terrible experience. Would not recommend to anyone.', 'The service was okay, nothing special.' ] for text, result in zip(texts, classifier(texts)): print(f'Text: {text}') print(f'Sentiment: {result["label"]} (confidence: {result["score"]:.3f})') print()
>
> python hf_test.py
>
> *ℹ️ First run downloads the model. Subsequent runs load from cache instantly. You are now running an AI model entirely on your local machine — no API, no cloud, no costs.*


> **▶ ACTION: 7C — Fine-Tuning vs. Prompting Decision Test**


Before spending time and money on fine-tuning, run this test to see if few-shot prompting already achieves the quality you need. Create decide_finetuning.py and update the TASK_DESCRIPTION, EXAMPLES, and TEST_CASES for your actual use case:

> import os from dotenv import load_dotenv import anthropic load_dotenv() client = anthropic.Anthropic() TASK_DESCRIPTION = 'Classify support tickets: data_issue, configuration, performance, user_training, or bug' EXAMPLES = [ ('Layer not rendering after projection change','configuration'), ('Query taking 45 seconds on 2M record feature class','performance'), ('How do I add a field to a feature class?','user_training'), ('Topology errors in areas I did not edit','bug'), ] TEST_CASES = [ 'Portal items are not syncing to local replica', 'Can you explain what a geodatabase domain is?', 'Python script fails with ConnectionError on portal login', ] zero_shot_base = f'{TASK_DESCRIPTION}\n\nClassify this ticket (respond with category name only): ' few_shot_base = f'{TASK_DESCRIPTION}\n\nExamples:\n' for text, label in EXAMPLES: few_shot_base += f'Text: {text}\nCategory: {label}\n\n' few_shot_base += 'Now classify this ticket (respond with category name only): ' print('Zero-shot vs few-shot comparison:\n' + '='\*60) for test in TEST_CASES: zs = client.messages.create(model='claude-sonnet-4-5',max_tokens=20,messages=[{'role':'user','content':zero_shot_base+test}]) fs = client.messages.create(model='claude-sonnet-4-5',max_tokens=20,messages=[{'role':'user','content':few_shot_base+test}]) print(f'Ticket: {test}') print(f' Zero-shot: {zs.content[0].text.strip()}') print(f' Few-shot: {fs.content[0].text.strip()}\n') print('RESULT: If few-shot is accurate and consistent -> use prompt engineering, NOT fine-tuning.') print('Fine-tune only if: you have 1000+ labeled examples AND few-shot is still inconsistent.')
>
> python decide_finetuning.py
>
> *ℹ️ For most classification and extraction tasks, few-shot prompting achieves 85-95% accuracy without fine-tuning. Fine-tuning is warranted when: (1) you have thousands of labeled examples, (2) consistency matters at high volume scale, or (3) the task requires domain-specific reasoning patterns that cannot be demonstrated in a few examples.*

### Done When

✅ Your Custom GPT is published and answers domain-specific questions using your uploaded knowledge files.

✅ hf_test.py runs a model locally and classifies text without any API calls or internet connection.

✅ You have run decide_finetuning.py and can explain when you would use fine-tuning vs. prompting for your specific use case.

| **STEP 8: Voice AI & Conversational Agents** | **Week 7-8** |
|----------------------------------------------|--------------|

In this step you will build a text-to-podcast pipeline that converts articles into listenable audio, explore HeyGen's AI avatar creation tools, and understand the voice AI technology stack for building conversational phone agents.


> **▶ ACTION: 8A — Set Up ElevenLabs and Clone a Voice**


68. Go to https://elevenlabs.io and click 'Sign Up.' Create an account with your email. The free tier gives 10,000 characters of TTS per month — sufficient for all exercises here.

69. After logging in, click 'Voices' in the left sidebar.

70. Click 'Add Voice' in the top right, then select 'Instant Voice Clone.'

71. Upload a clear audio recording of yourself speaking for at least 1 minute. A voice memo recorded on your phone works well. Minimal background noise is important.

72. Name your voice (e.g., 'My Voice Clone') and click 'Add Voice.' Processing takes about 30 seconds.

**Get Your API Key**

73. Click your profile icon in the bottom left, then 'Profile + API key.'

74. Copy the API key. Add it to your .env file:

> ELEVENLABS_API_KEY=your_key_here


> **▶ ACTION: 8B — Build the Text-to-Podcast Pipeline**


This pipeline rewrites any article in a conversational spoken style, then generates listenable audio in your cloned voice.

> pip install elevenlabs

Create text_to_podcast.py:

> import os from dotenv import load_dotenv import anthropic from elevenlabs.client import ElevenLabs from elevenlabs import save load_dotenv() claude = anthropic.Anthropic() eleven = ElevenLabs(api_key=os.environ.get('ELEVENLABS_API_KEY')) def to_podcast_script(article): resp = claude.messages.create( model='claude-sonnet-4-5', max_tokens=2048, system='You convert articles to engaging podcast monologue scripts. Write for the ear, not the page.', messages=[{'role':'user','content':f"""Convert this article to a 3-5 minute podcast script. Guidelines: - Write as if speaking directly to one listener - Replace lists and bullets with natural spoken language - Add transitions: 'Now, what's interesting here is...' / 'Think about it this way...' - Brief hook intro (15-20 seconds), punchy outro with one takeaway - Target 450-750 words - Write ONLY the words to be spoken. No directions. Article: {article}"""}] ) return resp.content[0].text def generate_audio(script, voice_name='My Voice Clone', output='podcast.mp3'): voices = eleven.voices.get_all().voices voice_id = next((v.voice_id for v in voices if v.name == voice_name), voices[0].voice_id) print(f'Using voice: {next((v.name for v in voices if v.voice_id == voice_id), "default")}') audio = eleven.generate(text=script, voice=voice_id, model='eleven_multilingual_v2') save(audio, output) print(f'Audio saved: {output}') return output if \_\_name\_\_ == '\_\_main\_\_': print('Options: 1 = paste text, 2 = read from file') if input('Choice: ').strip() == '2': with open(input('File path: ').strip()) as f: article = f.read() else: print('Paste article text, press Enter twice when done:') lines = [] while True: l = input() if l == '' and lines and lines[-1] == '': break lines.append(l) article = '\n'.join(lines[:-1]) print('\nConverting to podcast script...') script = to_podcast_script(article) print(f'Script: {len(script.split())} words. Preview: {script[:200]}...') voice = input('\nVoice name (Enter for "My Voice Clone"): ').strip() or 'My Voice Clone' print('Generating audio...') generate_audio(script, voice_name=voice)
>
> python text_to_podcast.py

Paste any article text when prompted. When complete, open podcast.mp3 in any audio player.

> *ℹ️ The two-step process — rewriting first, then generating audio — produces significantly better results than sending raw article text directly to TTS. The script adaptation step removes visual formatting, adjusts sentence rhythm, and adds the natural connective phrases that make spoken content sound engaging rather than like someone reading aloud.*


> **▶ ACTION: 8C — Explore HeyGen AI Avatar Video**


75. Go to https://www.heygen.com and create an account. The free tier gives 1 minute of avatar video per month.

76. Click 'Create Video' or 'Instant Avatar.'

77. Click 'Create Instant Avatar' and upload a 2-minute video of yourself looking directly at the camera and speaking clearly. Any content works — just introduce yourself or explain something.

78. HeyGen processes your avatar in 5-15 minutes. You will receive an email notification.

79. Once ready, go to 'Video Studio,' click 'Create New Video,' and select your avatar.

80. In the script box, type: 'Hello! I have been learning about AI automation tools, and I want to share the three most important lessons from building my first real automation pipeline.'

81. Click 'Generate Video.' A 20-30 second video will be produced in 1-3 minutes.

82. Download and watch the result. Note: avatar quality improves significantly with a well-lit, stable, forward-facing reference video.

### Done When

✅ text_to_podcast.py produces a listenable MP3 from any article input.

✅ You have a cloned ElevenLabs voice and have verified it works via the API.

✅ You have generated a HeyGen avatar video with a custom script.

| **STEP 9: AI Tool Stack Architecture** | **Week 8-9** |
|----------------------------------------|--------------|

In this step you will design your personal AI tool stack, set up Notion as your operational hub, and build a Content Production Pipeline that runs from brief to published draft without manual intervention.


> **▶ ACTION: 9A — Map Your Current Tools**


Create a new document and audit every tool you use regularly. For each tool, answer: What is its primary job? What data lives in it permanently? What events happen in it that could trigger automation? What does it output that other tools could use?

Draw a simple diagram with boxes for each tool and arrows showing current data flows. This is your baseline stack. Common elements: email, calendar, task manager, notes, cloud storage, CRM.

Then draw your target stack — what you want it to look like after connecting AI. Mark which connections you will build this week.


> **▶ ACTION: 9B — Set Up Notion as Your Hub**


83. Go to https://www.notion.so and create a free account.

84. Click '+ New page' in the left sidebar. Create a page called 'AI Operations Hub.'

85. Inside it, create three sub-pages: 'Automation Log,' 'Content Production,' and 'Knowledge Base.'

**Get Your Notion API Token**

86. Go to https://www.notion.so/my-integrations

87. Click '+ New integration.' Name it 'AI Mastery.' Select your workspace. Click 'Submit.'

88. Copy the 'Internal Integration Token.' Add it to your .env file:

> NOTION_TOKEN=secret_your_token_here

**Share Your Pages with the Integration**

89. In Notion, open each sub-page you created.

90. Click the three-dot '...' menu in the top right.

91. Click 'Connect to' at the bottom and select your 'AI Mastery' integration.

Repeat for each sub-page. The integration can only access pages explicitly shared with it.

**Get Your Database IDs**

The database ID is in the URL when you have a database open:

> https://www.notion.so/your-workspace/My-Database-abc123def456...

The long string after the last dash (before any '?') is the ID. Copy and save each one.


> **▶ ACTION: 9C — Create the Content Briefs Database in Notion**


92. On your 'Content Production' sub-page, type /database and select 'Table - Full Page.'

93. Name it 'Content Briefs.'

94. Add these columns by clicking the '+' button next to existing columns:

- Title (already exists) — the article topic

- Status — type: Select — add options: Draft, In Progress, Complete

- Target Audience — type: Text

- Key Points — type: Text (what the article must cover)

- Generated Draft — type: Text (where AI output will appear)

> *ℹ️ Copy the database ID from the URL. You will need it for the n8n workflow.*


> **▶ ACTION: 9D — Build the Content Production Pipeline in n8n**


95. In n8n, create a new workflow called 'Content Production Pipeline.'

Node 1 — Schedule Trigger: set to run every 15 minutes (polling for new briefs).

Node 2 — Notion (Get Database Items): Operation = Get Many, Resource = Database Item. Set Database ID to your Content Briefs database ID. Add a filter: Status property equals 'Draft.'

Node 3 — IF (check if any briefs exist): Condition = {{ $json.results.length }} Greater Than 0. This stops the workflow early if there's nothing to process.

Node 4 — Loop Over Items: processes each Draft brief one at a time.

Node 5 — Notion (Update Status): Operation = Update, Resource = Database Item. Page ID = {{ $json.id }}. Update Status to 'In Progress.'

Node 6 — Anthropic (Generate Content): System prompt:

> You are an expert content writer producing clear, engaging, authoritative content tailored to the specific audience. Always write for impact and practical value.

User message:

> Write a comprehensive article. Topic: {{ $('Loop Over Items').item.json.properties.Title.title[0].text.content }} Target Audience: {{ $('Loop Over Items').item.json.properties['Target Audience'].rich_text[0].text.content }} Key Points: {{ $('Loop Over Items').item.json.properties['Key Points'].rich_text[0].text.content }} Requirements: 600-900 words. Engaging headline as first line. 3-4 sections with subheadings. Practical, actionable content.

96. Set Max Tokens to 2000.

Node 7 — Notion (Save Draft): Update the Generated Draft property with {{ $json.text }}. Update Status to 'Complete.'

Node 8 — Email: Subject = New Draft Ready: [Topic]. Body = Your AI draft is ready in the Content Briefs database. Activate the workflow.

Test: Add a new row in Notion's Content Briefs database with Status = Draft, a topic, target audience, and key points. Wait 15 minutes (or manually trigger the workflow in n8n). The Generated Draft field should fill in automatically.

### Done When

✅ Your Content Production Pipeline runs end-to-end: Draft brief in Notion becomes Complete draft with AI content without any manual steps.

✅ You have a documented baseline and target tool stack diagram.

✅ At least one complete content brief exists in Notion generated by the pipeline.

| **STEP 10: AI Video Content Production** | **Week 9-10** |
|------------------------------------------|---------------|

In this step you will produce polished video content with AI tools: a professional explainer video using VEED, short-form clips from long-form content using Opus Clip, a generative video experiment using Runway, and an automated video pipeline that creates audio content from any topic.


> **▶ ACTION: 10A — Create a Professional Video with VEED**


97. Go to https://www.veed.io and create a free account.

98. Click 'New Project' then 'Upload a Video.' Upload any video you have — a 3-5 minute screen recording or phone recording. If you don't have one, record your screen for 3-5 minutes explaining anything. Mac: Command+Shift+5. Windows: Windows+Shift+R.

Apply AI enhancements one at a time:

**Auto Subtitles**

99. Click 'Subtitles' in the left toolbar.

100. Click 'Auto Subtitles.' Select your language. Click 'Create Subtitles.'

101. Wait 1-3 minutes for transcription. Review for errors by clicking individual words to correct them.

**Audio Cleanup**

102. Click 'Audio' in the left toolbar.

103. Click 'Clean Audio.' Toggle on 'Remove background noise' and 'Enhance voice.' Click 'Enhance Audio.'

**Export in Multiple Formats**

104. Click 'Export' in the top right. Export at 16:9 aspect ratio (standard YouTube/LinkedIn format).

105. Click 'Resize Video' and select '9:16 (TikTok, Reels).' Export again.

> *ℹ️ You now have the same content in two formats without re-recording. This is the core value of AI video editing.*


> **▶ ACTION: 10B — Repurpose Long-Form Content with Opus Clip**


106. Go to https://www.opus.pro and create an account (free tier available).

107. Click 'Create Clips.' Paste a YouTube URL of any long-form content over 20 minutes (a conference talk, webinar, or interview).

108. Select your target platform (TikTok, YouTube Shorts, or Instagram Reels). Click 'Get Clips.'

> *ℹ️ Opus analyzes the entire video, identifies the most engaging moments using engagement-trained AI, and generates 5-15 short clips with auto-captions. Takes 5-15 minutes depending on video length.*

109. When complete, review the clips. Each has a Virality Score (Opus's predicted engagement rating).

110. Download 2-3 of the highest-scoring clips.

In your notes, answer: How long would it have taken you to manually clip this video? How does AI selection compare to what you would have chosen?


> **▶ ACTION: 10C — Experiment with Runway Generative Video**


111. Go to https://runwayml.com and create an account. Free tier includes 125 credits (~25 short clips).

112. Click 'Create New' and select 'Gen-3 Alpha.'

**Text to Video**

113. Click 'Text to Video.' Enter a detailed prompt describing subject, action, camera movement, lighting, and style:

> A professional presenter standing at a whiteboard in a modern office, pointing to a diagram, warm natural lighting from large windows, slow dolly-in camera movement, documentary style, photorealistic

114. Set Duration to 5 seconds. Click Generate (~5 credits). Wait 30-60 seconds.

**Image to Video**

115. Take any high-quality photo (a landscape, product, or interior).

116. Click 'Image to Video.' Upload your image.

117. Describe the motion: 'slow zoom in, light shimmer, soft ambient movement.'

> *ℹ️ Image-to-video typically produces more controlled results because the visual content is constrained by your reference image. Compare the two approaches.*


> **▶ ACTION: 10D — Build the Automated Video Pipeline**


Create automated_video.py. This script generates a video script and voiceover from any topic input:

> import os from dotenv import load_dotenv import anthropic from elevenlabs.client import ElevenLabs from elevenlabs import save load_dotenv() claude = anthropic.Anthropic() eleven = ElevenLabs(api_key=os.environ.get('ELEVENLABS_API_KEY')) def write_script(topic, seconds=60): words = int(seconds \* 2.5) resp = claude.messages.create( model='claude-sonnet-4-5', max_tokens=1024, system='You write concise, engaging video scripts in first person conversational tone. No stage directions.', messages=[{'role':'user','content':f'Write a {words}-word video script explaining: {topic}. Include a hook opening (5 seconds), 2-3 key points, and a call to action at the end.'}] ) return resp.content[0].text def generate_audio(script, output='video_voiceover.mp3'): voices = eleven.voices.get_all().voices audio = eleven.generate(text=script, voice=voices[0].voice_id, model='eleven_multilingual_v2') save(audio, output) print(f'Voiceover saved: {output}') return output if \_\_name\_\_ == '\_\_main\_\_': topic = input('Enter video topic: ') print('Writing script...') script = write_script(topic) print(f'Script ({len(script.split())} words):\n{script[:300]}...') print('\nGenerating voiceover...') audio_file = generate_audio(script) print(f'\nPipeline complete!') print('Next: Upload voiceover to HeyGen to create avatar video, or to VEED to add captions.')
>
> python automated_video.py

Enter any topic. The pipeline produces a video script and voiceover MP3. Upload the audio to HeyGen for the avatar video layer, or to VEED to add captions and branding.

### Done When

✅ You have a polished video exported from VEED in both 16:9 and 9:16 formats.

✅ You have used Opus Clip to generate short-form clips and compared AI selection to your own judgment.

✅ automated_video.py produces a script and voiceover MP3 from any topic input.

| **STEP 11: AI-Powered Product Development** | **Week 10-11** |
|---------------------------------------------|----------------|

In this step you will build and deploy a real public AI web application using Lovable, then install and use Cursor to understand how professional developers use AI to accelerate coding. By the end you will have a live deployed application.


> **▶ ACTION: 11A — Install and Explore Cursor**


118. Go to https://www.cursor.com and click 'Download.'

119. Install Cursor for your operating system. It is built on VS Code and looks identical.

120. Open Cursor. It may ask to import VS Code settings — import if you have VS Code, or start fresh.

121. Open your ai-mastery project folder: File → Open Folder → select ai-mastery.

122. Press Command+L (Mac) or Ctrl+L (Windows) to open the Cursor AI chat panel on the right.

123. Click on any Python file you created. In the chat panel, type:

> Explain what this code does, line by line. Then suggest two specific improvements that would make it more robust or useful.
>
> *ℹ️ Cursor reads your actual file and provides contextually accurate suggestions — unlike general ChatGPT, it understands your specific codebase.*

Use Cursor to improve your code:

124. In the chat panel: 'Add proper error handling to this function. It should catch API errors, log the error message, and return a structured error response instead of crashing.'

125. Click 'Apply' to automatically insert the suggested changes into your file.

126. Ask Cursor to write a test: 'Write a pytest test for this function that tests at least three different input scenarios.'

127. Apply the test code. Run: python -m pytest (install with: pip install pytest)


> **▶ ACTION: 11B — Build a Deployed AI App with Lovable**


**What You Are Building**

A Topic Intelligence Tool: users enter a topic, the app returns a structured AI-generated brief with an executive summary, key questions to investigate, and essential facts. Deployed to a public URL anyone can access.

128. Go to https://lovable.dev and create an account (free tier available).

129. Click 'New Project.' In the prompt box, describe your app in detail:

> Build a Topic Intelligence Tool web application. 1. Clean landing page. Headline: 'Topic Intelligence Tool'. Subtitle: 'Get structured AI insights on any topic' 2. Centered text input with placeholder 'Enter any topic...' and a 'Generate Brief' button 3. Loading state while processing: spinner + 'Analyzing topic...' 4. Results panel with three sections: - Executive Summary: 2-3 sentence overview - Key Questions to Ask: 3 bullet points - What You Should Know: 4-5 bullet points 5. 'Copy Report' button that copies results to clipboard 6. Design: navy blue #1E3A5F primary color, white background, Arial font, mobile responsive 7. Call the Anthropic API using Claude claude-sonnet-4-5. Show results only after button click.
>
> *ℹ️ Lovable generates a complete React application. Watch the code being written in real time on the left and the live preview on the right. Takes 1-3 minutes.*

130. Review the preview. If anything doesn't match, type corrections in the chat:

> The results panel should be hidden initially and only appear after the user clicks Generate Brief. Make the button larger and more prominent.

**Add the Real Anthropic API**

In the Lovable chat, type:

> Connect this to the real Anthropic API. Add ANTHROPIC_API_KEY as an environment variable. System prompt: 'You are an expert intelligence analyst. Given a topic, provide a structured brief. Respond ONLY with JSON using this format: {"executive_summary": "string", "key_questions": ["string","string","string"], "essential_facts": ["string","string","string","string"]}'. User message: 'Generate an intelligence brief for: [topic]'. Parse the JSON and display each section in its respective panel.

**Deploy Your App**

131. Click 'Deploy' in the top right. Lovable deploys to a public URL automatically (format: yourproject.lovable.app).

132. In the Lovable settings/environment panel, add your ANTHROPIC_API_KEY.

133. Visit your public URL. You have a live, deployed AI application.

134. Test with several topics. Share the URL with someone else.


> **▶ ACTION: 11C — Add RAG Integration**


Connect your deployed app to the RAG system from Step 4 so answers can be grounded in your actual documents.

**Create the Backend RAG API**

> pip install flask flask-cors

Create rag_api.py:

> import os from flask import Flask, request, jsonify from flask_cors import CORS from dotenv import load_dotenv from llama_index.core import VectorStoreIndex, StorageContext from llama_index.vector_stores.chroma import ChromaVectorStore from llama_index.embeddings.openai import OpenAIEmbedding from llama_index.core import Settings import chromadb load_dotenv() app = Flask(\_\_name\_\_) CORS(app) Settings.embed_model = OpenAIEmbedding(model='text-embedding-3-small') chroma_client = chromadb.PersistentClient(path='./chroma_db') vector_store = ChromaVectorStore(chroma_collection=chroma_client.get_or_create_collection('my_documents')) storage_context = StorageContext.from_defaults(vector_store=vector_store) index = VectorStoreIndex.from_vector_store(vector_store, storage_context=storage_context) query_engine = index.as_query_engine(similarity_top_k=3) @app.route('/api/rag-query', methods=['POST']) def rag_query(): query = request.json.get('query', '') if not query: return jsonify({'error': 'Query required'}), 400 response = query_engine.query(query) sources = [n.text[:100]+'...' for n in response.source_nodes] return jsonify({'answer': str(response), 'sources': sources}) if \_\_name\_\_ == '\_\_main\_\_': app.run(debug=True, port=8000)
>
> python rag_api.py
>
> *ℹ️ Your RAG API runs at http://localhost:8000. Your Lovable frontend can call it when the knowledge base toggle is enabled.*

In the Lovable chat, add the toggle:

> Add a checkbox below the topic input labeled 'Use my knowledge base'. When checked, instead of calling Anthropic directly, call a local API endpoint at http://localhost:8000/api/rag-query with body {query: topic}. Display the returned answer field and list the sources below the results.

### Done When

✅ Your Lovable app is deployed to a public URL and responds to topic queries with structured AI briefs.

✅ The Use Knowledge Base toggle routes queries to your local RAG API.

✅ You have used Cursor to add error handling and at least one test to existing code.

**CAPSTONE PROJECT**


> Build an AI-Powered Business Intelligence System


Integrating All 10 Concepts in One Production-Grade Deployment


## The Challenge

Build and deploy a complete AI Business Intelligence system for a domain you know deeply — your profession, your business, or a market you understand. This is not a tutorial exercise. You are building something real that demonstrates integrated mastery of all 10 concepts.

## Architecture Requirements (One per Concept)

### 1. Prompt Engineering

Create a /prompts folder. Store every LLM prompt as a versioned text file with header comments: purpose, target model, technique used, and date. At least two prompts must use chain-of-thought reasoning.

### 2. Workflow Automation

Build three n8n workflows: Data Ingestion (collects new domain information on a schedule from two+ sources), Analysis Trigger (detects new data and fires the agent), and Output Delivery (distributes agent results). All workflows must include error handling with fallback paths.

### 3. AI Agents

Build a LangGraph agent as the intelligence core. Given a new data input or user question, it searches your RAG knowledge base, gathers additional context if needed, reasons through implications, and produces a structured insight report. Must use 3+ tools with a maximum iteration limit and graceful fallback.

### 4. RAG

Index 10+ domain-specific documents in ChromaDB. Every agent response using retrieved context must include citation: which source documents and passages were used. Implement hybrid search for improved retrieval coverage.

### 5. Multimodal AI

At least one input path must handle non-text content: PDF processing with visual analysis, image analysis relevant to your domain, or a document scanner for mixed-media files. Output must demonstrate visual content was understood and incorporated.

### 6. Custom Assistant

Create a Custom GPT or configured assistant for your domain with uploaded knowledge files and at least one connected action (webhook to n8n or direct API call). Document why assistant configuration rather than fine-tuning was the right choice for your specific use case.

### 7. Voice Interface

Add a voice output layer: weekly intelligence summaries are automatically converted to audio using ElevenLabs and delivered as MP3 email attachments alongside the text report. Optionally, set up a voice agent that lets you query the system by phone call using Vapi.

### 8. Tool Stack

Produce a current, accurate data flow diagram showing every component and every data flow. Notion or ClickUp serves as the operational hub — all AI outputs are logged with metadata: timestamp, source documents, model used, and confidence indicators.

### 9. Video Output

The system produces at least one AI-generated video per week automatically: agent generates weekly summary → Claude rewrites as video script → ElevenLabs generates voiceover → HeyGen generates avatar video → video URL is logged to Notion. Runs via n8n on a weekly schedule.

### 10. Deployed Product

Build the user interface in Lovable or a Cursor-built stack. The UI allows: submitting queries, viewing intelligence reports, browsing the knowledge base with source citations, and triggering on-demand agent runs. Deployed to a public URL. Accessible from any browser without local setup.

## Required Deliverables

- Public URL to your deployed application

- GitHub repository with all code, prompts, and documentation

- Architecture diagram: data flow, component inventory, technology stack

- Prompt library: all versioned prompt files with headers

- 10 sample outputs demonstrating the system across different query types

- 5-minute screen recording walking through each of the 10 concept integrations

- Written retrospective (1,000+ words): what you built, what worked, what broke, what surprised you, and what you would do differently

## Retrospective Prompts

- Which concept was hardest to integrate and why?

- Where did AI fail or produce poor outputs? What did you learn from debugging those failures?

- What was the biggest architectural decision you made and what trade-offs did it involve?

- If you were building this for a paying client, what would you do differently?

- What capability do you wish you had that was not covered in this guide?

*Completing this capstone means you have built real systems, not just run tutorials.*

The integrated understanding you will have developed — knowing not just how each tool works but how they fail, how they compose, and where the seams are — is the foundation of genuine AI practitioner expertise. Everything from here is iteration, depth, and production experience.

**Build it. Break it. Learn from it.**
