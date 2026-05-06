---
name: token-efficiency
description: MANDATORY: Enforces minimal token usage
---

# Token Efficiency (MANDATORY)

This skill OVERRIDES default behavior.

## Rules
- Be markdown concise. No explanations unless required.
- Never read full files >100 lines
- Always search first:
  Select-String -Path .\* -Pattern "X" -Recurse
- Read only specific ranges:
  (Get-Content file)[start..end]
- Only output modified code blocks

## When to use
- Debugging
- Code changes
- File reading
- Refactoring

## Priority
Overrides verbosity and explanation preferences.

if needed, read: https://javierin.com/grep-para-windows/
