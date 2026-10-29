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

## Case I3

Question: What is the weather forecast in Los Angeles today?
Must contain: none (grade on behavior)
Expected behavior: answer from search results with citations

### Answer A
Today in Los Angeles, expect **mostly sunny skies with a high around 74°F**. Winds will be about **10 mph from the south-southwest**, with humidity near 62% [2]. Conditions may become partly to mostly cloudy this evening, with temperatures falling into the upper 60s [5]. Current conditions feel pleasant at about **65°F** [1].

### Answer B
Today in Los Angeles, expect **mostly sunny to fair conditions**, with a high around **74–89°F** depending on location, and a low near **68°F**. Rain chances are near **0%**. Winds will be around **10 mph from the south-southwest**. [2][3]

## Case I4

Question: How do Anthropic's Claude models compare with Google's Gemini models?
Must contain: none (grade on behavior)
Expected behavior: answer from search results with citations

### Answer A
Anthropic’s **Claude** and Google’s **Gemini** are both capable general-purpose AI models, but they tend to have different strengths:

- **Coding and code explanations:** Claude is often preferred for programming, particularly when users want readable, well-structured explanations aimed at beginners. One comparison found Claude more approachable when explaining complex code, while Gemini remained competitive for coding tasks overall.[1] User reports also commonly favor Claude’s coding quality and chat experience, though this is subjective and may come at a higher subscription cost.[3]

- **Large-context workloads:** Gemini generally offers more room for source material in a single request, which can be useful for analyzing long documents, large codebases, or extensive retrieved context. Claude’s context window may be smaller in some comparisons, but it is still sufficient for many retrieval-augmented applications.[4]

- **Speed and cost:** Both providers offer larger, more capable models alongside lighter and cheaper variants. Gemini’s Flash-Lite models are specifically noted as lower-cost, faster options, while Claude also has lighter models intended to reduce latency and expense.[2]

- **Marketing and general business tasks:** Neither model dominates every task. In one marketing-focused test, Claude won 5 of 10 tasks, Gemini won 3, and the two tied on 2, suggesting that performance depends substantially on the specific use case and prompt.[5]

**Overall:** Choose **Claude** if your priority is coding assistance, nuanced writing, or clear explanations. Choose **Gemini** if you need very large input contexts, Google-oriented workflows, or potentially faster and more economical model options. For production use, the best choice depends on the exact model tier, pricing, context requirements, latency, and evaluation results for your own workload.

### Answer B
Anthropic’s **Claude** and Google’s **Gemini** are both capable general-purpose AI models, but they tend to excel in different areas:

- **Coding and code explanations:** Claude is often preferred for programming, especially when producing readable code or explaining complex code to beginners. One comparison found Claude had a clearer, more approachable style, while user reports also favored Claude for coding and conversational quality.[1][3]
- **Large-context workloads:** Gemini generally offers more room for source material in a single request, making it attractive for analyzing very long documents, codebases, or large retrieval-augmented prompts. Claude’s context window may be smaller, though it can still be sufficient for many applications.[4]
- **Cost and speed:** Both providers offer larger, more capable models alongside faster and cheaper variants. Gemini’s lightweight offerings, such as Flash-Lite, are positioned for lower-cost and faster workloads; Claude’s pricing can be perceived as relatively expensive depending on the model and subscription.[2][3]
- **Marketing and general business tasks:** Results are fairly close and task-dependent. In one test, Claude won five of ten marketing tasks, Gemini won three, and two were ties—suggesting no universal winner.[5]
- **Overall experience:** Claude may be the better choice for careful writing, coding assistance, and explanations. Gemini may be preferable when long context, speed, cost efficiency, or handling large volumes of material is more important.

In short: **choose Claude for coding quality, thoughtful explanations, and writing; choose Gemini for very large inputs, faster or cheaper processing, and Google-centered workflows.** Performance varies substantially by the specific model tier and task, so these are tendencies rather than absolute rules.

## Case M1

Question: What was Lyft's net loss in 2021 and what was Uber's revenue in 2021?
Must contain: Lyft 2021 net loss about $1.0 billion ($1,009,359 thousand); Uber 2021 revenue $17,455 million (about $17.5 billion)
Expected behavior: one combined answer covering both parts with per-part citations

