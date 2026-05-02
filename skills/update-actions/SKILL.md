---
name: update-actions
description: Comprehensively update all actions referenced in all GitHub Actions workflow files to their latest version.
---

# Update GitHub Actions

- Use `gh` to fetch the very latest version of every action referenced in every GitHub Actions workflow file in this repo.
- Update all the action references to their latest version. All actions must be pinned to a full length sha hash.
- Update all `uses: docker` references within workflow files to their very latest version. All references must use the full length sha256 hash.
- Update all `run: uvx` references within workflow files to their very latest version. Use exact tag names.
- Ensure every sha reference includes a trailing comment containing the exact tag name which may or may not include a leading `v`, eg. `# 1.2.3` or `# v4.0.0`. Floating references such as `v4` are not allowed.
- Don't update any actions that use a branch reference such as `trunk`, `develop`, `main`, but do flag them in your output.
