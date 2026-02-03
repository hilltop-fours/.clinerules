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

### css:S4666 — Duplicate CSS selectors
Do not declare the same selector twice in a stylesheet. This happens when layout overrides are split across modifier classes and parent-selector blocks that resolve to the same compiled selector.

Fix: Consolidate all rules for a selector into one place. If a modifier class already targets an element, do not also use a parent-selector (`&`) pattern to target the same element.

### Web:S6853 — Form label must be associated with a control
Labels must be associated with their input controls. Use either:
- Explicit: `<label for="myId">` paired with `<input id="myId">`
- Implicit: nest the `<input>` directly inside the `<label>`

Note: This rule can produce false positives in Angular standalone components where Sonar cannot statically resolve `for`/`id` pairs. If the association is correct and Sonar still flags it, suppress on the Sonar server side rather than changing working code.
