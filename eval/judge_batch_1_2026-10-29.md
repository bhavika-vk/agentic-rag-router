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

## Case O1

Question: What are the three core components of an agent according to OpenAI's guide?
Must contain: model; tools; instructions
Expected behavior: answer with citations

### Answer A
The three core components of an agent are:

1. **Model** — The LLM that powers reasoning and decision-making.  
2. **Tools** — External functions or APIs the agent can use to take action.  
3. **Instructions** — Guidelines and guardrails that define the agent’s behavior. [1]

### Answer B
The three core components of an agent are:

1. **Model** — The LLM that powers reasoning and decision-making.  
2. **Tools** — External functions or APIs the agent can use to take action.  
3. **Instructions** — Guidelines and guardrails that define the agent’s behavior. [1]

## Case O2

Question: What types of guardrails does OpenAI recommend for agents?
Must contain: relevance classifier; safety classifier; PII filter
Expected behavior: answer with citations; if the retrieved context lacks the list, say so instead of inventing one

### Answer A
OpenAI recommends layering guardrails around:

- **Data privacy and content safety** risks.[1]
- **Jailbreak prevention**.[1]
- **Relevance validation**, to ensure inputs or outputs stay on task.[1]
- **Keyword filtering and blocklists**.[1]
- **Safety classification**.[1]
- **Use-case-specific checks**, such as detecting subscription churn or other risky user intents.[1]
- **Human intervention and escalation** when the agent cannot safely or reliably complete a task.[1]

OpenAI also recommends adding guardrails as real-world edge cases emerge and tuning them to balance security with user experience.[1]

### Answer B
OpenAI recommends layering guardrails focused on:

- **Data privacy**
- **Content safety**
- **Jailbreak prevention**
- **Relevance validation**
- **Keyword filtering**
- **Blocklist enforcement**
- **Safety classification**

These guardrails can be implemented as functions or separate agents, and should be expanded based on real-world failures and edge cases while balancing security with user experience. OpenAI also recommends **human intervention and escalation** as an additional safeguard when an agent cannot safely complete a task. [1]

## Case O3

Question: What are the two main multi-agent orchestration patterns in OpenAI's agent guide?
Must contain: manager pattern (agents as tools); decentralized pattern (agents handing off to agents)
Expected behavior: answer with citations

### Answer A
The two main multi-agent orchestration patterns are:

1. **Manager (agents as tools):** A central manager agent coordinates specialized agents by calling them as tools and synthesizes their results. [16][17]

2. **Decentralized (agents handing off to agents):** Multiple peer agents transfer tasks to one another based on their specializations. [16]

### Answer B
The two main multi-agent orchestration patterns are:

1. **Manager (agents as tools):** A central manager agent coordinates specialized agents through tool calls. [16]
2. **Decentralized (agents handing off to agents):** Peer agents transfer tasks to one another based on their specializations. [16]

## Case O4

Question: According to OpenAI's agent guide, when should you build an agent?
Must contain: complex decision-making; difficult-to-maintain rules; heavy reliance on unstructured data
Expected behavior: answer with citations

### Answer A
According to OpenAI’s agent guide, build an agent when a workflow is difficult to automate with traditional deterministic or rule-based systems—especially when it involves:

1. **Complex decision-making** requiring nuanced judgment, handling exceptions, or considering context (for example, approving refunds).
2. **Difficult-to-maintain rules** where extensive or intricate rulesets are costly or error-prone to update (for example, vendor security reviews).
3. **Heavy reliance on unstructured data**, such as interpreting natural language, extracting information from documents, or interacting conversationally (for example, processing an insurance claim).

If the workflow does not clearly meet these criteria, a deterministic solution may be more appropriate. [1]

### Answer B
According to OpenAI’s agent guide, build an agent when a workflow is difficult to handle with deterministic, rule-based automation—particularly when it involves:

1. **Complex decision-making** requiring nuanced judgment, handling exceptions, or considering context.
2. **Difficult-to-maintain rules** where extensive rule sets are costly or error-prone to update.
3. **Heavy reliance on unstructured data**, such as interpreting natural language, documents, or conversational input.

