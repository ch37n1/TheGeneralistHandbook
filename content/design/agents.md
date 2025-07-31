These principles define a comprehensive approach to designing modular, reasoning-capable, and composable AI applications. They are built on practical patterns like **ReMVP (Recursive Model-View-Presenter)**, the **Agent Context Protocol (ACP)**, and **Function-based composition**. Inspired by DDD and layered architecture, this methodology is tailored for the demands of modern LLM and agent-based systems.

> AI Systems Development Principles emphasize:
> - Structured reasoning as a first-class concern
> - Interoperable agents and subfunctions
> - Stateless, schema-driven communication
> - Recursive, modular function architecture
> - Clear separation of responsibilities

### Key concepts
* No life without observability.
	* Phoenix
	* Grafana
	* Langfuse
* No life without quality control.
	* DoD tests for increment.
	* Benchmarks for ML.
	* Modules for scripts.
### Function

Functions in AI systems vary in complexity and cognitive depth. Use clear classifications to guide their architecture:

|Type|Description|
|---|---|
|**Tool Functions**|Stateless utilities that fetch data, run calculations, or manipulate inputs. They are often wrappers over APIs, scripts, or legacy logic.|
|**LLM Workflow Functions**|Structured, task-specific functions involving reasoning and multiple LLM steps (e.g., search → summarize → evaluate). Usually require formatting, prompt design, and orchestration.|
|**Agentic Functions**|Sub-agents with their own goals, internal workflows, reasoning loops, and sometimes memory/state. They can respond directly to the user or collaborate with other agents. Often implemented as nested ReMVP modules.|
> Each function larger that script should follow the ReMVP structure internally when complex enough. Tool functions may remain flat.

### Interaction Patterns

AI systems must support both synchronous and asynchronous communication, depending on function latency, complexity, and user context.

#### Supported Modes

| Interaction Type | Use When…                                      | Recommended Transport                  |
| ---------------- | ---------------------------------------------- | -------------------------------------- |
| **Synchronous**  | When we need simplicity or task is fast        | REST, Streaming REST                   |
| **Asynchronous** | Long-running, multi-stage, or background tasks | AsyncAPI, TaskAPI, Futures, WebSockets |

> Even async responses must conform to the **ACP** format for consistency.

### Patterns
- Use **ACP (Agent Context Protocol)** for all agent-to-agent and agent-to-function communication. (Check [[structure]]).
- Use **ReMVP (Recursive Model-View-Presenter)** as an architectural pattern designed specifically for modular, composable, AI-driven applications.
- Use DDD and layered architecture additional to ReMVP. (Check [[сode]])

### Reasoning Layer

LLM reasoning is not just a prompt — it's a pattern.
Encapsulate your reasoning logic as part of the **Presenter** or in a dedicated **Reasoning Layer**, which may include:
- Prompt templates (with rules, examples)
- Prompt chaining logic
- Dynamic prompt building (based on context)
- Output interpretation logic (e.g., extract structured results from text)

> Use external tools or reuse your prompt logic across functions using a common library or formatter module.

### Prompts as Functions

Treat prompts as **first-class code**. Each complex prompt or chain should behave like a function:
- Input: structured `params` / `query` / `dialogue`
- Output: `text`, `data`, or both
- Reusability: isolated, documented, testable
### Composition and Nesting

AI systems are **compositional**: each function can contain or call other functions that can be agent. Architect for nesting:
- Build agentic functions with clear interfaces
- Let orchestrators handle recursive calls
- Maintain consistent context propagation (caller, trust, params)
### Design Recommendations
- Design for **modularity first**: agents, tools, workflows, formatters, and reasoning logic should be plug-and-play
### Prioritized Reasoning
- Which function's response appears in prompt first
- How to order or combine outputs from multiple agents
- How to re-prompt or retry if needed

> Consider using scoring, trust levels, or confidence indicators in orchestration logic.
