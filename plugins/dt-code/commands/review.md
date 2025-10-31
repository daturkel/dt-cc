---
description: Review code changes in modified files since origin/HEAD
argument-hint: [file_path]
allowed-tools: Read, Bash(git diff:*), Bash(git log:*), Bash(git status:*), Task
---

Review all modified files since origin/HEAD, or focus on specific files if provided.

Focus on (if provided): $ARGUMENTS

## Context

- Modified files: !`git status --short`
- Branch comparison: !`git diff origin/HEAD...HEAD --stat`
- Recent commits: !`git log origin/HEAD..HEAD --oneline`

## Your task

1. Identify files that have been modified since origin/HEAD
2. If the user specified files in arguments, filter to only those files
3. For each modified file, launch an agent to review it (keeping in context the diff of related files)
4. Each review agent should analyze:
   - **Code quality**: Logic clarity, maintainability, adherence to best practices
   - **Potential issues**: Bugs, edge cases, error handling gaps
   - **Security concerns**: Vulnerabilities, unsafe patterns, input validation
   - **Performance**: Obvious inefficiencies or optimization opportunities
   - **Testing**: Whether the changes would benefit from additional tests
   - **Documentation**: Whether the changes are sufficiently documented

5. Compile the reviews and present a summary for each file

Review guidelines:
- **Be constructive**: Focus on actionable feedback
- **Prioritize**: Highlight critical issues before minor suggestions
- **Be specific**: Reference line numbers and code snippets
- **Context-aware**: Consider the file's purpose and the nature of changes

Important:
- Each agent should receive the git diff for their specific file and related files
- Focus primarily on the actual changes (diff), not the entire file
- If no files are modified, inform the user
- Format output clearly with file names as headers