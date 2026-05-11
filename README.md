# 100xBot Skills

This repository contains official 100xBot skills. Skills are prompt packages that teach the agent how to handle a domain, provider, or repeated task without inventing behavior from scratch.

## Structure

The supported repository layout is:

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

Do not add ad-hoc skill JSON files such as `skills/github.com/repo-maintenance.json`.

## Files

- `catalog.json` is the root index used for discovery and installation.
- `SKILL.md` contains the skill instructions and frontmatter metadata.
- `manifest.json` contains machine-readable package metadata.
- `tests.json` contains prompt-level validation cases and is recommended for all maintained skills.

## Provider Integration Skills

Provider integration skills should prefer Daptin's `integration` capability over browser automation:

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

The agent should not ask for OAuth token ids, credential ids, or account selectors. Daptin resolves the connected account internally.

## Contributing

1. Create a branch for the skill change.
2. Add or update a package under `skills/<skill-package>/`.
3. Update `catalog.json`.
4. Include or update `tests.json`.
5. Open a pull request for review.
