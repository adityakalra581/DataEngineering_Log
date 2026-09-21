# AWS Gen AI Essentials Training — Day 2 Notes
**Date:** September 18, 2026  
**Training:** Gen AI Essentials on AWS 
**Certification Target:** AWS Certified AI Practitioner (AIF-C01)

---

## 1. Evolution — LLMs to Agents

| Stage | What it does |
|---|---|
| LLM (base) | Answers a question — single turn, no memory, no action |
| GenAI Assistant | Responds to user queries with context — multi-turn conversation |
| AI Agent | Reasons, plans, uses tools, takes actions, achieves goals autonomously |
| Multi-Agent System | Multiple agents collaborating — each specialised, orchestrated together |

### Why Agentic AI is Needed
- Some tasks cannot be solved in a single LLM call
- Real-world tasks require multiple steps, tool use, decision making, and iteration
- LLMs alone cannot browse the web, query a database, run code, or call an API — agents can
- Enterprises need systems that work autonomously across long workflows, not just answer questions

---

## 2. Limitations of LLMs

| Limitation | What it means |
|---|---|
| **Context window** | LLM can only see a fixed amount of text at once — cannot process very long documents in one call |
| **No persistent memory** | Each call starts fresh — LLM does not remember previous conversations unless explicitly passed |
| **No tool use** | Cannot take actions — cannot search, calculate, write to a database, or call an API natively |
| **Hallucination** | Confidently generates false information when it does not know the answer |
| **Chain of thought fallacy** | Model can produce plausible-sounding reasoning steps that lead to a wrong conclusion |
| **Fallacy of composition** | Assumes what is true of parts is true of the whole — a logical error LLMs can make |
| **Single-shot limitation** | Complex tasks requiring iteration, feedback, and replanning exceed what one LLM call can do |

---

## 3. What is an Agent

**Agent = Reasoning + Actions**

An AI agent is an LLM that can:
- Perceive input (text, data, tool results)
- Reason about what to do next
- Select and use tools (APIs, databases, code execution, search)
- Observe the result
- Replan and repeat until the goal is achieved

### ReAct Principle
**Re**ason + **Act** — the core loop of an agent

```
Thought: What do I need to do to achieve this goal?
Action: Call this tool with these parameters
Observation: Here is what the tool returned
Thought: Based on this result, what should I do next?
Action: ...
[Repeat until goal achieved]
```

---

## 4. Innovations Powering Agents

| Innovation | What it enables |
|---|---|
| **Tool use / function calling** | LLM can call external APIs, databases, code interpreters |
| **Chain of thought reasoning** | Model thinks step by step before answering — more accurate on complex tasks |
| **Decomposition** | Break a complex goal into smaller subtasks |
| **Task mapping** | Map each subtask to the right tool or model |
| **Planning** | Sequence subtasks in the right order before executing |
| **Memory** | Short-term (conversation history) and long-term (vector DB retrieval) |
| **Multi-model inference** | Route different subtasks to the best model for that task |
| **Multimodal inference** | Process text, images, audio, video in a single agent workflow |

### How AI Software Integration Works
Agent receives goal → decomposes into subtasks → maps subtasks to tools/APIs → executes step by step → observes results → replans if needed → returns final output

---

## 5. Internal Model Process

| Step | What happens |
|---|---|
| Input processing | Tokenise input, embed into vector space |
| Context assembly | Combine system prompt + conversation history + retrieved docs + tool results |
| Reasoning | Transformer layers process context, generate next token probabilities |
| Output generation | Sample next token based on temperature/top-P/top-K, repeat until stop token |
| Tool call decision | If model decides to call a tool, output is structured as a tool call instead of text |
| Tool execution | External system executes the tool, result passed back to model |
| Final response | Model generates final answer based on all accumulated context |

---

## 6. Agent Frameworks — Definitions

| Framework | What it is |
|---|---|
| **LangChain** | Most widely used — provides chains, agents, tools, memory, and vector store integrations. Good for rapid prototyping and connecting LLMs to external tools |
| **LangGraph** | Built on LangChain — designed for multi-step, stateful agent workflows as a graph. Each node is a step, edges define flow. Better for complex branching logic and cycles |
| **LlamaIndex** | Specialised in data ingestion, indexing, and retrieval — best for RAG-heavy applications and connecting LLMs to large document stores |
| **CrewAI** | Multi-agent framework — define agents with roles, goals, and backstories. Agents collaborate as a crew to complete complex tasks |
| **Strands** | AWS-native agent framework — lighter weight, designed to work natively with Amazon Bedrock and AWS services |

---

## 7. Evolution of AI Capability

