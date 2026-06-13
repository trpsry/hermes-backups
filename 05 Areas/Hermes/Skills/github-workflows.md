---
copied_on: 2026-06-12
skill_name: github-workflows
skill_path: github/github-workflows/SKILL.md
source: Hermes skill
tags:
- hermes
- skills
- github
- git
- workflow
---
# GitHub Workflows

> Copied from Hermes skill `github-workflows`.
>
> Source path: `github/github-workflows/SKILL.md`

## What this skill is for
A practical guide for GitHub operations from Hermes: authentication, repository management, issues, pull requests, code review, releases, and GitHub Actions.

## Use when
- Checking GitHub auth
- Cloning or creating repos
- Managing issues and PRs
- Reviewing code
- Handling releases and CI/CD

## Quick start

### Check auth
```bash
git --version
gh --version 2>/dev/null || echo "gh not installed"
gh auth status 2>/dev/null || echo "gh not authenticated"
git config --global credential.helper 2>/dev/null || echo "no git credential helper"
```

### If gh is available
- Use `gh auth login`
- Then `gh auth setup-git`
- Verify with `gh auth status`

### If gh is not available
- Use git with a GitHub token or SSH
- Configure git identity:
```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

## Core repository tasks
- Clone: `git clone ...` or `gh repo clone ...`
- Create repo: `gh repo create ...`
- Fork: `gh repo fork ...`
- Sync fork: `git fetch upstream` then merge/push

## Issues
- List: `gh issue list`
- View: `gh issue view <num>`
- Create: `gh issue create ...`
- Comment / label / assign / close / reopen with `gh issue edit|comment|close|reopen`

## Pull requests
- Branch: `git checkout -b feat/...`
- Commit: Conventional Commits
- Push: `git push -u origin HEAD`
- Create PR: `gh pr create ...`
- Review: `gh pr review ...`
- Merge: `gh pr merge ...`

## Code review checklist
- Correctness
- Security
- Code quality
- Testing
- Performance
- Documentation

## Releases and Actions
- Releases: `gh release create ...`
- Workflows: `gh workflow list`
- Runs: `gh run list`, `gh run view`, `gh run rerun`

## Troubleshooting
- Use token/SSH when password auth fails
- Check scopes if push is denied
- Clear bad cached credentials if auth breaks

## Reference
If you need the full source text, load the original skill from Hermes skill storage again.
