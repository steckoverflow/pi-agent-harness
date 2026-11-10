---
name: build-loop
description: Implement a spec test-first, one acceptance criterion at a time, red to green. Use when the user says "build", "implement", "build loop", "start coding DDM-123", or after ticket-to-spec has produced a spec.
argument-hint: "[JIRA-KEY or spec path]"
---

# Build Loop

Implement a spec in small, verified steps. Each step goes red, then green, then gets committed.

## 0. Load the spec

Find the spec: the path given, `specs/<KEY>.md`, or ask. No spec and no ticket? Offer `ticket-to-spec` first. For a trivial change, the user may waive the spec; then write the acceptance criteria inline in your first reply.

Read `GLOSSARY.md`, ADRs, and `CODING_STANDARDS.md`/`CONTRIBUTING.md` if present. Work on a branch named after the key (`DDM-123-short-name`), never directly on `main`.

## 1. Find the loop command

Identify one fast command that runs the relevant tests (e.g. `pnpm vitest run path`, `pytest tests/x`, `go test ./pkg/...`). Run it once before changing anything. It must be green, or the pre-existing failures must be recorded so you do not mistake them for yours. If no test setup exists, say so and agree on the smallest one with the user before continuing.

## 2. Per acceptance criterion

Take the criteria in the order the spec lists them, unless one depends on another.

1. **Red.** Write one test at the seam that matches how a caller sees the behaviour. Run the loop and watch it fail *for the expected reason*: an assertion on the behaviour, not an import or syntax error.
2. **Green.** Write the smallest change that passes (ponytail rules apply). Run the loop.
3. **Widen.** Run the full suite plus typecheck and lint when they are cheap. Fix what you broke.
4. **Commit.** One commit per criterion: `<KEY>: <criterion in imperative mood>`. Tick the box in the spec in the same commit.

No correct seam for a test (e.g. pure config or infra)? State that, verify by another observable check (a command, a curl), and note it in the commit message.

Stuck after two failed green attempts on one criterion? Stop guessing. Switch to the `diagnosing-bugs` skill with the red test as the feedback loop.

## 3. Scope discipline

- Something the spec did not ask for: do not build it. Add it under "Open questions" in the spec and tell the user.
- A spec criterion that turns out wrong or impossible: stop and ask; do not reinterpret it silently.

## 4. Done

Done when every criterion is ticked, the full suite is green, and the working tree is clean. Then:

1. Report: criteria done, tests added, anything skipped or unverified.
2. Offer `/skill:code-review` against the merge-base with `main`.
