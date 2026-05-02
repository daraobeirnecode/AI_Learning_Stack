[How to Build Claude Skills: Complete Guide + MCP Comparison (2026)](https://www.thetoolnerd.com/p/how-to-build-claude-skills-step-by-step-guide-thetoolnerd?utm_campaign=post&utm_medium=web)

When Anthropic dropped Claude Skills on October 16, 2025, I didn’t realise it is so powerful that it can completely change how I work with AI. I had built two custom skills—one for generating BPMN diagrams and another for creating educational math videos.

In just a month, 100s of skills are made available by developers, content creators, financial experts etc.. for us try out (references will be shared at the end of the article). No servers to configure, no APIs to wrestle with, just markdown files and Claude’s code execution environment.

Here’s the thing:

> **If you can write documentation and clear what you want, you can build Claude Skills.**

And that’s exactly why this might be a bigger deal than the Model Context Protocol (MCP) that had everyone buzzing last year.

---

## No Time to Read? Here’s the Scoop

- Claude Skills are lightweight, folder-based instruction packs that teach Claude _how you work_. You write your workflow once—Claude follows it forever.
    
- **Skills ≠ MCP.**  
    MCP connects Claude to external systems (APIs, databases). Skills teach Claude how to _think and execute_ your workflows.
    
- **Why Skills matter:**  
    They load only ~50–100 tokens until needed, then expand to full instructions. No servers, no complex setup, no coding required.
    
- **What you can do with them  
    **Generate BPMN diagrams, Enforce brand guidelines, Format documents, Automate niche workflows, Encode your expertise once and reuse it everywhere
    
- **The magic:**  
    Claude automatically activates the right skills when your task matches their description.
    
- **Skill-creator exists.**  
    Claude can now create new skills for you—from scratch—by asking clarifying questions and packaging everything into a ready-to-use zip.
## What Are Claude Skills (And Why They Matter)

Claude Skills are folder-based instruction packages that teach Claude your specific workflows. **Think of them as custom onboarding materials—you package your expertise once, and Claude automatically loads that knowledge whenever it’s relevant to your task.**

The structure is dead simple:

```
skill-name/
├── SKILL.md              # Required: Instructions with YAML metadata
├── scripts/              # Optional: Helper scripts
├── references/           # Optional: Documentation files
└── templates/            # Optional: Document templates
```

Each skill needs just one file—`SKILL.md`—with YAML frontmatter:

```
---
name: your-skill-name
description: Clear description of what this does and when to use it
---

# Your Skill Name

[Instructions for Claude on how to execute this skill]
```

That’s it. No protocol specifications. No server infrastructure. No complex API integrations.

**How Skills work:** When Claude gets a task, it quickly scans all available Skills to find the ones that match. It does this using progressive disclosure—first loading only lightweight metadata (~100 tokens) so it knows which Skills are relevant. When a Skill is actually needed, Claude loads the full instructions (<5k tokens), and only then pulls in any bundled files or scripts.

**When to use Skills:** Use Skills when you need Claude to handle specialized tasks with consistent accuracy. They’re best suited for:

- **Organizational workflows:** Brand guidelines, compliance processes, document templates
    
- **Domain expertise:** Excel formulas, PDF operations, data analysis
    
- **Personal preferences:** Note-taking formats, coding styles, research methods
    
    **Example:** Create [a brand guidelines Skill](https://github.com/anthropics/skills/tree/main/brand-guidelines) that includes your company’s color palette, typography rules, and layout specifications. When Claude creates presentations or documents, it automatically applies these standards without you needing to explain them each time.
    

**What makes Skills powerful:**

- **Progressive Disclosure**: Skills use ~50-100 tokens until needed, then load full instructions (~5000 tokens). Compare that to MCP servers consuming 10,000+ tokens constantly.
    
    [
    
    ![](https://substackcdn.com/image/fetch/$s_!n83D!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F69335d3a-cc10-435a-a439-844f9c24484a_1700x826.png)
    
    
    
    ](https://substackcdn.com/image/fetch/$s_!n83D!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F69335d3a-cc10-435a-a439-844f9c24484a_1700x826.png)
    
- **Automatic Activation**: Claude scans skill descriptions and loads relevant ones based on your task. No manual triggering required.
    
- **True Portability**: The same skill works in Claude.ai, Claude Code CLI, and the Claude API. Write once, use everywhere.
    
- **Composability**: Multiple skills automatically stack together. Claude coordinates which skills to use and when.
    

_How Claude loads skills: From metadata to full instructions_

[

![](https://substackcdn.com/image/fetch/$s_!5gte!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb14d2731-f56a-4edc-b9a3-812f8e923baa_1218x1690.png)



](https://substackcdn.com/image/fetch/$s_!5gte!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb14d2731-f56a-4edc-b9a3-812f8e923baa_1218x1690.png)

## Claude Skills vs Claude MCP: Different Tools, Different Jobs

Let me clear this up quickly because people keep asking: Skills aren’t replacing MCP. They solve different problems.

[

![](https://substackcdn.com/image/fetch/$s_!Tno7!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2d169195-775f-4805-aef7-ea31bcf2a4a7_2414x813.png)



](https://substackcdn.com/image/fetch/$s_!Tno7!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2d169195-775f-4805-aef7-ea31bcf2a4a7_2414x813.png)

**MCP (Model Context Protocol):**

- Connects Claude TO external systems
    
- Accesses live data from databases, APIs, services
    
- Complex protocol specification
    
- High token cost ( As the system loads the whole tool list at once in the context)
    
- Setup time: Hours to days
    

**Claude Skills:**

- Teaches Claude HOW to perform tasks
    
- Encodes procedures, workflows, standards
    
- Simple markdown files
    
- Minimal token cost (progressive loading)
    
- Setup time: Minutes
    

**When to use MCP:** You need real-time data from GitHub, Slack, databases, or external APIs.

**When to use Skills:** You need consistent execution of workflows, brand guidelines, formatting standards, or specialized procedures.

**When to use both:** MCP fetches the data, Skills tell Claude how to format it. For example, MCP pulls customer data from your CRM, while a skill applies your company’s reporting template.

---

You can read my previous articles as well to know more about MCP and other tools:

AI Learning Roadmap for Beginners: [https://www.thetoolnerd.com/p/ai-learning-roadmap-for-beginners](https://www.thetoolnerd.com/p/ai-learning-roadmap-for-beginners)

MCP + ChatGPT Projects: [https://www.thetoolnerd.com/p/ai-learning-roadmap-for-beginners-part-2-mcp-agents-projects](https://www.thetoolnerd.com/p/ai-learning-roadmap-for-beginners-part-2-mcp-agents-projects)

MCP Basics: [https://www.thetoolnerd.com/p/anthropic-model-context-protocol-mcp-thetoolnerd](https://www.thetoolnerd.com/p/anthropic-model-context-protocol-mcp-thetoolnerd)

## The Game-Changer Claude Skill: Creator Skill

Here’s where it gets meta and brilliant:

> **Anthropic created a skill that creates skills.**

The `skill-creator` skill is included by default (you just need to enable it in Settings). When you ask Claude to create a skill, it:

1. Asks clarifying questions about your workflow
    
2. Generates the folder structure
    
3. Writes the SKILL.md file with proper formatting
    
4. Adds YAML frontmatter
    
5. Includes examples and best practices
    
6. Bundles everything into a working skill
    

**No manual file editing required.** This is what makes Skills accessible to everyone, not just developers.

[

![](https://substackcdn.com/image/fetch/$s_!dP-r!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F913450c6-4d2b-4d32-9a3b-90c2755b26e3_1912x1022.png)



](https://substackcdn.com/image/fetch/$s_!dP-r!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F913450c6-4d2b-4d32-9a3b-90c2755b26e3_1912x1022.png)

_The skill-creator workflow: From idea to working skill in minutes_

[

![](https://substackcdn.com/image/fetch/$s_!Fs7W!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F75b5614f-4f01-4edb-a714-cac327a85112_1871x768.png)



](https://substackcdn.com/image/fetch/$s_!Fs7W!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F75b5614f-4f01-4edb-a714-cac327a85112_1871x768.png)

I used skill-creator to build both of my skills. Let me show you how.

## Case Study 1: Building the Eraser.io BPMN Skill

**The Problem**: I needed to create professional BPMN diagrams for process documentation, but [Eraser.io](https://app.eraser.io/dashboard/all)’s diagram-as-code syntax has specific rules, special character restrictions, and five different diagram types to remember.

**The Solution**: Build a skill that teaches Claude the entire Eraser.io syntax so I never have to look up documentation again.

### Creation Process

**Step 1: Research the Domain**

First, I gathered [Eraser.io](https://app.eraser.io/dashboard/all) documentation for all five diagram types:

- Flowcharts
    
- Entity Relationship Diagrams (ERDs)
    
- Cloud Architecture Diagrams
    
- Sequence Diagrams
    
- BPMN/Swimlane Diagrams
    

I used Claude to fetch and analyze each documentation page, understanding the syntax patterns, properties, and common features.

It did a detailed analysis of the links and structured its analysis

It did a detailed analysis of the links and structured its analysis

[

![](https://substackcdn.com/image/fetch/$s_!XeyX!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F12f826f5-f7eb-45d6-8374-460364ae0401_1516x536.png)



](https://substackcdn.com/image/fetch/$s_!XeyX!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F12f826f5-f7eb-45d6-8374-460364ae0401_1516x536.png)

**Step 2: Use Skill-Creator**

I asked Claude: “Create a skill for generating [Eraser.io](https://app.eraser.io/dashboard/all) diagrams using their diagram-as-code syntax.”

It generated the complete skill structure:

```
eraser-diagrams.zip contains:
├── SKILL.md (13KB main instructions)
└── references/ (6 detailed guides)
    ├── flowchart_syntax.md
    ├── erd_syntax.md
    ├── architecture_syntax.md
    ├── sequence_syntax.md
    ├── bpmn_syntax.md
    └── icon_library.md
```

[

![](https://substackcdn.com/image/fetch/$s_!1is7!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fea7e7838-3d57-4792-9fe5-f4a5f6868c0e_1116x1024.png)



](https://substackcdn.com/image/fetch/$s_!1is7!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fea7e7838-3d57-4792-9fe5-f4a5f6868c0e_1116x1024.png)

**Step 3: Refinement Through Real Usage**

The first version worked, but I discovered an important edge case: BPMN labels can’t contain special characters like `{ } [ ] < > : , /`.

When I tried generating a diagram with a label like “Amount > 100K?”, it failed. I updated the skill with:

- Clear documentation of restricted characters
    
- Correct vs incorrect examples
    
- Alternative patterns (use “greater than” instead of “>”)
    

This kind of real-world testing is how you build robust skills.

### Generated Claude Skill Example: Expense Approval Workflow

Here’s a BPMN diagram I generated using the skill:

[

![](https://substackcdn.com/image/fetch/$s_!J4SO!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0d407173-cab0-41ca-86da-04b8c6da14fc_2214x1631.png)



](https://substackcdn.com/image/fetch/$s_!J4SO!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0d407173-cab0-41ca-86da-04b8c6da14fc_2214x1631.png)

```
title Expense Submission and Approval Flow

User [color: lightblue] {
  Login [icon: log-in]
  Access confirmed [type: event, icon: check-circle]
  Start new expense report [icon: file-plus]
  Upload receipts [icon: upload]
  Reimbursed [type: event, icon: dollar-sign]
}

System [color: orange] {
  OCR extract date & amount [icon: calendar]
  Check amount [type: gateway, icon: filter]
  “Auto-approve?” [type: gateway, icon: check]
  Process reimbursement [icon: credit-card]
}

Manager [color: green] {
  Review expense [icon: eye]
  Approve? [type: gateway, icon: check]
  Approved [type: event, icon: thumbs-up]
}

// Flows
Login > Access confirmed
Access confirmed > Start new expense report
Start new expense report > Upload receipts
Upload receipts -- OCR extract date & amount : Receipts

OCR extract date & amount > Check amount
Check amount > “Auto-approve?” : Amount less than 500
Check amount --> Rejected : Amount greater than 500

“Auto-approve?” > Process reimbursement : Amount less than 50
“Auto-approve?” > Review expense : Between 51-500

Review expense > Approve?
Approve? > Approved : Yes
Approved --> Process reimbursement
Process reimbursement --> Reimbursed
```

I just described the workflow in plain English:

**Prompt:**

> “Create a BPMN diagram for expense submission with three actors: User, System, and Manager. Include approval gates based on amount.”

Claude loaded the eraser-diagrams skill and generated perfectly formatted, syntactically correct code.

**Steps for [Eraser.io](https://app.eraser.io/dashboard/all):  
**Once the code is generated, open [eraser.io](https://app.eraser.io/dashboard/all)  
- Click on Diagram as a code  
- Select the workflow like BPMN, Flowchart etc..

[

![](https://substackcdn.com/image/fetch/$s_!WN9U!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4582a165-61b6-40f7-993f-bf0186bbb806_1548x932.jpeg)



](https://substackcdn.com/image/fetch/$s_!WN9U!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4582a165-61b6-40f7-993f-bf0186bbb806_1548x932.jpeg)

  
- Paste the Code in the Code Space that appears

[

![](https://substackcdn.com/image/fetch/$s_!Eo2n!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0970691d-4791-413c-b708-70d4c2d712c0_2648x1214.png)



](https://substackcdn.com/image/fetch/$s_!Eo2n!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0970691d-4791-413c-b708-70d4c2d712c0_2648x1214.png)

There might be few mistakes and that’s where we as humans are required to solve it.

### What I Learned

**Key Takeaways from Building This Skill:**

1. **Documentation quality matters**: The more specific your instructions, the better Claude performs. Include examples of what works AND what doesn’t.
    
2. **Edge cases are gold**: Real usage reveals restrictions you didn’t anticipate. Document them immediately.
    
3. **Reference files scale better**: Instead of cramming everything into SKILL.md, break detailed syntax into separate reference files. Claude only loads them when needed.
    
4. **Progressive disclosure works**: My skill has 5 diagram types and extensive syntax documentation (~5000 tokens total), but Claude only uses ~50-100 tokens until one requests a diagram.
    

**Download the Eraser Skill here : [https://github.com/akhil20187/toolnerd-skills](https://github.com/akhil20187/toolnerd-skills)**

## Case Study 2: Building the Vedic Manim Skill to create Videos

**The Problem**: I always wanted to explore creating educational videos explaining Vedic Mathematics techniques using Manim (the animation library from 3Blue1Brown), but remembering Manim’s code patterns, timing functions, and animation sequences is tough and also I am from non coding background.

**The Solution**: Build a skill with pre-configured scene classes for common Vedic Math techniques.

### Creation Process

This was the first skill I created and I just asked this simple way to create the skill

[

![](https://substackcdn.com/image/fetch/$s_!h_OX!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc7edb055-1b43-47fc-826b-9463a8182b98_1210x160.png)



](https://substackcdn.com/image/fetch/$s_!h_OX!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc7edb055-1b43-47fc-826b-9463a8182b98_1210x160.png)

Using skill-creator skill, It structured the skill with:

```
vedic-manim/
├── SKILL.md                      # Main skill documentation
├── scripts/
│   ├── vedic_scenes.py          # Pre-built scene classes
│   └── render_vedic.py          # Rendering utility
└── references/
    ├── vedic_methods.md         # Vedic Math techniques
    └── manim_reference.md       # Manim API patterns
```

### What’s Inside

- **Pre-built Scene Classes**: 4 ready-to-use animation scenes for common Vedic Math techniques
    
    - Crosswise Multiplication (Urdhva Tiryagbhyam)
        
    - Nikhilam Method (numbers near bases)
        
    - Squaring Near Base (Yavadunam)
        
    - Digit Sum Verification (Beejank)
        
- **Rendering Utility**: Easy command-line tool to generate videos
    

**Download the Eraser Skill here : [https://github.com/akhil20187/toolnerd-skills](https://github.com/akhil20187/toolnerd-skills)**

## Now Your Turn: Build Your First Claude Skill

Create your own skill:

### Prerequisites

1. Claude Pro, Max, Team, or Enterprise account
    
2. Enable “Code execution and file creation” in Settings > Capabilities
    
3. Enable “Skills” feature preview in Settings > Capabilities
    
4. Toggle on “skill-creator” skill
    

### Quick Start Process

**Step 1: Describe What You Need**

Ask Claude: “Create a skill for [your workflow].”

Be specific. Instead of “email writing,” try “writing internal company announcements following our brand voice and format.”

**Step 2: Answer the Questions**

Skill-creator will/might ask further questions:

- What triggers this skill? (when should it activate)
    
- What’s the desired output format?
    
- Are there specific rules or restrictions?
    
- What examples would help?
    

**Step 3: Download the skill file (.zip)**

Download the skill file (which is a zip file) and upload it in the skills section.

[

![](https://substackcdn.com/image/fetch/$s_!afQ0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff3aadf04-cea5-4042-b693-c64e1347b71a_1464x198.png)



](https://substackcdn.com/image/fetch/$s_!afQ0!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff3aadf04-cea5-4042-b693-c64e1347b71a_1464x198.png)

Upload Skill page

**Step 4: Test It**

The skill is ready immediately. Try using it:

- Give Claude a task that should trigger the skill
    
- Check if it loads (you’ll see it in Claude’s chain of thought)
    
- Verify the output matches your expectations
    

**Step 5: Refine**

Found an edge case? Tell Claude: “Update the [skill-name] skill to handle [scenario].”

The skill-creator can modify existing skills just as easily as creating new ones.

### Simple Skill Ideas to Start

**If you’re a developer:**

- Code review checklist for your team’s standards
    
- API documentation formatter
    
- Git commit message generator (following conventional commits)
    

**If you’re a content creator:**

- Blog post formatter for your CMS
    
- Social media thread creator
    
- SEO meta description generator
    

**If you’re in business:**

- Meeting notes template
    
- Status report formatter
    
- Proposal structure (following your company template)
    

**The pattern**: Look for tasks you do repeatedly with specific formatting or rules. Those are perfect skill candidates.

## Community Resources & Skill Repositories

The Skills ecosystem is growing fast. Here’s where to find more:

### Official Anthropic Skills

- **Repository**: [github.com/anthropics/skills](https://github.com/anthropics/skills)
    
- **Includes**: docx, pdf, pptx, xlsx document skills
    
- **Quality**: Production-ready, maintained by Anthropic
    
- **Reference Articles:** [https://www.claude.com/blog/skills](https://www.claude.com/blog/skills) , [https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)
    

### ComposioHQ Awesome Claude Skills

- **Repository**: [github.com/ComposioHQ/awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills)
    
- **Includes**: 50+ curated community skills
    
- **Categories**: Development, Content, Business, Data, Creative, Automation
    
- **Highlights**: MCP builder, brand guidelines, D3.js visualization
    

### Obra Superpowers

- **Repository**: [github.com/obra/superpowers](https://github.com/obra/superpowers)
    
- **Focus**: Engineering workflow skills
    
- **Includes**: Git worktrees, TDD, brainstorming, root-cause tracing
    

### AI Labs Skills

- **Repository**: [github.com/ailabs-393/ai-labs-claude-skills](https://github.com/ailabs-393/ai-labs-claude-skills)
    
- **Focus**: Experimental and cutting-edge use cases
    

**My recommendation**: Start with skill-creator to build your own, then explore these repositories for inspiration on structure and patterns.

## My Final Take: Claude Skills Are a Turning Point

Before Claude Skills, extending an AI’s capabilities meant choosing between three worlds:  
**prompt hacks**, **MCP servers**, or **full-on fine-tuning**. Each required more effort, more tokens, or more technical expertise than most people could justify.

Skills change that equation completely.

They bring a new middle layer to AI — one that’s:

- **powerful enough** for complex, multi-step workflows
    
- **simple enough** that anyone can build them
    
- **lightweight enough** to scale across different tasks and platforms
    

And most importantly:  
You no longer explain your workflow every day.  
You explain it **once**, and Claude follows it **forever**.