| Level | What it does | Example |
|---|---|---|
| **Question answering** | Single turn — responds to one query | ChatGPT basic use |
| **GenAI assistant** | Multi-turn — maintains conversation context | Claude, Copilot |
| **AI agent** | Uses tools, takes actions, achieves a goal | Bedrock Agent calling an API |
| **Multi-agent system** | Multiple agents collaborate, each specialised | Crew of agents — researcher + writer + reviewer |
| **Autonomous system** | Runs entire workflows with minimal human input | Fully automated data pipeline management |

---

## 8. MCP — Model Context Protocol

- Open standard protocol for connecting AI models to external tools and data sources
- Think of it as a USB-C standard for AI — any MCP-compatible tool plugs into any MCP-compatible model
- Eliminates need to write custom integration code for every tool-model combination
- Client (model) communicates with MCP server (tool) via a standardised request/response schema
- AWS Bedrock supports MCP — agents can use MCP servers to access external systems

---

## 9. Amazon Bedrock Agent Services

| Service | What it does |
|---|---|
| **Bedrock Agents** | Build agents that reason, plan, and call APIs or knowledge bases to complete tasks |
| **Bedrock Flows** | Visual workflow builder — drag and drop nodes to build multi-step GenAI pipelines without code |
| **Bedrock Agent Core** | Foundational runtime layer for building production-grade agents — handles memory, tool orchestration, session management |
| **Bedrock Knowledge Bases** | Managed RAG — retrieval tool agents can call to get relevant documents |
| **Bedrock Guardrails** | Safety layer applied to agent inputs and outputs |

---

## 10. Types of AI Agents

| Type | What it does | When to use |
|---|---|---|
| **Workflow agent** | Follows a predefined sequence of steps — deterministic, predictable | Structured business processes with known steps |
| **Autonomous agent** | Decides its own steps at runtime based on goal — no predefined flow | Open-ended tasks where steps are unknown upfront |
| **Hybrid agent** | Predefined workflow with autonomous decision points — structured but flexible | Most enterprise use cases — balance of control and flexibility |

---

## 11. Workflow Agent Patterns

| Pattern | What it means |
|---|---|
| **Orchestration** | One orchestrator agent coordinates multiple sub-agents or tools |
| **Routing** | Based on input, route to the most appropriate agent or tool |
| **Parallelization** | Run multiple agent tasks simultaneously — faster for independent subtasks |
| **Sequential** | One step must complete before the next begins — for dependent tasks |
| **Feedback loop** | Output of one step becomes input to the next — iterative refinement |

---

## 12. Agent Maturity vs Task Complexity

| Task complexity | Appropriate agent type |
|---|---|
| Simple, single-step | Direct LLM call — no agent needed |
| Structured multi-step, known flow | Workflow agent |
| Complex, unknown steps, requires reasoning | Autonomous agent |
| Mix of structured and open-ended | Hybrid agent |
| Multiple specialised domains | Multi-agent system |

Rule of thumb: match agent sophistication to task complexity. Over-engineering with a fully autonomous agent for a simple structured task adds cost and unpredictability unnecessarily.

---

## 13. Agentic AI Architecture

```
User Goal
    ↓
Orchestrator Agent (plans, decomposes, routes)
    ↓
┌─────────────┬──────────────┬─────────────┐
Tool Agent 1  Tool Agent 2   Tool Agent 3
(search)      (code exec)    (database)
    ↓              ↓              ↓
Results collected by Orchestrator
    ↓
Final Response to User
```

**Core components:**
- **Planner** — decomposes goal into subtasks
- **Executor** — runs each subtask using appropriate tool or sub-agent
- **Memory Store** — short-term (session) and long-term (vector DB)
- **Tool Registry** — list of available tools the agent can call
- **Event Loop** — plan → act → observe → replan cycle
- **Orchestration layer** — schedules tasks, handles priority, manages failure recovery

---

## Certification Exam Relevance

| Day 2 Topic | Exam Domain | Weight |
|---|---|---|
| LLM limitations, context window | Domain 2 — Gen AI Fundamentals | 24% |
| Agents, ReAct, tool use | Domain 3 — Foundation Model Applications | 28% |
| Bedrock Agents, Flows, Agent Core | Domain 3 — Foundation Model Applications | 28% |
| MCP | Domain 3 — Foundation Model Applications | 28% |
| Agent types, orchestration patterns | Domain 3 — Foundation Model Applications | 28% |

**Domain 3 is 28% of the exam — the biggest domain. Day 2 content maps almost entirely to it.**

---

*See Day 1 notes for: AI evolution, AWS services, foundation models, embeddings, RAG, guardrails, responsible AI, security and compliance*
