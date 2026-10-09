# STAND.md — Standards & Working Agreements

The standards this repository holds itself to. [CLAUDE.md](./CLAUDE.md) defers to
this file on questions of *how* work gets done; this file is the authority.

These are defaults, not laws. Deviating is fine when there is a reason — state the
reason in the pull request rather than leaving the reader to guess.

> **Status: greenfield.** Tool-specific rules are marked `TODO` until the stack is
> chosen. The principles below hold regardless of language.

## Code

- **Clarity over cleverness.** Code is read far more often than it is written.
  A longer, obvious implementation beats a short, subtle one.
- **Make the boundaries explicit.** A module should be describable in one
  sentence. If it needs "and" twice, it is doing too much.
- **Errors are part of the interface.** Handle them or propagate them
  deliberately. Never swallow an error silently, and never log-and-continue
  where the caller needed to know.
- **No dead code.** Delete it rather than commenting it out; version control is
  the archive.
- **Comments explain *why*.** The code already says what it does. Comment the
  constraint, the trade-off, or the bug being worked around.
- **Formatting is not a discussion.** It is enforced by a tool, applied on save
  or in a pre-commit hook, and never debated in review.

Language-specific style: TODO (formatter, linter, and their configuration).

## Commits

- One logical change per commit. If the message needs a bulleted list of
  unrelated items, it should have been several commits.
- Imperative subject line, under ~72 characters, no trailing period:
  `Add retry policy to payment client`, not `added retries` or `fixes`.
- The body explains *why* the change is being made and anything a reviewer could
  not infer from the diff. Wrap at ~72 characters.
- Reference the issue it closes (`Closes #123`) when there is one.
- Never commit secrets, large binaries, or generated artifacts.

## Branches

- Branch off the default branch; keep branches short-lived and single-purpose.
- Naming: `<type>/<short-description>` — e.g. `feat/order-aggregate`,
  `fix/duplicate-charge`, `chore/bump-deps`.
- Integrate the base branch into a long-running branch rather than letting it
  drift. Do not rewrite history on a branch anyone else may have checked out.

## Tests

- Every bug fix lands with a test that fails before the fix and passes after.
  That test is the proof, and the regression guard.
- Test behaviour through the public interface, not private implementation
  details — tests coupled to internals block the refactors they should enable.
- A test must be able to fail. A test that passes against a broken
  implementation is worse than no test, because it buys false confidence.
- Deterministic by default: no reliance on wall-clock time, network access,
  ordering, or shared mutable state. Inject the clock and the I/O.
- A flaky test is a bug. Fix it or delete it; never leave it to be re-run until
  it passes.

Coverage expectations and the test runner: TODO.

## Review

- **Every change is reviewed before it merges**, including small ones.
- Reviewers look for: correctness, unhandled failure modes, missing tests, and
  whether the change fits the design. Style belongs to the linter.
- Distinguish blocking concerns from suggestions, and say which is which. Prefix
  optional notes with `nit:`.
- Review the change that was asked for. Adjacent improvements are follow-up
  issues, not review comments that hold a merge hostage.
- Authors respond to every thread — implemented, or with a reason why not.
  Silence is not a response.
- Keep pull requests small enough to review carefully. A diff nobody can hold in
  their head gets rubber-stamped, which is the same as not being reviewed.

## Documentation

- The README covers: what this is, how to run it, and how to run the tests.
  If a newcomer cannot get to a passing test suite from the README, it is wrong.
- Update `CLAUDE.md`'s command table in the same change that alters a command.
  A stale command table is actively misleading.
- Document decisions that were genuinely contested and their reasoning, so the
  next person does not relitigate them without the context.

## Dependencies

- A new runtime dependency needs a justification: what it does, why writing it
  ourselves is worse, and how maintained it is.
- Pin versions and commit the lockfile.
- Keep development and runtime dependencies separated.
