# dt-cc

A collection of useful Claude Code plugins for enhanced development workflows.

## Overview

dt-cc provides a set of plugins that extend Claude Code with powerful commands for git workflows and code quality improvements. These plugins streamline common development tasks with AI-powered automation.

## Plugins

### dt-git

Tools for interacting with git and GitHub.

**Commands:**

- `/dt-git:commit [all]` - Generate AI-powered commit messages and create commits
  - `/dt-git:commit` - Commit currently staged changes
  - `/dt-git:commit all` - Stage all changes, then commit
  - Automatically analyzes changes and creates meaningful, concise commit messages
  - Follows repository commit message conventions
  - Warns about sensitive files before committing

- `/dt-git:create-pr` - Create pull requests with AI-generated descriptions
  - Analyzes all commits and changes in your branch
  - Generates comprehensive PR title and description
  - Automatically pushes branch if needed
  - Returns PR URL when complete

### dt-code

Tools for improving code quality.

**Commands:**

- `/dt-code:document [file_path]` - Add documentation to modified functions and classes
  - Focuses on code that appears in git diff
  - Adds concise, meaningful documentation
  - Follows language-specific documentation conventions
  - Preserves existing code style and formatting

- `/dt-code:review [file_path]` - Review code changes since origin/HEAD
  - Analyzes code quality, potential issues, and security concerns
  - Provides actionable, prioritized feedback
  - Can focus on specific files or review all changes
  - References specific line numbers in feedback

## Installation

To add the marketplace:

```shell
/plugin marketplace add daturkel/dt-cc
```

To add the plugins:

```shell
/plugin enable dt-git@dt-cc
/plugin enable dt-code@dt-cc
```

## Usage

Once installed, use the commands via Claude Code's slash command interface:

```
/dt-git:commit all
/dt-git:create-pr
/dt-code:document src/main.py
/dt-code:review
```

## License

MIT License - see [LICENSE](LICENSE) for details.
