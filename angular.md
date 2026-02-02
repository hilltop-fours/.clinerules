# Angular Best Practices

## Modern Angular Patterns

### Control Flow
- Use `@if`, `@else`, `@for`, `@let` (NOT `*ngIf`, `*ngFor`, etc.)
- Modern control flow syntax is cleaner and more performant

### Signals
- Prefer using Angular signals where appropriate
- Use signal-based `input()` and `output()` instead of decorators
- Use signals for reactive state when it makes sense
- Do NOT force signals everywhere - if something doesn't fit, use traditional approach
- Avoid `toSignal()` casting unless absolutely necessary

### Component Structure
- Keep components focused and single-responsibility
- Use standalone components when possible
- Organize imports clearly

### Index Files
- Always create `index.ts` files for better import management
- When creating new components, services, or types: add an `index.ts` file that re-exports the main exports
- This allows cleaner imports: `import { MyComponent } from './components/my-component'`

## Code Quality

### Comments
- Keep code clean with minimal comments
- Code should be self-explanatory through good naming and structure
- Only add comments for:
  - Temporary/mocked data (mark clearly for easy removal later)
  - Complex logic that needs explanation of "why" (not "what")
- Avoid obvious comments that just describe what the code does

### Naming Conventions
- Use clear, descriptive names
- Follow Angular naming conventions (component.ts, service.ts, etc.)
- Use kebab-case for file names
- Use PascalCase for class names
- Use camelCase for variables and methods
