# Agent Skills — The Modular Layer for AI Capability — Week of June 5, 2026

**Issue #4** · 2026-W23 · June 5, 2026 · Topics: Agent Skills, Skill Packaging, Dynamic Loading, MCP, Governance, Skill Libraries

> Prompts told an agent what to do. Tools gave it hands. Skills are the missing middle: portable, versioned packages of know-how an agent loads only when the task demands it.

---

## 01 · What an Agent Skill actually is

An **Agent Skill** is a reusable capability package that teaches an agent how to perform a specific task or workflow. Strip away the marketing and it is something refreshingly boring: a folder. Inside sits a manifest, a set of instructions, and any supporting templates, examples, scripts, and reference files the agent might need.

> "Agent Skills are a lightweight, open format for extending AI agent capabilities with specialized knowledge and workflows." — *agentskills.io*

The framing that stuck: skills are **folders of instructions, scripts, and resources the model loads dynamically**. The key word is *dynamically* — a skill is not baked into a system prompt and is not a fine-tune. It sits inert on disk until the agent decides the task matches it, then its contents flow into context.

Why it matters: it decouples **capability** from the **model** and the **prompt**. You ship a skill the way you ship an npm package — write once, version it, hand it to every agent in the org. The skill becomes the unit of reuse instead of copy-pasted guidance.

---

## 02 · Packaging: the SKILL.md and its contracts

The spec defines a skill as a folder containing a **SKILL.md** plus optional resources. Metadata lives in frontmatter; instructions in the body. The metadata is the routing layer — the `description` is what the agent reads to decide relevance, so a vague description means a skill that never fires.

```
---
name: soc2-evidence-request
description: Draft & format SOC 2 evidence responses from a control ID.
version: 1.4.0
tags: [compliance, security, audit]
---

# Instructions
1. Resolve the control ID against controls.csv
2. Pull the evidence template from templates/
3. Validate completeness with rules below
```

A well-formed skill carries:

- **Metadata** — name, description, version, tags
- **Input / output contracts** — what it expects and promises to return
- **Instructions & workflow** — the step-by-step procedure
- **Templates & examples** — concrete artifacts, not abstract advice
- **Supporting scripts & assets** — deterministic code instead of hallucination
- **Dependency declarations** — tools or other skills it relies on

> **So what:** Treat the description and contract as the public API. Everything else is implementation detail you can refactor freely — change the contract and you've shipped a breaking change to every dependent agent.

---

## 03 · Discovery: how an agent finds the right skill

Past a handful of skills, finding the right one is the whole game. Mature platforms behave like a package registry crossed with a search engine:

- Searchable catalogue with categories and tags
- Semantic search over skill descriptions
- Recommendations based on the current task
- Surfacing of popular and trending skills

Public libraries already list **thousands** of skills across engineering, analytics, marketing, and design. That scale is why discovery is hard — a flat list of 5,000 skills is useless to an agent with a finite context window. Retrieval quality (does it surface the *right three*?) now matters as much as the skills themselves. Same lesson RAG taught us, applied to capability instead of knowledge.

---

## 04 · Dynamic loading: pay for context only when you use it

The defining behaviour is **progressive disclosure**. The agent loads a skill's full contents only when the task calls for it. At rest it knows just the name and one-line description; the expensive payload arrives on demand.

- Load skills only when needed — context-aware activation
- Keep the base prompt small and cheap
- Lazy loading and caching of skill bodies

The economics are the point. Context is a fixed budget paid every turn. A 4,000-token skill loaded eagerly costs you on every request; loaded lazily it costs nothing until the one task that needs it. This is what lets a single agent plausibly "know" hundreds of skills without drowning in its own prompt.

> **Practitioner note:** Cache the skill body once loaded within a session. Re-reading the same SKILL.md every turn quietly burns the savings progressive disclosure bought you.

---

## 05 · The execution framework: how to think, made repeatable

A skill encodes a procedure, not just advice. The execution layer holds workflows, tool orchestration, validation checks, error handling, and retry strategies.

- Step-by-step workflows the agent follows deterministically
- Orchestration of the tools the skill depends on
- Validation checks between steps
- Explicit error handling and retry strategies

