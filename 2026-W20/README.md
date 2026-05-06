# Agentic AI Harness — What It Is & Why It Matters

**Issue #1** · Week 20 · May 2026 · Topics: Agentic AI, ReAct Loop, LLM Tooling

---

## The Harness, Not the Horse

An **Agentic AI Harness** is the minimal, extensible runtime that sits between a raw LLM and a working AI application. It gives a language model the tools to read, write, edit, and execute — then gets out of the way.

You shape the workflow. It powers the work.

---

## What Problem Does It Solve?

Calling an LLM directly gives you text in, text out. That's useful for summarization, drafting, and classification. But the moment you want an AI to *do* something — touch a file, run a command, query a database — a raw API call isn't enough.

A harness solves this by wrapping the LLM in a loop:

1. **Reason** — the model looks at the task and decides what action to take
2. **Act** — it calls a tool (read file, run bash, search)
3. **Observe** — the tool result is fed back into context
4. **Repeat** — until the task is done or a stop condition is hit

This is the **ReAct pattern** (Reason + Act), and it's the foundation of every serious agentic system in production today.

---

## Five Building Blocks

### 1. System Prompt
The instruction layer that defines the agent's persona, constraints, and available tools. Good system prompts are declarative, not procedural — tell the model *what* it is, not *how* to think.

### 2. Tools
Typed functions the model can call. Standard primitives: `read_file`, `write_file`, `edit_file`, `bash`. The model sees a schema; the harness handles execution.

### 3. Memory
Context beyond the current conversation window. Can be short-term (summarized session history), long-term (vector search over a knowledge base), or working memory (a scratchpad the model writes to during a task).

### 4. The Loop
The orchestration logic: call LLM → check for tool calls → dispatch → inject result → repeat. Most harnesses cap iterations (e.g. max 10) and surface the trace for debugging.

### 5. Output Handler
What happens when the loop ends: write to a file, return a structured object, send to another agent, post a message. Often the most overlooked part.

---

## A Minimal Working Example

```python
while not done:
    response = llm.call(messages=history, tools=tools)
    
    if response.stop_reason == "tool_use":
        result = execute_tool(response.tool_call)
        history.append(tool_result(result))
    else:
        final_output = response.text
        done = True
```

That's the core. Everything else — permissions, branching, streaming, sub-agents — is layered on top.

---

## When You Need a Harness vs. a Single LLM Call

| Scenario | Raw LLM call | Harness |
|---|---|---|
| Summarize a document | ✅ | overkill |
| Write a blog post | ✅ | overkill |
| Refactor a module | ❌ | ✅ |
| Analyze a codebase | ❌ | ✅ |
| Answer a question with retrieval | ❌ | ✅ |
| Multi-step data pipeline | ❌ | ✅ |

Rule of thumb: if the task requires more than one piece of external state, use a harness.

---

## Why It Matters Now

The LLM is no longer the hard part. GPT-4, Claude, Gemini — they're all capable enough for most coding and reasoning tasks. The differentiator is the harness: how cleanly it connects the model to the world, how observable it is, how safely it handles errors.

The engineers winning with AI today aren't the ones with the best prompts. They're the ones who built better harnesses.

---

*Published every Friday. [View web version](index.html) · [Archive](../index.html) · [weekly.mandar.me](https://weekly.mandar.me/) · [GitHub](https://github.com/manddar/weekly)*
