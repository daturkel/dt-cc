---
description: Add documentation to functions/classes that have been modified
argument-hint: [file_path]
allowed-tools: Read, Edit, Bash(git status:*), Bash(git diff:*), Task
---

Add or improve documentation for functions and classes that have been modified in the current git diff.

Focus on (if provided): $ARGUMENTS

## Context

- Modified files: !`git status --short`
- Changes in staged files: !`git diff --cached`
- Changes in unstaged files: !`git diff`

## Your task

1. Read the relevant files, making note of what the user requested to focus on (if anything)
2. Analyze the git diff output above to identify which functions/classes/methods were modified
3. Focus ONLY on documenting the functions/classes that appear in the diff
4. Use the Edit tool to add documentation directly to the file

Documentation style guidelines:
- **Concise but complete**: Explain what's non-obvious, skip what's self-evident
- **Focus on "why" and "what"**: Explain purpose and behavior, not just repeating the code
- **Include types**: Document parameter and return types if not already typed
- **Keep it maintainable**: Don't over-document trivial code

Important:
- Do NOT change any code logic, only add/improve documentation
- Preserve existing code style and formatting
- Use language-appropriate documentation conventions
  - For Python, use Google-style docstrings.
- For files with existing docs, improve them rather than duplicating
- ONLY document functions/classes that appear in the git diff context above
