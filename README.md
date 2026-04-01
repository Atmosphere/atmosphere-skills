# Atmosphere Skills

Curated agent skill files for the [Atmosphere Framework](https://github.com/Atmosphere/atmosphere).

A skill file is a Markdown document that defines an AI agent's personality, capabilities, tools, and guardrails. The Atmosphere CLI pairs each skill with a runnable sample application that delivers it over WebSocket, SSE, gRPC, MCP, A2A, or any supported transport.

## Quick Start

```bash
# Install the CLI
brew install Atmosphere/tap/atmosphere

# Browse available samples interactively
atmosphere install

# Run a sample directly (the skill file is embedded in the sample)
atmosphere run spring-boot-dentist-agent
atmosphere run spring-boot-ai-tools --env GEMINI_API_KEY=your-key
```

Each sample reads its skill file from `src/main/resources/prompts/` at startup.
The `@AiEndpoint` annotation wires the skill to a real-time streaming endpoint automatically.

## Available Skills

| Skill | Sample | Category | Description |
|-------|--------|----------|-------------|
| [dentist-agent](skills/dentist-agent/) | `spring-boot-dentist-agent` | Healthcare | Emergency dental assistant with triage, first aid, and multi-channel delivery |
| [startup-ceo](skills/startup-ceo/) | `spring-boot-multi-agent-startup-team` | Business | CEO coordinator that dispatches to 4 specialist agents via A2A |
| [ai-assistant](skills/ai-assistant/) | `spring-boot-ai-chat` | General | Streaming chat assistant with conversation memory |
| [classroom](skills/classroom/) | `spring-boot-ai-classroom` | Education | Multi-room classroom where all students see AI responses simultaneously |
| [tool-assistant](skills/tool-assistant/) | `spring-boot-ai-tools` | Tools | Tool-calling pipeline with cost metering and approval workflows |
| [rag-assistant](skills/rag-assistant/) | `spring-boot-rag-chat` | RAG | Knowledge base assistant with document retrieval and citation |

## How Skills Work

A skill file has two parts: **YAML frontmatter** declaring metadata and a **Markdown body** containing the agent's system prompt.

```markdown
---
name: my-agent
description: "Brief description for search and discovery"
category: general
tags: chat, streaming
---

# My Agent

You are a helpful assistant that...

## Skills
- Capability one
- Capability two

## Tools
- tool_name: What this tool does

## Guardrails
- Never do X without confirmation
```

### Frontmatter Fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Identifier used by the CLI and registry |
| `description` | Yes | One-line summary. Drives search and auto-triggering |
| `category` | No | Grouping: `general`, `healthcare`, `business`, `education`, `tools`, `rag` |
| `tags` | No | Comma-separated keywords for filtering |

### Body Sections

The Markdown body is the system prompt sent to the LLM at the start of every conversation. The following sections are conventions, not hard requirements:

| Section | Purpose |
|---------|---------|
| **Skills** | What the agent can do. Helps the LLM scope its behavior |
| **Tools** | Backend tools the agent can invoke via `@AiTool` methods |
| **Channels** | Delivery channels the agent supports (web, Slack, Telegram) |
| **Guardrails** | Safety constraints and behavioral boundaries |

### From Skill File to Running Agent

The skill file is the agent's brain. The Atmosphere sample is the body:

```
SKILL.md (personality + tools + guardrails)
    |
    v
@AiEndpoint(systemPromptResource = "prompts/dentist-skill.md",
            tools = DentalTools.class)
    |
    v
Atmosphere Runtime (WebSocket / SSE / gRPC / MCP / A2A)
    |
    v
React frontend (atmosphere.js/chat components)
```

The `@AiEndpoint` annotation reads the skill file, registers the declared tools as callable functions, and exposes everything over the configured transport. Your application code declares **what** the agent does -- the framework handles **how** it is delivered.

## Ecosystem Compatibility

The skill file format follows the [Agent Skills](https://agentskills.io/specification) convention used across the AI agent ecosystem. Skill files from other repositories can serve as inspiration for Atmosphere agents:

- [Anthropic Skills](https://github.com/anthropics/skills) -- Claude Code community skills
- [Antigravity](https://github.com/sickn33/antigravity-awesome-skills) -- Cross-platform agent skills

The key difference: Atmosphere skills are backed by a **server-side runtime** with real transport, streaming, tool execution, and multi-channel delivery. The skill file is the starting point, not the whole story.

## Contributing

Add your skill to `skills/<your-skill>/SKILL.md`, update `registry.json`, and open a PR.

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full guide.

## License

Apache 2.0
