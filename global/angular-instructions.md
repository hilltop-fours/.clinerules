# ANGULAR CODING RULES

## CONTROL FLOW - MANDATORY SYNTAX

ALWAYS use new control flow syntax:
- `@if (condition)` - NOT `*ngIf="condition"`
- `@else` - NOT `*ngIf="!condition"`
- `@for (item of items; track item.id)` - NOT `*ngFor="let item of items"`
- `@let variable = expression` - for template variables

NEVER use old syntax: `*ngIf`, `*ngFor`, `*ngSwitch`

## SIGNALS - USAGE RULES

PREFER signals for reactive state:
- Use `input()` for component inputs - NOT `@Input()` decorator
- Use `output()` for component outputs - NOT `@Output()` decorator
- Use signals for component state that needs reactivity
- Use `computed()` for derived values

DO NOT force signals when not needed:
- Traditional approaches are acceptable if signals complicate the code
- Avoid `toSignal()` casting unless absolutely necessary
- If signal refactoring is complex, note as potential future task instead of implementing

**PREFER `computed()` over methods for derived values:**
When a method or getter derives its value from signals and has no side effects, consider converting it to `computed()`.

Benefits:
- Memoization: recalculates only when dependencies change
- Performance: avoids recalculation on every change detection cycle
- Consistency: aligns with signal-based reactivity

Example - WRONG (method recalculates every change detection):
```typescript
content = input<Publication>();
readonly twoMonthsAgo = subMonths(new Date(), 2);

isNew() {
  const content = this.content();
  if (!content?.createdOn) {
    return false;
  }
  return new Date(content.createdOn) >= this.twoMonthsAgo;
}
```

Example - CORRECT (computed with memoization):
```typescript
content = input<Publication>();
readonly #twoMonthsAgo = subMonths(new Date(), 2);

isNew = computed(() => {
  const createdOn = this.content()?.createdOn;
  return createdOn ? new Date(createdOn) >= this.#twoMonthsAgo : false;
});
```

Example - Also CORRECT (computed for derived display values):
```typescript
// Instead of a method that formats every call
getFullName = computed(() => `${this.firstName()} ${this.lastName()}`);

// Instead of getter that checks permissions every call
canEdit = computed(() => this.authService.hasPermission(this.content()));
```

SELF-CHECK — BEFORE finalising any added or edited code, verify that no legacy decorators slipped in:
- `@Input()` → should be `input()`
- `@Output()` with `new EventEmitter()` → should be `output()`
- `@ViewChild()` → should be `viewChild()` (Angular 19+)
- `@ViewChildren()` → should be `viewChildren()` (Angular 19+)
- `@HostListener()` → should be `hostListener()` (Angular 19+)

This check is needed because training data is dominated by the older decorator syntax. When in doubt, scan the file you just touched for any of these decorators and replace with the signal equivalent before finishing.

CRITICAL — WHEN converting decorators to signals, UPDATE ALL REFERENCES:

Converting decorators to signals changes how values are accessed. You MUST search for and update all usages:

**@ViewChild / @ViewChildren conversions:**
- Old (decorator): `this.myChild.someMethod()` or `this.children.forEach(...)`
- New (signal): `this.myChild().someMethod()` or `this.children().forEach(...)`
- Return type changes: `QueryList<T>` → `Signal<readonly T[]>`

**@Input conversions:**
- Old (decorator): `this.myInput` (direct property access)
- New (signal): `this.myInput()` (function call)
- Type changes: `T` → `InputSignal<T>`

**Steps when converting:**
1. Change the decorator to signal syntax
2. Search the entire file for ALL references to that property name
3. Update each reference to use `()` for signal access
4. Update any type annotations (e.g., `QueryList<T>` → use signal return type)
5. Test that the functionality still works

Example - WRONG (forgot to update usage):
```typescript
// Converted declaration but forgot to update usage
filterComponents = viewChildren(BaseFilterDirective);

clearFilters() {
  // ❌ WRONG: Missing () for signal access
  this.filterComponents.forEach((filter) => filter.deselectAll());
}
```

Example - CORRECT (updated both declaration and usage):
```typescript
// Converted to signal
filterComponents = viewChildren(BaseFilterDirective);

clearFilters() {
  // ✅ CORRECT: Added () for signal access
  this.filterComponents().forEach((filter) => filter.deselectAll());
}
```

## INDEX FILES - MANDATORY

WHEN creating new directories containing:
- Components
- Services
- Repositories
- Types
- Models

THEN create `index.ts` file that re-exports main exports.

WHEN modifying code in directories without `index.ts`:
- Create the `index.ts` file

PURPOSE: Enable imports like `import { MyComponent } from './components/my-component'` instead of `import { MyComponent } from './components/my-component/my-component.component'`

## COMMENTS - RULES

