# steering

Generate and maintain a `STEERING.md` file that defines non-negotiable tech stack, coding conventions, and architecture rules for AI agents working in your codebase.

Inspired by [Kiro's steering file concept](https://kiro.dev), adapted for Factory Droid and Claude Code.

## What It Does

- `/init-steering` -- deep-scans your codebase (manifests, linter configs, docs, CI) and generates a `STEERING.md` tailored to your project. Also updates `AGENTS.md` to enforce reading it and installs a project-local `tech-stack` skill.
- `/update-steering` -- guided command to add or modify rules in `STEERING.md` (new "do" rules, "don't" rules, tech stack changes, conventions, etc.)
- `/remove-steering` -- removes all steering artifacts: deletes `STEERING.md`, removes the `tech-stack` skill, and strips the steering section from `AGENTS.md`.
- `tech-stack` skill -- an auto-loaded project skill installed by `/init-steering` that reinforces reading `STEERING.md` before every technology or architecture decision.

## Installation

```bash
# Factory (Droid)
droid plugin install steering@mb-ai-tools

# Claude Code
/plugin install steering@mb-ai-tools
```

## Usage

### Initialize

Run once per project to generate your steering file:

```
/init-steering
```

This will:
1. Scan `package.json`, `Cargo.toml`, `go.mod`, `pyproject.toml`, and other manifests for your tech stack
2. Read linter configs (ESLint, Prettier, Ruff, RuboCop, etc.) to extract code style rules
3. Read `CONTRIBUTING.md`, `STYLEGUIDE.md`, architecture docs, and CI workflows for conventions
4. Detect architecture patterns from your directory structure
5. Generate `STEERING.md` with sections for tech stack, code style, hard boundaries, build commands, and contribution rules
6. Create `.factory/skills/tech-stack/SKILL.md` -- a project-local auto-loaded skill that enforces reading STEERING.md
7. Update or create `AGENTS.md` to reference STEERING.md as non-negotiable

After running, review `STEERING.md` and customize any sections, especially **Off-Limits Libraries**.

### Update

Add or change rules at any time:

```
/update-steering
```

The command guides you through choosing the type of change and writing the rule.

### Remove

Remove all steering artifacts from the project:

```
/remove-steering
```

Deletes `STEERING.md`, removes `.factory/skills/tech-stack/`, and strips the steering section from `AGENTS.md`. Asks for confirmation before making any changes.

## STEERING.md Structure

```markdown
# Steering

## Tech Stack
| Layer | Technology | Version | Notes |

## Code Style & Linting

## Architecture & Patterns

## Hard Boundaries

## Off-Limits Libraries

## Build, Test & CI

## Contribution Rules
```

## Platform Compatibility

| Feature | Factory (Droid) | Claude Code |
|---------|:-:|:-:|
| `/init-steering` | Yes | Yes |
| `/update-steering` | Yes | Yes |
| `tech-stack` skill | Yes | Yes |
