# API Documentation Patterns

API docs come in four flavours — each with a distinct structure.

| Subtype | Emphasises |
|---------|-----------|
| **REST** | Endpoints, methods, status codes, request/response schemas |
| **GraphQL** | Schema, queries, mutations, subscriptions |
| **Library / SDK** | Imports, types, function signatures, examples |
| **CLI** | Commands, flags, exit codes, examples |

## Common structure

1. **Overview** — what the API does in 2–3 sentences
2. **Authentication** — how to authenticate (or "none" if unauthenticated)
3. **Base URL** / **Installation** / **Invocation** — depending on subtype
4. **Reference** — the bulk of the doc, organised by subtype
5. **Errors** — error format and common codes
6. **Examples** — end-to-end usage examples
7. **Changelog** — version history (link to a separate changelog if it's long)

## REST endpoint format

Every endpoint follows this shape:

```markdown
## `GET /resources/{id}`

Brief description of what this endpoint does.

**Path parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `id` | string | Yes | Resource identifier |

**Query parameters**

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `expand` | string | No | — | Comma-separated list of fields to expand |

**Response — 200**

​```json
{ "id": "abc", "name": "Example" }
​```

**Response — 404**

​```json
{ "error": "not_found", "message": "Resource not found" }
​```
```

## GraphQL format

```markdown
## Query: `user(id: ID!)`

Fetches a user by ID.

**Arguments**

| Name | Type | Description |
|------|------|-------------|
| `id` | `ID!` | User identifier |

**Returns** — `User`

**Example**

​```graphql
query {
  user(id: "abc") {
    name
    email
  }
}
​```
```

## Library / SDK format

```markdown
## `functionName(options)`

Brief description.

**Parameters**

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `options.foo` | `string` | Yes | — | Description |
| `options.bar` | `number` | No | `0` | Description |

**Returns** — `Promise<Result>`

**Example**

​```typescript
import { functionName } from 'my-library';

const result = await functionName({ foo: 'value' });
​```
```

## CLI format

```markdown
## `mytool command [options]`

Brief description.

**Usage**

​```bash
mytool command --flag value
​```

**Options**

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--flag` | string | — | Description |
| `--verbose` | boolean | `false` | Print debug output |

**Exit codes**

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | Generic error |
| 2 | Invalid arguments |
```

## Errors

Always document the error response shape and the canonical error codes. Link to [RFC 7807 (Problem Details)](https://www.rfc-editor.org/rfc/rfc7807) when applicable.
