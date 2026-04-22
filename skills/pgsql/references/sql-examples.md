# SQL Examples

## Query (SELECT)

```sql
-- Select all
SELECT * FROM users;

-- With conditions
SELECT id, name, email FROM users WHERE status = 'active' ORDER BY created_at DESC LIMIT 10;

-- Joins
SELECT u.name, COUNT(o.id) as order_count
FROM users u LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id;
```

## Insert (INSERT)

```sql
INSERT INTO users (name, email, status) VALUES ('John', 'john@example.com', 'active');

-- Bulk insert
INSERT INTO users (name, email) VALUES
  ('John', 'john@example.com'),
  ('Jane', 'jane@example.com'),
  ('Bob', 'bob@example.com');
```

## Update (UPDATE)

```sql
-- Single record
UPDATE users SET email = 'new@example.com' WHERE id = 1;

-- Bulk update
UPDATE products SET price = price * 0.9 WHERE category = 'electronics' AND stock > 100;
```

## Delete (DELETE)

```sql
-- Delete specific record
DELETE FROM users WHERE id = 5 AND status = 'inactive';

-- Delete with subquery first
DELETE FROM orders WHERE user_id IN (SELECT id FROM users WHERE status = 'deleted');
```

## Create Table (CREATE TABLE)

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL REFERENCES users(id),
    total_amount DECIMAL(10,2) NOT NULL DEFAULT 0,
    status VARCHAR(20) NOT NULL DEFAULT 'pending',
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_orders_user_id ON orders(user_id);
CREATE INDEX idx_orders_status ON orders(status);
CREATE INDEX idx_orders_created_at ON orders(created_at DESC);
```

## Add Column (ALTER TABLE)

```sql
ALTER TABLE users ADD COLUMN phone VARCHAR(20);
ALTER TABLE users ADD COLUMN last_login_at TIMESTAMP;
ALTER TABLE users DROP COLUMN IF EXISTS old_field;
```

## Create Index (CREATE INDEX)

```sql
-- Regular index
CREATE INDEX idx_users_email ON users(email);

-- Unique index
CREATE UNIQUE INDEX idx_users_email ON users(email);

-- Composite index
CREATE INDEX idx_orders_user_status ON orders(user_id, status);

-- Partial index
CREATE INDEX idx_orders_active ON orders(created_at DESC) WHERE status = 'active';
```

## Transaction

```sql
BEGIN;
-- Do your operations
SELECT * FROM users WHERE id = 1 FOR UPDATE;
UPDATE users SET status = 'active' WHERE id = 1;
COMMIT;
-- Or ROLLBACK; to undo
```

## Common Scenarios

### Create Users Table
```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

### Paginated Query
```sql
SELECT id, username, email, created_at
FROM users
ORDER BY id
LIMIT 10 OFFSET 10;
```

### Daily Order Stats
```sql
SELECT
    DATE(created_at) as order_date,
    COUNT(*) as order_count,
    SUM(total_amount) as total_revenue
FROM orders
WHERE created_at >= '2024-01-01'
GROUP BY DATE(created_at)
ORDER BY order_date;
```

### Soft Delete Users
```sql
DELETE FROM users
WHERE status = 'inactive'
  AND last_login_at < CURRENT_TIMESTAMP - INTERVAL '1 year';
```
