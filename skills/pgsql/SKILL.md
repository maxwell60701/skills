---
name: pgsql
description: |
  Use this skill whenever the user mentions PostgreSQL, pgsql, postgres, PostgreSQL operations,
  connecting to a database, running SQL queries, database management, table creation,
  data insertion/update/deletion, or any task requiring interacting with a PostgreSQL database.

  This skill handles connecting to PostgreSQL, executing SELECT queries, generating
  INSERT/UPDATE/DELETE/CREATE/ALTER/DROP scripts, and all other SQL operations.

  Make sure to use this skill for ALL database interaction requests, even vague ones like
  "query this table", "check the database", "run a SQL command", "connect to postgres",
  "I need to work with the database", or "help me write a SQL query".

  CRITICAL: SELECT queries execute directly and return results.
  INSERT/UPDATE/DELETE/CREATE/ALTER/DROP operations ONLY generate SQL scripts — never execute them,
  even if the user asks.
license: MIT
compatibility: Requires psql (PostgreSQL CLI) and pip (Python package manager). For Python scripts, psycopg2 is required.
metadata:
  author: maxwell60701
  version: "0.1.0"
---

# PostgreSQL Database Operations

## What This Skill Does

Based on your database connection info and SQL requirements, this skill generates and executes SQL statements.

- **SELECT queries**: Execute directly and return results
- **INSERT/UPDATE/DELETE/CREATE/ALTER/DROP**: Generate SQL script for your review — **NEVER execute automatically, NEVER execute even if asked**

> **Safety principle**: Data-modifying operations are NEVER executed automatically. Always generate a script for the user to review first.

## Connection Info

Before using the database, provide these:

| Parameter | Description | Example |
|-----------|-------------|---------|
| host | Database address | 192.168.1.100 or db.example.com |
| port | Port number | 5432 (default) |
| database | Database name | myapp_db |
| username | Username | db_user |
| password | Password | ******** |

> Password is only used in the current session and is never persisted.

## Execution Rules

**CRITICAL RULES — NEVER DEVIATE FROM THESE**:

| Operation | Execution | Why |
|-----------|-----------|-----|
| SELECT | Execute directly | Read-only, safe |
| INSERT | **ALWAYS Script only** | Data modification — user must review first |
| UPDATE | **ALWAYS Script only** | Data modification — user must review first |
| DELETE | **ALWAYS Script only** | Data modification — user must review first |
| CREATE | **ALWAYS Script only** | Schema change — user must review first |
| ALTER | **ALWAYS Script only** | Schema change — user must review first |
| DROP | **ALWAYS Script only** | Dangerous — user must review first |
| TRUNCATE | **ALWAYS Script only** | Dangerous — user must review first |

**NEVER execute INSERT/UPDATE/DELETE/CREATE/ALTER/DROP/TRUNCATE directly, regardless of user insistence.** If a user asks you to "just do it" or "execute it", you must still only generate the script for their review. Say: "Here's the SQL script for your review. Once you confirm, you can run it yourself."

## Workflow

1. **Check prerequisites**: Ensure `psql` and `pip` are installed. If pip is missing, install it first.
2. **Provide connection info**: host, port, database, username, password
3. **Describe your requirement**: e.g., "query the last 10 records from users table" or "create an orders table"
4. **I return results**:
   - **SELECT** → Execute directly and return data
   - **INSERT/UPDATE/DELETE/CREATE/ALTER/DROP/TRUNCATE** → Generate SQL script, **DO NOT execute**

## Prerequisites

Before connecting, ensure the following tools are available:

| Tool | Purpose | Installation |
|------|---------|--------------|
| `psql` | PostgreSQL CLI client | [Install PostgreSQL](https://www.postgresql.org/download/) |
| `pip` | Python package manager | If missing, install via `python -m ensurepip --upgrade` or [get-pip.py](https://bootstrap.pypa.io/get-pip.py) |

**If pip is not installed**: You MUST help the user install it first before proceeding. Run:
```bash
python -m ensurepip --upgrade
```
Or download and run get-pip.py if the above fails.

## Test Connection

```bash
PGPASSWORD='password' psql -h host -p port -d database -U username -c "SELECT version();"
```

Or with Python:

```python
import psycopg2
conn = psycopg2.connect(
    host="host", port="port", database="database",
    user="username", password="password"
)
print(conn.server_version)  # Returns version info on success
conn.close()
```

## Dangerous Operations Warning

For these high-risk operations, generate the script and add extra warnings:

- `DROP TABLE`, `DROP DATABASE` — table/database will be permanently deleted
- `TRUNCATE TABLE` — all data will be cleared
- Bulk `DELETE` (no WHERE or loose WHERE)
- `ALTER TABLE DROP COLUMN` — column and data will be permanently deleted

**Recommendation**: Wrap these in a transaction — `BEGIN`, verify, `COMMIT`, or `ROLLBACK` if issues.

## Output Formats

### SELECT Results

Results displayed in table format:

```
 id | name  | email               | status
----+-------+---------------------+--------
  1 | John  | john@example.com    | active
  2 | Jane  | jane@example.com    | active
```

### Generated Script

Scripts displayed in code blocks:

```sql
-- Insert a record into users table
INSERT INTO users (name, email, status)
VALUES ('New User', 'new@example.com', 'active');
```

## SQL Examples

For detailed SQL examples (SELECT, INSERT, UPDATE, DELETE, CREATE TABLE, ALTER, indexes, transactions), see:
- `references/sql-examples.md` — Common SQL patterns and operations
- `references/error-codes.md` — PostgreSQL error codes and resolutions
