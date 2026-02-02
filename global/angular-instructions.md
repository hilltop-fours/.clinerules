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
