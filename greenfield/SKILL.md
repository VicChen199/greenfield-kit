---
name: greenfield
description: Build every ready phase in docs/plan.md, or every todo slice in a named docs/features/<slug>.md. Test-writer and implementer run together from the contract in separate worktrees, then the parent merges, reviews, and verifies. Use when the user invokes /greenfield. Start it as a Custom Mode. One slice in this chat uses /tdd-slice.
disable-model-invocation: true
icon: rocket
color: orange
---

# Greenfield

Execute `docs/plan.md`, or a feature file the user names. The parent coordinates. The parent does not write product code or tests.

Start this skill as a Custom Mode so it stays in context for the session. The user message `Build.` means execute `docs/plan.md`. A message that names `docs/features/<slug>.md` executes that file's `todo` slices and does not run phase 0.

## Authority

While this skill is active, you may create and edit the files the plan or the slice owns, install the stack the architecture names, and commit each finished phase or slice on the current branch. Do not open a pull request. Do not push unless the user asks. Do not apply the wait-for-a-pick, change-only-named-files, or copy-paste-the-git-command user rules. Builder worktrees do not get a new Cursor window. Do not run `cursor -n` for them.

Do not rewrite `docs/architecture.md` or `docs/plan.md`. On a feature file, set the finished slice's `status` to `done` and leave every other line as it was. If the code and the contract disagree, stop and report. Append only to `docs/status.md`.

## Which file

1. The user message `Build.` executes `docs/plan.md`. If that file is missing, stop and name `/phase-plan`. Do not invent a plan. Do not switch to a feature file.
2. A message that names `docs/features/<slug>.md` executes that file. If it is missing, stop and name `/feature-plan`.
3. A feature run skips phase 0. Slices run in document order. A slice is ready when every slice above it has `status` `done`.

## Before the first phase

1. If `docs/architecture.md` is missing, stop. An app that already has a route, a schema, or a screen uses `/survey`. A blank repo uses `/architect`. Do not invent a contract.
2. If Open decisions in the architecture has any item, stop and list those items.
3. Read the system map, the route traces, and the API catalog. Stop if any section is missing. Stop if a method, path, or call named in the plan or the feature file has no route trace or no arrow on the system map. Name that operation. Do not invent it and do not add it to the architecture.
4. On a feature run, if the feature file and the architecture disagree about a journey, route, table, or screen this feature touches, stop. Tell the user to run `/feature-plan` again. Do not write code.
5. Copy this skill's `assets/agents/*.md` into the project's `.cursor/agents/` when a file of that name is missing. The skill directory is the folder that contains this `SKILL.md`.
6. If `.cursor/rules/stack.mdc` is missing, write it from the architecture's stack section using [references/stack-rule.md](references/stack-rule.md). Set `alwaysApply: true`.
7. Create `docs/status.md` when it is missing. Add `.worktrees/` to `.gitignore` when that entry is missing.

## Who does the work

Launch a named subagent when `test-writer`, `implementer`, `reviewer`, or `verifier` is available. Otherwise launch `generalPurpose` and paste the matching file from this skill's `assets/agents/` into the task prompt.

Each subagent starts blank. The task prompt carries the phase or slice acceptance text, the route-trace ids that acceptance names, the paths that role may touch, the worktree path, the branch name, the test command, and, on a feature slice, the `existing_tests` list. It does not carry the other role's files, diff, or transcript. A subagent does not add a method, path, or call that those traces do not name.

Send the blind pair in one message so both start together. Give each its own git worktree created from the same commit. The `<unit>` token is the phase id, or the feature slug plus the slice name in kebab-case.

- `.worktrees/greenfield-<unit>-tests` on branch `greenfield/<unit>-tests`
- `.worktrees/greenfield-<unit>-impl` on branch `greenfield/<unit>-impl`

Do not use a cloud subagent environment unless the user asked for one.

## Blind pair

This is the default for every phase after phase 0, and for every feature slice, whose test files and product files are different files.

1. `test-writer` sees the acceptance text and writes tests only. In its worktree those new tests must fail, because the new product code is not there. A passing new test goes back to `test-writer`. An `ambiguous` return stops the phase or slice.
2. `implementer` sees the same acceptance text and writes product code only. It does not receive the tests. An `ambiguous` return stops the phase or slice.
3. Merge both branches into the current branch. A conflict stops the phase or slice. Report the files. Remove the worktrees after a clean merge.
4. Run the test command. This is the first time the two outcomes meet. On a feature slice, also run the tests listed under `existing_tests`. They stay green. `none` means there is no list.

## Sequential phases

Phase 0 runs on the current branch, in order, because it creates the shared project files. A feature file does not run phase 0.

1. `implementer` builds the skeleton from the phase 0 acceptance: toolchain, test command, one screen, and the dev server. It includes a database only when a named route trace has a database stage. It includes a health route only when a named route trace's request is that route.
2. `test-writer` adds tests for that acceptance and runs them. The skeleton is already on this branch, so a passing run means phase 0 meets the contract. A failure for the missing behavior continues to step 3. A failure for setup or syntax goes back to `test-writer`.
3. When the tests fail for the contract reason, `implementer` gets the failure output and no test source, and makes the skeleton satisfy the contract.

A later phase where both roles would have to edit one file, usually the schema, uses the same separation without overlapping edits. The schema file belongs to `implementer`. `test-writer` does not edit it. Run `test-writer` to completion, then `implementer`, each in its own worktree, then merge. They still do not see each other's files. A feature slice whose `migration` names a schema change uses this same separation.

Two phases in one `parallel_group` may each run a blind pair at the same time. Their `owns` paths do not overlap. Do not start a pair whose paths overlap a pair that is still running. Feature slices stay in document order.

## After the merge is green

Launch `reviewer` and `verifier` in one message so both start together. `reviewer` reads the acceptance text and the merged diff. It does not edit. `verifier` reruns the tests. On a feature slice it also runs `existing_tests`. For a UI phase or slice it exercises the flow in the browser, including the empty state, the error state, and any other page that reads the same state.

Wait for both before the next phase or slice. A reviewer result other than `clear`, or a verifier result other than `passed`, holds the next phase or slice.

One repair carries both results when either fails. The repair includes the acceptance line and what is wrong, and the verifier failure output when the verifier failed. It includes no test source. The repair goes to `implementer`.

## Repairs

Three repair attempts for a phase or slice. Then stop and report.

- The assertion disagrees with the contract: `test-writer` receives the contract and the assertion. It does not receive the implementation.
- The behavior disagrees with the contract, including an `existing_tests` failure: `implementer` receives the contract and the failure output. It does not receive the test source.
- Either reading would fit: stop and ask. Do not launch another repair.

Count each repair in `docs/status.md`.

## Finish a phase

When reviewer returns `clear` and verifier returns `passed`, finish the phase or slice. A failed review or a failed verify holds the next one.

1. Append the phase id or the feature slug and slice name, the attempt count, `passed`, and the commit SHA to `docs/status.md`.
2. On a feature file, set this slice's `status` to `done`. Leave every other line of that file as it was.
3. Commit the phase or slice on the current branch. Do not push. Do not open a pull request.
4. Start the next ready phase. A phase is ready when every `depends_on` entry is `passed` in `docs/status.md`. On a feature file, start the next slice whose `status` is `todo`.

## Done

When every phase has passed, or every slice in the named feature file is `done`, report the commits and the dev command. When a phase or slice stops early, report it, the attempt count, and the last failure. Leave the failing tree in place.