ONLY add comments when:
- Marking temporary/mocked data: Use `// TODO: remove mock data` or `// Temporary for testing`
- Explaining complex logic: Explain "why" not "what"

DO NOT add comments for:
- Obvious code that is self-explanatory
- Describing what code does (use clear naming instead)

NEVER remove existing comments:
- Preserve ALL comments that were already in the code
- This includes organizational comments (e.g., `// Tab state`, `// Filters section`)
- This includes explanatory comments from other developers
- Only remove comments if they become factually incorrect after your changes

## TYPESCRIPT TYPING - STRICT RULES

NEVER use `any` type:
- Always define proper interfaces, types, or use generics
- If data structure is unknown, use `unknown` and add type guards
- Create interface/type definitions for complex objects
- SonarQube will flag `any` usage as an error

Example - WRONG:
```typescript
getCategoryDisplay(categories: any[]): string {
  return categories.map(c => c.name).join(', ');
}
```

Example - CORRECT:
```typescript
interface Category {
  id: string;
  name: string;
}

getCategoryDisplay(categories: Category[]): string {
  return categories.map(c => c.name).join(', ');
}
```

## NAMING CONVENTIONS - ENFORCE

File names: kebab-case
- `user-profile.component.ts`
- `auth.service.ts`

Class names: PascalCase
- `UserProfileComponent`
- `AuthService`

Variables and methods: camelCase
- `userName`
- `getUserData()`

Follow Angular file naming patterns:
- Components: `*.component.ts`
- Services: `*.service.ts`
- Models: `*.model.ts`
- Repositories: `*.repository.ts`

## CODE STYLE PREFERENCES

These are preferences, not hard rules. Use judgment for readability.

### Ternary operator for simple conditionals

PREFER the ternary operator (`condition ? valueIfTrue : valueIfFalse`) for simple value assignments or returns.

Example - PREFERRED (simple return):
```typescript
getStatusLabel(): string {
  return this.isActive() ? 'Active' : 'Inactive';
}
```

Example - PREFERRED (with nullish check):
```typescript
isNew = computed(() => {
  const createdOn = this.content()?.createdOn;
  return createdOn ? new Date(createdOn) >= this.threshold : false;
});
```

Example - PREFERRED (assignment):
```typescript
const displayName = user.nickname ? user.nickname : user.fullName;
```

Example - AVOID ternary (complex logic):
```typescript
// ❌ Too complex for ternary
const result = condition1 && condition2
  ? this.complexOperation(value).transform()
  : this.otherOperation(fallback).process();

// ✅ Better as if/else
if (condition1 && condition2) {
  return this.complexOperation(value).transform();
}
return this.otherOperation(fallback).process();
```

### Nullish coalescing for default values

PREFER nullish coalescing (`??`) over logical OR (`||`) when providing default values for potentially null/undefined values.

Difference:
- `??` returns right side only if left is `null` or `undefined`
- `||` returns right side if left is any falsy value (`0`, `''`, `false`, `null`, `undefined`)

Example - PREFERRED (nullish coalescing):
```typescript
// Only use default if value is null/undefined
const count = inputCount ?? 0;           // preserves 0 if passed
const name = user.name ?? 'Anonymous';   // preserves '' if intentionally empty
const enabled = config.enabled ?? true;  // preserves false if set
```

Example - WRONG (loses valid falsy values):
```typescript
// ❌ These lose valid falsy values!
const count = inputCount || 0;           // inputCount of 0 becomes 0 (ok by accident)
const name = user.name || 'Anonymous';   // empty string '' becomes 'Anonymous'
const enabled = config.enabled || true;  // false becomes true!
```

Example - CORRECT use of `||` (when you want falsy fallback):
```typescript
// ✅ Use || when empty string SHOULD trigger fallback
const displayText = user.bio || 'No bio provided';
```

### Readonly for immutable values

PREFER `readonly` for class properties that are assigned once and never change.

Example - PREFERRED:
```typescript
readonly #router = inject(Router);
readonly #twoMonthsAgo = subMonths(new Date(), 2);
readonly ReviewStatusEnum = ReviewStatusEnum;
```

Example - WRONG:
```typescript
#router = inject(Router);           // ❌ Missing readonly
twoMonthsAgo = subMonths(new Date(), 2);  // ❌ Missing readonly
```

### Private fields with # syntax

PREFER the `#` prefix for private class members over the `private` keyword.

Benefits:
- True runtime privacy (not just TypeScript compile-time)
- Cleaner syntax
- Modern JavaScript standard

Example - PREFERRED:
```typescript
readonly #router = inject(Router);
readonly #twoMonthsAgo = subMonths(new Date(), 2);

#calculateTotal() {
  return this.items().reduce((sum, item) => sum + item.price, 0);
}
```

