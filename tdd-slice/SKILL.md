---
name: tdd-slice
description: Build one slice in this chat. Write failing tests from the feature plan or the named slice, then implement until they pass. Use when the user invokes /tdd-slice. A from-scratch multi-phase build uses /greenfield. A bugfix uses /tdd.
disable-model-invocation: true
icon: beaker
color: green
---

# TDD slice

Build one slice in this chat. Tests come from the contract, they fail for the right reason, then the product code is written. Do not launch the greenfield subagents. Do not run a blind pair.

## Authority

While this skill is active, you may create and edit the files this slice owns, install what the stack contract names, and commit the finished slice on the current branch. Do not open a pull request. Do not apply the wait-for-a-pick, change-only-named-files, or copy-paste-the-git-command user rules. Do not start the next slice.

## Which slice

1. If the user names `docs/features/<slug>.md`, read it and `docs/architecture.md`.
2. If those two disagree about a journey, route, table, or screen this feature touches, stop. Tell the user to run `/feature-plan` again. Do not write code.
3. If the slice names a method, path, or call that has no route trace or no arrow on the system map, stop. Tell the user to run `/feature-plan`. Do not write that route.
4. Build the slice they named. If they did not name one, build the first slice whose `status` is `todo`.
5. If there is no feature file, build the one slice they described and follow `docs/architecture.md` when it exists. Say so when the contract file is missing. The slice still only builds operations that have a route trace and an arrow on the system map.

## Tests, then code

1. Write the smallest tests that lock the slice acceptance: one schema or invariant test, one route test, and one user-visible behavior when the slice has a UI. Encode the contract, not the current implementation.
2. Run those tests before any product change for this slice. They must fail because the behavior is absent. A test that already passes gets corrected before implementation. A test that fails for a setup or syntax reason gets corrected before implementation.
3. Implement the slice until those tests pass. Do not edit the tests. Do not weaken an existing assertion.
4. Run the tests listed under `existing_tests`. They stay green.
5. When the slice has a UI, exercise the flow in the browser: the main action, the empty state, the error state, and any other page that reads the same state.
6. Set this slice's `status` to `done` in the feature file. Leave every other line of that file as it was.
7. Commit the slice on the current branch. Do not push. Do not open a pull request.

## Done

Report the failing-before test run and the passing-after test run. Stop. The user starts the next slice with another `/tdd-slice` message.
