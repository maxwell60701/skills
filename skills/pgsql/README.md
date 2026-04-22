# PostgreSQL Database Skill

A skill for interacting with PostgreSQL databases using SQL.

## Overview

This skill enables Claude to connect to PostgreSQL databases and execute SQL operations based on user requirements. Users provide database credentials (host, port, database, username, password) and describe their SQL needs, and Claude generates and optionally executes the appropriate SQL statements.

## Execution Rules

- **SELECT queries**: Execute directly and return results
- **INSERT/UPDATE/DELETE/CREATE/ALTER/DROP**: Generate SQL scripts only — never execute automatically

This safety-first approach ensures data modifications are reviewed before execution.

## Usage

```text
# Provide connection info, then describe your requirement:
# "Query the last 10 records from users table"
# "Create an orders table with id, user_id, total_amount, created_at"

Claude will return:
- SELECT → executed results
- INSERT/UPDATE/DELETE/DDL → SQL script for your review
```

## Installation

```bash
npx skills add maxwell60701/pgsql-skill
```

## Connection Parameters

| Parameter | Description | Default |
| --------- | ----------- | ------- |
| host | Database address | - |
| port | Port number | 5432 |
| database | Database name | - |
| username | Username | - |
| password | Password | - |

## SQL Execution Policy

| Operation | Execution | Reason |
| --------- | --------- | ------ |
| SELECT | Execute directly | Read-only, safe |
| INSERT | Script only | Data modification |
| UPDATE | Script only | Data modification |
| DELETE | Script only | Data modification |
| CREATE | Script only | Schema change |
| ALTER | Script only | Schema change |
| DROP | Script only | Dangerous |
| TRUNCATE | Script only | Dangerous |

## Examples

### Query

```sql
SELECT id, name, email, status, created_at
FROM users
WHERE status = 'active'
ORDER BY created_at DESC
LIMIT 10;
```

### Insert

```sql
INSERT INTO users (name, email, status)
VALUES ('John Doe', 'john@example.com', 'active');
```

### Update

```sql
UPDATE users
SET email = 'new@example.com', status = 'active'
WHERE id = 1;
```

### Delete

```sql
DELETE FROM users
WHERE id = 5 AND status = 'inactive';
```

### Create Table

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL REFERENCES users(id),
    total_amount DECIMAL(10,2) NOT NULL DEFAULT 0,
    status VARCHAR(20) NOT NULL DEFAULT 'pending',
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

## License

MIT
