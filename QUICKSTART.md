# Quickstart

## Note

This project only converts the instruction layer of a Skill.

It does not replace code execution, file access, database access, API calls, or other runtime tools.

```text
Skill folder → Prompt document
```

Use the generated prompt in environments that do not support Skills directly, such as websites, apps, servers, or simple LLM API calls.

## 1. Choose a compiler

For normal Skills:

```text
skill-core-prompt-compiler
```

For validator Skills:

```text
skill-core-validator-prompt-compiler
```

## 2. Prepare a Skill

A normal Skill usually looks like this:

```text
your-skill/
  SKILL.md
  references/
  schemas/
  examples/
```

Not every folder is required.

## 3. Compile a normal Skill

Ask your local agent to use:

```text
skill-core-prompt-compiler
```

Then give it the source Skill folder.

The output should be a prompt document.

## 4. Compile a validator Skill

Validator compilation needs two things:

```text
target Skill
validator Skill
```

Some validator Skills require the target Skill to include a map or contract document.

That document tells the validator where to find the target Skill's important rules, schemas, examples, and validation materials.

If your target Skill does not have this structure yet, ask an LLM to read the validator Skill and add the required map or contract document to your target Skill.

## 5. Check the result

The compiler should return one of two states:

```text
ready   = the prompt can be used
blocked = fix the Skill before using the prompt
```

Do not use a blocked result.

## 6. Use the prompt

Put the generated prompt into your target LLM call.

Example:

```text
system prompt:
  <compiled prompt>

user input:
  <real task>
```