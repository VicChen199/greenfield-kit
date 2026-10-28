---
name: architect
description: Write or update docs/architecture.md for a new product, then stop. Use when the user invokes /architect for a blank repo or a full contract change. A feature on an existing app uses /feature-plan instead.
disable-model-invocation: true
icon: book-open
color: blue
---

# Architect

Write the system contract and stop. The contract is the path from a person's action on a screen, through the server and the database, and back to that screen.

## Authority

While this skill is active, ignore the wait-for-a-pick user rule except for the one question batch below. Write only `docs/architecture.md`. Do not write application code, a phase plan, or a feature plan. Do not commit. Do not open a pull request.

## When the architecture file is missing

This is the blank-repo path.

1. Read the request.
2. If the user said "hackathon", "pick for me", or "one shot", choose the stack and schema and record every choice under Assumptions. Do not ask. For a browser product with no named stack, use Next.js, TypeScript, and Tailwind, and Postgres when the product stores data.
3. Otherwise ask one batched round, and only about stack or schema choices the message does not answer. Put a recommendation on each question. Write nothing until they answer.
4. Write `docs/architecture.md` from [references/architecture-template.md](references/architecture-template.md). Every primary journey has all six hops: screen, request, handler, tables and invariant, response, and what the screen does with that response, including empty and error states. Write each route trace once. An operation is one exact HTTP route or one in-process call. Fill the system map and the API catalog from those traces: one arrow and one catalog row per trace, using that trace's id, request, and stages. Do not add an arrow, a row, or a journey operation that has no trace.
5. Leave Open decisions empty only when the map and the catalog match the traces and every trace has caller, request, handler, stages, response, screen, and a sequence diagram. Stop after the file is written.

## When the architecture file already exists

Read it. Change only the sections the user named. Leave settled journeys and the stack in place. Do not replace the file. Do not write `docs/features/` or `docs/plan.md`.

Write or update the route trace, then refill that operation's system-map arrow and API catalog row from the trace. Do not describe that operation only in a journey.

If they are adding a feature to an app that already runs, update the contract when they asked for that, then tell them the next command is `/feature-plan`.

## Done

Stop. The next command for a blank repo is `/phase-plan`. The next command for one feature on an existing app is `/feature-plan`.
