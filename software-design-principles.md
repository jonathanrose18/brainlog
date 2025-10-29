## ADP — Acyclic Dependencies Principle

* **Description:** Package/module dependency graph must not contain cycles.
* **Why:** Cycles make builds, reasoning, and deployments brittle.
* **Do:** Break cycles via interfaces, events, or dependency inversion.
* **Example:**
    ```typescript
    // ui -> app -> domain (no back edge)
    export interface PaymentGateway {
      charge(cents: number): Promise<void>
    } // domain
    
    // app wires ui to domain via adapter implementing PaymentGateway
    ```

## APO — Application Programming Orientation

* **Description:** Optimize code for the application’s core workflows over generic frameworks.
* **Why:** Keeps focus on business value; frameworks come and go.
* **Do:** Start from use cases; wrap frameworks at boundaries.
* **Example:** Place `processOrder()` in `application/`, not inside a controller class.

## BSR — Black Sheep Rule

* **Description:** Unusual/exceptional code paths must stand out clearly.
* **Why:** Prevents hidden traps and speeds reviews.
* **Do:** Name the “weird thing” explicitly (comments, separate modules).
* **Example:** `// Black-sheep: legacy VAT calc for CH only` in dedicated function.

## CCP — Common Closure Principle

* **Description:** Group classes that change for the same reason into the same package.
* **Why:** Reduces churn across many packages when requirements change.
* **Do:** Package by business capability/use case, not by layer.
* **Example:** `billing/` contains models, services, validators for billing.

## CoI — Coincidental Cohesion

* **Description:** Anti-pattern: files grouped without a single purpose.
* **Why:** Hard to navigate; changes ripple unpredictably.
* **Do:** Regroup by capability; delete “misc” modules.
* **Example:** Split `utils.ts` into `string/`, `date/`, `money/`.

## CQS — Command Query Separation

* **Description:** A method either changes state (command) *or* returns data (query).
* **Why:** Predictability, easier testing and caching.
* **Do:** Separate `createUser()` from `getUser()`.
* **Example:**
    ```typescript
    // query
    function findUser(id: string): Promise<User | null> { /* no side effects */ }
    
    // command
    async function activateUser(id: string): Promise<void> { /* changes state */ }
    ```

## CRP — Composite Reuse Principle

* **Description:** Favor object composition over inheritance.
* **Why:** Avoids brittle hierarchies; improves flexibility.
* **Do:** Inject behavior via interfaces.
* **Example:**
    ```typescript
    interface Logger { log(msg: string): void }
    class Service { constructor(private log: Logger) {} }
    ```

## DIP — Dependency Inversion Principle

* **Description:** High-level policy depends on abstractions, not concrete details.
* **Why:** Swap implementations without rewriting policies.
* **Do:** Define ports in domain; implement adapters at the edges.
* **Example:**
    ```typescript
    // domain
    export interface EmailPort { send(to:string, body:string): Promise<void> }
    
    // infra adapter
    class SmtpEmail implements EmailPort { /* ... */ }
    ```

## DRY — Don’t Repeat Yourself

* **Description:** One source of truth for every piece of knowledge.
* **Why:** Fix once, fix everywhere.
* **Do:** Extract duplication when semantics truly match.
* **Example:** Shared `Money` value object reused across services.

## EUHM — Each Unit Has Meaning

* **Description:** Every function/class/module must have a clear, single intent.
* **Why:** Aids readability and maintenance.
* **Do:** Rename or split vague units.
* **Example:** Rename `process()` → `recalculateCartTotals()`.

## EWV — Explicitly Write Variables

* **Description:** Prefer explicit variable names/types over implicit magic.
* **Why:** Reduces cognitive load and bugs.
* **Do:** Avoid unnamed booleans/“magic” numbers.
* **Example:**
    ```typescript
    const MAX_RETRIES = 3;
    const shouldThrottle = requestsInWindow > limit;
    ```

## FF — First Function

* **Description:** Identify the primary function of a module and design around it.
* **Why:** Keeps modules purpose-driven.
* **Do:** Put the “first function” at the top; helpers below.
* **Example:** File `applyDiscounts.ts` starts with `applyDiscounts(cart)`.

## FTSE — Fail to Success Early

* **Description:** Validate early; bail out on bad inputs fast.
* **Why:** Shortens error paths, clearer code.
* **Do:** Guard clauses, narrow types.
* **Example:**
    ```typescript
    if (!email) return Err('email required');
    // proceed with safe assumptions
    ```

## GRASP — General Responsibility Assignment Software Patterns

* **Description:** Set of patterns (Controller, Creator, Low Coupling, High Cohesion, etc.).
* **Why:** Guides responsibility placement.
* **Do:** Use “Information Expert” to place logic where data lives.
* **Example:** `Order.calculateTotal()` lives on `Order`, not in a random service.

