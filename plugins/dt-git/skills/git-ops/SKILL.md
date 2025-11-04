---
name: git-ops
description: Get information about the state of a git repo, get diffs, make commits, make PRs, etc.. Use this skill when the user needs to accomplish git tasks.
allowed-tools:
  - Bash(git:*)
  - Bash(ls:*)
  - Bash(pwd)
  - Bash(gh:*)
---

# Git Operations Skill

This skill provides comprehensive git repository information for commits, PRs, and general repository status.

## Instructions

Follow these steps to gather and present git information:

### 1. Basic Repository Status

```bash
git status
```

This shows:
- Current branch name
- Tracking status (ahead/behind remote)
- Staged files (green)
- Unstaged files (red)
- Untracked files

### 2. Staged Changes (Ready for Commit)

```bash
git diff --cached --stat
```
Shows statistics of staged files.

```bash
git diff --cached
```
Shows full diff of what would be committed.

**Present**: Summarize the number of staged files and the nature of changes.

### 3. Unstaged Changes (Modified but Not Staged)
```bash
git diff --stat
```
Shows statistics of modified but unstaged files.

```bash
git diff
```
Shows full diff of unstaged changes.

**Present**: List files with unstaged modifications.

### 4. Creating Commits

When the user asks to create a commit:

1. **Determine what to commit**:
   - If user says "commit all" or similar: stage everything with `git add .`
   - Otherwise only commit staged changes

2. **Draft commit message** based on recent history style:
```bash
git log --oneline -10
```
   - Keep it 1-2 sentences maximum
   - Focus on "why" not "what"
   - Follow the repository's commit message style

3. **Create the commit**:
```bash
git commit -m "Your concise commit message"
```

**Important rules**:
- ALWAYS create NEW commits, NEVER use `--amend`
- Warn about sensitive files (.env, credentials.json, etc.) before committing
- Don't push unless explicitly asked
- If nothing staged and not staging all, inform user (don't create empty commit)

### 5. Recent Commit History
```bash
git log --oneline -10
```
Shows last 10 commits for context on commit message style.

### 6. Branch Comparison (For PR Context)

Show changes since branching:
```bash
git diff main...HEAD --stat
```
Statistics of all changes from base branch.

```bash
git log main..HEAD --oneline
```
All commits that would be in a PR.

```bash
git diff main...HEAD
```
Full diff for PR review (use sparingly, can be large).

### 7. Remote Status
```bash
git rev-list --left-right --count HEAD...@{upstream} 2>/dev/null || echo "No upstream configured"
```
Shows how many commits ahead/behind the remote.

### 8. Creating Pull Requests with `gh` CLI

When the user asks to create a PR, follow these steps:

1. **Verify branch status**:
```bash
git branch --show-current
git status
```
   - If on main/master, warn user and don't create PR
   - Check if branch needs to be pushed

2. **Understand the full scope of changes** from when the branch diverged:
```bash
git log origin/main...HEAD --oneline
git diff origin/main...HEAD --stat
git diff origin/main
```
   - If no commits ahead of default branch, inform user

3. **Push to remote if needed**:
```bash
git push -u origin $(git branch --show-current)
```

4. **Create PR with `gh` CLI**:
```bash
gh pr create --title "Concise title (50 chars or less)" --body "$(cat <<'EOF'
## Summary

Detailed explanation of the major changes, what was modified, and why.
Analyze ALL commits and the full diff, not just the latest commit.

- Major change 1
- Major change 2
- etc.

Focus on the "why" and user impact, not just the "what".
EOF
)"
```

5. **Return the PR URL** when done

**Important notes**:
- Analyze ALL commits that will be included, not just the latest one
- Use HEREDOC format for the body to ensure proper formatting
- Do NOT use the Task or TodoWrite tools when creating PRs
- Adapt base branch name (try `main`, `master`, `origin/main`, etc.)

## Presenting Information

Organize your response based on what the user needs:

### For Commit Questions
- **Staged files**: List files ready to commit with brief summary
- **Commit message suggestion**: Based on changes and recent history
- **Warnings**: Flag any concerns (large files, potential secrets)

### For PR Questions
- **Branch comparison**: Summarize all changes since base branch
- **Commit list**: Show commits that would be included
- **Files changed**: List all modified files with stats
- **Create PR**: Use `gh pr create` to create the PR with appropriate title and description
- **PR description template**: If only describing, suggest title and description

### For General Status
- **Current state**: Branch name, tracking status
- **Staged vs unstaged**: Clear breakdown of what's where
- **Next steps**: Actionable suggestions

## Best Practices

1. **Be concise**: The user is experienced, provide summaries not explanations
2. **Adapt base branch**: Try `main`, `master`, `develop` as needed
3. **Handle errors gracefully**: Not all repos have remotes or follow conventions
4. **Skip noise**: Ignore lock files, build artifacts in summaries
5. **Security awareness**: Flag potential secrets or sensitive files

## Examples

### Example 1: Commit message for staged changes

**User**: "Write a commit message"

```bash
git diff --cached --stat
git diff --cached
git log --oneline -10
```

**Response**:
```
Staged: 3 files (parser.py, test_parser.py, README.md)

Suggested commit message:
"Add config parser with tests and documentation"
```

### Example 2: Commit message for unstaged changes

**User**: "Commit my changes"

```bash
git status
git diff --stat
git diff
```

If nothing staged, stage modified files and commit:
```bash
git add -u
git commit -m "Add config parser with tests"
```

### Example 3: PR description for branch

**User**: "Write a PR description"

```bash
git log main..HEAD --oneline
git diff main...HEAD --stat
git diff main...HEAD
```

**Response**:
```
**Add configuration parser with validation**

Implements config parsing and validation system.

- Added parse_config() for YAML/JSON parsing
- Added validate_config() with schema validation
- Full test coverage for both modules
- Updated docs with examples

6 files changed: +365 -8
```

