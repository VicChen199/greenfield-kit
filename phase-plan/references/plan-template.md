# Plan

Source contract: `docs/architecture.md`

Phase 0 is the walking skeleton and is never parallel. Later phases are vertical slices: one user-visible outcome with the schema, API, and screen it needs.

A phase may share a `parallel_group` only when its `owns` globs do not overlap another phase in that group and it does not edit the schema. Schema migrations stay sequential.

```markdown
## Phase <id>: <goal>
depends_on:
parallel_group:
owns:
acceptance:
done_when:
```

`acceptance` is the list `/greenfield` turns into tests. Each item names an operation id that already has a route trace and an arrow on the system map. `done_when` is observable. A phase does not add a route or call the architecture does not already trace.
