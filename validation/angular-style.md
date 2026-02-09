# Angular Code Style Validation Rules

These are style preferences and conventions for Angular code. They represent best practices for readability and consistency.

## Ternary operator for simple conditionals

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

## Nullish coalescing for default values

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

## Readonly for immutable values

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

## Class and style bindings over directives

PREFER built-in `[class]` and `[style]` bindings over `ngClass` and `ngStyle` directives.

Benefits:
- More straightforward syntax aligned with standard HTML attributes
- Better performance (no directive overhead)
- Easier to read and understand

Example - PREFERRED (individual properties):
```html
<div [class.admin]="isAdmin" [class.dense]="density === 'high'">
  <div [style.color]="textColor" [style.background-color]="backgroundColor">
  </div>
</div>
```

Example - PREFERRED (object syntax):
```html
<div [class]="{admin: isAdmin, dense: density === 'high'}">
  <div [style]="{'color': textColor, 'background-color': backgroundColor}"></div>
</div>
```

Example - AVOID (directives):
```html
<div [ngClass]="{admin: isAdmin, dense: density === 'high'}">
  <div [ngStyle]="{'color': textColor, 'background-color': backgroundColor}"></div>
</div>
```

Reference: [Angular bindings guide](https://angular.dev/guide/templates/binding#css-class-and-style-property-bindings)

## Event handler naming - describe the action, not the event

PREFER naming event handlers for the action they perform rather than the triggering event.

This makes it immediately clear what an event does from reading the template, rather than requiring you to find the handler implementation.

Example - PREFERRED (action-based names):
```html
<button (click)="saveUserData()">Save</button>
<textarea (keydown.control.enter)="commitNotes()" (keydown.control.space)="showSuggestions()"></textarea>
```

Example - AVOID (event-based names):
```html
<button (click)="handleClick()">Save</button>
<textarea (keydown)="handleKeydown($event)"></textarea>
```

**Exception - Complex event handling:**

When event handling logic is especially long or complex, it's acceptable to use a generic handler name and delegate to more specific behaviors based on the event details:

```typescript
// Acceptable when the handler contains complex conditional logic
handleKeydown(event: KeyboardEvent) {
  if (event.ctrlKey && event.key === 'Enter') {
    this.commitNotes();
  } else if (event.ctrlKey && event.key === ' ') {
    this.showSuggestions();
  }
}
```

Reference: [Angular event binding guide](https://angular.dev/guide/templates/event-binding)

## Private fields with # syntax

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
