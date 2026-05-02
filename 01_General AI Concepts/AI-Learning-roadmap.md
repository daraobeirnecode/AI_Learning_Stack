# AI Skills Development Roadmap: 2025–2030
### A Prioritized, Timeline-Driven Learning Plan

---

> **How to use this document:** Skills are sequenced by strategic value and prerequisite logic — not alphabetically or by difficulty. Each phase builds on the last. Resist the urge to skip ahead. The timeline assumes roughly 8–12 focused hours per week of dedicated learning time. Adjust proportionally if your available time differs.

---

## Strategic Context: Why This Order

The sequencing follows a specific logic:

1. **Immediate skills (Weeks 1–8)** require the least infrastructure to start and produce feedback loops fastest. They also make every subsequent skill easier to learn.
2. **Foundation skills (Months 2–6)** build the technical bedrock that agent and data work requires. Nothing in the advanced tier lands without these.
3. **Compound skills (Months 6–18)** require real experience to internalize. Reading about evals without having a system to evaluate is largely wasted effort.
4. **Deep specialty skills (Months 18+)** have high activation cost and are best pursued once you have production systems to apply them against.

---

## Phase 0: Orientation (Week 0 — Before You Start)

Before doing anything else, complete this orientation to calibrate your starting position.

### Orientation Tasks

- [ ] **Audit your current state.** For each of the 10 skills below, rate yourself 1–5 honestly. Write it down. This is your baseline.
- [ ] **Set up your learning environment:**
  - Create a private GitHub repo called `ai-skills-lab`. Every project in this roadmap lives here.
  - Set up a learning journal (Obsidian, Notion, or plain markdown). Log what you build, what broke, and what you learned every week. The compounding value of this cannot be overstated.
  - Create accounts on: Hugging Face, Braintrust (free tier), LangSmith (free tier), Weights & Biases (free tier).
- [ ] **Read these two things first** — they are short and will frame everything that follows:
  - Anthropic's "Core Views" page (anthropic.com/news/core-views-on-ai-safety) — understand the organization shaping the current moment
  - LangChain's "State of AI Agents" report (2024) — the best current survey of where agent architectures actually are

---

## Phase 1: Immediate Start (Weeks 1–8)
### Skills: Production AI Operations · Advanced Prompt Engineering · Domain × AI Application

These three skills have the lowest barrier to entry, the fastest feedback loops, and compound value immediately. You can begin all three in parallel during this phase.

---

### Skill 1 of 10: Production AI System Operations (AI Ops)

> **Classification:** Table Stakes — anyone deploying AI seriously needs this  
> **Why first:** You likely already have or will soon have AI systems running. Operating them blindly is burning value. This skill has an immediate, tangible payoff and requires no new infrastructure to begin.

---

#### Week 1–2: Observability Foundations

**Core Concept: You cannot improve what you cannot measure.**

The central idea is that AI systems need the same telemetry infrastructure as any production software system — plus additional layers specific to language model behavior (token costs, latency by model, output quality signals).

**Study:**
- [ ] Read the OpenTelemetry "Getting Started" documentation (opentelemetry.io/docs/getting-started). Understand the three pillars: traces, metrics, logs. You do not need to implement it yet — understand the concepts.
- [ ] Read "Observability Engineering" by Charity Majors et al., Chapters 1–4. This is the foundational text on modern observability thinking. The rest of the book is a bonus.

**Do:**
- [ ] Set up **Langfuse** (open-source, self-hostable) on a project you already have running, OR use their cloud free tier. Configure it to capture: latency, token counts, model used, and prompt/response pairs.
  - Langfuse quickstart: langfuse.com/docs/get-started
  - Specific goal: get 50 real traces captured and visible in the dashboard before moving on.
- [ ] Set up **LangSmith** on a second project (or the same one). Compare what each platform shows you. Note the differences in what they surface.

**Concepts to understand by end of Week 2:**
- What a trace is and how it differs from a log
- The difference between structured and unstructured logs and why it matters for AI systems
- What "token cost per request" means and how to calculate it
- What p50/p95/p99 latency means and why you track all three

---

#### Week 3–4: AI-Specific Monitoring

**Core Concept: LLM failure modes are different from traditional software failure modes.**

Traditional systems fail with errors. LLM systems fail *silently* — they return a 200 OK with confidently wrong content. Your monitoring infrastructure needs to catch this.

**Study:**
- [ ] Read the Evidently AI blog post series on "LLM Monitoring" (evidentlyai.com/blog — search LLM monitoring). Read all six parts. This is the best free treatment of the topic.
- [ ] Read the MLOps Community post "What Should You Monitor in Production ML Systems" — covers data drift concepts you'll apply to LLMs.

