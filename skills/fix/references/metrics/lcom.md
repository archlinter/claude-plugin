# Low Cohesion (LCOM)

**What:** Class methods don't share common fields (Lack of Cohesion of Methods).

**Why it's bad:**
- Class has multiple responsibilities
- Should probably be split

## Fix Strategy: Split by Cohesion

```typescript
// BEFORE: Low cohesion class
class UserManager {
  private db: Database;
  private mailer: Mailer;
  private logger: Logger;

  // Group 1: Uses db
  findUser(id: string) { return this.db.find(id); }
  saveUser(user: User) { this.db.save(user); }

  // Group 2: Uses mailer
  sendWelcome(user: User) { this.mailer.send(user); }
  sendReset(user: User) { this.mailer.send(user); }

  // Group 3: Uses logger
  logAccess(user: User) { this.logger.log(user); }
}

// AFTER: Split into cohesive classes
class UserRepository {
  constructor(private db: Database) {}
  find(id: string) { return this.db.find(id); }
  save(user: User) { this.db.save(user); }
}

class UserMailer {
  constructor(private mailer: Mailer) {}
  sendWelcome(user: User) { this.mailer.send(user); }
  sendReset(user: User) { this.mailer.send(user); }
}

class UserLogger {
  constructor(private logger: Logger) {}
  logAccess(user: User) { this.logger.log(user); }
}
```
