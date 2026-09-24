<!-- Template: Release Notes -->
<!-- Replace all {{PLACEHOLDER}} values. Choose Developer or End-user variant. -->

# Release v{{X.Y.Z}} — {{YYYY-MM-DD}}

> {{One-sentence summary of the release.}}

<!-- DEVELOPER VARIANT — use when audience = Developers -->

## Highlights

- {{Bullet point}}
- {{Bullet point}}

## Breaking Changes

- **`{{name}}`** — {{what changed}}. Migrate via: `{{example}}`. See [migration guide](#migration-guide).

## Deprecations

- **`{{name}}`** — will be removed in v{{X.Y}}. Use `{{replacement}}` instead.

## Features

- {{feature}} ({{[#PR](url)}})

## Fixes

- {{fix}} ({{[#PR](url)}})

## Migration Guide

### {{Breaking change topic}}

**Before:**

```{{lang}}
{{old code}}
```

**After:**

```{{lang}}
{{new code}}
```

<!-- END DEVELOPER VARIANT -->

<!-- END-USER VARIANT — use when audience = End users -->

## What's New

- {{Feature in plain language}}

## Improvements

- {{Improvement}}

## Fixes

- {{User-visible fix}}

## Known Issues

- {{Issue + workaround}}

<!-- END END-USER VARIANT -->

## References

- {{Link to changelog}}
- {{Link to milestone / sprint}}