**Do:**
- [ ] Instrument a real LLM call with **all** of the following being captured: input tokens, output tokens, model version, latency, a quality score (even if it's just a placeholder integer for now), and a session/user identifier.
- [ ] Build a simple cost dashboard. Use Python + Matplotlib or a Grafana free cloud instance. Track: cost per day, cost per user (if applicable), cost breakdown by model. Make it ugly but functional.
- [ ] Set up your first **alert**: trigger a notification (email or Slack webhook) when the p95 latency for any model exceeds 8 seconds, OR when daily cost exceeds a threshold you set.

**Concepts to understand by end of Week 4:**
- Data drift vs. concept drift and how each affects LLM systems
- The difference between input distribution monitoring and output quality monitoring
- Why latency has a bimodal distribution in LLM systems (prompt caching hits vs. misses)
- What a "quality signal" is and why proxy metrics (length, sentiment, user thumbs up/down) are imperfect but necessary

---

#### Week 5–6: Cost Optimization Strategies

**Core Concept: Inference cost is the primary operational lever you control.**

**Study:**
- [ ] Read Anthropic's prompt caching documentation thoroughly — understand exactly what gets cached, for how long, and what the pricing difference is.
- [ ] Read OpenAI's batching API documentation and compare with Anthropic's batch processing model.
- [ ] Study semantic caching concept: the GPTCache GitHub README is a good introduction.

**Do:**
- [ ] Implement **prompt caching** on at least one production or practice system. Measure the cost reduction. Write down the before/after numbers in your learning journal.
- [ ] Build a model router — a simple function that takes a request classification (simple/medium/complex) and routes to different models. Use a cheap model (GPT-4o-mini or Claude Haiku) for simple requests and an expensive model only when needed. Measure the cost difference on 100 test requests.
- [ ] Implement **semantic caching** with GPTCache or a manual approach (embed the query, check cosine similarity against a cache of recent queries, return cached response if similarity > 0.95). Even if you never use this in production, building it teaches you how it works.

**Concepts to understand by end of Week 6:**
- What prompt caching is, what it requires (static prefix), and what it saves
- The difference between semantic caching and exact-match caching
- Model routing strategies: complexity classification, intent routing, cost-cap routing
- How to calculate ROI on AI infrastructure investments

---

#### Week 7–8: Incident Response and Reliability Patterns

**Core Concept: Production AI systems will fail. The question is how gracefully.**

**Study:**
- [ ] Read the Netflix Tech Blog post on the Circuit Breaker pattern (search "Netflix Hystrix circuit breaker"). Apply the mental model to AI systems.
- [ ] Read "Release It!" by Michael Nygard, Chapter 5 (Stability Patterns). Specifically: circuit breakers, timeouts, bulkheads, and handshaking. These patterns are directly applicable to LLM system design.

**Do:**
- [ ] Implement a **model fallback chain**: Primary model → Secondary model → Cached/static response → Graceful error message with user notification. Test each failure scenario intentionally.
- [ ] Write a **runbook** for your AI system. A runbook is a document that answers: What are the top 5 things that can go wrong? How do you detect each? What do you do? Who do you notify? This exercise forces you to think through failure modes systematically.
- [ ] Conduct a **chaos test**: intentionally throttle your API key to 0 calls/minute and verify your fallback chain works. Then restore it and measure the time to recovery.

**Phase 1, Skill 1 — Competency Milestone:**
> You have a running AI system with full observability (traces, metrics, costs), at least two alerts configured, a working fallback chain, and a runbook. You can explain any system failure from logs alone within 10 minutes.

---

### Skill 2 of 10: Advanced Prompt Engineering as Systems Design

> **Classification:** Differentiator at depth — surface-level prompting is table stakes  
> **Why now:** You are likely already writing prompts. The gap between prompts that work sometimes and system prompts that work reliably at scale is enormous. This skill has an immediate ROI on everything else you build.

---

#### Week 1–2: Foundational Techniques Audit

**Core Concept: Most practitioners use 20% of available prompting techniques and don't know what they're missing.**

**Study:**
- [ ] Read "The Prompt Report: A Systematic Survey of Prompting Techniques" (arxiv.org/abs/2406.06608). This is a 76-page academic survey — read the abstract, introduction, and the taxonomy sections (Sections 2–4). Skim but note the techniques you haven't tried.
- [ ] Read Anthropic's official prompt engineering guide end-to-end (docs.anthropic.com/en/docs/build-with-claude/prompt-engineering). Even if you've read it before, re-read it now with fresh attention.

**Do:**
- [ ] Run a **prompting technique audit**: pick one task you do repeatedly. Write 6 versions of the prompt using 6 different techniques: zero-shot, few-shot (3 examples), chain-of-thought, self-consistency (run 5 times, take majority vote), role prompting, and XML-structured output. Evaluate all 6 on 20 test cases. Write down the results.
- [ ] Build a personal **prompt library**: a markdown file with 20+ reusable prompt patterns, each with a template, example, and notes on when to use it.

**Concepts to understand by end of Week 2:**
- The difference between zero-shot, one-shot, and few-shot prompting and when each is appropriate
- Why chain-of-thought works (the scratchpad hypothesis) and when it helps vs. hurts
- What self-consistency prompting is and its cost/quality tradeoff
- Why example order in few-shot prompts matters and how to select representative examples

---

#### Week 3–4: System Prompt Engineering for Agents

**Core Concept: A system prompt for an agentic context is a software specification, not a conversation starter.**

**Study:**
- [ ] Read Anthropic's "Build effective agents" documentation (docs.anthropic.com/en/docs/build-with-claude/agents). Focus on the system prompt architecture recommendations.
- [ ] Read the PromptBase blog post "Anatomy of a Production System Prompt." If unavailable, substitute: search for "leaked system prompts" on GitHub — studying real production system prompts from tools like Cursor, Devin, and others is highly instructive.
- [ ] Study the Anthropic "Claude's Character" and "Model Specification" public documentation — understanding how Claude processes system prompts at a behavioral level helps you write better ones.

**Do:**
- [ ] Write a complete production-grade system prompt for a domain-specific agent. Requirements: minimum 800 words, covers persona, scope boundaries, tool use instructions, output format specification, edge case handling, and escalation behavior.
- [ ] **Adversarial test** your system prompt: try 20 inputs designed to break the agent's intended behavior. Prompt injection, scope violations, ambiguous inputs, contradictory instructions. Document every failure mode and patch the prompt.
- [ ] Build a **system prompt version control practice**: every significant change to a system prompt gets a new version number, a summary of what changed and why, and a test result comparison. Even if informal, this discipline compounds.

**Concepts to understand by end of Week 4:**
- How XML structuring affects model attention in Anthropic models specifically
- What "prompt injection" is and how system prompt design can mitigate it
- The tradeoff between prompt specificity (fewer failures, less flexibility) and prompt generality (more flexible, more failure modes)
- How context window position affects instruction following (primacy and recency effects)

---

#### Week 5–6: DSPy and Automated Prompt Optimization

**Core Concept: Prompts are learnable parameters, not fixed strings.**

**Study:**
- [ ] Read the DSPy paper: "DSPy: Compiling Declarative Language Model Calls into Self-Improving Pipelines" (arxiv.org/abs/2310.03714). Read the abstract, introduction, and Section 3. The rest is optional at this stage.
- [ ] Complete the DSPy official tutorial at dspy.ai — specifically the "Optimizer" section.

**Do:**
- [ ] Implement a DSPy pipeline for a classification or extraction task you care about. Use the **BootstrapFewShot** optimizer to automatically generate and select few-shot examples. Compare the optimized prompt performance against your hand-written prompt on a held-out test set.
- [ ] Build a prompt A/B testing framework: two prompt variants, 100 test inputs, automated scoring using an LLM judge, statistical significance check. This becomes a reusable tool.

**Phase 1, Skill 2 — Competency Milestone:**
> You have a prompt library of 20+ production-grade patterns, a versioned production system prompt that has survived adversarial testing, and a working DSPy pipeline that outperforms a hand-written prompt on a real task.

---

### Skill 3 of 10: Domain Expertise × AI Application

> **Classification:** Your most durable and defensible differentiator  
> **Why now:** This is not a skill you "learn" — it's a practice you begin immediately and sustain indefinitely. The earlier you start, the more compounding you get.

---

#### Week 1–2: Domain Problem Mapping

**Core Concept: The most valuable AI applications solve specific domain problems that generalists would never find.**

**Do (this week, before anything else):**
- [ ] Write a **Domain Problem Inventory**: list the 20 most significant workflow bottlenecks, data gaps, manual processes, or decision latencies in your domain. Be specific. Not "data processing is slow" but "converting incoming as-built PDFs into structured GIS-compatible feature data takes 3 hours per project and introduces 15% error rate."
- [ ] For each problem, score it on two axes (1–5): **Impact** (how much time/money/quality does solving it affect?) and **AI Tractability** (how well-suited is current AI for this problem?). Plot them. The top-right quadrant is your opportunity zone.
- [ ] Select the **top 3 problems** from your quadrant. For each, write a one-page problem spec: current state, existing data available, what better looks like, who benefits, failure modes if the AI is wrong.

**Concepts to understand by end of Week 2:**
- The difference between problems that are AI-tractable (structured inputs, assessable outputs) vs. problems that sound AI-tractable but aren't (highly subjective, no ground truth, catastrophic failure modes)
- Why "time savings" is often a weaker AI product value proposition than "error reduction" or "access to expertise"
- What "minimum lovable product" means in the context of internal AI tools

---

#### Week 3–4: First Domain AI Tool

**Core Concept: One shipped, imperfect tool teaches you more than ten detailed plans.**

**Do:**
- [ ] Choose the most tractable problem from your top 3. Scope it down ruthlessly until you can build a working version in 8–12 hours of focused work.
- [ ] Build it. Ship it to at least one other person who has the same domain expertise. It does not need to be polished. It needs to work on real inputs.
- [ ] Conduct **3 structured feedback sessions** with domain-expert users. Use the Mom Test format: ask about their actual workflow, not whether they like your tool. Record the sessions (with permission). Listen more than you talk.

---

#### Week 5–8: Domain Intelligence Gathering (Ongoing Practice)

**Core Concept: Domain intelligence is the raw material for AI product opportunity.**

**Establish these as permanent habits starting now:**
- [ ] **Weekly:** Read one trade publication or domain-specific technical resource. Not AI publications — domain publications. The problems worth solving are described there in plain language.
- [ ] **Monthly:** Have one substantive conversation with a practitioner in your domain about where they feel friction. Take notes. Add to your problem inventory.
- [ ] **Quarterly:** Review your problem inventory and update scores based on what you've learned. Move problems between tiers as AI capabilities evolve.

**Phase 1, Skill 3 — Competency Milestone:**
> You have a domain problem inventory of 20+ items, one shipped AI tool with feedback from at least 3 domain-expert users, and an established weekly practice of domain intelligence gathering.

---

## Phase 2: Technical Foundation (Months 2–6)
### Skills: Agent Architecture & Orchestration · Data Engineering for AI Systems

These are the two heaviest technical investments on this roadmap. They require consistent focused effort over multiple months. Do not try to rush them.

---

### Skill 4 of 10: Agent Architecture and Orchestration

> **Classification:** Differentiator — the single highest-leverage technical skill of the next three years  
> **Why this order:** Agent architecture requires you to understand systems, debugging, and observability — which Phase 1 gave you. You will build better agents having already instrumented a production system.

---

#### Month 2, Week 1–2: Agent Fundamentals from First Principles

**Core Concept: Understand what frameworks are abstracting before you use them.**

**Study:**
- [ ] Read the **ReAct paper**: "ReAct: Synergizing Reasoning and Acting in Language Models" (arxiv.org/abs/2210.03629). This is the foundational pattern underlying most agent architectures. Read it fully.
- [ ] Read the **MemGPT paper**: "MemGPT: Towards LLMs as Operating Systems" (arxiv.org/abs/2310.08560). Focus on the memory architecture — this is where most agent systems fail in practice.
- [ ] Complete DeepLearning.AI's "AI Agents in LangGraph" short course (free, ~4 hours at learn.deeplearning.ai).

**Do:**
- [ ] Build a **two-agent system from scratch without any framework**. No LangChain, no LangGraph, no CrewAI — just Python and the model API directly. Agent 1 receives a task and breaks it into steps. Agent 2 executes each step. Agent 1 evaluates the result and decides whether to continue or finish.
  - This exercise is painful and instructive. The pain is the lesson.
  - Specific requirement: the system must handle at least one tool call (web search, file read, or API call).
  - Log everything: every message sent to every model, every decision made, every tool call result.

**Concepts to understand by end of Week 2:**
- What the "think-act-observe" loop is and how it maps to ReAct
- The difference between an agent's working memory (current context), episodic memory (conversation history), and semantic memory (knowledge base)
- Why agents fail in loops, how to detect looping behavior, and how to break out of it
- The difference between a tool call and a sub-agent and when to use each

---

#### Month 2, Week 3–4: LangGraph Deep Dive

**Core Concept: LangGraph is currently the best production-grade framework for stateful agent workflows.**

**Study:**
- [ ] Read the LangGraph conceptual documentation (python.langchain.com/docs/concepts/architecture) — specifically: StateGraph, nodes, edges, conditional edges, and checkpointing. Read the source code for at least one LangGraph example in the official repo.
- [ ] Complete the official LangGraph tutorial series: the "Introduction to LangGraph" notebook sequence. Do not just run the cells — type out every example manually and modify it.

**Do:**
- [ ] Rebuild your two-agent system from Week 1 in LangGraph. Notice exactly what LangGraph is handling for you that you had to write manually before.
- [ ] Extend it with **persistent state**: use LangGraph's checkpointing to save state between runs. Resume a paused agent workflow from a checkpoint. This is where most tutorials stop and where real production complexity begins.

---

#### Month 3: Multi-Agent Communication Patterns

**Core Concept: Multi-agent systems have predictable coordination failure modes. Learn them before you hit them.**

**Study:**
- [ ] Read "Enterprise Integration Patterns" by Hohpe and Woolf, Chapters 1–3 and Chapter 9 (Routing). The patterns described for enterprise messaging systems map directly to multi-agent coordination.
- [ ] Read the LangGraph multi-agent documentation — specifically the sections on supervisor/worker patterns and hierarchical agents.
- [ ] Read the AutoGen paper: "AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation" (arxiv.org/abs/2308.08155).

**Do:**
- [ ] Build a **three-agent system** using the supervisor/worker pattern: one supervisor agent receives a complex task and delegates to two specialist agents, then synthesizes their outputs. The task should require genuinely different specializations.
- [ ] Implement **human-in-the-loop** approval: the supervisor must request human approval before taking any irreversible action (sending an email, modifying a file, making an API call that costs money). Test that the approval gate actually works.
- [ ] Instrument the entire system with your Phase 1 observability setup. Every agent interaction should be visible as a trace.

---

#### Month 4: Tool Design and MCP

**Core Concept: Tools are the interface between agents and the world. Poor tool design is the most common cause of agent failure.**

**Study:**
- [ ] Read the Model Context Protocol specification at modelcontextprotocol.io/specification. Read the full spec, not just the tutorials.
- [ ] Read Anthropic's "Tool Use" documentation thoroughly. Specifically study: how to write tool descriptions that the model understands reliably, how to handle tool call errors, how to design tool schemas that minimize ambiguous inputs.

**Do:**
- [ ] Build a **custom MCP server** that exposes a real data source you use regularly. Requirements: at least 3 tools, proper error handling, input validation, and documentation strings that would help a model use the tools correctly.
- [ ] Design a **tool testing suite**: for each tool in your MCP server, write 10 test cases covering: happy path, malformed input, missing input, rate limit handling, and unexpected output format.
- [ ] Study 5 existing open-source MCP servers (the MCP servers repository on GitHub has many). For each: what design decisions did they make? What would you do differently?

---

#### Month 5–6: Production Agent Patterns and Failure Recovery

**Core Concept: Production agents fail in ways demos never do. Design for failure from the start.**

**Study:**
- [ ] Read "Designing Distributed Systems" by Brendan Burns (free at azure.microsoft.com/resources/designing-distributed-systems). Chapters on sidecar, ambassador, and adapter patterns apply directly to agent system design.
- [ ] Study the Temporal workflow orchestration documentation (temporal.io/docs) — specifically understand how Temporal handles long-running, failure-prone workflows. You don't need to implement it immediately, but understanding durable execution is critical.

**Do:**
- [ ] **Build a production-grade agent project** that meets all of the following requirements:
  - Minimum 3 agents with distinct roles
  - Persistent state that survives process restarts
  - At least 5 tools with proper error handling
  - Human-in-the-loop approval for consequential actions
  - Full observability (traces, costs, quality scores)
  - Documented failure modes and recovery behavior
  - A runbook (from your Phase 1 practice)
- [ ] Write a **post-mortem** on every significant failure your agent system produces during development. What failed? Why? What did you change? This document is one of your most valuable learning artifacts.

**Phase 2, Skill 4 — Competency Milestone:**
> You have built a production-grade multi-agent system from scratch, can explain every architectural decision and its tradeoff, can read and extend LangGraph source code, and have a documented library of agent failure modes and mitigations.

---

### Skill 5 of 10: Data Engineering for AI Systems

> **Classification:** Table Stakes for builders, Differentiator at depth  
> **Why this order:** Your agents need data. Good data pipelines are what separate agents that work in demos from agents that work in production.

---

#### Month 2–3: Modern Data Stack Foundations

**Core Concept: The bottleneck in almost every enterprise AI project is not the model — it's the data.**

**Study:**
- [ ] Complete the **dbt (data build tool) Learn** free certification (courses.getdbt.com). Do not skip this even if you know SQL well — dbt introduces a way of thinking about data transformation that is important context.
- [ ] Read "Fundamentals of Data Engineering" by Joe Reis and Matt Housley, Chapters 1–5. This is the best current overview of the data engineering landscape.
- [ ] Study Apache Parquet format documentation (parquet.apache.org/docs). Understand why columnar storage matters for AI workloads. Read the comparison between Parquet, Avro, and ORC.

**Do:**
- [ ] **SQL deep dive**: if you cannot write window functions, CTEs, and recursive queries confidently, spend two weeks on Mode Analytics SQL Tutorial and SQLZoo advanced problems before moving on. This is non-negotiable foundational work.
- [ ] Set up a **local data stack**: DuckDB + dbt + Parquet files. DuckDB is the fastest way to work with columnar data locally. Build a small pipeline: ingest a CSV, transform it with dbt, output a Parquet file, query it with DuckDB. Make it fast and repeatable.

---

#### Month 3–4: Vector Storage and Retrieval Architecture

**Core Concept: Vector retrieval is the foundation of RAG, and most RAG implementations are mediocre because of poor retrieval design.**

**Study:**
- [ ] Read the **original RAG paper**: "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks" (arxiv.org/abs/2005.11401). Understand the original architecture before studying variants.
- [ ] Read the LlamaIndex documentation on "Node Parsers and Text Splitters" in full — this is the best treatment of chunking strategy available publicly.
- [ ] Read the **"Lost in the Middle" paper** (arxiv.org/abs/2307.03172): understanding how language models use context retrieved from vector stores — and why middle-of-context information is often ignored — should fundamentally change how you design retrieval.

**Do:**
- [ ] Build a retrieval evaluation benchmark before building any RAG system: define 50 question-answer pairs from a document corpus. Test 4 different chunking strategies (fixed-size, semantic, sentence-window, document hierarchy) against your benchmark. Measure: retrieval recall (does the right chunk come back?), answer quality (does the LLM answer correctly given the chunk?). Write down the numbers.
- [ ] Implement **hybrid search** (combining dense embedding search with sparse BM25 search) using Qdrant or Weaviate. Compare retrieval quality against pure dense search on your benchmark. Understanding when hybrid outperforms dense-only is important practical knowledge.
- [ ] Study and implement **metadata filtering**: design a metadata schema for a real document corpus that enables pre-filtering before vector search. Measure the latency and quality improvement.

---

#### Month 4–5: Pipeline Architecture and Orchestration

**Core Concept: Data pipelines for AI systems need to be automated, observable, and fault-tolerant.**

**Study:**
- [ ] Complete the **Apache Airflow fundamentals** course on the official Astronomer Academy (free). Alternatively, study Prefect (prefect.io/docs) if you prefer a more Pythonic approach.
- [ ] Read the **OpenLineage** documentation (openlineage.io) — understand data lineage at a conceptual level. Being able to trace "where did this chunk come from and when was it last updated" is critical for debugging AI system failures.

**Do:**
- [ ] Build a **complete automated data pipeline**:
  - Source ingestion (web scrape, API pull, or file watch)
  - Data cleaning and normalization
  - Chunking with your chosen strategy
  - Embedding generation (batch, not one-by-one)
  - Vector store upsert with change detection (don't re-embed unchanged documents)
  - Retrieval quality check (run your benchmark after each pipeline run)
  - Scheduled to run automatically (daily or on file change)
- [ ] Implement **data freshness tracking**: every chunk in your vector store should have metadata including source URL/path, ingestion timestamp, content hash, and schema version. Build a dashboard that shows data freshness across your corpus.

---

#### Month 5–6: Advanced Retrieval Patterns

**Core Concept: Context window inflation does not eliminate the need for good retrieval — it changes what good retrieval means.**

**Study:**
- [ ] Read "HyDE: Hypothetical Document Embeddings for Improved Retrieval" (arxiv.org/abs/2212.10496). Understand and implement this technique — it's simple and often effective.
- [ ] Study the **RAPTOR paper** (Recursive Abstractive Processing for Tree-Organized Retrieval): arxiv.org/abs/2401.18059. This represents a more sophisticated approach to hierarchical retrieval.
- [ ] Read about **late interaction models** — specifically the ColBERT paper. You don't need to implement it, but understanding multi-vector retrieval expands your architectural vocabulary.

**Do:**
- [ ] Add **HyDE retrieval** to your pipeline and measure the effect on your benchmark.
- [ ] Build a **re-ranking layer**: after initial retrieval, use a cross-encoder model (the `cross-encoder/ms-marco-MiniLM-L-12-v2` model from Hugging Face is a good default) to rerank the top-20 retrieved chunks to top-5 before passing to the LLM. Measure quality improvement.

**Phase 2, Skill 5 — Competency Milestone:**
> You have a complete automated data pipeline with observability, a retrieval benchmark with quantitative scores for multiple strategies, and a production-quality RAG system with hybrid search, metadata filtering, and re-ranking.

---

## Phase 3: Compound Skills (Months 6–18)
### Skills: AI Evaluation Design · Systems Integration · AI Product Sense

These skills require a foundation of practical experience to truly internalize. You will learn them faster now that you have real systems to apply them to.

---

### Skill 6 of 10: AI Evaluation Design and Engineering

> **Classification:** Differentiator — the most underrated skill in the entire AI space  
> **Why this order:** You cannot design good evals until you've experienced what it feels like to have an AI system fail in production in ways you couldn't measure. That experience is now behind you.

---

#### Month 6–7: Evaluation Foundations and Philosophy

**Core Concept: An eval suite is a precise statement of what you want your AI system to do and how you'll know if it's doing it.**

**Study:**
- [ ] Read the **HELM paper**: "Holistic Evaluation of Language Models" (arxiv.org/abs/2211.09110). Focus on Sections 1–3 and the evaluation scenario design methodology.
- [ ] Read the **G-Eval paper**: "G-Eval: NLG Evaluation Using GPT-4 with Better Human Alignment" (arxiv.org/abs/2303.16634). This is the foundational technique for automated LLM-as-judge evaluation.
- [ ] Read the Anthropic "Red-teaming Language Models to Reduce Harms" paper (arxiv.org/abs/2209.07858). Red-teaming is a form of evaluation; understanding the methodology is important even outside safety contexts.

**Do:**
- [ ] Write an **evaluation philosophy document** for one of your existing AI systems. Define: What are the 5 most important properties this system should have? For each property, how will you measure it? What's an acceptable score? What's a failing score? This document forces clarity that most teams never achieve.
- [ ] Build a baseline eval suite of 100 test cases for one of your systems. Classify each test case by: type (correctness, instruction following, format compliance, safety), difficulty (easy/medium/hard), and failure mode it's testing for.

---

#### Month 7–8: Automated Evaluation Infrastructure

**Core Concept: Manual evaluation doesn't scale. Automated evaluation is approximate but necessary.**

**Study:**
- [ ] Complete the **Braintrust documentation** (braintrust.dev/docs) — specifically the sections on experiments, scoring functions, and dataset management. Braintrust is currently the best purpose-built LLM eval platform.
- [ ] Study the **RAGAS framework** (ragas.io/docs) — specifically for evaluating RAG systems. The faithfulness, answer relevancy, and context precision metrics are important concepts.
- [ ] Read "Who Validates the Validators?" — the meta-problem of using AI to evaluate AI and how to calibrate AI judges against human judgment.

**Do:**
- [ ] Set up **Braintrust** for one of your systems. Connect it to your existing test cases. Run automated evaluations on every prompt change. Make this a CI/CD-like habit.
- [ ] Build a **custom scoring function** for a domain-specific quality criterion that off-the-shelf eval tools can't measure. This is where your domain expertise compounds — you know what "correct" looks like in your domain better than any generic eval framework.
- [ ] **Calibrate your AI judge**: take 50 outputs from your system. Have yourself and at least one other domain expert rate them. Have your LLM judge rate them. Calculate the correlation. If the correlation is below 0.7, redesign the judge prompt.

---

#### Month 8–10: Adversarial Evaluation and Red-Teaming

**Core Concept: Your system's quality is defined by its worst failures, not its average performance.**

**Study:**
- [ ] Read OWASP's "Top 10 for LLM Applications" (owasp.org/www-project-top-10-for-large-language-model-applications). Each item in the Top 10 should become a category in your adversarial eval suite.
- [ ] Study "Prompt Injection Attacks and Defenses in LLM-Integrated Applications" (arxiv.org/abs/2310.12815).

**Do:**
- [ ] Build a **red-team eval suite** of 50+ adversarial inputs for each system you own. Categories: prompt injection, scope violations, jailbreak attempts, ambiguous inputs designed to expose unclear instructions, edge cases in your specific domain.
- [ ] Run a **structured red-team session**: invite 2–3 people unfamiliar with your system to try to break it for 1 hour. Watch what they try. Document every unexpected behavior. Patch and retest.
- [ ] Build a **regression eval suite**: every bug you fix in your AI system gets a corresponding test case in a regression suite. This prevents the same failure from recurring silently.

**Phase 3, Skill 6 — Competency Milestone:**
> You have an automated eval suite covering correctness, instruction following, format compliance, and adversarial robustness. Eval runs automatically on every significant prompt or configuration change. You have calibrated your AI judge against human judgment and the correlation exceeds 0.75.

---

### Skill 7 of 10: Systems Integration and the "Last Mile" Problem

> **Classification:** Differentiator — where most enterprise AI value lives  
> **Why this order:** Systems integration requires understanding what you're integrating your AI system with. After Phase 2, you have production systems to integrate. The patterns make more sense with real context.

---

#### Month 6–8: Integration Fundamentals

**Core Concept: Every enterprise system is a special case. The patterns are universal; the implementation is always a negotiation.**

**Study:**
- [ ] Read **"Enterprise Integration Patterns"** by Hohpe and Woolf in full. Yes, the full book. This is a long-term reference — read it slowly over two months. Focus on: pipes and filters, message routing, content-based routing, scatter-gather, saga pattern, and dead letter channel.
- [ ] Study OAuth 2.0 deeply: read the RFC 6749 (or a well-written tutorial like auth0.com/docs/authenticate/protocols/oauth). Understanding authorization flows at the protocol level (not just "add OAuth library") is required for enterprise integrations.
- [ ] Read the OpenAPI 3.1 specification overview (spec.openapis.org/oas/v3.1.0). Practice reading API specs as a primary artifact, not documentation.

**Do:**
- [ ] Build integrations with at least four different system types:
  - A SQL database (with connection pooling, parameterized queries, and connection error handling)
  - A REST API (with OAuth authentication, rate limit handling, retry with exponential backoff, and response validation)
  - A file system or object store (with change detection, locking, and concurrent access handling)
  - A messaging system (Slack, email, or similar — with webhook verification and idempotency)
- [ ] For each integration: write an integration test that covers the happy path AND the top 3 failure modes.

---

#### Month 8–12: AI-Specific Integration Patterns

**Core Concept: MCP is standardizing AI-to-system integration the way REST standardized web APIs — learn it while the patterns are still forming.**

**Study:**
- [ ] Read the full MCP specification (modelcontextprotocol.io/specification/2025-03-26) including the transport, authorization, and server lifecycle sections.
- [ ] Study **Temporal** workflow documentation (temporal.io/docs/concepts) — specifically: workflows, activities, signals, and queries. Temporal solves the "what happens when a multi-step process is interrupted" problem that kills most agent systems in production.

**Do:**
- [ ] Build a **production-grade MCP server** that exposes a real enterprise system (database, API, or data source you actually use). Requirements: proper authentication, input validation, error codes with actionable messages, rate limiting, and documentation strings that would help an LLM use the tools correctly.
- [ ] Implement a **durable workflow** using Temporal or a similar system (n8n with persistent state also works) for one of your agent processes that involves multiple steps and could fail mid-execution. Test the recovery behavior explicitly.
- [ ] Study the **specific integration challenges** for the top 5 systems in your domain. Write up a one-page integration guide for each: what the authentication model is, what the rate limits are, what the common failure modes are, and what the API coverage gaps are.

**Phase 3, Skill 7 — Competency Milestone:**
> You have production integrations with at least 4 different system types, a custom MCP server with proper error handling, and at least one durable workflow that survives process restarts and recovers from mid-execution failures.

---

### Skill 8 of 10: AI Product Sense and System Design

> **Classification:** The rarest differentiator on this list  
> **Why this order:** Product sense requires pattern recognition across multiple attempts. After Phase 1–2, you have built several AI systems and observed real users interacting with them. That experience is the raw material for developing genuine product judgment.

---

#### Month 7–10: Product Thinking Foundations

**Core Concept: Technical AI capability far exceeds organizational and market capacity to absorb it. The constraint is product judgment, not engineering.**

**Study:**
- [ ] Read **"Continuous Discovery Habits"** by Teresa Torres. Read it slowly. Apply the opportunity solution tree framework to your domain immediately.
- [ ] Read **"The Mom Test"** by Rob Fitzpatrick. Short book, high density. Internalize the core rule: never ask someone if they like your idea. Ask about their actual behavior and constraints.
- [ ] Read **"Competing Against Luck"** by Clayton Christensen — the deep treatment of Jobs to Be Done theory. The oversimplified "job story" version is useful; the full framework is more powerful.
- [ ] Study the "AI Product Playbook" by a16z (andreessen horowitz blog, search the title) — a useful current-state overview of AI product design patterns.

**Do:**
- [ ] **Conduct 15 structured user interviews** using The Mom Test methodology. These should be with domain practitioners — people who actually do the work your AI tools would assist. Record them (with permission). Write a synthesis document after each batch of 5.
- [ ] Analyze **20 AI products** you use or have used: for each, write a structured analysis covering: the core job to be done, how AI specifically enables the solution (and whether AI is actually necessary vs. just present), the primary failure mode you've observed or can infer, and the pricing model and why it fits or doesn't fit the value delivered.

---

#### Month 10–14: AI-Specific Product Design Patterns

**Core Concept: AI products have failure modes that software products don't. Good AI product design accounts for them explicitly.**

**Study:**
- [ ] Read the Nielsen Norman Group research on "AI Writing Assistants" and "Conversational AI UX" — the empirical usability research on how people actually interact with AI is more valuable than product design theory.
- [ ] Study **trust calibration** in AI systems: read "Overreliance on AI: Literature Review" (Microsoft Research, 2022). Understanding how users incorrectly calibrate trust in AI outputs is foundational to designing systems that don't cause harm through over-reliance.
- [ ] Study pricing models for AI products: read "Pricing for AI" by Kyle Poyar (OpenView blog). Understand the difference between seat-based, usage-based, and outcome-based pricing, and what each implies for product design.

**Do:**
- [ ] Write **3 structured product specs** for AI applications in your domain — one for each of your top-3 domain problems. Each spec must include:
  - Problem statement (specific, quantified if possible)
  - User persona (specific role, context, current behavior)
  - Proposed AI solution (precise about what the AI does)
  - Why AI vs. non-AI (specific capability argument)
  - Success metrics (quantified, measurable)
  - Top 3 failure modes and mitigations
  - Pricing hypothesis and reasoning
- [ ] For each spec, write a **pre-mortem**: argue as forcefully as you can for why this product will fail. Then revise the spec to address the strongest failure arguments.
- [ ] **Validate one spec**: design and run a lightweight test before building. This could be a Wizard of Oz test (you manually do the AI's work), a landing page, or a structured demo to 5 potential users with a follow-up purchase/commitment ask.

**Phase 3, Skill 8 — Competency Milestone:**
> You have conducted 15+ structured user interviews, produced 3 validated product specs for domain-specific AI applications, and have a working framework for assessing AI product opportunities that you can apply consistently.

---

## Phase 4: Deep Specialization (Months 18+)
### Skills: Fine-tuning & Model Customization · Secure & Compliant AI Deployment

These skills have high activation cost and deliver the most value when applied to real production systems. Start here only after you have systems worth fine-tuning and deploying into compliance-sensitive environments.

---

### Skill 9 of 10: Fine-tuning, Model Customization, and the Decision Framework

> **Classification:** Differentiator — especially for domain-specific applications  
> **Why this order:** Fine-tuning on the wrong task with inadequate data is a waste of weeks. You will make far better fine-tuning decisions after 18 months of building production AI systems and accumulating real domain-specific data.

---

#### Month 18–20: Conceptual Foundation and Decision Framework

**Core Concept: The most important fine-tuning skill is knowing when not to fine-tune.**

**Study:**
- [ ] Read the **InstructGPT paper**: "Training Language Models to Follow Instructions with Human Feedback" (arxiv.org/abs/2203.02155). This is how base models become instruction-following models. Understanding this process changes how you think about what fine-tuning can and cannot do.
- [ ] Read the **LoRA paper**: "LoRA: Low-Rank Adaptation of Large Language Models" (arxiv.org/abs/2106.09685). This is the foundational parameter-efficient fine-tuning technique. Read the full paper, including the mathematical formulation.
- [ ] Read the **DPO paper**: "Direct Preference Optimization: Your Language Model is Secretly a Reward Model" (arxiv.org/abs/2305.18290). DPO is replacing RLHF as the primary alignment technique and is much more accessible.

**Do:**
- [ ] Build the **fine-tuning decision framework** for your domain. Answer these questions in writing:
  - What tasks do I run repeatedly where a specialized model would outperform a prompted general model?
  - What data do I have (or can generate) to train on?
  - What are my latency requirements — does fine-tuning a smaller model solve a latency problem?
  - What are my cost requirements — does fine-tuning a smaller model solve a cost problem?
  - What are my privacy requirements — does data sovereignty require a locally-deployed model?
  - What is my evaluation benchmark — how will I know if the fine-tuned model is better?

---

#### Month 20–22: Practical Fine-tuning

**Core Concept: Dataset quality is more important than training configuration.**

**Study:**
- [ ] Complete the **Hugging Face Fine-tuning course** (huggingface.co/learn/nlp-course, Chapter 3 and Chapter 7). Do not skip this even if you've done it before.
- [ ] Read the **Unsloth documentation** (unsloth.ai/docs) — Unsloth provides 2–5x faster training than vanilla HuggingFace with the same hardware.
- [ ] Study the **Alpaca data format** and the **ShareGPT data format** — these are the two most common formats for instruction fine-tuning datasets. Understand when to use each.

**Do:**
- [ ] Run your **first fine-tuning experiment** on a 7B model (Llama 3.1 8B or Qwen 2.5 7B are good choices):
  - Select a specific, narrow task — structured data extraction from domain-specific documents is a good starting point
  - Assemble a dataset of 500–2000 high-quality examples (quality over quantity)
  - Fine-tune using Unsloth + QLoRA (4-bit quantization) — this runs on a single consumer GPU or a free Google Colab A100 session
  - Evaluate against: base model with few-shot prompting, base model with system prompt, and GPT-4o with few-shot prompting
  - Write up the comparison: accuracy, cost, latency. Be rigorous.
- [ ] Implement **synthetic data generation** to augment your dataset: use a frontier model (GPT-4o or Claude) to generate additional training examples in the style of your hand-curated data. Measure whether synthetic data improves or degrades model quality. The answer is task-dependent and worth knowing.

---

#### Month 22–24: Advanced Fine-tuning and Evaluation

**Core Concept: Fine-tuning is an optimization process. Treat it like one.**

**Study:**
- [ ] Learn **Axolotl** (github.com/axolotl-org/axolotl) — a more flexible training framework than Unsloth, better for complex training configurations.
- [ ] Study the **MMLU and TruthfulQA benchmarks** — running these on your fine-tuned models before and after training is a sanity check to ensure you haven't degraded general capabilities while improving task-specific performance.

**Do:**
- [ ] Run a **catastrophic forgetting experiment**: fine-tune a model on a narrow task, then test it on general capabilities. Measure the degradation. Learn to set your learning rate and number of epochs to minimize forgetting while maximizing task improvement.
- [ ] Build a **fine-tuning experiment tracker** using Weights & Biases: log hyperparameters, training curves, and eval results for every experiment. You should be able to look back at any fine-tuning run 6 months later and reproduce it.
- [ ] Deploy your best fine-tuned model using **Ollama** for local inference or **vLLM** for high-throughput deployment. Measure actual production latency and cost against the API alternatives.

**Phase 4, Skill 9 — Competency Milestone:**
> You have fine-tuned at least 2 models on real domain-specific tasks, run rigorous comparisons against baseline approaches, have a clear decision framework for when fine-tuning is worth the investment, and can deploy a fine-tuned model to production.

---

### Skill 10 of 10: Secure and Compliant AI Deployment

> **Classification:** Differentiator — increasingly the enterprise sales blocker  
> **Why this order:** Compliance work is most efficiently done against real systems. After 18 months of building, you have production systems to harden and compliance frameworks to apply them against. The theoretical knowledge from here scales further when applied to real deployments.

---

#### Month 18–21: AI Security Foundations

**Core Concept: AI systems have novel attack surfaces that traditional security training doesn't cover.**

**Study:**
- [ ] Read **OWASP Top 10 for LLM Applications** (owasp.org/www-project-top-10-for-large-language-model-applications) — read all 10 items with their technical descriptions. For each, identify whether your existing systems are vulnerable.
- [ ] Read the **"Prompt Injection Attacks against GPT-3"** paper by Perez and Ribeiro (arxiv.org/abs/2211.09527). Understand the attack taxonomy.
- [ ] Read the **Adversarial Machine Learning threat taxonomy** from NIST (NIST IR 8269). This gives you the vocabulary to discuss AI security with technical and compliance audiences.

**Do:**
- [ ] Conduct a **security audit** of your most production-critical AI system. Use the OWASP Top 10 as a checklist. For each item: is this system vulnerable? Rate the severity (1–5). Propose a mitigation. Prioritize by severity.
- [ ] Implement **guardrails** on a production system using **Guardrails AI** (guardrails-ai.com) or **NeMo Guardrails** (NVIDIA):
  - Input validation: detect and reject prompt injection attempts, PII in inputs, out-of-scope requests
  - Output validation: detect PII in outputs, factual inconsistencies (basic), policy violations
  - Rate limiting: per-user, per-session, per-day limits with graceful degradation
- [ ] Build a **PII detection and redaction pipeline**: before any user input hits an external model API, scan for and redact PII (names, emails, phone numbers, SSNs, account numbers). Log the redaction for audit purposes but never the original PII.

---

#### Month 21–24: Regulatory Compliance Frameworks

**Core Concept: Compliance is not a checkbox — it's an architecture decision made at design time.**

**Study:**
- [ ] Read the **EU AI Act** summary by the Future of Life Institute (futureoflife.org/eu-ai-act-summary). Understand the risk tiers (unacceptable, high, limited, minimal) and what's required at each level.
- [ ] Read the **NIST AI Risk Management Framework** (NIST AI 100-1, available at nist.gov). This is the emerging US standard and is already referenced in federal procurement requirements.
- [ ] Read the **CISA AI Security Best Practices** guidance — specifically relevant for any government or critical infrastructure adjacent work.

**Do:**
- [ ] Complete a **compliance risk assessment** for your highest-stakes AI system:
  - Classify it under the EU AI Act risk tier
  - Map it against NIST AI RMF framework dimensions
  - Identify the top 5 compliance gaps
  - Write a remediation plan with estimated effort for each gap
- [ ] Implement **AI audit logging**: every inference call on a production system should log: timestamp, user ID (anonymized), input hash (not the raw input), output hash, model version, and a quality/policy flag. This log must be tamper-resistant and retained per your domain's record-keeping requirements.
- [ ] Build a **model card** for each fine-tuned model you've created (follow the Hugging Face model card template). This is becoming a regulatory requirement and is good practice regardless.

**Phase 4, Skill 10 — Competency Milestone:**
> You have completed a formal security audit of a production AI system, implemented guardrails and audit logging, produced a compliance risk assessment mapped to both EU AI Act and NIST AI RMF, and can brief a compliance officer or legal team on your system's risk posture.

---

## Master Timeline Summary

| Phase | Months | Skills | Primary Focus |
|-------|--------|--------|---------------|
| Phase 0 | Week 0 | Orientation | Baseline audit, environment setup |
| Phase 1 | Weeks 1–8 | AI Ops, Prompt Engineering, Domain×AI | Fast feedback, immediate value |
| Phase 2 | Months 2–6 | Agent Architecture, Data Engineering | Technical foundation |
| Phase 3 | Months 6–18 | Evals, Systems Integration, Product Sense | Compound skills on real systems |
| Phase 4 | Months 18–24 | Fine-tuning, Compliance/Security | Deep specialization |

---

## Skills Being Deprioritized — and Why

| Skill | Why It's Losing Value | What to Do Instead |
|-------|----------------------|-------------------|
| Basic prompt engineering ("think step by step") | Models are outgrowing the need for this scaffolding | Learn system prompt engineering and DSPy |
| No-code AI tools as a primary skill | Too abstracted to be a technical differentiator | Learn the underlying APIs directly |
| General Python scripting (beginner level) | Commoditized by AI coding assistants | Focus on architecture and system design |
| Single-model chatbot development | Table stakes, rapidly commoditized | Build multi-agent, multi-tool systems |
| Vector DB selection as a deep specialty | Increasingly a configuration choice | Understand all the tradeoffs; don't specialize in one |
| Traditional ML/statistical modeling (unless domain-required) | Lower ROI than the skills in this roadmap for most applied contexts | Stay aware; don't go deep unless your domain demands it |

---

## Weekly Practice Habits (Sustain Throughout)

These are not phases — they run continuously from Week 1 through Month 24 and beyond.

- **Weekly (1 hour):** Read one domain publication or trade resource. Note problems worth solving.
- **Weekly (30 min):** Review your learning journal. What did you build? What broke? What did you learn?
- **Monthly (2 hours):** Review your domain problem inventory. Update scores. Add new problems.
- **Monthly (1 hour):** Review your eval dashboards. Are your systems getting better or degrading?
- **Quarterly (half day):** Audit your skill progress against the milestones in this document. Identify the biggest gap between where you are and where the milestones say you should be.
- **Quarterly (2 hours):** Read 5 AI failure post-mortems or incident reports. They are more instructive than success stories and far harder to find. Seek them out.

---

## Key Resources Quick Reference

### Papers (Read These — They Are Not Optional)
- ReAct (Agent architecture): arxiv.org/abs/2210.03629
- MemGPT (Agent memory): arxiv.org/abs/2310.08560
- RAG original: arxiv.org/abs/2005.11401
- Lost in the Middle (Retrieval position effects): arxiv.org/abs/2307.03172
- DSPy (Prompt optimization): arxiv.org/abs/2310.03714
- LoRA (Fine-tuning): arxiv.org/abs/2106.09685
- DPO (Preference optimization): arxiv.org/abs/2305.18290
- InstructGPT (Alignment): arxiv.org/abs/2203.02155
- G-Eval (LLM evaluation): arxiv.org/abs/2303.16634
- HELM (Benchmark methodology): arxiv.org/abs/2211.09110
- The Prompt Report (Prompting survey): arxiv.org/abs/2406.06608

### Books
- "Observability Engineering" — Majors, Fong-Jones, Miranda
- "Fundamentals of Data Engineering" — Reis, Housley
- "Enterprise Integration Patterns" — Hohpe, Woolf
- "Continuous Discovery Habits" — Torres
- "The Mom Test" — Fitzpatrick
- "Competing Against Luck" — Christensen
- "Release It!" — Nygard
- "Designing Distributed Systems" — Burns

### Platforms and Tools (Build Fluency)
- LangGraph (agent orchestration)
- Langfuse + LangSmith (LLM observability)
- Braintrust (eval platform)
- Evidently AI (data/model drift monitoring)
- Unsloth + Axolotl (fine-tuning)
- Qdrant + Weaviate (vector search)
- dbt (data transformation)
- Temporal (durable workflow execution)
- Guardrails AI (output validation)
- Weights & Biases (experiment tracking)

### Courses
- DeepLearning.AI "AI Agents in LangGraph" (free)
- DeepLearning.AI "Multi AI Agent Systems with CrewAI" (free)
- Hugging Face NLP Course Chapters 3 + 7 (free)
- dbt Learn Fundamentals Certification (free)
- Astronomer Academy Airflow Fundamentals (free)

---

*Last updated: April 2026. The AI landscape moves fast — revisit and revise this roadmap quarterly. The foundational principles are durable; the specific tools and frameworks are not.*
