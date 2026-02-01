# Primitive Obsession

**What:** Function has too many primitive parameters instead of domain objects.

**Why it's bad:**
- Weak type safety
- Logic duplication
- Easy to mix up parameters

## Fix Strategy: Introduce Value Objects

```typescript
// BEFORE
function createUser(
  name: string,
  email: string,
  phone: string,
  street: string,
  city: string,
  zip: string,
  country: string
) { }

// AFTER: Value objects
interface Email { value: string; }
interface Phone { value: string; }
interface Address {
  street: string;
  city: string;
  zip: string;
  country: string;
}

function createUser(
  name: string,
  email: Email,
  phone: Phone,
  address: Address
) { }
```
