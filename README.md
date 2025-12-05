# liquibase-command-cheat-sheet

A quick reference guide for common Liquibase CLI commands.

## General Syntax

```bash
liquibase [options] [command] [command-parameters]
```

## Basic Commands

| Command                   | Description                                                                                       |
|:--------------------------|:--------------------------------------------------------------------------------------------------|
| `update`                  | Deploys any changesets in your changelog file that have not been deployed to your database.       |
| `update-testing-rollback` | Updates the database, then rolls back changes to test rollback scripts, then updates again.       |
| `rollback <tag>`          | Rolls back the database to the state it was in when the tag was applied.                          |
| `rollback-count <value>`  | Rolls back the last `<value>` number of changesets.                                               |
| `rollback-to-date <date>` | Rolls back the database to the state it was in at the specific date/time.                         |
| `generate-changelog`      | Reverse engineers an existing database and generates a changelog file.                            |
| `snapshot`                | Captures the current state of the database.                                                       |
| `diff`                    | Compares two databases (or a database and a changelog) and reports differences.                   |
| `diff-changelog`          | Compares databases and generates a changelog with the differences (useful for appending changes). |
| `tag <tag_string>`        | Tags the current database state for future rollbacks.                                             |
| `status`                  | Reports which changesets have been applied and which are pending.                                 |
| `validate`                | Checks the changelog for errors.                                                                  |
| `clear-checksums`         | Clears current checksums from the DATABASECHANGELOG table (useful if you modified a changeset).   |
| `history`                 | Lists the deployed changesets.                                                                    |
| `db-doc <output_dir>`     | Generates Javadoc-style documentation for the existing database schema.                           |

## Common Command Parameters

These are often defined in `liquibase.properties`, but can be passed via CLI.

| Parameter         | Description                                                             |
|:------------------|:------------------------------------------------------------------------|
| `--changeLogFile` | The root changelog file (e.g., `db.changelog.xml` or `changelog.yaml`). |
| `--url`           | The JDBC connection URL for the database.                               |
| `--username`      | Database username.                                                      |
| `--password`      | Database password.                                                      |
| `--driver`        | The JDBC driver class name (e.g., `org.postgresql.Driver`).             |
| `--classpath`     | Path to the JDBC driver jar file.                                       |
| `--contexts`      | Contexts to execute (e.g., `test`, `prod`).                             |
| `--labels`        | Label expression to filter changesets.                                  |

## Examples

### 1. Update Database

Applies pending changes to the database defined in `db.changelog.xml`.

```bash
liquibase --changeLogFile=db.changelog.xml update
```

### 2. Rollback the Last 3 Changes

```bash
liquibase --changeLogFile=db.changelog.xml rollback-count 3
```

### 3. Rollback to a Specific Tag

```bash
liquibase --changeLogFile=db.changelog.xml rollback v1.0
```

### 4. Generate Changelog from Existing Database

Creates a `changelog.xml` from the database connected via URL.

```bash
liquibase --changeLogFile=changelog.xml --url=jdbc:postgresql://localhost:5432/mydb --username=user --password=pass generate-changelog
```

### 5. Diff Changelog (Generate Migration Script)

Compares the target database against a reference database (or model) and appends the differences to a changelog.

```bash
liquibase --changeLogFile=new_changes.xml \
  --url=jdbc:postgresql://localhost:5432/target_db \
  --referenceUrl=jdbc:postgresql://localhost:5432/reference_db \
  diff-changelog
```

### 6. Tag the Database

Marks the current state with a version tag.

```bash
liquibase tag v1.0
```

### 7. Check Status

See which changesets represent "pending" migrations.

```bash
liquibase --changeLogFile=db.changelog.xml status
```