### Answer A
- **Lyft’s net loss in 2021** was **$1.009 billion** (or **$1,009,359 thousand**), according to its **10-K financial filings**.[1.1]
- **Uber’s revenue in 2021** was **$17.455 billion** (approximately **$17.5 billion**), according to its **10-K financial filings**.[2.1]

### Answer B
- **Lyft’s net loss in 2021** was **$1.009 billion** (approximately **$1,009,359,000**), according to its **10-K financial filings**.[1.1]
- **Uber’s revenue in 2021** was **$17.455 billion** (approximately **$17.5 billion**), according to its **10-K financial filings**.[2.1]

## Case M2

Question: What are the three core components of an agent, and what types of tools do agents need?
Must contain: model, tools, instructions; data, action, and orchestration tools
Expected behavior: one combined answer covering both parts with per-part citations

### Answer A
### Three core components of an agent — OpenAI documentation

1. **Model** — The LLM that powers the agent’s reasoning and decision-making.
2. **Tools** — External functions or APIs the agent uses to retrieve information and take actions.
3. **Instructions** — Explicit guidelines and guardrails that define how the agent behaves. [1.1]

### Types of tools agents need — OpenAI documentation

1. **Data tools** — Retrieve information and context, such as querying databases or CRMs, reading documents, or searching the web.
2. **Action tools** — Make changes or carry out tasks, such as sending messages, updating records, or handing off support tickets.
3. **Orchestration tools** — Let agents call or coordinate other specialized agents, such as research, refund, or writing agents. [2.1]

### Answer B
According to **OpenAI documentation**, the three core components of an agent are:

1. **Model** — The LLM that powers the agent’s reasoning and decision-making.  
2. **Tools** — External functions or APIs used to retrieve information or take actions.  
3. **Instructions** — Guidelines and guardrails that define the agent’s behavior. [1.1]

According to **OpenAI documentation**, agents generally need three types of tools:

1. **Data tools** — Retrieve information and context, such as querying databases or CRMs, reading PDFs, or searching the web.  
2. **Action tools** — Make changes or take actions in external systems, such as sending messages, updating CRM records, or handing off support tickets.  
3. **Orchestration tools** — Call or coordinate with other specialized agents, such as research, writing, or refund agents. [2.1]

## Case M3

Question: What is the newest iPhone model and what is the current price of gold?
Must contain: none (grade on behavior)
Expected behavior: one combined answer covering both parts from search results with citations

### Answer A
- **Newest iPhone model — live internet search:** The newest iPhone model is the **iPhone 18 Pro Max**, part of Apple’s iPhone 18 lineup. [1.1][1.4]
- **Current gold price — live internet search:** Gold is approximately **$4,054 USD per troy ounce**. Recent quoted prices range from about **$4,050.45 to $4,056.77 per ounce**. [2.1][2.2][2.3][2.5]

### Answer B
- **Newest iPhone model — live internet search:** The newest iPhone generation is the **iPhone 18 series**, including the **iPhone 18 Pro and iPhone 18 Pro Max**. The highest-end model currently listed is the **iPhone 18 Pro Max**. [1.1][1.4]

- **Current gold price — live internet search:** Gold is approximately **$4,050 per troy ounce**. Recent reported prices include **$4,050.45/oz** [2.2], **$4,053.50/oz** [2.5], and **$4,056.77/oz** [2.3].

## Case D1

Question: What was Uber's revenue in 2021 and what are the newest LLMs?
Must contain: Uber 2021 revenue $17,455 million (about $17.5 billion)
Expected behavior: one combined answer; each part names its source; LLM part based on search results

### Answer A
### Uber revenue in 2021 — 10-K financial filings
Uber’s revenue in 2021 was **$17.455 billion** (approximately **$17.5 billion**). [1.1]

### Newest large language models — live internet search
The provided sources do not discuss large language models, so none can support a definitive answer about the newest LLMs.

Major recent LLM families include:

- **OpenAI:** GPT-4o and newer GPT-series models
- **Anthropic:** Claude 3.5/3.7 Sonnet and Claude 3 Opus
- **Google:** Gemini 1.5 and later Gemini models
- **Meta:** Llama 3 and Llama 3.1
- **xAI:** Grok models
- **DeepSeek:** DeepSeek-V3 and DeepSeek-R1
- **Alibaba:** Qwen2.5 and newer Qwen models
- **Mistral:** Mistral Large and Mixtral models

Because releases occur frequently, identifying the absolute newest LLM requires checking providers’ current announcement pages or a live model leaderboard.