The clean mental model (from IBM's developer writing): **skills define how the agent should perform a task, while tools provide the actions available.** A skill is the recipe; tools are the kitchen. That separation makes skills portable — the same "reconcile an invoice" skill runs against whatever payment tools a deployment exposes, as long as the contract holds.

---

## 06 · Skills vs. MCP: how to think vs. how to act

The most useful distinction in the space, and the one most often gotten wrong. Skills and MCP servers are complementary layers, not competitors.

| Skills | MCP Servers |
|---|---|
| How to **think** | How to **act** |
| Instructions, workflows, judgement | Discoverable tools, external access |
| Knowledge that lives in context | The actions the agent can take |

MCP handles server discovery and tool capability descriptions; the skill maps its workflow onto available tools. Great tools with no idea how to sequence them = capable but aimless. A beautiful workflow with no way to touch the world = a plan with no hands. Production agents need both.

---

## 07 · Versioning: skills are software, treat them that way

Once shared across teams, a skill inherits every problem of shared software. Workflows evolve, regulations change, internal APIs deprecate — the skill must move without silently breaking dependents.

- Version control on the skill folder
- Backward compatibility guarantees on the contract
- Controlled upgrades and change tracking
- Rollback when an upgrade misbehaves

Mundane and essential, especially where an agent's behaviour is part of an audited process. A bump from `1.4.0` to `2.0.0` should signal a breaking contract change as loudly as any library would. Teams getting this right pin skill versions per agent and roll upgrades like dependency bumps — staged, observable, reversible.

---

## 08 · Governance: the lifecycle of a skill library

Once skills carry organizational authority — SOPs, security guidelines, coding standards — who may publish becomes a governance question. Recent research focuses on the **lifecycle** of large libraries: collection, recommendation, evolution.

- Approval workflow before a skill goes live
- Security review of any scripts a skill ships
- Quality scoring and clear ownership
- Lifecycle management — deprecation and retirement

> "SkillsVote: Lifecycle Governance of Agent Skills from Collection, Recommendation to Evolution." — *arXiv:2605.18401*

The security angle: a skill that ships executable scripts is code you let an autonomous agent run on your behalf. An unreviewed skill in a shared catalogue is a supply chain risk dressed up as a productivity feature. Apply the same review rigor you give a dependency — provenance, what it executes, what it can reach.

---

## 09 · Analytics & composition: measure, then chain

You can't improve a library you don't measure. Analytics close the loop: which skills fire, succeed, cost the most, quietly fail.

- Usage frequency and success rate
- User feedback on outcomes
- Cost and latency per skill
- Effectiveness — finding high-value vs. underperforming skills

The frontier is **composition**. Real work needs several skills chained, with parent-child relationships and dependency graphs. Research shows agents do better when skills are retrieved and grouped as *workflow units* rather than isolated instructions.

> "Group of Skills: Group-Structured Skill Retrieval for Agent Skill Libraries." — *arXiv:2605.06978*

Implication: stop authoring skills as lonely islands. Design them to compose — explicit dependencies, clean contracts at the seams — so the retrieval layer can assemble the right *bundle* instead of betting on one perfect monolith.

---

## 10 · The library ecosystem: where skills live today

Public skill libraries already exist, and their design choices preview where this is heading:

- **Skills Library** ([skills-library.com](https://skills-library.com/)) — large catalogue, copy-paste install model, categories across writing, design, analytics, productivity. Bet: simplicity and discoverability win.
- **MCPServers Agent Skills** ([mcpservers.org/agent-skills](https://mcpservers.org/agent-skills)) — reusable skills for Claude Code, Codex, Cursor; instructions plus executable assets, deep MCP integration. Sample skills: Frontend Design, Figma Code Connect, Document Processing, Data Analysis, Skill Creator.
- **Skills.rest** ([skills.rest](https://skills.rest/)) — marketplace framing: vetted repository, on-demand install, categories across engineering, finance, research, legal, analytics.

The recommended architecture looks like a well-structured software package:

```
Skill/
├── Metadata      # name, description, version, tags
├── Instructions  # the workflow
├── Resources     # templates, examples, knowledge
├── Tool Bindings # MCP tools, APIs, local functions
├── Validation    # rules
└── Tests
```

The throughline: **skills are turning organizational knowledge into reusable agent capability**, and the disciplines that make them work — packaging, versioning, review, measurement — are ones software engineering already knows by heart. Teams that treat their skill library like a codebase, not a junk drawer of prompts, are the ones whose agents compound.

---

## Sources & further reading

- [Agent Skills overview — agentskills.io](https://agentskills.io/home)
- [What are skills? — Claude Help Center](https://support.claude.com/en/articles/12512176-what-are-skills)
- [Build intelligent agents with skills and MCP servers — IBM Developer](https://developer.ibm.com/articles/agent-skills-mcp-servers/)
- [SkillsVote — arXiv:2605.18401](https://arxiv.org/abs/2605.18401)
- [Group of Skills — arXiv:2605.06978](https://arxiv.org/abs/2605.06978)
- [skills-library.com](https://skills-library.com/) · [mcpservers.org/agent-skills](https://mcpservers.org/agent-skills) · [skills.rest](https://skills.rest/)

---

*Published every Friday. [View web version](index.html) · [Archive](../index.html) · [weekly.mandar.me](https://weekly.mandar.me/) · [GitHub](https://github.com/manddar/weekly)*
