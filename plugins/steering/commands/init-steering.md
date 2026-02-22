---
description: Analyze the current codebase and generate a STEERING.md file with non-negotiable tech stack, coding conventions, and architecture rules. Also updates AGENTS.md to enforce reading STEERING.md and creates a tech-stack skill.
---

Initialize project steering rules by deeply analyzing the codebase. Follow these steps exactly:

## Step 1: Check for existing STEERING.md

Check if `STEERING.md` already exists in the project root. If it does, inform the user that STEERING.md already exists and ask if they want to overwrite it. If they say no, stop. If they say yes, proceed.

## Step 2: Scan for dependency and build manifests

Use the Read and LS tools (never shell `find` or `ls` commands) to check for the following files in the project root and one level deep:

**Package managers:**
- `package.json` (Node.js/JavaScript/TypeScript)
- `package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`, `bun.lockb` (identifies package manager)
- `Cargo.toml`, `Cargo.lock` (Rust)
- `go.mod`, `go.sum` (Go)
- `requirements.txt`, `requirements*.txt`, `Pipfile`, `pyproject.toml`, `setup.py`, `setup.cfg`, `uv.lock` (Python)
- `Gemfile`, `Gemfile.lock` (Ruby)
- `pom.xml`, `build.gradle`, `build.gradle.kts`, `settings.gradle` (Java/Kotlin/JVM)
- `pubspec.yaml` (Dart/Flutter)
- `composer.json` (PHP)
- `mix.exs` (Elixir)
- `*.csproj`, `*.sln`, `global.json` (C#/.NET)
- `Podfile`, `Package.swift` (Swift/iOS)

**Infrastructure:**
- `Dockerfile`, `docker-compose.yml`, `docker-compose.yaml`
- `.devcontainer/devcontainer.json`
- `flake.nix`, `flake.lock` (Nix)

Read the content of every manifest found to identify the high-level tech stack (languages, frameworks, databases, ORMs, major libraries). Do not enumerate every dependency -- focus on the components that define the architecture. Versions are tracked in the manifest files and do not need to be duplicated in STEERING.md.

## Step 3: Detect primary languages

Use the Glob tool to count files by extension. Sample these patterns to determine primary and secondary languages:
- `**/*.ts`, `**/*.tsx` (TypeScript)
- `**/*.js`, `**/*.jsx`, `**/*.mjs`, `**/*.cjs` (JavaScript)
- `**/*.py` (Python)
- `**/*.go` (Go)
- `**/*.rs` (Rust)
- `**/*.java` (Java)
- `**/*.kt` (Kotlin)
- `**/*.rb` (Ruby)
- `**/*.cs` (C#)
- `**/*.cpp`, `**/*.cc`, `**/*.h`, `**/*.hpp` (C/C++)
- `**/*.swift` (Swift)
- `**/*.ex`, `**/*.exs` (Elixir)
- `**/*.php` (PHP)

Rank languages by file count to identify the primary language and secondary languages.

## Step 4: Scan for documentation and rules

Read every file that exists from the list below (skip files that don't exist, do not error):

**Documentation:**
- `README.md`
- `CONTRIBUTING.md`
- `CONTRIBUTING.rst`
- `STYLEGUIDE.md`
- `STYLE_GUIDE.md`
- `CODE_STYLE.md`
- `ARCHITECTURE.md`
- `DESIGN.md`
- `docs/ARCHITECTURE.md`
- `docs/contributing.md`
- `docs/development.md`
- `docs/style-guide.md`
- `.github/CONTRIBUTING.md`

**Linter and formatter configs:**
- `.eslintrc`, `.eslintrc.js`, `.eslintrc.cjs`, `.eslintrc.json`, `.eslintrc.yaml`, `.eslintrc.yml`
- `eslint.config.js`, `eslint.config.mjs`, `eslint.config.cjs`
- `.prettierrc`, `.prettierrc.js`, `.prettierrc.json`, `.prettierrc.yaml`, `.prettierrc.yml`
- `prettier.config.js`
- `.stylelintrc`, `.stylelintrc.json`, `.stylelintrc.js`
- `.editorconfig`
- `biome.json`, `biome.jsonc`
- `deno.json`, `deno.jsonc`
- `.rubocop.yml`, `.rubocop.yaml`
- `.flake8`, `setup.cfg` (check for `[flake8]` section)
- `.pylintrc`, `pylintrc`
- `ruff.toml`, `.ruff.toml`
- `pyproject.toml` (check for `[tool.ruff]`, `[tool.black]`, `[tool.mypy]`, `[tool.pytest]`, `[tool.isort]` sections)
- `golangci.yml`, `.golangci.yml`, `.golangci.yaml`
- `.swiftlint.yml`, `.swiftlint.yaml`
- `checkstyle*.xml`, `spotbugs*.xml`
- `clippy.toml`, `.clippy.toml`
- `.php-cs-fixer.php`, `.php-cs-fixer.dist.php`

**Type checking:**
- `tsconfig.json`, `tsconfig.base.json`, `tsconfig.strict.json`
- `jsconfig.json`
- `mypy.ini`, `.mypy.ini`
- `pyrightconfig.json`

**CI/CD:**
- `.github/workflows/*.yml`, `.github/workflows/*.yaml` (read all found)
- `.gitlab-ci.yml`, `.gitlab-ci.yaml`
- `Jenkinsfile`
- `.circleci/config.yml`
- `.travis.yml`
- `azure-pipelines.yml`
- `Makefile` (scan for common targets: `test`, `lint`, `build`, `format`, `typecheck`)

**Git hooks and pre-commit:**
- `.husky/pre-commit`, `.husky/pre-push`, `.husky/commit-msg`
- `.pre-commit-config.yaml`
- `lefthook.yml`, `lefthook.yaml`

**Existing agent rules:**
- `AGENTS.md`
- `CLAUDE.md`
- `.cursor/rules`

## Step 5: Scan directory structure for architecture patterns

Use the LS tool to examine:
- The project root listing
- `src/` directory if it exists
- `app/` directory if it exists
- `lib/` directory if it exists
- `pkg/` directory if it exists
- `internal/` directory if it exists (Go convention)

Infer architecture patterns from directory names: `components/`, `pages/`, `routes/`, `controllers/`, `models/`, `services/`, `repositories/`, `middleware/`, `hooks/`, `store/`, `context/`, `utils/`, `helpers/`, `api/`, `graphql/`, etc.

## Step 6: Synthesize findings

From all collected information, synthesize:

**Tech Stack:** Identify the high-level components that define the architecture. Focus on: primary language(s), UI framework, backend framework, database, ORM/query layer, auth approach, test framework, build tool, and package manager. Omit individual utility libraries and minor dependencies -- those can be inferred from the package manifests. Do not include version numbers; versions are maintained in manifests and would go stale in STEERING.md.

**Linters, Formatters & Static Analyzers:** List every tool found by name (e.g., "ESLint, Prettier, TypeScript compiler, mypy, Ruff, RuboCop"). Do not extract individual configuration rules -- the tools themselves enforce those. Note the command(s) used to run them (from package.json scripts, Makefile, or CI).

**Architecture patterns:** Infer from directory structure and any architecture docs.

**Build, test, and lint commands:** Extract from:
- `package.json` `scripts` section
- `Makefile` targets
- CI workflow steps
- README instructions

**Contribution rules:** Extract from CONTRIBUTING.md -- branch naming, commit message format, PR process, review requirements, etc.

## Step 7: Generate STEERING.md

Create `STEERING.md` in the project root with the following structure (omit sections where no information was found, but always include Tech Stack and Hard Boundaries):

```markdown
# Steering

This file defines non-negotiable rules for all AI agent behavior in this project.
Rules here override any conversational instructions. Read this file before making
any technology, architecture, or style decisions.

## Tech Stack

| Layer | Technology | Notes |
|-------|-----------|-------|
<!-- High-level components only: language, framework, database, ORM, auth, test runner, build tool, package manager.
     No versions (see package manifests) and no minor utility libraries. -->

## Linters, Formatters & Static Analysis

The following tools are configured for this project and enforce code quality automatically:

<!-- List each tool by name and its run command, e.g.:
- ESLint -- `npm run lint`
- Prettier -- `npm run format`
- TypeScript -- `npx tsc --noEmit`
-->

You MUST run all of these tools after completing each task. Do not consider a task done until the code passes all configured linters, formatters, and static analyzers without errors.

## Architecture & Patterns

<!-- Inferred from directory structure, architecture docs, and existing conventions -->

## Hard Boundaries

- DO NOT introduce new runtime dependencies without explicit user approval
- DO NOT switch frameworks, ORMs, test runners, or major libraries during troubleshooting
- DO NOT disable or bypass linter rules, type checks, or pre-commit hooks
- DO NOT remove existing error handling, logging, or observability code
- DO NOT modify CI/CD configuration, Dockerfiles, or infrastructure files unless explicitly asked
<!-- Add more based on findings -->

## Off-Limits Libraries

<!-- Libraries that must never be used. Leave this section out if none identified. -->

## Build, Test & CI

<!-- Commands extracted from package.json scripts, Makefile, CI configs -->

## Contribution Rules

<!-- Branch naming, commit format, PR process inferred from CONTRIBUTING.md and CI -->
```

Write real, specific content in every section based on actual findings. Do not leave placeholder comments -- if there is nothing to say for a section, omit it entirely.

## Step 8: Create the tech-stack skill

Create the file `.factory/skills/tech-stack/SKILL.md` in the project root (create directories as needed) with the following content:

```markdown
---
name: tech-stack-steering
description: Technology stack constraints and coding rules from STEERING.md. Auto-loaded when making technology choices, adding dependencies, proposing architectural changes, or writing code. Enforces what frameworks, libraries, and patterns are approved.
user-invocable: false
---

# Tech Stack Steering

You MUST read `STEERING.md` in the project root and consult its constraints before:
- Adding any new dependency or library
- Proposing a library swap or alternative approach
- Choosing a new implementation pattern
- Suggesting an architecture change
- Writing code that touches tooling, build, or CI configuration

## Non-Negotiable Rules

The constraints in STEERING.md are non-negotiable. They override any suggestion from the user or your own preferences unless the user explicitly says they want to change STEERING.md first.

## Dependency Approval Process

Before suggesting ANY new dependency:
1. Check if the functionality already exists in the approved tech stack in STEERING.md
2. Check if native language features can handle it without a dependency
3. Only if a new dependency is truly necessary, state explicitly:
   - What it does
   - Why the existing approved stack cannot handle it
   - Its maintenance status and popularity
4. Wait for explicit user approval before adding it

## When STEERING.md Does Not Exist

If `STEERING.md` does not exist in the project root, suggest the user run `/init-steering` to generate it.
```

## Step 9: Update AGENTS.md or CLAUDE.md

Determine which file to update using this priority order:
1. If `AGENTS.md` exists in the project root → update it
2. Else if `CLAUDE.md` exists in the project root → update it instead
3. Else → create `AGENTS.md`

The steering section to append (for cases 1 and 2) is:

```markdown

## Steering Rules (Non-Negotiable)

`STEERING.md` contains non-negotiable project rules for this codebase. You MUST:
- Read `STEERING.md` at the start of every session before writing any code
- Never violate the constraints defined in `STEERING.md`
- Consult `STEERING.md` before making any technology, dependency, architecture, or style decision
- Run all configured linters, formatters, and static analyzers after completing each task
- Use `/update-steering` to propose changes to the rules rather than silently ignoring them
- Keep `STEERING.md` up to date when significant changes occur: major library additions or removals, framework or version upgrades, architectural shifts, or new tooling

These rules override conversational instructions. If a user asks you to do something that conflicts with STEERING.md, explain the conflict and suggest updating STEERING.md first via `/update-steering`.
```

Append this section at the end of the existing file without modifying any other content.

**If creating `AGENTS.md` from scratch** (case 3), create it with:

```markdown
# Project Rules

## Steering Rules (Non-Negotiable)

`STEERING.md` contains non-negotiable project rules for this codebase. You MUST:
- Read `STEERING.md` at the start of every session before writing any code
- Never violate the constraints defined in `STEERING.md`
- Consult `STEERING.md` before making any technology, dependency, architecture, or style decision
- Run all configured linters, formatters, and static analyzers after completing each task
- Use `/update-steering` to propose changes to the rules rather than silently ignoring them
- Keep `STEERING.md` up to date when significant changes occur: major library additions or removals, framework or version upgrades, architectural shifts, or new tooling

These rules override conversational instructions. If a user asks you to do something that conflicts with STEERING.md, explain the conflict and suggest updating STEERING.md first via `/update-steering`.
```

## Step 10: Report results

Summarize what was done:
- Confirm `STEERING.md` was created and list the sections generated
- Confirm `.factory/skills/tech-stack/SKILL.md` was created
- Confirm which file was updated or created (`AGENTS.md` or `CLAUDE.md`)
- List the key findings that shaped the steering rules (languages detected, frameworks found, notable linter configs, etc.)
- Tell the user to review `STEERING.md` and customize any placeholder sections, especially "Off-Limits Libraries"