Example - AVOID:
```typescript
private readonly router = inject(Router);      // ❌ Use # instead
private readonly twoMonthsAgo = subMonths(new Date(), 2);

private calculateTotal() {                      // ❌ Use # instead
  return this.items().reduce((sum, item) => sum + item.price, 0);
}
```

Note: For Angular lifecycle hooks and methods that need to be accessed by the framework or templates, keep them public (no modifier).

## RXJS - NESTED SUBSCRIBES

NEVER nest subscribes inside subscribes. This causes memory leaks, makes unsubscription unreliable, and complicates error handling.

When the inner observable depends on a value from the outer observable, use a flattening operator instead:
- `switchMap` — preferred default. Cancels the previous inner observable when a new outer value arrives. Use when only the latest result matters (e.g. fetching data based on a changing selection).
- `exhaustMap` — ignores new outer values while the inner observable is still active.
- `mergeMap` — does not cancel; runs all inner observables concurrently. Only use when you intentionally want parallel execution.

Example - WRONG:
```typescript
this.form.controls.organizationId.valueChanges
  .pipe(untilDestroyed(this))
  .subscribe((organizationId) => {
    this.repository.listUsers(organizationId)
      .pipe(untilDestroyed(this))
      .subscribe((users) => {
        this.userList = users;
      });
  });
```

Example - CORRECT:
```typescript
this.form.controls.organizationId.valueChanges.pipe(
  switchMap((organizationId) => this.repository.listUsers(organizationId)),
  untilDestroyed(this)
).subscribe((users) => {
  this.userList = users;
});
```

## STATE MANAGEMENT

State management: NgRx (ngrx)
Async operations: RxJS Observables
Follow existing patterns in codebase

## MODELS AND CONSTANTS - AVOID HARDCODED STRINGS

AVOID hardcoding strings - check for models first

Use models extensively for single source of truth:
- Status values
- Constants
- Enum-like data

Before hardcoding string values:
1. Search codebase for existing models or constants
2. Check design system for component models
3. Use model reference instead of hardcoding

Example: Instead of hardcoding `'DEFINITIVE'`, find publication status model

Purpose: Prevent duplication, centralize updates

## DEAD CODE CLEANUP - MANDATORY

AFTER any change that removes or replaces functionality, actively check for and remove code that is no longer used. Claude Code does not have IDE greying-out indicators, so this check must be done manually.

ALWAYS check and remove when applicable:
- **Inputs/outputs** on components or base classes that are no longer bound in any template
- **Imports** in `.ts` files that are no longer referenced (both `import` statements and `imports: []` array entries)
- **Methods and properties** on components/services that are no longer called
- **Template bindings** (`[input]`, `(event)`) that reference removed inputs or methods
- **SCSS classes** that are no longer used in any template
- **Translation keys** in `nl.json` / `en.json` that are no longer referenced in templates or code

HOW to check:
1. After making changes, search the codebase for any remaining references to what was removed
2. Pay special attention to base classes — removing an input from a child does not remove it from the parent
3. Use barrel exports (`index.ts`) as a signal: if something is exported but never imported anywhere, it may be dead

PURPOSE: Prevents accumulation of unreferenced code that is invisible without IDE feedback

## SONARQUBE RULES - KNOWN ISSUES

These SonarQube rules are actively enforced. When making changes, avoid triggering them.

### typescript:S4202 — Do not use `any` type
NEVER use `any`. Always define proper interfaces, types, or use generics. If data structure is unknown, use `unknown` and add type guards. See the TYPESCRIPT TYPING section above for examples.

### typescript:S109 — Magic numbers should not be used
Do not use numeric literals directly in code when their meaning is unclear. Extract them as named constants to improve readability.

Fix: Define constants with descriptive names at the file or component level.

Exceptions: Common values like 0, 1, -1 are generally acceptable.

**IMPORTANT**: This rule is about NUMBERS, not arrays or named values. Do NOT extract:
- Arrays of enum values used once (e.g., `[StatusEnum.ACTIVE, StatusEnum.PENDING]`)
- Named constants/enums that are already self-explanatory
- Values that are only used in one place and are already readable

The goal is readability. If a value is already self-explanatory (like named enum values), extracting it to a constant just adds indirection.

Example - WRONG (magic numbers):
```typescript
export class MyComponent {
  readonly showMoreAfter = input(3);  // ❌ What does 3 represent?
  readonly maxItems = 10;             // ❌ Magic number

  calculate(value: number) {
    return value * 100;               // ❌ What does 100 represent?
  }
}
```

