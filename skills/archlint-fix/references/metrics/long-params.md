# Long Parameter List

**What:** Function with too many parameters.

**Why it's bad:**
- Easy to mix up parameter order
- Hard to remember all parameters
- Indicates function does too much

## Fix Strategies

### Strategy 1: Parameter Object

```typescript
// BEFORE
function createUser(
  name: string,
  email: string,
  age: number,
  street: string,
  city: string,
  country: string,
  role: string,
  department: string
) { }

// AFTER
interface CreateUserParams {
  name: string;
  email: string;
  age: number;
  address: Address;
  employment: Employment;
}

function createUser(params: CreateUserParams) { }
```

### Strategy 2: Builder Pattern

```typescript
// AFTER: Builder for complex construction
const user = new UserBuilder()
  .withName('John')
  .withEmail('john@example.com')
  .withAddress({ city: 'NYC', country: 'US' })
  .build();
```

### Strategy 3: Split Function

```typescript
// If many params because function does too much
// BEFORE
function processOrder(items, user, payment, shipping, discount, notify) { }

// AFTER
function validateOrder(items, user) { }
function processPayment(payment, discount) { }
function arrangeShipping(shipping) { }
function notifyUser(user, notify) { }
```
