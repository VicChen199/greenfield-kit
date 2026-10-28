---
name: greenfield
description: Build every ready phase in docs/plan.md. Test-writer and implementer run together from the contract in separate worktrees, then the parent merges, reviews, and verifies. Use when the user invokes /greenfield. Start it as a Custom Mode. One feature on an existing app uses /tdd-slice.
disable-model-invocation: true
icon: rocket
color: orange
---

# Greenfield

Execute `docs/plan.md`. The parent coordinates. The parent does not write product code or tests.

Start this skill as a Custom Mode so it stays in context for the session. The user message `Build.` means execute the plan.

## Authority

While this skill is active, you may create and edit the files the plan owns, install the stack the architecture names, and commit each finished phase on the current branch. Do not open a pull request. Do not push unless the user asks. Do not apply the wait-for-a-pick, change-only-named-files, or copy-paste-the-git-command user rules. Builder worktrees do not get a new Cursor window. Do not run `cursor -n` for them.

Do not rewrite `docs/architecture.md` or `docs/plan.md`. If the code and the contract disagree, stop and report. Append only to `docs/status.md`.

## Before the first phase

1. If `docs/architecture.md` or `docs/plan.md` is missing, stop and name the skill that writes it: `/architect`, then `/phase-plan`. Do not invent a plan.
2. If Open decisions in the architecture has any item, stop and list those items.
3. Read the system map, the route traces, and the API catalog. Stop if any section is missing. Stop if a method, path, or call named in `docs/plan.md` has no route trace or no arrow on the system map. Name that operation. Do not invent it and do not add it to the architecture.
4. Copy this skill's `assets/agents/*.md` into the project's `.cursor/agents/` when a file of that name is missing. The skill directory is the folder that contains this `SKILL.md`.
5. If `.cursor/rules/stack.mdc` is missing, write it from the architecture's stack section using [references/stack-rule.md](references/stack-rule.md). Set `alwaysApply: true`.
6. Create `docs/status.md` when it is missing. Add `.worktrees/` to `.gitignore` when that entry is missing.

## Who does the work

Launch a named subagent when `test-writer`, `implementer`, `reviewer`, or `verifier` is available. Otherwise launch `generalPurpose` and paste the matching file from this skill's `assets/agents/` into the task prompt.

Each subagent starts blank. The task prompt carries the phase acceptance text, the route-trace ids that acceptance names, the paths that role may touch, the worktree path, the branch name, and the test command. It does not carry the other role's files, diff, or transcript. A subagent does not add a method, path, or call that those traces do not name.

Send the blind pair in one message so both start together. Give each its own git worktree created from the same commit:

- `.worktrees/greenfield-<phase>-tests` on branch `greenfield/<phase>-tests`
- `.worktrees/greenfield-<phase>-impl` on branch `greenfield/<phase>-impl`

Do not use a cloud subagent environment unless the user asked for one.

## Blind pair

This is the default for every phase after phase 0 whose test files and product files are different files.

1. `test-writer` sees the acceptance text and writes tests only. In its worktree those tests must fail, because the new product code is not there. A passing test goes back to `test-writer`. An `ambiguous` return stops the phase.
2. `implementer` sees the same acceptance text and writes product code only. It does not receive the tests. An `ambiguous` return stops the phase.
3. Merge both branches into the current branch. A conflict stops the phase. Report the files. Remove the worktrees after a clean merge.
4. Run the test command. This is the first time the two outcomes meet.

## Sequential phases

Phase 0 runs on the current branch, in order, because it creates the shared project files.

1. `implementer` builds the skeleton from the phase 0 acceptance: toolchain, test command, one screen, and the dev server. It includes a database only when a named route trace has a database stage. It includes a health route only when a named route trace's request is that route.
2. `test-writer` adds tests for that acceptance and runs them. The skeleton is already on this branch, so a passing run means phase 0 meets the contract. A failure for the missing behavior continues to step 3. A failure for setup or syntax goes back to `test-writer`.
3. When the tests fail for the contract reason, `implementer` gets the failure output and no test source, and makes the skeleton satisfy the contract.

A later phase where both roles would have to edit one file, usually the schema, uses the same separation without overlapping edits. The schema file belongs to `implementer`. `test-writer` does not edit it. Run `test-writer` to completion, then `implementer`, each in its own worktree, then merge. They still do not see each other's files.

Two phases in one `parallel_group` may each run a blind pair at the same time. Their `owns` paths do not overlap. Do not start a pair whose paths overlap a pair that is still running.

## After the merge is green

Launch `reviewer` and `verifier` in one message so both start together. `reviewer` reads the acceptance text and the merged diff. It does not edit. `verifier` reruns the tests. For a UI phase it exercises the flow in the browser, including the empty state, the error state, and any other page that reads the same state.

Wait for both before the next phase. A reviewer result other than `clear`, or a verifier result other than `passed`, holds the next phase.

One repair carries both results when either fails. The repair includes the acceptance line and what is wrong, and the verifier failure output when the verifier failed. It includes no test source. The repair goes to `implementer`.

## Repairs

Three repair attempts for a phase. Then stop and report.

- The assertion disagrees with the contract: `test-writer` receives the contract and the assertion. It does not receive the implementation.
- The behavior disagrees with the contract: `implementer` receives the contract and the failure output. It does not receive the test source.
- Either reading would fit: stop and ask. Do not launch another repair.

Count each repair in `docs/status.md`.

## Finish a phase

When reviewer returns `clear` and verifier returns `passed`, finish the phase. A failed review or a failed verify holds the next phase.

1. Append the phase id, the attempt count, `passed`, and the commit SHA to `docs/status.md`.
2. Commit the phase on the current branch. Do not push. Do not open a pull request.
3. Start the next ready phase. A phase is ready when every `depends_on` entry is `passed` in `docs/status.md`.

## Done

When every phase has passed, report the commits and the dev command. When a phase stops early, report the phase, the attempt count, and the last failure. Leave the failing tree in place.