Example - CORRECT (extracted numbers):
```typescript
const DEFAULT_SHOW_MORE_THRESHOLD = 3;
const MAX_DISPLAY_ITEMS = 10;
const PERCENTAGE_MULTIPLIER = 100;

export class MyComponent {
  readonly showMoreAfter = input(DEFAULT_SHOW_MORE_THRESHOLD);
  readonly maxItems = MAX_DISPLAY_ITEMS;

  calculate(value: number) {
    return value * PERCENTAGE_MULTIPLIER;
  }
}
```

Example - CORRECT (self-explanatory enum values, no extraction needed):
```typescript
// ✅ GOOD - enum values are already named and self-explanatory
[ReviewStatusEnum.IMPORTED, ReviewStatusEnum.PENDING_REVIEW].map((status) => ...)

// ❌ UNNECESSARY - adds indirection for no readability benefit
const ALLOWED_STATUSES = [ReviewStatusEnum.IMPORTED, ReviewStatusEnum.PENDING_REVIEW];
ALLOWED_STATUSES.map((status) => ...)
```

### typescript:S134 — Control flow statements should not be nested too deeply
Do not nest more than 3 levels of `if`/`for`/`while`/`switch`/`try` statements. Deeply nested code is harder to read, test, and maintain.

Fix: Extract nested logic into separate private methods. Each method should handle one level of the logic tree.

Example - WRONG (4 levels of nesting):
```typescript
for (const option of this.options()) {              // Level 1
  if (option.value === value) {                     // Level 2
    return option;
  }
  if (option.children) {                            // Level 2
    for (const child of option.children) {          // Level 3
      if (child.value === value) {                  // Level 4 ❌ TOO DEEP
        return child;
      }
    }
  }
}
```

Example - CORRECT (max 3 levels):
```typescript
for (const option of this.options()) {              // Level 1
  if (option.value === value) {                     // Level 2
    return option;
  }
  const childMatch = this.#findChild(option.children, value);  // Level 2
  if (childMatch) {                                 // Level 2
    return childMatch;
  }
}

// Extract nested logic into separate method
#findChild(children: Item[] | undefined, value: string): Item | null {
  if (!children) {                                  // Level 1
    return null;
  }
  for (const child of children) {                   // Level 1
    if (child.value === value) {                    // Level 2
      return child;
    }
  }
  return null;
}
```

### typescript:S1192 — String literals should not be duplicated
Do not duplicate string literals more than 2 times (i.e., the same literal appearing 3+ times). Duplicated strings make maintenance harder and are error-prone when values need to change.

Fix: Extract the duplicated string into a named constant. If the string represents a type union member or specific value, create both a type and a constant.

Exceptions: Short strings like empty strings `''`, single characters, or common tokens may not need extraction if they serve different semantic purposes.

Example - WRONG (string duplicated 3 times):
```typescript
export class MyComponent {
  columnLayout = input<'theme-date' | 'theme-date-menu' | 'theme-date-pill-owner-menu'>(
    'theme-date-pill-owner-menu'  // Duplication 1
  );

  headerClass = computed(() => {
    const layout = this.columnLayout();
    return {
      'header--theme-date': layout === 'theme-date',
      'header--theme-date-pill-owner-menu': layout === 'theme-date-pill-owner-menu', // Duplication 2
    };
  });

  showOwner = computed(() => {
    return this.columnLayout() === 'theme-date-pill-owner-menu'; // Duplication 3 ❌
  });
}
```

Example - CORRECT (extracted constant):
```typescript
type ColumnLayout = 'theme-date' | 'theme-date-menu' | 'theme-date-pill-owner-menu';

const DEFAULT_COLUMN_LAYOUT: ColumnLayout = 'theme-date-pill-owner-menu';

export class MyComponent {
  columnLayout = input<ColumnLayout>(DEFAULT_COLUMN_LAYOUT);

  headerClass = computed(() => {
    const layout = this.columnLayout();
    return {
      'header--theme-date': layout === 'theme-date',
      'header--theme-date-pill-owner-menu': layout === DEFAULT_COLUMN_LAYOUT, // ✅
    };
  });

  showOwner = computed(() => {
    return this.columnLayout() === DEFAULT_COLUMN_LAYOUT; // ✅
  });
}
```

### css:S4666 — Duplicate CSS selectors
Do not declare the same selector twice in a stylesheet. This happens when layout overrides are split across modifier classes and parent-selector blocks that resolve to the same compiled selector.

Fix: Consolidate all rules for a selector into one place. If a modifier class already targets an element, do not also use a parent-selector (`&`) pattern to target the same element.

### Web:S6853 — Form label must be associated with a control
Labels must be associated with their input controls. Use either:
- Explicit: `<label for="myId">` paired with `<input id="myId">`
- Implicit: nest the `<input>` directly inside the `<label>`

Note: This rule can produce false positives in Angular standalone components where Sonar cannot statically resolve `for`/`id` pairs. If the association is correct and Sonar still flags it, suppress on the Sonar server side rather than changing working code.