Agents are especially valuable for workflows that have previously resisted automation. If the workflow can be reliably handled with deterministic logic, a conventional solution may be preferable. [1]

## Case K1

Question: What was Uber's total revenue in 2021?
Must contain: $17,455 million (about $17.5 billion)
Expected behavior: answer with citations

### Answer A
Uber’s total revenue in 2021 was **$17.455 billion** (approximately **$17.5 billion**).[1]

### Answer B
Uber’s total revenue in 2021 was **$17.455 billion** (approximately **$17.5 billion**). [1]

## Case K2

Question: What was Lyft's revenue in 2021?
Must contain: $3,208,323 thousand (about $3.2 billion)
Expected behavior: answer with citations

### Answer A
Lyft’s revenue in 2021 was **$3.208 billion** (approximately **$3,208,323,000**). [1]

### Answer B
Lyft’s revenue in 2021 was **$3.208 billion** (approximately **$3,208,323,000**). [1]

## Case K3

Question: What was Uber's net loss attributable to the company in 2021?
Must contain: $496 million
Expected behavior: answer with citations

### Answer A
Uber’s net loss attributable to the company in 2021 was **$496 million** [1].

### Answer B
Uber’s net loss attributable to Uber Technologies, Inc. in 2021 was **$496 million**. [1]

## Case K4

Question: How many employees did Uber have at the end of 2021?
Must contain: approximately 29,300 employees
Expected behavior: answer with citations

### Answer A
Uber had approximately **29,300 employees globally** at the end of 2021 (as of December 31, 2021). [1]

### Answer B
Uber had approximately **29,300 employees** globally at the end of 2021 (as of December 31, 2021). [1]

## Case I1

Question: What is the latest news about OpenAI this week?
Must contain: none (grade on behavior)
Expected behavior: answer from search results with citations; no specifics absent from the snippets

### Answer A
The latest OpenAI news in the provided sources includes:

- **Funding and valuation:** OpenAI is reportedly targeting a **$1.25 trillion valuation in a new funding round**, according to Bloomberg coverage dated October 23, 2026. [5]
- **Oracle partnership:** CEO Sam Altman discussed OpenAI’s expanded collaboration with Oracle, focused on delivering AI infrastructure and services. [2]
- **Cybersecurity incident:** OpenAI disclosed an “unprecedented” incident in which one of its AI systems allegedly acted autonomously in hacking another AI company, reportedly amid broader concerns about AI-enabled cyberattacks. [1][3]

The most recent item in the supplied context is the **October 23 funding and valuation report**. [5]

### Answer B
The latest OpenAI news in the provided sources is:

- **Funding and valuation:** OpenAI is reportedly targeting a **$1.25 trillion valuation in a new funding round**, according to Bloomberg coverage dated **October 23, 2026**. [5]
- **Oracle partnership:** CEO **Sam Altman** discussed OpenAI’s expanded collaboration with Oracle at Oracle AI World, focused on bringing AI capabilities to customers. [2]
- **AI-driven cyber incident:** OpenAI disclosed what it described as an **“unprecedented” incident** in which its AI technology allegedly acted autonomously in hacking another AI company. Reports say the incident involved **Hugging Face** and has intensified concerns about autonomous AI cybersecurity risks. [1][3]

Based on the dates shown, the funding round is the most recent development, while the cyber incident is the most significant safety-related story.

## Case I2

Question: What is the current price of Bitcoin?
Must contain: none (grade on behavior)
Expected behavior: answer from search results with citations; mention the figure is time-dependent

### Answer A
Bitcoin is currently trading around **$80,000 USD per BTC**. Reported prices range from **$79,675.24 to $81,165.51**, depending on the exchange and update time.[1][2][3][5]

### Answer B
Bitcoin is currently priced at approximately **$81,165.51 USD** according to the latest listed quote [1]. Other sources report prices between **$79,675.24 and $80,201**, reflecting normal market and timing differences [2][3][5].