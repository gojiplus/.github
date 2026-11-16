# .github

Organization profile and language-neutral reusable workflows for the fleet.

## Link checking

Links are checked in two halves, both by [lychee](https://lychee.cli.rs), and
both read a repo's exclusions from `lychee.toml` at its root:

- **Within the repo, on every commit:** lychee's pre-commit hook, offline.

  ```yaml
  - repo: https://github.com/lycheeverse/lychee
    rev: lychee-v0.24.2
    hooks:
      - id: lychee
        args: [--offline, --include-fragments, --no-progress]
        types_or: [markdown, html]
  ```

- **Web links, weekly:** `reusable-link-check.yml`.

  ```yaml
  # .github/workflows/link-check.yml
  name: link-check
  on:
    push:
      branches: [main, master]
    schedule:
      - cron: "0 6 * * 1"
    workflow_dispatch:
  permissions:
    contents: read
  jobs:
    links:
      uses: gojiplus/.github/.github/workflows/reusable-link-check.yml@v1
      with:
        paths: |
          README.md
          docs/**/*.md
  ```

A URL that is alive but fails a bare GET goes in `lychee.toml`:

```toml
exclude = ['^https://api\.example\.org/$']
```
