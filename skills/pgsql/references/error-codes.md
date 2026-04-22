# Error Handling

## PostgreSQL Error Codes

| Error Code | Meaning | Resolution |
|------------|---------|------------|
| 28P01 | Authentication failed | Check username/password |
| 3D000 | Database does not exist | Check database name |
| 42P01 | Table does not exist | Check table name spelling |
| 23505 | Unique constraint violation | Check for duplicate unique fields |
| 23503 | Foreign key constraint violation | Handle related data in other tables first |
| 42703 | Column does not exist | Check column name spelling |
| 42P07 | Table already exists | Use `IF NOT EXISTS` or rename |

## Connection Errors

| Error | Likely Cause | Resolution |
|-------|--------------|------------|
| could not connect to server | PostgreSQL not running or host unreachable | Check PostgreSQL service status and host |
| Connection refused | Wrong port or firewall blocking | Verify port (default 5432) |
| FATAL: database does not exist | Database name mismatch | Verify database name |
| FATAL: password authentication failed | Wrong credentials | Check username and password |

## SQL Injection Prevention

**ALWAYS use parameterized queries:**

```python
# Correct: parameterized query
cur.execute("SELECT * FROM users WHERE name = %s AND email = %s", (name, email))

# Wrong: string concatenation (SQL injection risk!)
cur.execute(f"SELECT * FROM users WHERE name = '{name}'")
```

**Key rules:**
- Never concatenate user input directly into SQL strings
- Use `%s` placeholders (psycopg2) or `$1, $2` (psycopg3)
- Pass values as a tuple in the second argument to `execute()`
