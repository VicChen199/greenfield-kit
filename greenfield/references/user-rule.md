# User Rules — replace the whole box

Cursor stores User Rules in account settings. This file is not applied on its own. In Customize → Rules, replace the existing User Rules with the text below. Do not append it. The old outline menu, the tradeoff menu, and the implement-the-plan block are omitted on purpose. The brainstorm skill owns that procedure.

---

When implementing or fixing anything in a web application (UI, layout, styling, routing, client state, or rendered data), verify your work in the browser before declaring the task complete.

Use this verification workflow:

- Open the app with the available browser tools and exercise the changed feature end to end the way a real user would: click, type, submit, navigate.
- A single render screenshot of the changed screen is not verification. Confirm behavior, not just appearance.
- Check every page and route that shares the state, data, or components you touched. Application state must stay consistent across pages: if you changed how state is written or derived, verify the other surfaces that read it.
- Hunt for regressions. The most common failure mode is a change that works in isolation but breaks existing behavior elsewhere in the app. Navigate the surrounding flows and look for what broke.
- Verify the paths and edge states your change touches (empty states, error states, route and flag variants), not only the main path.
- When layout or styling changed, consider whether you need to verify both desktop and mobile viewports.
- If verification finds a problem, fix it and re-verify. Do not finish with unverified UI work.

If no browser tools are available, verify through the closest available substitute (tests, curl against the dev server, rendering scripts) and say what you could not verify.

State points directly in affirmative language. Avoid unnecessary contrastive negation such as "X, not Y," especially clarifications about alternatives the user did not mention.

Whenever you create a git worktree for this project, also open that worktree in a new Cursor window. Run `cursor -n <absolute-worktree-path>` after the worktree exists. Do not move this conversation's workspace root to the worktree. Do not search for or reuse another window that already has the worktree open. Skip only when the user explicitly says not to open a window or the directory is not a git worktree. If `cursor` is not on PATH, say so and give the absolute path and command to run.

When I name specific files or line ranges, change only those. Do not refactor, reformat, rename, or clean up anything else in those files or elsewhere unless I explicitly ask. If another change is required to make the requested edit work, stop and tell me before doing it.

When I ask for a git, shell, or gh command, or for anything I could run myself, give me the exact command to copy and paste. Do not run commit, push, or other side-effecting commands unless I explicitly say to run, commit, or push it. Read-only checks such as git status, git diff, or git log are fine when they help you answer.

When `/architect`, `/survey`, `/phase-plan`, `/feature-plan`, `/tdd-slice`, or `/greenfield` is active, follow that skill's authority section. `/tdd-slice` and `/greenfield` may create and edit the files the slice or phase owns, install the stack the contract names, and commit finished work on the current branch. Do not open a pull request. Do not apply the wait-for-a-pick, change-only-named-files, or copy-paste-the-git-command rules while one of these skills is running. Outside those skills, those rules stay in force. Builder worktrees do not get a new Cursor window.

When I ask for a plan, a design, or a brainstorm, and none of those skills is active, follow the brainstorm skill. Offer two or three options and wait. Do not write code or a long plan until I pick. Do not implement until I explicitly say to implement or I press Build. Do not change a plan's approach while implementing it.
