---
name: phase-plan
description: Turn docs/architecture.md into docs/plan.md for a from-scratch build, then stop. Use when the user invokes /phase-plan. A single feature on an existing app uses /feature-plan instead.
disable-model-invocation: true
icon: git-branch
color: purple
---

# Phase plan

Turn the finished architecture into the from-scratch plan `/greenfield` executes. A feature file is the other file `/greenfield` can run.

## Authority

While this skill is active, ignore the wait-for-a-pick user rule. Write only `docs/plan.md`. Do not edit `docs/architecture.md`. Do not write application code. Do not commit. Do not open a pull request.

## Steps

1. Read `docs/architecture.md`. If it is missing, stop. An app that already has a route, a schema, or a screen uses `/survey`. A blank repo uses `/architect`.
2. If Open decisions has any item, stop and list those items. Do not plan around an open stack or schema choice.
3. Read the system map, the route traces, and the API catalog. If any of those sections is missing, stop and tell the user to run `/architect`. If an operation id is not in all three, stop and name it. Do not plan that operation.
4. Write `docs/plan.md` using [references/plan-template.md](references/plan-template.md).
5. Phase 0 is the walking skeleton: toolchain, test command, one screen, and the dev server. It has no `parallel_group`. Include a database only when a route trace names a database stage. Include a health route only when a route trace's request is that route. Do not add a route or call that has no trace and no arrow on the system map.
6. Every later phase is a vertical slice. A person can demo the phase on its own. A phase that is only "all migrations" or only "all routes" is not a phase.
7. Give each phase `id`, `goal`, `depends_on`, `parallel_group`, `owns`, `acceptance`, and `done_when`. Acceptance names operation ids that already have a route trace and an arrow on the system map.
8. Put two phases in the same `parallel_group` only when their `owns` globs do not overlap and neither phase edits the schema.

## Done

Stop. The next command is `/greenfield`, started as a Custom Mode, with the message `Build.`
