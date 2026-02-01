# Layer Violation

**What:** Module imports from a layer it shouldn't (e.g., domain imports infrastructure).

**Why it's bad:**
- Circular dependencies between layers
- Cannot test domain logic in isolation
- Implementation details leak into business logic

## Fix Strategy: Dependency Inversion

```typescript
// BEFORE: Domain depends on infrastructure
// domain/order.service.ts
import { PostgresRepository } from '../infrastructure/postgres'; // VIOLATION!

// AFTER: Depend on abstraction
// domain/ports/order-repository.ts
export interface OrderRepository {
  save(order: Order): Promise<void>;
}

// domain/order.service.ts
import { OrderRepository } from './ports/order-repository';
export class OrderService {
  constructor(private repo: OrderRepository) {}
}

// infrastructure/postgres.repository.ts
import { OrderRepository } from '../domain/ports/order-repository';
export class PostgresRepository implements OrderRepository { }
```
