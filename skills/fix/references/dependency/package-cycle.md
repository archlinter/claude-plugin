# Package Cycle

**What:** Circular dependency between packages/folders (not just files).

**Why it's bad:**
- Packages cannot be developed or deployed in isolation
- Modular structure becomes a big ball of mud

## Fix Strategy: Extract Shared Package

```
BEFORE:
packages/auth → packages/users
packages/users → packages/auth

AFTER:
packages/shared-types (extracted common types)
packages/auth → packages/shared-types
packages/users → packages/shared-types
```
