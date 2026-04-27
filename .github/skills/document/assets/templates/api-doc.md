<!-- Template: API Documentation -->
<!-- Choose the variant matching the API subtype. Replace all {{PLACEHOLDER}} values. -->

# {{API Name}}

> {{One sentence describing what this API does.}}

## Overview

{{2–3 sentences. Who uses this API and why.}}

## Authentication

{{How to authenticate. e.g. Bearer token in Authorization header. Link to docs for obtaining credentials.}}

## Base URL / Installation / Invocation

{{REST: Base URL. Library: install command + import. CLI: install + invocation. GraphQL: endpoint URL.}}

```bash
{{install or base URL}}
```

## Reference

<!-- REST EXAMPLE -->

### `{{METHOD}} {{/path/{param}}}`

{{What this endpoint does.}}

**Path parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `{{param}}` | {{type}} | Yes | {{description}} |

**Query parameters**

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `{{param}}` | {{type}} | No | — | {{description}} |

**Request body**

```json
{{example request}}
```

**Response — {{status}}**

```json
{{example response}}
```

<!-- LIBRARY / SDK EXAMPLE -->

### `{{functionName(options)}}`

{{What this function does.}}

**Parameters**

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `options.{{name}}` | `{{type}}` | Yes | — | {{description}} |

**Returns** — `{{type}}`

**Example**

```{{lang}}
{{example usage}}
```

## Errors

{{Describe the error response shape and list canonical error codes.}}

| Code | Meaning |
|------|---------|
| `{{code}}` | {{description}} |

## Examples

{{End-to-end usage example showing common scenarios.}}

```{{lang}}
{{example}}
```

## Changelog

{{Brief version history or link to a CHANGELOG.md.}}