## HC — High Cohesion

* **Description:** A module’s elements strongly relate to a single task.
* **Why:** Easier to change and reuse.
* **Do:** Split overgrown services by capability.
* **Example:** `InventoryService` ≠ `UserService`.

## HLYW — How Long You Work

* **Description:** Optimize for sustainable pace, not heroics.
* **Why:** Prevents design debt and burnout.
* **Do:** Timebox, automate repetitive tasks.
* **Example:** Add scripts to run tests/formatters with one command.

## IH — Information Hiding

* **Description:** Hide implementation details behind stable interfaces.
* **Why:** Freedom to change internals safely.
* **Do:** Export minimal APIs; keep internals private.
* **Example:**
    ```typescript
    export function hashPassword(p: string): string { /* hides salt/algorithm */ }
    ```

## IoC — Inversion of Control

* **Description:** Delegate control flow to a container/framework.
* **Why:** Decouples construction from use, improves testability.
* **Do:** Use DI containers/factories.
* **Example:**
    ```typescript
    container.register({ emailPort: asClass(SmtpEmail) });
    ```

## IOSP — Interface Segregation Principle (alt. acronym)

* **Description:** Clients shouldn’t depend on methods they don’t use.
* **Why:** Prevents bloated interfaces.
* **Do:** Split interfaces by role.
* **Example:** `ReadableStream` vs `WritableStream` instead of `IOStream`.

## ISP — Interface Segregation Principle

(Same as IOSP; use one in code, document both acronyms.)

## KISS — Keep It Simple, Stupid

* **Description:** Prefer the simplest solution that works.
* **Why:** Simpler code fails less and is easier to change.
* **Do:** Avoid over-abstraction; delete unused layers.
* **Example:** Start with a function before introducing a class.

## LC — Least Coupling

* **Description:** Minimize inter-module knowledge.
* **Why:** Localizes changes and failures.
* **Do:** Depend on stable contracts; avoid global state.
* **Example:** Call `PaymentPort.charge()` instead of importing gateway SDKs everywhere.

## LoD — Law of Demeter

* **Description:** Only talk to your immediate friends.
* **Why:** Reduces fragile “train-wreck” chains.
* **Do:** Provide methods that do the job instead of exposing internals.
* **Example:**
    ```typescript
    // bad
    order.customer.address.city
    
    // good
    order.getCustomerCity()
    ```

## LOLA — Little Old Lady Algorithm

* **Description:** Prefer straightforward, robust algorithms that “an elder could follow.”
* **Why:** Clarity beats cleverness most of the time.
* **Do:** Choose readable loops over dense metaprogramming.
* **Example:** A clear for-loop instead of nested reduce-of-reduce.

## LSP — Liskov Substitution Principle

* **Description:** Subtypes must be usable wherever their base type is expected.
* **Why:** Prevents surprising runtime behavior.
* **Do:** Avoid weakening preconditions/strengthening postconditions.
* **Example:** A `Square` should not extend `Rectangle` if setters break invariants.

## OCP — Open/Closed Principle

* **Description:** Open for extension, closed for modification.
* **Why:** Add features without rewriting core logic.
* **Do:** Use polymorphism, configuration, or plugins.
* **Example:**
    ```typescript
    interface PricingRule { apply(cart: Cart): Money }
    const rules: PricingRule[] = [new BulkRule(), new SeasonalRule()]
    ```

## POLP — Principle of Least Privilege

* **Description:** Give components only the access they need.
* **Why:** Limits blast radius of bugs/compromises.
* **Do:** Narrow tokens, scopes, roles.
* **Example:** Service account with read-only Firestore role for reporting.

## POLS — Principle of Least Surprise

* **Description:** APIs should behave as users expect.
* **Why:** Lowers learning curve and errors.
* **Do:** Consistent naming, no hidden side effects.
* **Example:** `get*()` never mutates.

## PoMO — Principle of Modularity

* **Description:** Decompose into independent modules with clear interfaces.
* **Why:** Parallel work, reuse, and testability.
* **Do:** Define module boundaries and contracts first.
* **Example:** `modules/billing`, `modules/catalog`, `modules/identity`.

## REP — Reuse/Release Equivalence Principle

* **Description:** Reusable components should be released/versioned as a unit.
* **Why:** Ensures consistent, dependable reuse.
* **Do:** Package shared lib with semantic versioning.
* **Example:** `@acme/money@2.x` published once, reused everywhere.

## RoT — Rule of Three

* **Description:** Extract an abstraction after you’ve duplicated it ~3 times.
* **Why:** Avoids premature abstraction.
* **Do:** Wait for patterns to emerge.
* **Example:** After third similar “export to CSV,” extract `CsvWriter`.

