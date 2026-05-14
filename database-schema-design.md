# Database Schema Design

Quick reference for designing your database tables and relationships.

## Data Modeling Basics

Before writing code, identify your **entities**, their **attributes**, and **relationships**.

- **Entity** — a real-world object or concept (e.g., `User`, `Product`, `Order`)
- **Attribute** — a property of an entity (e.g., `User` has `email`, `name`)
- **Relationship** — how entities connect (e.g., `User` places many `Orders`)

## Relationships

| Type | Description | Example | How to implement |
|---|---|---|---|
| One-to-One | A relates to exactly one B | User ↔ UserProfile | Put `user_id` (unique) in UserProfile |
| One-to-Many | A relates to many B | Customer → Orders | Put `customer_id` in Orders table |
| Many-to-Many | A relates to many B, and vice versa | Students ↔ Courses | Create a junction table `student_courses` |

## Normalization (Reducing Redundancy)

Apply these rules to keep your data clean:

**1NF** — Each cell has a single value. Each row is unique (has a primary key).

**2NF** — In 1NF + every non-key column depends on the full primary key (not just part of it).

**3NF** — In 2NF + non-key columns don't depend on other non-key columns.

**Practical tip:** Normalize to 3NF by default. Denormalize only when you have a proven performance reason.

## Common Data Types

| Type | Use for | Examples |
|---|---|---|
| `INT` / `BIGINT` | Whole numbers | `id`, `quantity`, `age` |
| `DECIMAL(p,s)` | Money, precise decimals | `price`, `tax_rate` |
| `VARCHAR(n)` | Variable-length text | `name`, `email`, `description` |
| `TEXT` | Long-form text | `body`, `bio` |
| `BOOLEAN` | True/false | `is_active`, `is_verified` |
| `TIMESTAMP` | Date and time | `created_at`, `updated_at` |
| `UUID` | Globally unique ID | `id` (alternative to auto-increment) |

## Constraints

Enforce data integrity at the database level:

- **PRIMARY KEY** — uniquely identifies each row
- **FOREIGN KEY** — links to a row in another table
- **NOT NULL** — this column must have a value
- **UNIQUE** — no duplicate values in this column
- **CHECK** — value must satisfy a condition (e.g., `quantity > 0`)
- **DEFAULT** — automatically set if no value provided (e.g., `created_at = NOW()`)

## Indexing

Indexes speed up reads at the cost of slightly slower writes.

| Index Type | Best for | Tradeoff |
|---|---|---|
| B-Tree (default) | Equality and range queries | Standard overhead |
| Hash | Exact match lookups only | No range support |
| Full-text | Text search (LIKE, keywords) | Larger index size |

**When to add an index:**
- Columns used in `WHERE` clauses frequently
- Columns used in `JOIN` conditions
- Columns used in `ORDER BY`

**When NOT to index:**
- Tables with very few rows
- Columns with low cardinality (e.g., boolean)
- Columns that are frequently updated but rarely queried

## Design Tips

1. Always have `id`, `created_at`, and `updated_at` on every table
2. Use consistent naming: `snake_case` for tables and columns
3. Use singular table names (`user` not `users`) or plural consistently — pick one
4. Use foreign keys to enforce referential integrity
5. Store monetary values as `DECIMAL`, never `FLOAT`
6. Use soft deletes (`deleted_at` column) for important data you may need to recover
7. Don't store computed values — calculate them or use views/materialized views

---

Adapted from [java-tech-interviews-prep](https://github.com/marttp/java-tech-interviews-prep)
