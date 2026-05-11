---
name: "100xbot/skill-maintainer"
description: "Maintain the official 100xBot skills repository in the supported package format."
version: "1.0.0"
context: "inline"
tags:
  - skills
  - maintainer
  - 100xbot
intent_keywords:
  - skills repo
  - create skill
  - update skill
  - publish skill
  - maintain skills
allowed_tools:
  - skill_project
  - skill_manage
  - integration
---

# 100xBot Skill Maintainer

Use this skill when maintaining the official 100xBot skills repository.

The official skills repository is `100x-bot/skills`.

## Canonical Repository Structure

```text
catalog.json
skills/
  <skill-package>/
    SKILL.md
    manifest.json
    tests.json
```

Provider integration skills use the provider domain as the package name:

```text
skills/github.com/SKILL.md
skills/github.com/manifest.json
skills/asana.com/SKILL.md
skills/asana.com/manifest.json
```

## Required Files

- `catalog.json` is the root index for skill discovery.
- `skills/<skill-package>/SKILL.md` contains the human-readable skill instructions with frontmatter.
- `skills/<skill-package>/manifest.json` contains machine metadata for provider, integration, domains, source, status, and test prompts.
- `skills/<skill-package>/tests.json` is optional but recommended for prompt-level validation scenarios.

Do not create ad-hoc skill JSON files such as `skills/github.com/repo-maintenance.json`.

## Provider Integration Skill Rules

When creating or updating a provider integration skill:

1. Normalize provider identity to the domain, for example `github.com`, `asana.com`, or `airtable.com`.
2. Create or update `skills/<provider-domain>/SKILL.md`.
3. Create or update `skills/<provider-domain>/manifest.json`.
4. Update the matching root `catalog.json` entry.
5. Include test prompts that cover read, write, and auth-required paths when applicable.

The skill content must teach the runtime agent to use Daptin integration discovery and execution:

```js
integration({
  command: "search_operations",
  provider: "<provider-domain>",
  query: "<user intent>",
  page_size: 25
})
```

```js
integration({
  command: "describe_operation",
  provider: "<provider-domain>",
  operation: "<operation>"
})
```

```js
integration({
  command: "execute_operation",
  provider: "<provider-domain>",
  operation: "<operation>",
  input: {
    "...": "..."
  }
})
```

Do not expose OAuth token ids, credential ids, or account selection to the model. Daptin resolves the user's connected account internally.

## Publishing Flow

Use `skill_project` for official skill repo changes:

1. Run `skill_project` with `action: "plan"` to preview generated `SKILL.md`, `manifest.json`, and `catalog.json` changes.
2. Inspect the planned files for format drift and outdated tool names.
3. Publish with `skill_project` using `action: "publish_pr"` unless the user explicitly asks for a direct repository update.
4. Authenticated GitHub writes must go through Daptin's `github.com` integration. Public reads may use GitHub directly.

## Migration Rule

If an existing skill is stored as `skills/<domain>/<name>.json`, migrate it into:

- `skills/<domain>/SKILL.md`
- `skills/<domain>/manifest.json`
- a root `catalog.json` entry

Preserve useful description and instruction content, but rewrite tool references to the current integration capability contract. Remove the old ad-hoc JSON file after the canonical package is created.
