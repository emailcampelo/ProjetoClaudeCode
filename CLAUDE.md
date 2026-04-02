# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Status

This is a new, empty project. No build system, dependencies, or source code have been added yet.

Once the project is initialized, update this file with:
- Build, lint, and test commands
- Architecture overview and key design decisions
- Any non-obvious conventions or constraints

## GitHub Repository

Repository: https://github.com/emailcampelo/ProjetoClaudeCode

### Auto-sync to GitHub

Every change to this project is automatically pushed to GitHub via a git `post-commit` hook located at `.git/hooks/post-commit`.

**How it works:**
- After every `git commit`, the hook runs `git push origin main` automatically.
- No manual push needed — just commit and changes go to GitHub.

**To commit changes:**
```bash
git add -A
git commit -m "describe your changes"
# push happens automatically
```

**If auto-push ever fails:**
```bash
git push origin main
```

**GitHub CLI (gh) is required** and must be authenticated:
```bash
"C:\Program Files\GitHub CLI\gh.exe" auth status
```
