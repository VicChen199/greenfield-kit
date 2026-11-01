# Greenfield kit

This suite is a build pipeline.

**brainstorm** offers two or three approaches and waits. **architect** writes `docs/architecture.md` for a new product, including one sequence diagram per route and one system-map flowchart from the screen through the server and any table or file and back. **survey** writes that same contract for an app that already runs, at `docs/<area>.md`. The name is the area the user asked to map, or the repo directory name when they map the whole app. **phase-plan** turns `docs/architecture.md` into `docs/plan.md`. **greenfield** builds every ready phase from that plan, or every `todo` slice in a named feature file. A feature run reads the survey contract. Four subagents ship in `greenfield/assets/agents`: test-writer, implementer, reviewer, and verifier. **feature-plan** writes one feature on an existing app against the survey contract, and **tdd-slice** builds one slice of it in this chat. **tdd** is the bugfix path.

Invoke them as `/brainstorm`, `/architect`, `/survey`, `/phase-plan`, `/greenfield`, `/feature-plan`, `/tdd-slice`, and `/tdd`.

`greenfield/references/user-rule.md` is Cursor user-rules text. It is not applied by copying the skill. Paste it into Cursor Settings → Rules only if you want those rules.

## Paste this to your agent

```
Copy every skill folder in this repo into my skills directory, verbatim, and stop. The folders are brainstorm, architect, survey, phase-plan, greenfield, feature-plan, tdd-slice, and tdd. On Cursor, if a user Agent Store is listed in your context, write them to skills/<name>/ in that store. If no user store is listed, write them to ~/.cursor/skills/<name>/ and say they are on this machine only. For one project, write them to .cursor/skills/<name>/. On Antigravity, write them to ~/.gemini/config/skills/<name>/ for every workspace, or to .agents/skills/<name>/ for one project. Do not rewrite the files. Do not write into ~/.cursor/skills-cursor/.
```
