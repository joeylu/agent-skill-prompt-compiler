# Agent Skill Prompt Compiler

Convert Agent Skills into prompt documents for environments that do not support skills.

## Why this exists

Many AI Agent workflows use **Skills** to describe how an AI should perform a task.

A Skill may include instructions, input and output rules, schemas, examples, validation rules, and failure conditions.

But many real-world environments do not support loading a Skill folder directly. For example, a website backend, an app, a simple LLM API call, or a platform that only accepts a system prompt.

This project helps convert a Skill into a prompt document that can be used in those environments.

## What this project does

This repository contains two core skills:

| Skill | Purpose |
|---|---|
| `skill-core-prompt-compiler` | Converts a normal Skill into a prompt document |
| `skill-core-validator-prompt-compiler` | Converts a validator Skill into a prompt document for checking another Skill |

In simple terms:

```text
Skill folder -> Prompt document
```

## What this project is not

This is not a new agent framework.

This is not a universal Skill standard.

This is not a replacement for tools, function calling, MCP, or real runtime execution.

It is only a practical bridge for cases where you have a Skill, but your target environment can only receive a prompt.

## When to use it

Use this project when:

- you already have a Skill
- your target environment cannot load Skills directly
- you want to reuse the Skill logic as a prompt
- you want to reduce manual copy-paste mistakes
- you care about keeping important rules, schemas, and failure conditions

Do not use it when your Skill depends on real code execution, file access, database access, or API calls unless those parts are handled by another runtime.

## Basic workflow

```text
1. Prepare a Skill folder
2. Run the prompt compiler Skill
3. Generate a prompt document
4. Use that prompt in your website, app, server, or LLM call
```

## Fail-fast principle

This project prefers to stop instead of producing an unsafe prompt.

If important rules are missing, unclear, or cannot be safely carried into the prompt, the compiler should report the problem instead of pretending everything is fine.

## Project status

This is an early utility project for people building Agent workflows and trying to reuse Skills in prompt-only environments.
