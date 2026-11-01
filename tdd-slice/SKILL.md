---
name: tdd-slice
description: Build one slice in this chat. Write failing tests from the feature plan or the named slice, then implement until they pass. The contract is the survey file docs/<area>.md, or docs/architecture.md when that is the only contract. Use when the user invokes /tdd-slice. Every todo slice in a feature file uses /greenfield. A from-scratch multi-phase build uses /greenfield. A bugfix uses /tdd.
disable-model-invocation: true
icon: beaker
color: green
---

# TDD slice

Build one slice in this chat. Tests come from the contract, they fail for the right reason, then the product code is written. Do not launch the greenfield subagents. Do not run a blind pair.

## Authority

While this skill is active, you may create and edit the files this slice owns, install what the stack contract names, and commit the finished slice on the current branch. Do not open a pull request. Do not apply the wait-for-a-pick, change-only-named-files, or copy-paste-the-git-command user rules. Do not start the next slice.

## Contract file

Resolve the contract in this order:

1. The path the user named.
2. The path on the `Source contract:` line of the feature file, when that file exists.
3. The only survey contract in `docs/`. A survey contract is a file `docs/<area>.md` that contains the headings `## Route traces`, `## System map`, and `## API catalog`. `docs/architecture.md`, `docs/plan.md`, `docs/status.md`, and files under `docs/features/` are not survey contracts.
4. `docs/architecture.md`, when no survey contract exists and that file exists.

When more than one survey contract exists and steps 1 and 2 did not name one, stop and list the paths. Do not choose one.

When no contract exists, stop. An app that already runs uses `/survey`. A blank repo uses `/architect`.

## Which slice

1. If the user names `docs/features/<slug>.md`, read it and the contract resolved above.
2. If those two disagree about a journey, route, table, or screen this feature touches, stop. Tell the user to run `/feature-plan` again. Do not write code.
3. If the slice names a method, path, or call that has no route trace or no arrow on the system map, stop. Tell the user to run `/feature-plan`. Do not write that route.
4. Build the slice they named. If they did not name one, build the first slice whose `status` is `todo`.
5. If there is no feature file, build the one slice they described and follow the resolved contract. The slice still only builds operations that have a route trace and an arrow on the system map.

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
