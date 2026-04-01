# Contributing to Atmosphere Skills

## Adding a New Skill

### 1. Create the skill file

```
skills/
  your-skill/
    SKILL.md
```

The file must include YAML frontmatter with at least `name` and `description`:

```markdown
---
name: your-skill
description: "What this agent does, in one sentence"
category: general
tags: keyword1, keyword2
---

# Your Agent Name

System prompt instructions go here.

## Skills
- Capability one
- Capability two

## Guardrails
- Safety constraint one
```

### 2. Update the registry

Add an entry to `registry.json`:

```json
{
  "id": "your-skill",
  "name": "Your Skill Display Name",
  "description": "Same description as the frontmatter",
  "category": "general",
  "tags": ["keyword1", "keyword2"],
  "path": "skills/your-skill/SKILL.md",
  "atmosphere_sample": "spring-boot-your-sample"
}
```

The `atmosphere_sample` field links to a runnable sample in the [main repository](https://github.com/Atmosphere/atmosphere/tree/main/samples). If your skill does not have a corresponding sample yet, set this to `null`.

### 3. Open a PR

Include a brief description of what the agent does and how to test it.

## Skill Writing Guidelines

**Be specific.** A good skill file tells the LLM exactly what role it plays, what tools it has, and what it should never do. Vague instructions produce vague agents.

**Declare tools explicitly.** If the agent has backend tools (via `@AiTool`), list them in a `## Tools` section with a one-line description of each. This helps the LLM decide when to call them.

**Set guardrails.** Every skill should have a `## Guardrails` section. Common patterns:
- Scope limits ("Only answer questions about X")
- Safety boundaries ("Never prescribe medication")
- Honesty rules ("Say so when you don't know")
- Escalation triggers ("Direct to a human when Y happens")

**Keep it readable.** The skill file is both a system prompt and documentation. Write it so a developer can read the file and understand what the agent does without running it.

## Conventions

- Directory names use kebab-case: `dental-assistant`, not `DentalAssistant`
- One `SKILL.md` per directory
- Categories: `general`, `healthcare`, `business`, `education`, `tools`, `rag`, `agent`, `mcp`, `observability`, `infrastructure`
- Skill files are Apache 2.0 licensed unless otherwise noted in the frontmatter

## Testing

If your skill has a corresponding sample, verify it runs:

```bash
atmosphere run spring-boot-your-sample
```

If it requires an API key:

```bash
atmosphere run spring-boot-your-sample --env GEMINI_API_KEY=your-key
```
