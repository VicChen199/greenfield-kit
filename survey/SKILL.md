---
name: survey
description: Map an app that already runs into docs/architecture.md, then stop. Use when the user invokes /survey. A blank repo uses /architect. One feature on an app that already has this contract uses /feature-plan.
disable-model-invocation: true
icon: search
color: teal
---

# Survey

Read the app that already runs and write the system contract. Stop. The contract is the path from a person's action on a screen, through the server and the database, and back to that screen.

## Authority

While this skill is active, ignore the wait-for-a-pick user rule except for the one question batch below. Write only `docs/architecture.md`. Do not write application code, a phase plan, or a feature plan. Do not commit. Do not open a pull request.

## Steps

1. If `docs/architecture.md` already exists, stop. One feature uses `/feature-plan`. A change to sections the user names uses `/architect`. This skill does not replace a contract.
2. If the repo has no route, schema, or screen, stop and tell the user to run `/architect`.
3. Read the schema, routes, screens, and tests. The contract comes from that code.
4. If a stack or schema choice is not visible in the repo and the user message does not answer it, ask one batched round with a recommendation on each question. Write nothing until they answer.
5. Write `docs/architecture.md` from [../architect/references/architecture-template.md](../architect/references/architecture-template.md). Every primary journey has all six hops: screen, request, handler, tables and invariant, response, and what the screen does with that response, including empty and error states. Write each route trace once, from an operation the code already has. An operation is one exact HTTP route or one in-process call. Fill the system map and the API catalog from those traces: one arrow and one catalog row per trace, using that trace's id, request, and stages. Do not add an arrow, a row, or a journey operation that has no trace. Do not add a route or call the code does not have.
6. An operation you cannot trace from the code goes under Open decisions. Name the file. Do not put it on the system map.
7. Leave Open decisions empty only when the map and the catalog match the traces and every trace has caller, request, handler, stages, response, screen, and a sequence diagram.

## Done

Stop. The next command for one feature is `/feature-plan`. The next command to build this contract as a new product is `/phase-plan`. Desired behavior that the code does not implement waits for `/feature-plan`.
