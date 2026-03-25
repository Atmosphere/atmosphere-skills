# Atmosphere Skills

A curated collection of agent skill files for the [Atmosphere Framework](https://github.com/Atmosphere/atmosphere).

Each skill is a Markdown file that becomes a running AI agent — with WebSocket streaming, MCP, A2A, and multi-channel delivery — in one command.

## Quick Start

```bash
# Install the CLI
brew install Atmosphere/tap/atmosphere

# Run a skill from this registry
atmosphere skills run dentist-agent

# Import any skill from GitHub
atmosphere import https://raw.githubusercontent.com/Atmosphere/atmosphere-skills/main/skills/startup-ceo/SKILL.md

# Import from other ecosystems (Anthropic, Antigravity, etc.)
atmosphere import https://raw.githubusercontent.com/anthropics/skills/main/skills/frontend-design/SKILL.md
```

## Available Skills

| Skill | Category | Description |
|-------|----------|-------------|
| [dentist-agent](skills/dentist-agent/) | Healthcare | Emergency dental assistant — broken teeth, first aid, and triage |
| [startup-ceo](skills/startup-ceo/) | Business | CEO agent that orchestrates a multi-agent startup advisory team via A2A |
| [ai-assistant](skills/ai-assistant/) | General | General-purpose AI streaming chat assistant |
| [classroom](skills/classroom/) | Education | Multi-room AI classroom with subject-specific personas |
| [tool-assistant](skills/tool-assistant/) | Tools | AI assistant with framework-agnostic tool calling |
| [rag-assistant](skills/rag-assistant/) | RAG | RAG-powered assistant with document retrieval and embeddings |

## Skill File Format

Skills use the [Agent Skills](https://agentskills.io/specification) format — YAML frontmatter + Markdown body:

```markdown
---
name: my-agent
description: "What this agent does"
---

# My Agent
You are a helpful assistant that...

## Skills
- Skill one
- Skill two

## Tools
- tool_name: Description of what it does

## Guardrails
- Never do X without confirmation
```

Compatible with [Anthropic Skills](https://github.com/anthropics/skills), [Antigravity](https://github.com/sickn33/antigravity-awesome-skills), and any agent skills repository.

## Contributing

PRs welcome! Add your skill file to `skills/<your-skill>/SKILL.md` and update `registry.json`.

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

Apache 2.0 — Skills contributed under the same license unless otherwise noted.