## RTFM — Read The Fine Manual

* **Description:** Consult docs/specs before coding.
* **Why:** Saves time and avoids misuse.
* **Do:** Link key docs in the codebase/README.
* **Example:** Add “How to paginate Firestore” link in repository.

## SAP — Stable Abstractions Principle

* **Description:** The more stable a package, the more abstract it should be.
* **Why:** Stable packages shouldn’t depend on frequent details.
* **Do:** Keep domain ports stable; push volatility to adapters.
* **Example:** `domain/port/EmailPort.ts` changes rarely; SMTP adapter can churn.

## SDB — Single Database Principle

* **Description:** A service owns its data store (even if physically shared).
* **Why:** Avoids tight coupling via shared DB schemas.
* **Do:** Expose data via APIs/events.
* **Example:** Reporting reads through `ReportingAPI`, not other service tables.

## SDP — Stable Dependencies Principle

* **Description:** Depend in the direction of stability.
* **Why:** Unstable packages shouldn’t be depended upon by stable ones.
* **Do:** Point dependencies from volatile → stable.
* **Example:** `web` (volatile) depends on `domain` (stable), not vice versa.

## SLA — Single Level of Abstraction

* **Description:** Within a function, operate at one abstraction level.
* **Why:** Improves readability and refactorability.
* **Do:** Extract lower-level steps.
* **Example:**
    ```typescript
    function checkout(){
      validateCart();
      reserveStock();
      chargeCard();
    }
    ```

## SoC — Separation of Concerns

* **Description:** Separate different kinds of responsibilities.
* **Why:** Parallel change without interference.
* **Do:** Split policy, orchestration, IO, and presentation.
* **Example:** `domain/`, `application/`, `adapters/`, `presenter/`.

## SOLDIER — Single Object Lives, Does It Execute Repeatedly

* **Description:** Prefer long-lived, reusable objects for repeated work.
* **Why:** Cuts allocation/initialization overhead; improves locality.
* **Do:** Reuse clients (DB, HTTP) rather than recreate per call.
* **Example:** Singleton `PrismaClient` shared via DI.

## SOLID — Five principles set

* **Description:** SRP, OCP, LSP, ISP, DIP.
* **Why:** Together reduce coupling and increase maintainability.
* **Do:** Apply contextually; avoid dogma.
* **Example:** See `SRP`/`OCP`/`LSP`/`ISP`/`DIP` entries.

## SPOT — Single Point of Truth

* **Description:** Keep canonical data/logic in one place (synonym of DRY for facts).
* **Why:** Eliminates divergence.
* **Do:** Centralize config and invariants.
* **Example:** Currency list defined once in `money/CURRENCIES.ts`.

## SRP — Single Responsibility Principle

* **Description:** A module has one reason to change.
* **Why:** Localizes change and reduces merge conflicts.
* **Do:** Split by reason to change.
* **Example:**
    ```typescript
    class PasswordHasher { /* hashing only */ }
    class PasswordPolicy { /* rules only */ }
    ```

## TdA — Tell, don’t Ask

* **Description:** Tell objects what to do; don’t pull data to decide elsewhere.
* **Why:** Promotes encapsulation and richer objects.
* **Do:** Send commands to domain objects.
* **Example:**
    ```typescript
    order.applyCoupon(coupon)
    // not: if(coupon.valid) order.total -= ...
    ```

## WET — Write Everything Twice

* **Description:** Anti-pattern: needless repetition.
* **Why:** Divergence and extra work.
* **Do:** Refactor to DRY/SPOT when semantics align.
* **Example:** Extract shared validation schema.

## YAGNI — You Ain’t Gonna Need It

* **Description:** Don’t build for hypothetical futures.
* **Why:** Saves time and reduces complexity.
* **Do:** Implement the next required capability only.
* **Example:** Skip multi-tenant abstraction until a second tenant exists.

## ZOI — Zone of Influence

* **Description:** A change should affect the smallest possible area.
* **Why:** Limits risk and review scope.
* **Do:** Encapsulate behind narrow interfaces; use feature flags.
* **Example:** New pricing rule contained within `pricing/` module and flagged.

## Notes on relationships & quick mappings

* `SOLID` comprises `SRP`, `OCP`, `LSP`, `ISP/IOSP`, `DIP`.
* `DRY/SPOT` support `SLA`, `SoC`, `HC`.
* `ADP`, `SDP`, `SAP` regulate package dependency & stability.
* `CRP`, `LoD`, `LC` reduce coupling; `HC`, `SRP` increase cohesion.
* `YAGNI`, `KISS`, `LOLA`, `RoT` fight accidental complexity.
* `IoC` is a mechanism that enables `DIP/ISP` in practice.
