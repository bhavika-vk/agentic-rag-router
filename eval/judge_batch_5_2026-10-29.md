# Judge instructions

You are grading answers from a question-answering system. Each case shows a question, what a
correct answer must contain, the expected behavior, and two answers (A and B) from different
versions of the system. Grade each answer on its own; do not compare them or guess their origin.

Verdicts:
- correct: states every required fact (equivalent wording or number formats are fine, for
  example $17,455 million = $17.5 billion) and follows the expected behavior; no invented or
  contradicting facts.
- partial: only some required facts, or only part of the expected behavior.
- incorrect: required facts missing or wrong, invented figures, or the expected behavior is
  not followed.
When "Must contain" is none, grade on the expected behavior only.

Reply with one JSON object per case, one per line, and nothing else:
{"id": "<case id>", "A": "correct|partial|incorrect", "B": "correct|partial|incorrect", "reason_A": "<one sentence>", "reason_B": "<one sentence>"}

## Case H11

Question: What was Uber's revenue in 2021, and what was Uber's revenue in 2025?
Must contain: Uber 2021 revenue $17,455 million (about $17.5 billion)
Expected behavior: 2021 from the filing with citations; 2025 from search results, no invented figure

### Answer A
- **2021 revenue — 10-K financial filings:** Uber’s revenue was **$17.455 billion** (approximately **$17.5 billion**). [1.1]
- **2025 revenue — 10-K financial filings:** The provided context does not include Uber’s 2025 revenue; it reports only 2021 revenue of **$17.455 billion**. [2.1]

### Answer B
- **2021 revenue — 10-K financial filings:** Uber’s revenue was **$17.455 billion** (approximately **$17.5 billion**). [1.1]
- **2025 revenue — live internet search:** The provided context does not contain information about Uber’s 2025 revenue. The listed sources discuss 2025 generally and unrelated reports, not Uber’s financial results. [2.1][2.2][2.3][2.4][2.5]

## Case H12

Question: What orchestration patterns does OpenAI's agent guide describe, and what agent-building tools has OpenAI released recently?
Must contain: manager pattern (agents as tools); decentralized pattern (agents handing off to agents)
Expected behavior: patterns from the guide with citations; recent tools from search results

### Answer A
## Orchestration patterns — OpenAI documentation

OpenAI’s agent guide describes two broad patterns:

1. **Single-agent systems:** One agent uses its model, instructions, tools, guardrails, and hooks to execute a workflow in a loop. The run ends when an exit condition occurs, such as a final-output tool call, a response without tool calls, an error, or a maximum turn limit. [1.1]

2. **Multi-agent systems:** Workflow execution is distributed across multiple coordinated agents, with different agents handling specialized tasks or stages. [1.1]

## Recently released agent-building tools — live internet search

OpenAI has introduced several tools for building AI agents:

- **Agent Builder:** A visual, drag-and-drop workflow editor for designing agent workflows. [2.1]
- **AgentKit:** An all-in-one platform for creating, deploying, and testing AI agents, including Agent Builder. [2.3]
- **Agents SDK:** A developer framework that lets agents collaborate, delegate tasks, and automate complex workflows. [2.5]

**ChatGPT Agent** is also mentioned as an AI that performs tasks for users, but it is more an agent product than a dedicated agent-building tool. [2.2]

### Answer B
OpenAI’s agent guide describes two main orchestration patterns:

1. **Single-agent systems** — One agent uses appropriate tools and instructions to execute a workflow in a loop. Capabilities can be expanded by adding tools, keeping the system easier to evaluate and maintain. **(Source: OpenAI documentation)** [1.1]

2. **Multi-agent systems** — Workflow execution is distributed across multiple coordinated agents, with different agents handling distinct tasks or stages. **(Source: OpenAI documentation)** [1.1]

OpenAI has recently released the **Agents SDK**, which helps developers build agents around models, tools, and instructions. It includes:

- **WebSearchTool** for web searches
- **Function tools**, created with `@function_tool`, for custom actions such as saving results to a database
- Support for **computer-use models** to interact with legacy systems through web or app interfaces
- **Agent orchestration**, allowing one agent to call or delegate to another

These tools support data retrieval, system actions, and multi-agent workflows. **(Source: OpenAI documentation)** [2.1][2.2]

## Case H13

Question: What was Lyft's revenue in 2022, and how has Lyft's revenue changed since then?
Must contain: Lyft 2022 revenue $4,095,135 thousand (about $4.1 billion)
Expected behavior: 2022 from the filing with citations; later trend from search results, no invented figures

### Answer A
- **2022 revenue — 10-K financial filings:** Lyft’s revenue was **$4.095 billion** (approximately **$4,095,135 thousand**).[1.1]

- **Change since 2022 — live internet search:** Lyft’s revenue has generally increased:
  - **2022:** approximately **$4.1 billion**
  - **2023:** approximately **$4.4 billion**
  - **2024:** approximately **$5.8 billion**
  - **Latest reported fiscal year:** approximately **$6.32 billion**, up **9.2% from 2024** [2.2]

Overall, that is roughly **54% growth from 2022 to the latest year**. Recent quarterly revenue was approximately **$1.68–$1.69 billion**, about **11% higher year over year** [2.1][2.4], although the growth rate has varied by quarter.

### Answer B
According to **10-K financial filings**, Lyft’s revenue in 2022 was **$4.095 billion** (or **$4,095,135 thousand**). [1.1]

According to **10-K financial filings**, Lyft’s revenue increased significantly compared with prior years:

- **2022:** $4.095 billion  
- **2021:** $3.208 billion, a **28% increase** from 2021  
- **2020:** $2.365 billion, meaning 2022 revenue was about **73% higher** than in 2020  

The 2022 growth was primarily driven by rising active riders and higher revenue per active rider as demand recovered from the COVID-19 pandemic. [2.1]