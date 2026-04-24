# Agent Skill Prompt Compiler

Convert Agent Skill folders into prompt documents for prompt-only LLM environments.

Many AI Agent workflows use Skills, but websites, apps, servers, and simple LLM API calls often cannot load a Skill folder directly.

This project helps you reuse the instruction layer of a Skill as a normal prompt document.

## What this project does

This repository contains two compiler skills:

| Skill | Purpose |
|---|---|
| `skill-core-prompt-compiler` | Converts a normal Skill into a prompt document |
| `skill-core-validator-prompt-compiler` | Converts a validator Skill into a prompt document for checking another Skill |

Basic idea:

```text
Skill folder → Prompt document
```

## Why this exists

A Skill may include:

- instructions
- input and output rules
- schemas
- examples
- validation rules
- failure conditions

But many environments only accept a prompt.

This project is a small bridge for that situation.

## What this project is not

This is not a new agent framework.

This is not a universal Skill standard.

This is not a replacement for tools, function calling, MCP, or real runtime execution.

It only helps convert the instruction and rule layer of a Skill into a prompt document.

## When to use it

Use this project when:

- you already have a Skill
- your target environment cannot load Skill folders directly
- you want to reuse Skill logic as a prompt
- you want to reduce manual copy-paste mistakes
- you care about keeping important rules, schemas, examples, and failure conditions

Do not use it as a replacement for code execution, file access, database access, API calls, or other runtime tools.

## Quickstart

See:

```text
QUICKSTART.md
```

## Project status

This is an early utility project for people building AI Agent workflows and trying to reuse Skills in prompt-only environments.
