# PostgreSQL Database Skill

A skill for interacting with PostgreSQL databases using SQL.

## Overview

This skill enables Claude to connect to PostgreSQL databases and execute SQL operations based on user requirements. Users provide database credentials (host, port, database, username, password) and describe their SQL needs, and Claude generates and optionally executes the appropriate SQL statements.

## Execution Rules

- **SELECT queries**: Execute directly and return results
- **INSERT/UPDATE/DELETE/CREATE/ALTER/DROP**: Generate SQL scripts only — never execute automatically

This safety-first approach ensures data modifications are reviewed before execution.

## Usage

```
# Provide connection info, then describe your requirement:
# "Query the last 10 records from users table"
# "Create an orders table with id, user_id, total_amount, created_at"

Claude will return:
- SELECT → executed results
- INSERT/UPDATE/DELETE/DDL → SQL script for your review
```

## Installation

```bash
npx skills add your-username/pgsql-skill
```

Replace `your-username` with your GitHub username.

## License

Apache 2.0
