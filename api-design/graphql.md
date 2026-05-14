# GraphQL

A query language for APIs. The client specifies exactly what data it needs in a single request.

## How It Works

- **Single endpoint:** `POST /graphql` (one URL for everything)
- **Client defines shape:** the response matches the query structure exactly
- **Strongly typed schema:** defines all types, queries, and mutations upfront

## Schema Definition

```graphql
type User {
  id: ID!
  name: String!
  email: String!
  orders: [Order!]!
}

type Order {
  id: ID!
  total: Float!
  items: [Item!]!
}

type Query {
  user(id: ID!): User
  users(page: Int, limit: Int): [User!]!
}

type Mutation {
  createUser(input: CreateUserInput!): User!
  updateUser(id: ID!, input: UpdateUserInput!): User!
  deleteUser(id: ID!): Boolean!
}
```

## Operations

| Type | Purpose | Example |
|---|---|---|
| **Query** | Read data | Fetch user with specific fields |
| **Mutation** | Write data | Create, update, delete |
| **Subscription** | Real-time updates | Listen for new messages (uses WebSocket) |

## Query Examples

**Get specific fields only (no over-fetching):**
```graphql
query {
  user(id: "123") {
    name
    email
  }
}
```
Response:
```json
{ "data": { "user": { "name": "John", "email": "john@example.com" } } }
```

**Nested data in one request (no under-fetching):**
```graphql
query {
  user(id: "123") {
    name
    orders {
      id
      total
      items {
        name
        price
      }
    }
  }
}
```

**Mutation:**
```graphql
mutation {
  createUser(input: { name: "Jane", email: "jane@example.com" }) {
    id
    name
  }
}
```

## Error Handling

GraphQL always returns HTTP 200 (even on errors). Errors live in the response body:

```json
{
  "data": null,
  "errors": [
    {
      "message": "User not found",
      "locations": [{ "line": 2, "column": 3 }],
      "path": ["user"]
    }
  ]
}
```

## When to Use GraphQL

| Good fit | Not ideal |
|---|---|
| Complex nested data (e.g., social feed) | Simple CRUD with few resources |
| Multiple clients needing different data shapes | Caching is critical (REST + HTTP cache is simpler) |
| Mobile apps where bandwidth matters | Team is unfamiliar with GraphQL |
| Frontend teams wanting flexibility | File uploads (needs special handling) |

## Common Tools

| Tool | Purpose |
|---|---|
| Apollo Server / Client | Popular GraphQL implementation |
| GraphQL Yoga | Lightweight server |
| Relay | Facebook's client framework |
| GraphiQL / Apollo Sandbox | Interactive query playground |

## Tips

- Define your schema before writing resolvers
- Use DataLoader to batch database calls and avoid N+1 queries
- Add pagination to list fields (use cursor-based for large datasets)
- Set query depth limits to prevent abuse
- Keep mutations small and focused (one action per mutation)
