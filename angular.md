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
