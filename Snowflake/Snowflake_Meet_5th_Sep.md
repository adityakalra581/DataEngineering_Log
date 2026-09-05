# Snowflake Delhi User Group Meetup — Session Notes
*Event: Snowflake Community Meetup, Delhi | Format: 4 technical sessions*

---

## 1. Agentic Migration with Snowflake (AIM) — Ruchi Soni

**What it is:** Snowflake AIM (Agentic Migration) is an end-to-end, AI-agent-driven migration service covering the full lifecycle — assessment, dependency discovery, code conversion, validation, testing, and post-migration monitoring.

**Key takeaways:**
- **Lift-and-shift virtualization** — currently available for Teradata and Snowflake as source platforms. Open question on how much of the tuning is automated ("black box") vs. exposed for manual tuning.
- **ETL capabilities** — migration can lean on native Streams & Tasks for change-data-capture-style pipelines, alongside broader ETL functionality.
- **Connectors** — wide source connector support; containerized deployment is also available.
- **My question to the speaker:** why isn't this more widely adopted yet on active migrations (like the SQL Server → Snowflake one I've been on for ~1.5 years)? Is the barrier cost, data security concerns, or something else — worth revisiting as AIM matures.

**Relevance to my work:** Direct overlap with my ongoing SQL Server → Snowflake migration. Worth a follow-up POC to see how much of my manual assessment/validation work AIM could offload.

---

## 2. Horizon Context & Cortex Sense — Riya Khandelwal

**What it is:** Snowflake Horizon (catalog/governance layer) + Cortex Sense, focused on making enterprise data properly *understandable* to AI agents — not just accessible.

**Core concepts covered:**
- **Unmodeled data & semantic modeling** — raw/unmodeled data needs a semantic layer (business meaning, relationships) before AI agents can reason over it reliably.
- **Semantic views** — a modeling construct that exposes that business context to Cortex/agents in a governed way.
- **RBAC** — role-based access control, applied to what data/context an agent (not just a user) is allowed to see.
- **Semantic Studio** — tooling for building/managing these semantic layers.
- **Lineage** — tracking where data (and derived context) came from, referenced repeatedly as core to trustworthy AI answers.
- **Problem being solved:** AI hallucination — governed, well-modeled context reduces ungrounded answers from AI agents.
- **Select Star acquisition** — Snowflake acquired Select Star for metadata connectivity, enabling metadata pull from external sources (Databricks, SQL Server, Postgres, etc.) without full data movement.
- **Cortex Sense** — currently in private preview.
- Also touched on: knowledge graphs, region-specific availability constraints, and cost considerations for the governance/context layer.

**Relevance to my work:** Governance + lineage angle is useful context for any AI/agent work built on top of our Snowflake environment — good to understand before agents touch production data.

---

## 3. Document Intelligence — Deepti Agrawal

**What it is:** Using Snowflake to turn unstructured documents into structured, queryable, and actionable data — demoed via two projects.

**Project 1 — Document Q&A Agent:**
- Documents ingested and stored in Snowflake.
- An **agent** (not a chatbot) built on top — answers natural-language questions and cites the specific source documents the answer came from.

**Project 2 — Insurance Claims Processing (favorite session):**
- Input: claim-supporting documents (prescriptions, test reports, etc.)
- Pipeline: ingestion → information extraction → validation → decision (approve/reject) → workflow handoff to next stage.
- Goal: streamline claims approval by automating the document review step.
- Also shown: a **Streamlit app** front-end and the underlying **RAG pipeline** with an agent layered on top.
- **Speaker shared the GitHub repo** — plan to walk through the code myself and try rebuilding a version of the claims pipeline.

**Relevance to my work:** Best hands-on session of the day — a template worth adapting for a personal project or repo entry (document ingestion → extraction → validation → decision pipeline).

---

## 4. No More Ad-Hoc Requests: Snowflake CoWork — Priya Chauhan

**Problem statement:** Business users repeatedly go back to data engineers for one-off requests that existing dashboards can't answer, creating a constant stream of ad-hoc work for DE teams.

**What CoWork does:**
- An agent is set up with full context on the business domain (metrics, categories, definitions).
- Business users can then ask natural-language questions directly — e.g., "revenue for this category" — and get answers without filing a request to the data team.
- Built on semantic views, Cortex Search, custom tools, Automations, and Deep Research.

**Relevance to my work:** Potential answer to the "repetitive ad-hoc report" problem I've seen firsthand — worth exploring as a way to reduce DE team bottleneck on business-facing requests.

---
*Notes from live session — some feature names/availability (e.g., Cortex Sense) reflect private preview status at time of writing and may change.*
