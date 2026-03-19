# AskCodi Agent Collections

**73 ready-to-use AI agent plugins** for [AskCodi](https://www.askcodi.com) — the OpenAI-compatible AI gateway that gives you specialized coding agents, access to 30+ frontier models, and works with every IDE and tool you already use.

[Sign up for AskCodi](https://www.askcodi.com) to start using these agents today.

---

## What is AskCodi?

AskCodi is an AI-powered development platform that goes beyond generic chat. Instead of one-size-fits-all AI, AskCodi gives you **specialized agents** — AI teammates with deep domain expertise in backend architecture, security, DevOps, payments, ML ops, and dozens more.

Every agent is accessible through a single **OpenAI-compatible API**, which means you can use them from VS Code, JetBrains, Cursor, Windsurf, Claude Code, the OpenAI SDK, or any tool that supports the Chat Completions or Responses API. One API key, every model, every agent.

### Why AskCodi?

- **Specialized agents** — Not just a chatbot. Each agent is purpose-built with domain knowledge, structured workflows, and best practices baked in.
- **30+ frontier models, one API** — Claude 4.6, GPT 5.4, Gemini 3, Grok 4, DeepSeek, and more. Switch between models without switching providers.
- **Free-tier models included** — Several models available at no cost (GPT 5 Mini, DeepSeek V3.2, Gemini 3 Flash, Kimi K2, and others).
- **Works everywhere** — OpenAI-compatible API means zero lock-in. Use it from any IDE, CLI tool, or SDK.
- **Build your own agents** — Create custom agents from scratch or from pre-built asset collections on the platform, then use them in any integration.
- **External tools & human-in-the-loop** — Pass your own tool definitions and the agent will call them, pausing execution for your client to handle the call and return results.

---

## Get Started

### 1. Sign up at [askcodi.com](https://www.askcodi.com)

Create your account and grab your API key from the dashboard.

### 2. Pick your integration

AskCodi works with any OpenAI-compatible client. Set the base URL to `https://api.askcodi.com/v1` and use your AskCodi API key.

#### VS Code / Cursor / Windsurf / JetBrains

Set the API base URL to `https://api.askcodi.com/v1` and use any agent or model name (e.g., `askcodi/backend-architect` or `anthropic/claude-sonnet-4-6`) in your AI assistant settings.

#### Claude Code

Configure AskCodi as an OpenAI-compatible provider with your API key and base URL.

#### Python (OpenAI SDK)

```python
from openai import OpenAI

client = OpenAI(
    api_key="your-askcodi-api-key",
    base_url="https://api.askcodi.com/v1"
)

response = client.chat.completions.create(
    model="askcodi/backend-architect",
    messages=[{"role": "user", "content": "Design a webhook ingestion service that handles 10k events/sec"}],
    stream=True
)

for chunk in response:
    print(chunk.choices[0].delta.content or "", end="")
```

#### TypeScript (Vercel AI SDK)

```typescript
import { createOpenAICompatible } from "@ai-sdk/openai-compatible";
import { generateText } from "ai";

const askcodi = createOpenAICompatible({
  name: "askcodi",
  baseURL: "https://api.askcodi.com/v1",
  apiKey: process.env.ASKCODI_API_KEY,
});

const { text } = await generateText({
  model: askcodi("askcodi/backend-architect"),
  prompt: "Design a rate limiting strategy for my API",
});
```

#### cURL (Chat Completions API)

```bash
curl https://api.askcodi.com/v1/chat/completions \
  -H "Authorization: Bearer $ASKCODI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "askcodi/backend-architect",
    "messages": [
      {"role": "user", "content": "Design a webhook ingestion service that handles 10k events/sec"}
    ],
    "stream": true
  }'
```

#### cURL (Responses API)

```bash
curl https://api.askcodi.com/v1/responses \
  -H "Authorization: Bearer $ASKCODI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "askcodi/backend-architect",
    "instructions": "You are helping design a new microservice.",
    "input": "What patterns should I use for inter-service communication?"
  }'
```

### 3. Use an agent

Address any agent as a model using the `askcodi/` prefix:

```text
askcodi/backend-architect
askcodi/ml-engineer
askcodi/ui-designer
askcodi/payment-integration
```

Or use any provider model directly:

```text
anthropic/claude-sonnet-4-6
openai/gpt-5.4
google/gemini-3-pro
xai/grok-4
deepseek/deepseek-v3.2
```

---

## Available Agent Collections

This repository contains **73 plugin collections** spanning every area of software development and beyond. Each plugin includes specialized agents, optional slash commands for multi-step workflows, and optional skills with deep domain knowledge.

| Category | Plugins |
| --- | --- |
| **Backend & APIs** | `api-scaffolding`, `backend-api-security`, `backend-development`, `api-testing-observability` |
| **Frontend & UI** | `frontend-mobile-development`, `frontend-mobile-security`, `ui-design`, `meigen-ai-design` |
| **DevOps & Infrastructure** | `cicd-automation`, `cloud-infrastructure`, `deployment-strategies`, `deployment-validation`, `kubernetes-operations` |
| **Data & ML** | `data-engineering`, `data-validation-suite`, `machine-learning-ops`, `business-analytics` |
| **Security** | `security-compliance`, `security-scanning`, `incident-response`, `reverse-engineering` |
| **Testing & Quality** | `tdd-workflows`, `unit-testing`, `performance-testing-review`, `comprehensive-review` |
| **Databases** | `database-design`, `database-migrations`, `database-cloud-optimization` |
| **Code Quality** | `code-documentation`, `code-refactoring`, `codebase-cleanup`, `documentation-generation` |
| **Languages** | `javascript-typescript`, `python-development`, `julia-development`, `jvm-languages`, `dotnet-contribution`, `systems-programming`, `functional-programming`, `shell-scripting` |
| **SEO & Marketing** | `seo-analysis-monitoring`, `seo-content-creation`, `seo-technical-optimization`, `content-marketing` |
| **Workflow** | `conductor`, `agent-orchestration`, `agent-teams`, `context-management`, `git-pr-workflows`, `team-collaboration` |
| **Debugging** | `debugging-toolkit`, `distributed-debugging`, `error-debugging`, `error-diagnostics` |
| **Specialized** | `blockchain-web3`, `game-development`, `payment-processing`, `quantitative-trading`, `arm-cortex-microcontrollers`, `llm-application-dev`, `multi-platform-apps`, `web-scripting` |
| **Business** | `startup-business-analyst`, `customer-sales-automation`, `hr-legal-compliance` |
| **Observability** | `observability-monitoring`, `application-performance`, `dependency-management` |
| **Architecture** | `c4-architecture`, `full-stack-orchestration`, `framework-migration` |
| **Essentials** | `developer-essentials` |

---

## How AskCodi Agents Work

An AskCodi agent is more than a system prompt. Each agent bundles:

- **Instructions** — A detailed persona with domain expertise, behavioral traits, and a structured approach to problem-solving
- **Skills** — Injected knowledge modules with working code examples, patterns, and best practices (e.g., Stripe integration, responsive design, WCAG compliance)
- **Commands** — Multi-step interactive workflows with checkpoints where the agent pauses for your approval before continuing (e.g., guided component creation, incident response runbooks, ML pipeline setup)
- **Tools** — Access to code execution, file system, shell, MCP servers, and your own custom tools via human-in-the-loop interrupts
- **Subagents** — Ability to delegate specialized subtasks to other agents

When you send a request to `askcodi/backend-architect`, the AskCodi gateway resolves the agent, assembles its full configuration (instructions + skills + tools + subagents), and streams the response back in OpenAI-compatible format.

---

## Create Your Own Agents

### Option 1: Build from Asset Collections on AskCodi

The fastest way to get started:

1. Sign in at [askcodi.com](https://www.askcodi.com)
2. Browse the agent marketplace for pre-built collections
3. Install a collection into your project with one click
4. Customize the agents, commands, and skills to fit your workflow
5. Your agents are immediately available via the API as `askcodi/<your-agent-slug>`

### Option 2: Create a Plugin Manually

Every plugin is just a folder of markdown files. No build step, no compilation — write markdown, get an agent.

#### Plugin structure

```text
plugins/my-plugin/
  .claude-plugin/
    plugin.json
  agents/
    my-agent.md
  commands/         (optional)
    my-command.md
  skills/           (optional)
    my-skill/
      SKILL.md
```

#### Step 1: Create `plugin.json`

```json
{
  "name": "my-plugin",
  "description": "What this plugin collection does",
  "version": "1.0.0",
  "author": { "name": "Your Name" },
  "license": "MIT"
}
```

#### Step 2: Write an agent

Create a markdown file in `agents/` with YAML frontmatter:

```markdown
---
name: my-agent
description: One-line description of what this agent does and when to use it.
model: inherit
---

You are a [role] specializing in [domain].

## Purpose

What this agent is an expert at.

## Capabilities

- Skill area 1
- Skill area 2

## Behavioral Traits

- How this agent approaches problems
- What principles it follows

## Response Approach

1. How it understands the request
2. How it analyzes the problem
3. How it delivers the solution
```

**Agent frontmatter fields:**

| Field         | Required | Description                                                  |
| ------------- | -------- | ------------------------------------------------------------ |
| `name`        | Yes      | Unique identifier (kebab-case)                               |
| `description` | Yes      | What the agent does and when to trigger it                   |
| `model`       | No       | `inherit` (default), `sonnet`, or `opus`                     |
| `color`       | No       | Terminal color: `cyan`, `magenta`, `green`, etc.             |
| `tools`       | No       | Comma-separated list of tools the agent can access           |

#### Step 3 (optional): Add commands

Create markdown files in `commands/` for interactive multi-step workflows:

```markdown
---
description: "What this command does"
argument-hint: "[required-arg] [--optional-flag]"
---

# Command Name

## Pre-flight Checks

Detect project setup, load context...

## Phase 1: Analysis

Do the work...

### Checkpoint

1. Approve - continue
2. Request changes
3. Pause

## Phase 2: Implementation

Continue based on approval...
```

#### Step 4 (optional): Add skills

Create `skills/<skill-name>/SKILL.md` to give your agent deep domain knowledge:

```markdown
---
name: my-skill
description: What knowledge this skill provides and when to use it.
---

# Skill Name

## When to Use This Skill

- Scenario 1
- Scenario 2

## Key Patterns

### Pattern Name

\```python
# Working code example
\```

## Best Practices

1. Practice 1
2. Practice 2
```

#### Step 5: Use it

Once your plugin is in the collections and associated with your project, the agent is available via the API:

```text
askcodi/my-agent
```

Use it from any OpenAI-compatible integration — your IDE, CLI, SDK, or custom application.

---

## Supported Models

AskCodi gives you access to 30+ frontier models from every major provider through a single API key:

| Provider       | Models                                                                             |
| -------------- | ---------------------------------------------------------------------------------- |
| **Anthropic**  | Claude Opus 4.6, Claude Sonnet 4.6, Claude Opus 4.5, Claude Sonnet 4.5, Claude Haiku 4.5 |
| **OpenAI**     | GPT 5.4, GPT 5.3 Codex, GPT 5.2, GPT 5.1 Codex Max, GPT 5 Mini, GPT 5 Nano     |
| **Google**     | Gemini 3 Pro, Gemini 3 Flash, Gemini 2.5 Pro, Gemini 2.5 Flash                   |
| **xAI**        | Grok 4, Grok 4.20, Grok 4.20 Multi Agent, Grok Code Fast 1                       |
| **DeepSeek**   | DeepSeek V3.2                                                                      |
| **MoonShot AI**| Kimi K2 Thinking, Kimi K2.5                                                        |
| **Zhipu AI**   | GLM 4.6, GLM 4.7                                                                  |
| **MiniMax**    | MiniMax M2.5, M2.7                                                                 |

Many models have **free-tier variants** — get started without spending a cent.

---

## Start Building

1. **[Sign up for AskCodi](https://www.askcodi.com)** — Create your account and get an API key
2. **Connect your IDE** — Point any OpenAI-compatible tool at `https://api.askcodi.com/v1`
3. **Pick an agent** — Use any of the 73 collections in this repo, or build your own
4. **Ship faster** — Let specialized agents handle the domain complexity while you focus on building

---

## License

Apache-2.0 — see [LICENSE](./LICENSE).
