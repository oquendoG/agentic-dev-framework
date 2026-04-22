---
description: Review implementation
---

## Review current changes:

- Code quality
- Architecture alignment
- Edge cases
- Overengineering / underengineering

If issues:
- Fix them OR
- Add them to task as TODO

If a new architectural or technical decision is introduced:
- Add it to knowledge/decisions.md if it refers to the project
- Add it to feature folder/decisions.md if it refers to the feature
- Avoid duplicates
- Keep it concise

## Check for rule violations:

### Critical checks
- Any usage of `var` in .NET → FAIL
- Any unnecessary try/catch → FAIL
- Ignoring global middleware/interceptors → FAIL
- build errors → FAIL
- Warnings when building API → FAIL

If any violation:
- Fix it immediately
- Do not accept the implementation

Also:
- Ensure consistency with existing codebase

If the code introduces a pattern that contradicts existing architecture:
- Rewrite it to match the project

## Update:
	@knowledge/tasks/current.md