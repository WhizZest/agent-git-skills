# gh-cli Skill

Comprehensive GitHub CLI (gh) reference skill for AI agents.

## Overview

This skill provides complete documentation and reference for GitHub CLI (`gh`), enabling AI agents to work seamlessly with GitHub from the command line. It covers all major GitHub operations including repositories, issues, pull requests, Actions, projects, releases, gists, codespaces, organizations, and extensions.

**Key Features:**
- **Modular Design**: SKILL.md serves as a navigation hub (189 lines), with detailed content in 12 modular reference documents
- **Progressive Disclosure**: Load only the documentation you need, reducing context window usage by 92%
- **Complete Command Reference**: All `gh` commands with examples and options
- **Best Practices**: Critical warnings and workflows for common pitfalls

## Skill Structure

```
skills/gh-cli/
├── SKILL.md                    # Main navigation hub (189 lines)
└── references/                 # Modular reference documents
    ├── auth.md                 # Authentication & account management
    ├── repo.md                 # Repository operations
    ├── issues.md               # Issue management
    ├── prs.md                  # Pull request workflows ⚠️ Critical warnings
    ├── actions.md              # GitHub Actions (runs, workflows, secrets)
    ├── projects.md             # GitHub Projects
    ├── releases.md             # Release management
    ├── gists.md                # Gist operations
    ├── codespaces.md           # Codespaces management
    ├── search.md               # Search functionality
    ├── advanced.md             # API, extensions, aliases, labels, keys
    └── config.md               # Configuration & environment setup
```

### How It Works

1. **SKILL.md** provides quick start guide, critical best practices, and links to detailed references
2. **Reference documents** contain comprehensive command documentation organized by domain
3. AI agents load only the relevant reference files based on the task at hand
4. This modular approach reduces token usage and improves response quality

## Features

- **Authentication Guide**: Setup and manage GitHub authentication (see [auth.md](skills/gh-cli/references/auth.md))
- **Repository Management**: Create, clone, fork, and manage repositories (see [repo.md](skills/gh-cli/references/repo.md))
- **Issue Tracking**: Create, list, view, and manage issues (see [issues.md](skills/gh-cli/references/issues.md))
- **Pull Request Workflow**: Full PR lifecycle management (see [prs.md](skills/gh-cli/references/prs.md)) ⚠️ **Critical warnings inside**
- **GitHub Actions**: Workflow management and CI/CD operations (see [actions.md](skills/gh-cli/references/actions.md))
- **Project Boards**: Kanban-style project management (see [projects.md](skills/gh-cli/references/projects.md))
- **Releases & Packages**: Version management and package publishing (see [releases.md](skills/gh-cli/references/releases.md))
- **Codespaces**: Cloud development environment management (see [codespaces.md](skills/gh-cli/references/codespaces.md))
- **Search**: Code, commits, issues, PRs, and repository search (see [search.md](skills/gh-cli/references/search.md))
- **Organization Tools**: Team and organization management (see [advanced.md](skills/gh-cli/references/advanced.md))
- **Extensions**: Extend gh functionality with community extensions (see [advanced.md](skills/gh-cli/references/advanced.md))
- **API Access**: Direct REST and GraphQL API calls (see [advanced.md](skills/gh-cli/references/advanced.md))

## Critical Best Practices

### ⚠️ Always Use `--body-file` for PR/Issue Content

**NEVER use `--body` parameter!** Command-line argument parsing fails with special characters, especially backticks (\`) used in code blocks. This causes escaping issues that result in garbled text or parsing errors.

```bash
# ✅ CORRECT
gh pr create --title "Feature" --body-file description.md

# ❌ WRONG - backticks cause escaping issues
gh pr create --title "Feature" --body "Use `code()` function..."
```

### ⚠️ Export PR Content to Files Before Viewing

**NEVER view PR comments directly in terminal!** Terminal output may be truncated, causing loss of important information like review comments and code suggestions.

```bash
# ✅ CORRECT workflow
gh pr view 123 --comments > pr-123.md
cat pr-123.md

# ❌ WRONG (may lose data)
gh pr view 123 --comments
```

See [prs.md](skills/gh-cli/references/prs.md) for more details.

## Installation

### For AI Agents

Install this skill using your agent's skill manager:

```bash
# Using skills.sh
npx skills add https://github.com/WhizZest/gh-cli

# Or manually copy the SKILL.md file to your agent's skills directory
```

### GitHub CLI Installation

```bash
# macOS
brew install gh

# Linux
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt update
sudo apt install gh

# Windows
winget install --id GitHub.cli

# Verify installation
gh --version
```

## Quick Start

### Authentication

```bash
# Login to GitHub
gh auth login

# Check authentication status
gh auth status
```

### Repository Operations

```bash
# Create a new repository
gh repo create my-project --public --description "My awesome project"

# Clone a repository
gh repo clone owner/repo

# View repository info
gh repo view --web
```

### Issue Management

```bash
# List issues
gh issue list

# Create an issue
gh issue create --title "Bug report" --body-file issue-body.md

# View an issue
gh issue view 123
```

### Pull Requests

```bash
# List pull requests
gh pr list

# Create a pull request
gh pr create --title "Fix bug" --body-file pr-body.md

# Review a pull request
gh pr review 456 --approve
```

### GitHub Actions

```bash
# List workflows
gh workflow list

# Run a workflow
gh workflow run "CI Build"

# View workflow runs
gh run list
```

## Usage Examples

### Daily Development Workflow

```bash
# Check your open PRs
gh pr list --author @me

# Check recent issues assigned to you
gh issue list --assignee @me

# View notifications
gh api notifications

# Create a release
gh release create v1.0.0 --title "Version 1.0.0" --notes "Release notes"
```

### Project Management

```bash
# List projects
gh project list

# Add item to project
gh project item-add 123 --url https://github.com/org/repo/issues/456

# Update item status
gh project item-edit --id ITEM_ID --status-field-id FIELD_ID --single-select-option-id OPTION_ID
```

## Topics

- github-cli
- gh
- ai-agent
- skill
- cli-reference
- automation

## Version

Based on GitHub CLI version 2.85.0 (January 2026)

## Contributing

Contributions are welcome! Please feel free to submit issues and pull requests.

## License

This skill is provided as-is for use with AI agents. Refer to the official GitHub CLI documentation for licensing details.

## Resources

- [Official GitHub CLI Documentation](https://cli.github.com/manual/)
- [GitHub CLI GitHub Repository](https://github.com/cli/cli)
- [GitHub CLI Releases](https://github.com/cli/cli/releases)
