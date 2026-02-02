# NTM Publicatie Overzicht Project

## Working Directory
- **Primary codebase**: `ntm-frontend/`
- **ALL git operations** must be executed from `/Users/daniel/Developer/NTM-Publicatie-overzicht/ntm-frontend`
- The root folder (`/Users/daniel/Developer/NTM-Publicatie-overzicht`) is NOT a git repository
- When creating branches, committing, pushing, or any git operations: always work in `ntm-frontend/`

## Branch Naming Convention
- Bug fixes: `bug/[story-id]/[task-id]/[description]`
- Features: `feature/[story-id]/[task-id]/[description]`
- Always create branches from `main`
- Description must be in kebab-case
- Story ID and Task ID will be provided via Azure DevOps screenshots at the start of each session
- Example: `bug/106187/108922/fix-verkeersbesluit-id-uppercase-validation`

## Commit Messages
- Single line only, no body, no co-authored
- Format: `type(scope): #[story-id] #[task-id] description`
- Types: `bug`, `feat`, `chore` (type will be provided via Azure DevOps screenshots)
- Scope: Use the name of the main component/feature affected (e.g., `traffic-order`, `map-controls`, `traffic-sign`)
- Always in English (even if code/UI is in Dutch)
- Description: lowercase, concise action
- Example: `bug(traffic-order): #106187 #108922 change uppercase to lowercase in UI`

### WIP Commits
- User may request WIP commits during development for incremental progress
- Format: `WIP: [descriptive context of current work]`
- Purpose: Provide enough context for future reference to understand what's in the commit
- Example: `WIP: add traffic order validation logic` or `WIP: implement map zoom functionality`

### Git Operations Policy
- NEVER stage files unless explicitly asked
- NEVER create commits unless explicitly asked (except WIP commits when requested)
- NEVER push commits unless explicitly asked
- User will manage git operations manually except when specifically requested

## Project Structure
- This is a monorepo containing frontend, multiple backends, and design system
- **Frontend**: `/ntm-frontend/` - The only project we edit
- **Backend projects**:
  - `ntm-backend` - Primary backend service
  - `ntm-tracker-backend` - Tracker service
  - Used as reference only to understand API contracts, data formats, and expected payloads
  - NEVER edit backend code
- **Design system**: `/ndw-design/`
  - Contains NTM design system in `/ndw-design/libs/ntm`
  - We always use the NTM design system for the frontend project
  - Used as reference only for components and styling
  - NEVER edit design system code

## Working Directory Rules
- ALL edits must be in `/ntm-frontend/` only
- Backend and design system are read-only references
- Use backend projects to understand API behavior and data structures
- Use design system to understand available components and their usage

## Index Files
- Always create `index.ts` files for better import management
- When creating new components, services, repositories, or types: add an `index.ts` file that re-exports the main exports
- When making changes to existing code in directories without an `index.ts` file: create one
- This allows cleaner imports: `import { MyComponent } from './components/my-component'` instead of `import { MyComponent } from './components/my-component/my-component.component'`

## Design System & Styling
- Use the NTM design system from `/ndw-design/libs/ntm` for UI components
- Most common components (cards, tabs, buttons, forms, etc.) are available in the design system
- Design system components come with associated models - never hardcode strings that exist in models
- Assume design system components exist unless told otherwise
- When asked to create UI elements, use design system components by default

## Code Style & Technology Stack

### Language
- Code, comments, and commit messages: English
- UI text displayed to users: Dutch
- Project uses ngx-translate (`@ngx-translate/core`) for internationalization
- UI text should be placed in translation files rather than hardcoded strings

### TypeScript & Angular
- Everything is TypeScript
- Using modern Angular patterns (keep up-to-date with latest Angular practices)
- **Control flow**: Use `@if`, `@else`, `@for`, `@let` (NOT `*ngIf`, `*ngFor`, etc.)
- **Signals**: Prefer using Angular signals where appropriate
  - Use signal-based `input()` and `output()` instead of decorators
  - Use signals for reactive state when it makes sense
  - Do NOT force signals everywhere - if something doesn't fit, use traditional approach
  - Avoid `toSignal()` casting unless absolutely necessary
  - If refactoring to signals is complex, note it for potential future task

### State Management & Data Flow
- **State management**: NgRx (ngrx) for state management
- **Async operations**: RxJS Observables (used throughout the project)
- Follow existing patterns seen in the codebase

### Testing & Build
- NEVER run tests or build commands unless explicitly requested
- User has browser open to check if things work
- Focus on writing code, not validating it runs

## Hardcoded Strings & Models
- AVOID hardcoding strings - check for existing models first
- The project uses models extensively to maintain a single source of truth (e.g., status values, constants, enum-like data)
- Before hardcoding a string value:
  1. Search the codebase for existing models or constants that define it
  2. Check if design system components provide models for that data
  3. Use the model reference instead of hardcoding the string
- Example: Instead of hardcoding `'DEFINITIVE'` in the template, find the model that defines publication statuses and use that
- This prevents duplication and makes updates centralized

## Comments & Documentation
- Keep code clean with minimal comments
- Code should be self-explanatory through good naming and structure
- **When to add comments:**
  - Temporary/mocked data (mark clearly for easy removal later)
  - Complex logic that needs explanation of "why" (not "what")
- **When NOT to add comments:**
  - Obvious code that developers can understand by reading
  - Describing what the code does (use clear naming instead)
- Use comments like `// TODO: remove mock data` or `// Temporary for testing` for temporary code
- This allows easy cleanup with requests like "remove temporary code" or "remove mocked data"

## Communication & Clarification
- Ask for clarification when requirements are ambiguous
- Confirm understanding before implementing if something seems unclear
- Don't be overly confident - it's better to ask than assume incorrectly
- Example: "Just to confirm, you want me to add this validation to the form here, right?"

## Workflow
- User will start each task by providing Azure DevOps screenshots showing:
  - Story and Task IDs
  - Description and acceptance criteria
  - Comments (if any)
- These screenshots provide all necessary context for branch naming, commit type, and implementation details
- Work is often done incrementally with WIP commits between stages
