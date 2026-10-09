# CLAUDE.md

Guidance for [Claude Code](https://claude.com/claude-code) when working in this repository.

> **Status: greenfield.** This repo has no application code yet. Sections marked
> `TODO` are placeholders — fill them in as the project takes shape, and delete
> this notice once the first real code lands.

## Project overview

TODO — one paragraph: what this project does, who uses it, and the problem it solves.

## Commands

The canonical commands for working in this repo. Keep this table accurate; it is
the first thing Claude reads before running anything.

| Task | Command |
| --- | --- |
| Install dependencies | TODO |
| Run locally | TODO |
| Run all tests | TODO |
| Run a single test | TODO |
| Lint | TODO |
| Format | TODO |
| Type-check | TODO |
| Build | TODO |

Prefer running the narrowest check that covers a change (a single test file, the
affected package) over the full suite, then widen before committing.

## Repository layout

TODO — the directories that matter and what belongs in each. Describe the
*boundaries* ("HTTP handlers only; no business logic") rather than listing files,
which go stale.

## Conventions

Coding standards, commit format, review expectations, and testing requirements
live in [STAND.md](./STAND.md). Read it before making changes; it is the
authority on *how* work is done here, and this file defers to it.

## Working agreements for Claude

- **Match the surrounding code.** Naming, comment density, error handling, and
  module structure should look like what is already there, not like a generic
  best-practice template.
- **Ask before adding a dependency.** New runtime dependencies are a long-term
  commitment. Propose the dependency and the reason before installing it.
- **Don't widen the task.** Fix what was asked. Note adjacent problems you spot
  rather than silently folding them into the same change.
- **Never weaken a test to get green.** Skipping, deleting, or loosening a failing
  test to pass CI is not a fix. Root-cause it, or say plainly why you can't.
- **Verify before claiming done.** Run the relevant checks and report what
  actually happened, including failures and anything you skipped.
- **Secrets stay out of the repo.** No credentials, tokens, or API keys in code,
  config, commit messages, or test fixtures.

## Gotchas

TODO — the non-obvious things that waste time: required env vars, services that
must be running, slow or flaky suites, generated files that must not be hand-edited.
