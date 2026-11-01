# Feature: <slug>

Source contract: `docs/architecture.md`

Architecture patched: yes | no

## Slice: <name>
status: todo
goal:
owns:
routes:
acceptance:
done_when:
tables:
migration:
existing_tests:

Add the next slice under the previous one. `status` is `todo` or `done`. `/tdd-slice` builds the first `todo` slice in this chat. `/greenfield` builds every `todo` slice.

`migration` names the schema change. Adding a column, adding a table, or a backfill belongs here. Changing a column type in place does not.

`routes` lists the route-trace ids this slice may build. Each id already has a trace and an arrow on the system map. A slice does not add an operation that is absent from that list.

`existing_tests` lists tests that already pass and must still pass. Write `none` when the repo has no tests yet.
