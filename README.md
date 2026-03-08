#⚡ blitz
End AI Amnesia. Lightweight Knowledge Layer for GitHub Copilot.

Blitz builds a lightweight knowledge layer that gives GitHub Copilot ~80% of your project's context before it writes a single line of code. Your agent stops acting like a lost new hire and starts working like a senior engineer who already knows your architecture.

Initialize once: Persistent context across all sessions.

Cut costs & latency: Lower token usage and faster completions.

Senior Lift: Consistent, architecture-aware results.

Built exclusively for GitHub Copilot.

## Quick start

1. Copy `blitz.md` into `/.github/agents/` in the target repository.
2. In GitHub Copilot, choose the Blitz agent from the terminal workflow or IDE agent picker.
3. Run `init` to generate the first Blitz knowledge baseline.
4. Review the generated files: `/agent.md`, `/{scope}/agent.md`, `/.github/copilot-instructions.md`, and `/.github/instructions/*.instructions.md`.
5. After the first review, use `init` or `update` to refresh the knowledge layer and `audit` for read-only drift checks.


## What Blitz is

Blitz is an agent for repository knowledge maintenance.

It creates and updates the files that give AI tools the right project context.
This reduces repeated source exploration and keeps project guidance consistent.

Blitz does not change application code.
It changes only knowledge artifacts.

## Who this README is for

This README is for any software developer who needs to understand, use, review, or change Blitz.

You do not need prior knowledge of this repository to read it.

## What Blitz manages

Blitz manages the repository knowledge layer.

| Artifact | Purpose |
|---|---|
| `/agent.md` | Project-wide map of scopes, architecture, dependencies, settings, and workflow |
| `/{scope}/agent.md` | Scope-specific responsibilities, boundaries, key files, call chains, and change guidance |
| `/.github/copilot-instructions.md` | Auto-loaded project summary and global working rules |
| `/.github/instructions/*.instructions.md` | Path-scoped rules, tasks, gotchas, and verification guidance |

Managed files carry the marker `<!-- managed-by: blitz -->`.

Blitz updates only files in this managed output set.

## Files in this folder

| File | Role |
|---|---|
| `blitz.md` | Executable agent prompt |
| `blitz.spec.md` | Full specification and behavioral contract |
| `README.md` | Human-facing guide to Blitz |

## Official scope

Blitz exists to build and maintain a compact knowledge layer for a repository.

That knowledge layer should cover about 80% of what a strong developer needs before making changes:

- project identity and stack
- scope boundaries
- architecture and call chains
- key files and important methods
- configuration and dependency facts
- setup, run, test, build, and deployment commands
- rules, constraints, and workflow expectations

Blitz is not a general coding agent.
It is a bootstrap, maintenance, and audit agent for repository knowledge.

## Current support

Blitz currently targets GitHub Copilot.

## When to read each file

- Read `README.md` to understand what Blitz is and how to work with it.
- Read `blitz.md` to see the actual agent behavior.
- Read `blitz.spec.md` to understand the design rules, guarantees, and expected outputs.

## Supported modes

Blitz supports three modes.

### `initialize [scope]`

Create knowledge artifacts for a repository or scope that does not have a complete Blitz baseline.

Use this when:
- Blitz is added for the first time.
- A new scope has no Blitz-managed files yet.
- Existing knowledge files must be rebuilt from a clean baseline.

### `update [scope]`

Refresh existing Blitz-managed files to match the current repository state.

Use this when:
- Source structure changed.
- Public behavior, dependencies, or workflows changed.
- Existing knowledge files are stale.

### `audit`

Check whether Blitz-managed files still match the repository.

Use this when:
- You want a read-only verification pass.
- You changed Blitz itself.
- You suspect documentation drift.

These are the only supported modes.

## How Blitz works

Blitz follows these operating principles:

- Read before write.
- Update managed files surgically.
- Keep each fact in one place.
- Preserve `## Overrides` and `blitz:preserve` blocks.
- Detect user customizations in managed sections through fingerprints and merge them forward when safe.
- Treat workflow guidance, such as Docker or local execution preferences, as durable repository knowledge.

## Managed file behavior

Blitz uses a strict ownership model.

- Managed files should contain the marker `<!-- managed-by: blitz -->`.
- Blitz may update managed files.
- Blitz should not broadly overwrite files that are not clearly Blitz-managed.
- `## Overrides` sections are user-owned.
- `<!-- blitz:preserve:start -->` to `<!-- blitz:preserve:end -->` blocks are user-owned.
- User edits in managed sections should be detected through fingerprints and merged forward when safe.
- If a merge is unsafe, Blitz should preserve user intent and report the conflict.

## Workflow and execution knowledge

Blitz treats workflow instructions as repository knowledge.

Examples:

- Docker versus local test execution
- setup and build commands
- scope-specific execution constraints
- documented project wrappers and scripts

Commands should come from evidence, not guesses.
If a command is unclear, Blitz should mark it for confirmation instead of inventing it.

If Docker support exists, Blitz should detect that and document the preferred workflow.
If the preferred runtime is unclear and a real test execution is about to happen, Blitz should ask before choosing Docker or local execution.

## Official guarantees

Blitz is expected to provide these guarantees:

- read source and existing knowledge artifacts before writing
- patch existing managed files surgically during `update`
- avoid duplicate facts across artifacts
- preserve user-controlled sections
- keep workflow preferences in the managed knowledge layer
- avoid source changes outside the managed artifact set
- validate output before finishing `initialize` or `update`

## Validation

Before completing `initialize` or `update`, Blitz should validate at least these areas:

- coverage of scopes and instruction globs
- consistency across artifacts
- file coverage and method coverage
- workflow completeness
- documentation coverage
- workflow-constraint coverage
- fingerprint presence and merge reporting
- secret scanning

## What Blitz does not do

Blitz does not:

- modify application source files
- modify tests or runtime configuration outside the managed knowledge layer
- act as a background daemon
- invent undocumented commands or behavior

If a fact is uncertain, Blitz should prefer explicit confirmation over guesswork.

## Repository-specific policy

Project policy should usually live outside Blitz, in `/.github/instructions/`.

Typical examples are:

- testing requirements
- documentation requirements
- frontend verification rules
- Docker execution preferences

Use Blitz to maintain shared repository knowledge.
Use instruction files to define project policy.

This separation is important.
Blitz defines how knowledge is maintained.
Instruction files define what the repository expects.

## Changing Blitz

When you change Blitz itself:

1. Update `blitz.spec.md` first.
2. Mirror the same change in `blitz.md`.
3. Run Blitz in `initialize` or `update` mode.
4. Run `audit` to verify the result.

Keep `blitz.md` and `blitz.spec.md` aligned.

## What good output looks like

Blitz is working correctly when:

- managed knowledge files reflect the current repository
- scope boundaries and key files are clear
- instructions match real workflows
- user overrides are preserved
- no duplicate guidance appears across artifacts

## Source of truth

This README is the user-facing documentation.

For the full contract, read `blitz.spec.md`.
For the executable behavior, read `blitz.md`.