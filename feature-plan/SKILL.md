---
name: feature-plan
description: Plan one feature on an existing app. Patch docs/architecture.md only for the journey, route, table, or screen this feature changes, write docs/features/<slug>.md, and stop. Use when the user invokes /feature-plan. A blank repo uses /architect and /phase-plan instead.
disable-model-invocation: true
icon: code
color: cyan
---

# Feature plan

Plan one feature against the system that already exists. Patch the architecture only where this feature changes it, write a feature plan, and stop.

## Authority

While this skill is active, ignore the wait-for-a-pick user rule except for the one question batch below. You may edit `docs/architecture.md` only as the narrow patch below, and you may create `docs/features/<slug>.md`. Do not write application code. Do not edit `docs/plan.md`. Do not commit. Do not open a pull request.

## Steps

1. Read `docs/architecture.md`. If it is missing, stop and tell the user to run `/architect`. This skill does not create the first contract.
2. Read the current schema, routes, and tests in the repo so the plan matches the code, not only the document.
3. Name the feature with a kebab-case slug.
4. If a schema or stack choice is not already in the architecture or the user message, ask one batched round with a recommendation on each question. Write neither file until they answer.
5. Patch `docs/architecture.md` only when the feature adds or changes a journey, route, table, or screen. Add the missing hops in the existing format. Each new or changed journey keeps all six hops: screen, request, handler, tables and invariant, response, and what the screen does with that response, including empty and error states. A new operation is written into the architecture before the feature file. Write its route trace once, with caller, request, handler, stages, response, screen, and a sequence diagram. Fill its system-map arrow and its API catalog row from that trace. The journey names that trace id. An operation the architecture already traces keeps its id. Update that trace, then refill its arrow and catalog row from the trace, and leave every other trace as it was. Do not restyle the document, reorder it, or rewrite journeys and stack choices that this feature does not change. When the feature only changes behavior on an operation the architecture already traces, update that trace, refill its arrow and catalog row, and leave the rest of the file untouched.
6. Write `docs/features/<slug>.md` from [references/feature-template.md](references/feature-template.md). Record whether the architecture was patched. Each slice's `routes` lists operation ids that, after this patch, have a route trace and an arrow on the system map.
7. Split the feature into vertical slices a person could demo in order. Migration, then route, then screen is the usual split. Keep each slice small.
8. Give each slice `status`, `goal`, `owns`, `routes`, `acceptance`, `done_when`, `tables`, `migration`, and `existing_tests`. Set `status` to `todo`.

## Done

Stop. Tell the user the feature file path. They edit `docs/architecture.md` and the feature file before `/tdd-slice`. If they edit the architecture after this plan exists, they run `/feature-plan` again before `/tdd-slice`.

The build command is:

```text
/tdd-slice
Build the next slice in docs/features/<slug>.md.
```
