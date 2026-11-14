---
name: domain-driven-design
description: Apply Domain-Driven Design when designing, implementing, and structuring application code. Use when starting a new feature/service, modeling business logic, restructuring a codebase, or when the user mentions bounded contexts, aggregates, entities, value objects, domain events, or "future-proof" / "flexible" architecture.
---

# Domain-Driven Design

Model the business before writing code. The goal is a codebase where business rules live in one place, change is cheap, and the structure makes invalid states hard to express.

## 1. Design first

### Find the bounded contexts
- List the distinct business capabilities (billing, orders, inventory, identity...). Each is a **bounded context** with its own ubiquitous language.
- One context owns its rules. If two concepts share a name but behave differently, they are in different contexts — do not share types across them.
- Decide relationships: contexts talk through events/commands and contracts, not by reaching into each other's internals.

### Map the core
- Identify the **core domain** (where the business competes) and put most effort there. Supporting and generic subdomains (auth, file storage, emails) get the cheapest correct implementation.
- When requirements are fuzzy, run an event storm or write the ubiquitous language in `docs/domain/<context>.md`: terms, invariants, rules, example scenarios. Code reviews should reference this file.

## 2. Model the domain

### Entities vs value objects
- **Entity**: identity matters (Order, Customer). Identity = an explicit id, equality by id.
- **Value object**: defined by attributes, immutable, equality by value (Money, Address, Period). Prefer value objects wherever possible — they are easier to test and change.
- Never build a value object that can hold an invalid state: `Money(currency, amount)` should make `amount < 0` unrepresentable, not catchable.

### Aggregates
- Group entities/value objects with a single consistency boundary: **one aggregate root, one unit of transactional integrity**.
- Keep aggregates small. Cross-aggregate consistency is eventually consistent via domain events, not transactions.
- The root is the only entry point to the aggregate; clients must not mutate inner entities directly.
- Invariant rule: every mutation that could break an invariant goes through the aggregate root.

### Domain events
- Model significant business facts as events (`OrderPlaced`, `PaymentCaptured`).
- Events are the seams between contexts and the trigger for side effects (notify, reserve stock).
- Keep events immutable records of what happened, with a clear point in time.

### Behavior over data
- Put rules on the domain object, not in services: `order.cancel(reason)` instead of `orderService.cancel(order)`.
- Application services coordinate; they contain no business rules.

## 3. Code structure (flexible for change)

Layered structure, dependencies point inward only:

```
interface/  (HTTP/CLI/gRPC adapters, DTOs, error mapping)
application/ (use cases: orchestrate commands, call domain, publish events)
domain/      (entities, value objects, aggregates, events, domain services — no framework imports)
infrastructure/ (repos, persistence, external APIs, messaging — implement domain ports)
```

### Ports and adapters
- The domain declares **ports** (interfaces): `OrderRepository`, `Clock`, `Notifier`, `PaymentGateway`.
- Infrastructure implements ports. Swap implementations without touching domain code (test doubles, SQL→event store, fake payment gateway).
- Time, randomness, and external I/O always come through injected ports — never `Date.now()` in domain logic.

### Change-friendly structure
- **Open points**: any behavior expected to change (pricing rules, tax rates, providers) is a strategy/policy object or configuration, selected at the composition root.
- **Composition root**: one place (entry point / wiring module) maps ports → adapters. Feature flags, environments, and new adapters are wired here, not scattered.
- Additive over invasive: new rules should be added by registering/extending (new strategy, new event handler) rather than editing a giant `if/else`. If you find a growing switch on a type, extract a polymorphic hierarchy or strategy map.
- **Versioned contracts**: DTOs/API payloads and domain events carry a version. Evolve by adding, never by breaking existing consumers.
- Keep the domain pure: no framework annotations, no direct DB calls. This keeps the core testable with plain unit tests and swappable per language/framework.

## 4. Implementation checklist

1. Write `docs/domain/<context>.md` (language, invariants, scenarios) before code.
2. Define value objects and entities with invariants enforced in constructors/factories.
3. Model the aggregate root with its mutation methods; each method asserts and restores invariants.
4. Emit domain events for business facts; handlers live in the application layer.
5. Define ports for all external dependencies; implement them in `infrastructure/`.
6. Wire at the composition root.
7. Tests:
   - **Domain**: pure unit tests — invariants hold, invalid states unrepresentable, each rule from the language file.
   - **Application**: use cases with fake ports.
   - **Integration**: one pass through a real adapter (repo/DB).
8. When adding a feature: touch domain + one handler + one wiring line. If it requires editing many existing files, stop and reconsider the boundary.

## 5. Anti-patterns to reject

- Anemic domain: POJOs/DTOs with all logic in service functions.
- Shared god-entity used across contexts to "keep it simple".
- Invariants checked only with exceptions after construction instead of unrepresentable states.
- Framework-specific code in the domain layer.
- Big `switch`/`if-else` on type where polymorphism or strategy belongs.
- Cross-context transactions to force strong consistency where events suffice.
- Renaming domain terms to fit a technical layer (e.g. "User" meaning customer + account + identity).
