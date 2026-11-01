---
name: survey
description: Map an app that already runs into docs/<area>.md, then stop. The name is the area the user asked to map, or the repo directory name when they map the whole app. Use when the user invokes /survey. A blank repo uses /architect. One feature on an app that already has this contract uses /feature-plan.
disable-model-invocation: true
icon: search
color: teal
---

# Survey

Read the app that already runs and write the system contract. Stop. The contract is the path from a person's action on a screen, through the server and the database, and back to that screen.

## Authority

While this skill is active, ignore the wait-for-a-pick user rule except for the one question batch below. Write only `docs/<area>.md`, chosen below. Do not write `docs/architecture.md`. Do not write application code, a phase plan, or a feature plan. Do not commit. Do not open a pull request.

## Contract file

The contract is `docs/<area>.md`. `<area>` is the kebab-case name of the area the user asked to map. Kebab-case is lowercase words separated by hyphens, with every other character removed. When the user asks to map the whole app and names no area, `<area>` is the repo directory name, converted to kebab-case. The file is never `docs/architecture.md`.

If the derived name is `architecture`, `plan`, `status`, or `features`, stop and ask for an area name.

If `docs/<area>.md` already exists, stop. One feature uses `/feature-plan`. This skill does not replace a contract.

## Steps

1. If the repo has no route, schema, or screen, stop and tell the user to run `/architect`.
2. Read the schema, routes, screens, and tests. The contract comes from that code. When the user named an area, read the code for that area.
3. If a stack or schema choice is not visible in the repo and the user message does not answer it, ask one batched round with a recommendation on each question. Write nothing until they answer.
4. Write `docs/<area>.md` from [../architect/references/architecture-template.md](../architect/references/architecture-template.md). Every primary journey has all six hops: screen, request, handler, tables and invariant, response, and what the screen does with that response, including empty and error states. Write each route trace once, from an operation the code already has. An operation is one exact HTTP route or one in-process call. Fill the system map and the API catalog from those traces: one arrow and one catalog row per trace, using that trace's id, request, and stages. Do not add an arrow, a row, or a journey operation that has no trace. Do not add a route or call the code does not have.
5. An operation you cannot trace from the code goes under Open decisions. Name the file. Do not put it on the system map.
6. Leave Open decisions empty only when the map and the catalog match the traces and every trace has caller, request, handler, stages, response, screen, and a sequence diagram.

## Done

Stop. Tell the user the contract path. The next command for one feature is `/feature-plan`. Desired behavior that the code does not implement waits for `/feature-plan`.
