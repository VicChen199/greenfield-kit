---
name: brainstorm
description: "Offer two or three named approaches and wait. Use when the user asks for a plan, a design, options, or a brainstorm, and the approach is still open. Stay idle when /architect, /survey, /phase-plan, /feature-plan, /tdd-slice, or /greenfield is active, and stay idle for a bugfix, a small edit, or a build they already approved."
icon: lightbulb
color: yellow
---

# Brainstorm

Help the user choose an approach. Stop before a long plan and before code.

This skill loads on its own when the user is still deciding. It is also a Custom Mode: Option+Enter or Alt+Enter keeps it on for the session.

## Stay idle

Do not run this procedure when any of these skills is active: `/architect`, `/survey`, `/phase-plan`, `/feature-plan`, `/tdd-slice`, `/greenfield`.

Do not run it for a bugfix, a small edit, or a request that already names the approach and says to build it. Do not run it again after the user has picked and Plan mode is writing the implementation plan.

## Authority

While this procedure is running, do not write application code, `docs/architecture.md`, a survey contract `docs/<area>.md`, `docs/plan.md`, or `docs/features/`. Do not commit. Do not open a pull request.

A pick is the user choosing one option. It is not permission to build.

## When the approach is still open

Send two or three named options. Use three when the scopes really differ. Each option says what it covers, what it leaves out, and the main tradeoff. Mark one option as the recommendation and say why in one line.

Then stop. Do not write a file-by-file plan in that message.

Skip the menu when the user's message already picks the approach.

## After they pick

Stop. Name the choice in one line, then send them to the next step:

- A new product, and `docs/architecture.md` does not exist: `/architect`.
- An existing app, and no survey contract exists in `docs/`: `/survey`. A survey contract is `docs/<area>.md` with the headings `## Route traces`, `## System map`, and `## API catalog`.
- One feature, and a survey contract or `docs/architecture.md` exists: `/feature-plan`.
- A change to an existing repo that should be researched and built from Cursor's plan: switch to Plan mode with Shift+Tab. In the CLI, `/plan` is that mode switch.

If the user is already in Plan mode, stop offering options. Let Plan mode research the repo and write its one implementation plan.

## When they build

Build means they pressed Build, or they explicitly said to implement the plan.

Restate the chosen plan in a few bullets. Ask only the questions whose answers would change the implementation. If you asked one, wait. If you have none, say so in one line and implement.

Do not change the plan's approach or scope. Marking a task done is fine. Do not rewrite the plan file unless they explicitly asked for a plan change.

When the work is done, recap what changed, which files changed, how you verified it, and any follow-ups.
