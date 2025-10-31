---
description: Generate a commit message and create a commit (optionally stage all changes first)
argument-hint: [all]
allowed-tools: Bash(git status:*), Bash(git diff:*), Bash(git add:*), Bash(git commit:*), Bash(git log:*), Bash(git branch:*)
---

Create a git commit with an AI-generated message.

Usage:
- `/commit` - Commit currently staged changes
- `/commit all` - Stage all changes, then commit

## Context

- Current git status: !`git status`
- Current branch: !`git branch --show-current`
- Recent commits: !`git log --oneline -10`
- Staged changes (will be committed): !`git diff --cached`
- Unstaged changes (NOT staged yet): !`git diff`

## Your task

Based on the above context:

1. If `all` argument is provided:
   - Stage all changes with `git add .`
   - Base the commit message on BOTH staged and unstaged changes shown above
2. If `all` argument is NOT provided:
   - Only commit the staged changes shown in "Staged changes" section
   - Ignore unstaged changes when creating the commit message
3. Draft a concise, descriptive commit message that:
   - Summarizes the nature of the changes (feature, fix, refactor, etc.)
   - Focuses on the "why" rather than the "what"
   - Follows the repository's commit message style (from recent commits above)
   - Is 1-2 sentences maximum
4. Create the commit using the drafted message

Important:
- If no changes are staged (and `all` not used), inform the user and do not create an empty commit
- Warn the user if sensitive files (.env, credentials.json, etc.) are about to be committed
- Do NOT push the commit unless explicitly asked
