# REST API

The most common API style. Uses HTTP methods on resource URLs.

## URL Structure

```
GET    /api/v1/users          → List users
GET    /api/v1/users/123      → Get user 123
POST   /api/v1/users          → Create user
PUT    /api/v1/users/123      → Replace user 123
PATCH  /api/v1/users/123      → Partially update user 123
DELETE /api/v1/users/123      → Delete user 123
```

**Rules:**
- Use **nouns** for resources, not verbs (`/users` not `/getUsers`)
- Use **plural** for collections (`/users` not `/user`)
- Use **kebab-case** for multi-word resources (`/user-profiles`)
- Nest only one level deep (`/users/123/orders` is OK, `/users/123/orders/456/items` is not)

## HTTP Methods

| Method | Purpose | Idempotent | Safe |
|---|---|---|---|
| GET | Read resource | Yes | Yes |
| POST | Create resource | No | No |
| PUT | Replace entire resource | Yes | No |
| PATCH | Partial update | No | No |
| DELETE | Remove resource | Yes | No |

## HTTP Status Codes

| Code | When to use |
|---|---|
| 200 | Successful GET, PUT, PATCH, DELETE |
| 201 | Successful POST |
| 204 | Successful DELETE with no body |
| 400 | Invalid input, malformed JSON |
| 401 | Missing or invalid authentication |
| 403 | Authenticated but not allowed |
| 404 | Resource doesn't exist |
| 409 | Duplicate resource, state conflict |
| 422 | Valid JSON but validation failed |
| 500 | Unexpected server failure |

## Error Response Format

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Email format is invalid",
    "details": [
      { "field": "email", "message": "Must be a valid email address" }
    ]
  }
}
```

## Pagination

**Request:** `GET /api/v1/users?page=2&limit=20`

**Response:**
```json
{
  "data": [...],
  "pagination": {
    "page": 2,
    "limit": 20,
    "total": 156,
    "totalPages": 8
  }
}
```

## Filtering, Sorting, Search

- **Filter:** `GET /api/v1/users?status=active&role=admin`
- **Sort:** `GET /api/v1/users?sort=created_at&order=desc`
- **Search:** `GET /api/v1/users?q=john`

## Versioning

Put version in the URL path: `/api/v1/...`

Bump version only when making **breaking changes** (removing fields, changing types, renaming endpoints).

## When to Use REST

| Good fit | Not ideal |
|---|---|
| CRUD-heavy apps | Real-time updates |
| Public APIs | Complex nested queries (over-fetching) |
| Simple resource models | High-frequency small updates |

## General Tips

- Return the created resource in POST responses (not just an ID)
- Accept and return `application/json` by default
- Use consistent field naming (`snake_case` or `camelCase`, pick one)
- Document every endpoint with request/response examples
