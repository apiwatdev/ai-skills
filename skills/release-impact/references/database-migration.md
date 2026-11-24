# SQL and database migration impact

SQL analysis must be **evidence-based and environment-aware**. Finding a `.sql` file in a Git diff does **not** mean it must be executed.

## Scan candidates

Inspect changed paths and relevant current migration context, including:

- New, modified, removed or renamed `.sql` scripts.
- ORM/framework migration files and migration history/runner configuration.
- Schema, table/column/index/constraint changes.
- Stored procedures, functions, views, triggers and type definitions.
- Data backfill, corrections, seed/import and manual maintenance scripts.
- Configuration for migration tools, DB connection targets and deployment ordering.

Do not treat an unchanged file as new solely because it is present in the TO snapshot.

## Determine actual actions

For each candidate:

1. Record exact source file path, change type, and the observed DDL/DML or migration intent.
2. Establish how the project normally deploys migrations: ordered files, migration framework, manual SQL, pipeline step or external process. If not established, **ask**.
3. Establish the target database/environment and currently deployed baseline.
4. Check whether this migration (or equivalent change) has already been applied to the environment. Accept only verifiable migration tracking, deployment logs, database inspection provided by the developer, or explicit developer confirmation. **Do not execute database queries to collect history as an implicit side effect**; provide the necessary query for manual approval only if the environment requires it.
5. If a script changed **after** a previous application, do not automatically run the modified script again; inspect intended upgrade strategy, migration immutability and rollback/data considerations. Flag for developer/DBA decision.
6. Check SQL dependency sequence **only from evidence**, such as migration numbering, explicit orchestration definitions, FK dependencies or documented prerequisites. Lexical filename order alone does not prove safe execution order.
7. Check idempotency guards and safe re-run properties **without assuming they are adequate**. Assess transactions, existing data, backfill prerequisites, destructive DDL, performance/locking and one-way transformations when evident.
8. Mark script execution `Required` **only if** repository/developer evidence plus migration history establishes it is needed for this environment and safe applicable sequence is confirmed. Otherwise use `Review Required` or `Unknown`, and explain exactly what needs verification.
9. If no database impact is found in the compared repository, report "No database changes identified in the inspected change set" rather than claiming there are no database changes anywhere.

## Required SQL candidate table

| Column | Description |
| --- | --- |
| Script / migration | Exact repository file and object/migration ID |
| Change | Added / Modified / Deleted / Renamed; actual behavior |
| Execution state | Not applied / Applied / Unverified (only from real evidence) |
| Classification | Required / Review Required / Not Applicable / Unknown |
| Dependency / order | Supported order or Pending confirmation |
| Risk | Known non-idempotence, downtime, data loss, irreversible operation |
| Evidence | Paths, migration log reference or confirmed developer statement |

## Rollback

A rollback of application binaries does not necessarily undo schema/data changes. Never invent a SQL down-script, assume reversibility, or recommend restoring an unknown database backup. If rollback requires data preservation, migration correction, compensating SQL, point-in-time recovery, or DBA approval, state that as a **risk/confirmation item**, not an executable instruction.

Protect production data: do not execute, modify, or generate a risky migration for execution under this read-only Skill.
