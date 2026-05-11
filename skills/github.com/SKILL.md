---
name: "github.com/integration"
description: "Use the GitHub API integration for repositories, issues, pull requests, branches, and file changes."
version: "1.0.0"
context: "inline"
domain: "github.com"
tags:
  - integration
  - github.com
  - repository
  - pull-request
intent_keywords:
  - github integration
  - github repo
  - github issue
  - pull request
allowed_tools:
  - integration
---

# GitHub Integration Skill

Use this skill when the user asks to work with GitHub repositories, issues, pull requests, branches, refs, commits, or repository files.

Prefer the ready-made Daptin integration over browser automation for GitHub data and actions.

## Operation Flow

1. Discover relevant operations:

```js
integration({
  command: "search_operations",
  provider: "github.com",
  query: "<user intent>",
  page_size: 25
})
```

2. Inspect the selected operation before executing it:

```js
integration({
  command: "describe_operation",
  provider: "github.com",
  operation: "<operation>"
})
```

3. Execute only an operation id returned by discovery or description:

```js
integration({
  command: "execute_operation",
  provider: "github.com",
  operation: "<operation>",
  input: {
    "...": "..."
  }
})
```

## Rules

- Do not ask for OAuth token ids, credential ids, or account selectors. Daptin resolves the user's connected GitHub account internally.
- Do not invent operation ids. Search operations first, then describe the intended operation, then execute.
- If an operation is not found, search again for the same provider and user intent before changing strategy.
- Treat provider HTTP responses as API results. For example, a GitHub 404 means the GitHub endpoint returned 404; it does not mean the integration tool failed.
- Use browser automation only when the integration has no matching operation or when the user explicitly asks to operate the GitHub UI.

## Repository Maintenance Pattern

For repository file changes:

1. Read the current file content or repository state first.
2. Prefer branch plus pull request for multi-file or risky changes.
3. Use clear commit messages and PR bodies that explain the user-visible change.
4. Avoid direct writes to the default branch unless the user explicitly asks or the change is trivial and low risk.
