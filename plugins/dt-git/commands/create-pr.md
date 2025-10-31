---
description: Create a pull request with AI-generated description
allowed-tools: Bash(git status:*), Bash(git diff:*), Bash(git log:*), Bash(git branch:*), Bash(git push:*), Bash(gh:*)
---

Create a pull request with an AI-generated title and description based on your changes.

Usage:
- `/create-pr` - Create a PR from the current branch

## Context

- Current git status: !`git status`
- Current branch: !`git branch --show-current`
- Commits in this branch (not in default): !`git log --oneline origin/HEAD..HEAD`
- Full diff from default branch: !`git diff origin/HEAD...HEAD`

## Your task

Based on the above context, create a pull request:

1. Check if the current branch is pushed to remote. If not, push it first with `git push -u origin <branch-name>`

2. Analyze ALL commits (not just the latest) and the full diff to understand the complete scope of changes

3. Draft a PR that includes:
   - **Title**: Concise summary of the overall change (50 chars or less)
   - **Summary**: Detailed explanation of the major changes, what was modified, and why. Use multiple paragraphs or bullet points as needed to clearly describe the scope of work.

4. Create the PR using `gh pr create` with a HEREDOC for the body:
   ```
   gh pr create --title "Your title" --body "$(cat <<'EOF'
   ## Summary

   Detailed description of changes here...

   - Major change 1
   - Major change 2
   - etc.
   EOF
   )"
   ```

5. Return the PR URL when done

Important:
- Analyze the FULL diff and ALL commits, not just the latest commit
- If already on main/master, warn the user and don't create a PR
- If there are no commits ahead of the default branch, inform the user
- Focus the description on the "why" and user impact, not just the "what"
- Do NOT use the Task or TodoWrite tools