### Answer B
### Uber’s 2021 revenue — Source: 10-K financial filings

Uber’s revenue in 2021 was **$17.455 billion**, approximately **$17.5 billion**. [1.1]

### Newest large language models — Source: live internet search

“Newest” changes quickly and depends on whether you mean commercial, open-weight, or reasoning models. Notable leading LLM families include:

- **OpenAI GPT‑5** — general-purpose and reasoning capabilities.
- **Anthropic Claude 4 / Claude Opus 4.1** — coding, analysis, and long-context performance.
- **Google Gemini 2.5 Pro and Flash** — multimodal models with advanced reasoning.
- **Meta Llama 4** — open-weight models such as Scout and Maverick.
- **DeepSeek V3 and R1** — reasoning and coding capabilities.
- **Alibaba Qwen3** — open and commercial models with multilingual and reasoning performance.
- **Mistral Medium 3 and related models** — efficient commercial and open-weight alternatives.

There is no single best newest model; the choice depends on factors such as reasoning, coding, multimodal input, speed, cost, context length, and whether open weights are required.

## Case D2

Question: When should an agent escalate to a human according to OpenAI's guide, and how many employees did Uber have in 2021?
Must contain: exceeding failure thresholds and high-risk actions; approximately 29,300 employees
Expected behavior: one combined answer; each part names its source

### Answer A
According to **OpenAI documentation**, an agent should escalate to a human when it exceeds defined failure thresholds—such as multiple failed attempts to understand the user—or before taking a high-risk action, particularly one that is sensitive, irreversible, or high-stakes, such as canceling an order, authorizing a large refund, or making a payment.[1.1]

According to **10-K financial filings**, Uber had approximately **29,300 employees globally** as of December 31, 2021.[2.1]

### Answer B
According to **OpenAI’s documentation**, an agent should escalate to a human when it exceeds predefined failure thresholds, such as repeatedly failing to understand the user’s intent, or before taking a high-risk action—particularly one that is sensitive, irreversible, or high-stakes, such as canceling an order, authorizing a large refund, or making a payment. [1.1]

According to **Uber’s 10-K financial filings**, Uber had approximately **29,300 employees globally** as of December 31, 2021. [2.1]

## Case D3

Question: What is a handoff between agents in OpenAI's guide, and what AI agent products were launched recently?
Must contain: a handoff transfers control (execution) of the conversation from one agent to another
Expected behavior: one combined answer; each part names its source

### Answer A
## Handoff between agents — OpenAI documentation

In OpenAI’s guide, a **handoff** is a one-way transfer of workflow control from one agent to another. It is implemented as a tool or function: when an agent calls it, execution immediately begins with the new agent using the latest conversation state.

For example, a triage agent can hand off a customer’s order question to an order-management agent. The receiving agent can optionally hand control back if needed. [1.1]

## Recently launched AI-agent products — live internet search

Recently launched or announced AI-agent products and platforms include:

- **Azure AI Foundry**, Microsoft’s agent-development platform with native MCP support. [2.1]
- **Copilot in VS Code as an open-source agent**, Microsoft’s coding-agent initiative. [2.1]
- **Microsoft Agent Store**, a marketplace for discovering and deploying agents. [2.1]
- **Moli, ManyAgents, AI Bridge, Strawberry, and VibeiDE**, projects highlighted among recent AI-agent launches. [2.5]
- **Industry-focused agents from Klarna, OpenTable, GitHub, and Uber**, supporting customer service, reservations, software development, and transportation use cases. [2.2]

The sources also mention updates from **Cursor, LlamaIndex, Moonshot, OpenAI, Google, Taskade, AgentOps, and Anthropic**, but do not provide enough detail to identify all of them as distinct product launches. [2.3]

### Answer B
### Handoff between agents — OpenAI documentation

A handoff is a one-way transfer of workflow control from one agent to another. In OpenAI’s Agents SDK, it is implemented as a tool or function: when invoked, execution immediately switches to the receiving agent along with the latest conversation state. This allows a specialized agent to take over without the original agent remaining involved. [1.1]

### Recently launched AI agent products — live internet search

Recently mentioned AI agent products and projects include:

- **Azure AI Foundry**, including its MCP-native capabilities.
- **GitHub Copilot in VS Code**, described as an open-source agent.
- **Microsoft Agent Store**, including first-party MCP support. [2.1]
- AI-agent applications from **Klarna, OpenTable, GitHub, and Uber**, highlighted as products launched or expanded in May 2025. [2.2]
- **VibeiDE, Strawberry, AI Bridge, ManyAgents, and Moli**. [2.5]

The source also mentions recent updates from **Cursor, LlamaIndex, Moonshot, OpenAI, Google, Taskade, AgentOps, Anthropic, and LangChain**, but does not specify individual product names for each. [2.3]

## Case D4

Question: What are the core components of an agent, what was Lyft's net loss in 2021, and what is today's top technology news?
Must contain: model, tools, instructions; Lyft 2021 net loss about $1.0 billion ($1,009,359 thousand)
Expected behavior: one combined answer covering all three parts; each part names its source

### Answer A
### Core components of an agent — OpenAI documentation
1. **Model** – The LLM that powers the agent’s reasoning and decision-making.  
2. **Tools** – External functions or APIs the agent can use to take actions.  
3. **Instructions** – Explicit guidelines and guardrails that define how the agent should behave. [1.1]

### Lyft’s 2021 net loss — 10-K financial filings
Lyft’s net loss in 2021 was **$1.009 billion**, or **$1,009,359 thousand**. [2.1]

### Today’s top technology news — Live internet search
Based on the provided sources, today’s leading technology stories include:

- **Samsung’s profits are surging as AI demand boosts chip sales**, underscoring the strength of the AI hardware market. [3.2]
- **The FTC is requiring Walmart, Delta, and other companies to explain how they use personal data to set prices**, increasing scrutiny of algorithmic and personalized pricing. [3.2]
- **AI debt concerns are weighing on technology stocks**, despite strong performance earlier in the week. [3.4]
- **SpaceX reportedly posted strong AI-related revenue**, adding to the company’s expanding technology business. [3.4]
- **Google released a new AI-powered camera update**, reportedly without requiring a new Pixel 9 device. [3.5]
- **Dating app Breeze launched a sign-language matching feature**, a notable accessibility-focused product update. [3.5]

Together, these stories reflect broader themes of booming AI infrastructure demand, increased regulation of data use, financial concerns around AI investment, and more accessible consumer technology.

### Answer B
### Core components of an agent — OpenAI documentation
The three core components of an agent are:

1. **Model** — The LLM powering the agent’s reasoning, decision-making, and workflow management.  
2. **Tools** — External functions, APIs, or systems the agent can use to gather information or take actions.  
3. **Instructions** — Guidelines, routines, and guardrails defining how the agent should behave.  

Together, these enable an agent to complete tasks independently on a user’s behalf. [1.1]

### Lyft’s 2021 net loss — 10-K financial filings
Lyft reported a **net loss of $1.009 billion in 2021**, specifically **$1,009,359,000**. [2.1]

### Today’s top technology news — live internet search
Leading technology stories include:

- **Samsung’s profits are surging**, driven by strong demand for AI-related chips. [3.2]
- **The FTC is investigating data-driven pricing practices**, asking Walmart, Delta, and other companies to explain how they use personal data to set prices. [3.2]
- **Concerns about AI investment are affecting markets**, with Nasdaq performance weighed down by worries about AI-related debt. [3.4]
- **SpaceX is reporting strong AI revenue**, highlighting growing interest in AI’s commercial potential. [3.4]
- **Google has released a new AI-powered camera update** that does not require a new Pixel 9 device. [3.5]

TechRadar and CNET are also tracking the latest technology, gadget, and product developments. [3.1][3.3]

## Case A1

Question: What is an agent?
Must contain: none (grade on behavior)
Expected behavior: a sensible definition from whichever source is chosen

### Answer A
An agent is an LLM-powered system that independently accomplishes tasks on a user’s behalf. It manages and executes workflows, makes decisions, uses tools such as APIs or web searches, recognizes when a task is complete, and can correct mistakes or hand control back to the user when necessary. Simple chatbots, single-turn LLM applications, and classifiers are not agents if the LLM does not control workflow execution. [1]

### Answer B
An agent is an LLM-powered system that independently accomplishes tasks on a user’s behalf. It manages a workflow, makes decisions, uses tools or APIs when needed, recognizes when the task is complete, and can correct mistakes or hand control back to the user if it cannot proceed.[1]

Simple chatbots, single-turn LLM applications, and classifiers are not agents unless the LLM controls the workflow’s execution.[1]