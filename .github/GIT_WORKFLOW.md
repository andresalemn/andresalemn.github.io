# Git Workflow & Commit Conventions

This document outlines the conventions for using Git, staging changes, and formatting commit messages in this project.

## Commit Message Format

All commit messages follow the **Conventional Commits** specification, enriched with optional scopes and Gitmojis:

```text
<type>(<scope>): <gitmoji> <subject>
# or
<type>: <gitmoji> <subject>
```

### Commit Types & Emojis

- `feat`: New features (e.g. `feat(devcontainer): 🐳 add PDF extension to VSCode customizations`)
- `fix`: Bug fixes (e.g. `fix(cv): 🐛 parse markdown links in certificate and award title renderers`)
- `docs`: Documentation updates (e.g. `docs: 📝 add bilingual CV editing guidelines`)
- `style`: Formatting, visual tweaks, or style fixes (e.g. `style: 🎨 prettier formatting fixes`)
- `refactor`: Code restructuring without changing functionality
- `test`: Adding or updating integration/visual regression tests
- `chore` / `config`: Auxiliary tasks, configuration updates, or dependency maintenance

### Staging & Categorization Guidelines

- **Granular Commits by Functionality**: Group unstaged changes into distinct, logical commits based on functional area rather than committing everything in a single bulk commit.
- **Explicit Staging**: Always stage files explicitly using `git add <file1> <file2>` per functional group. Avoid `git add .` unless intentionally staging all reviewed changes.
- **Lockfile Note**: `Gemfile.lock` is managed dynamically by Jekyll entrypoint scripts and should be left out of standard commits unless gem dependencies are explicitly modified.

## Agent Guidelines for Commit Assistance

- **When to offer commit help**: Agents should only suggest commit breakdowns or offer help with commit message creation after major feature completions, breakthroughs, or when the user explicitly asks for commit assistance. Do not output commit suggestions after every minor code edit.
