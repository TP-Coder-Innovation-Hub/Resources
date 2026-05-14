# API Design Reference

Quick reference for building clean, consistent REST APIs.

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
- Use **kebab-case** for multi-word resources (`/user-profiles` not `/userProfiles`)
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

**Use these consistently:**

| Code | Meaning | When to use |
|---|---|---|
| 200 | OK | Successful GET, PUT, PATCH, DELETE |
| 201 | Created | Successful POST |
| 204 | No Content | Successful DELETE with no body |
| 400 | Bad Request | Invalid input, malformed JSON |
| 401 | Unauthorized | Missing or invalid authentication |
| 403 | Forbidden | Authenticated but not allowed |
| 404 | Not Found | Resource doesn't exist |
| 409 | Conflict | Duplicate resource, state conflict |
| 422 | Unprocessable Entity | Valid JSON but validation failed |
| 500 | Internal Server Error | Unexpected server failure |

## Error Response Format

Always return a consistent error structure:

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

For list endpoints returning many results:

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

## Filtering & Sorting

- **Filter:** `GET /api/v1/users?status=active&role=admin`
- **Sort:** `GET /api/v1/users?sort=created_at&order=desc`
- **Search:** `GET /api/v1/users?q=john`

## API Versioning

Put version in the URL path: `/api/v1/...`

Bump version only when you make **breaking changes** (removing fields, changing types, renaming endpoints).

## General Tips

- Return the created resource in POST responses (not just an ID)
- Use `PATCH` for partial updates, `PUT` for full replacements
- Accept and return `application/json` by default
- Document every endpoint with request/response examples
- Use consistent field naming (choose `snake_case` or `camelCase` and stick to it)
