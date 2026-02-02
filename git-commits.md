# Git Commit Standards

## Commit Message Format
- Single line only, no body
- Format: `type(scope): description`
- Always in English
- Description: lowercase, concise action describing what was done

## Commit Types
- `feat`: New feature
- `bug`: Bug fix
- `chore`: Maintenance tasks (refactoring, dependencies, config)
- `docs`: Documentation changes
- `test`: Adding or updating tests

## Scope
- Use the name of the main component/feature affected
- Examples: `auth`, `api`, `ui`, `map-controls`, `user-profile`
- Keep scopes consistent within a project

## Examples
- `feat(auth): add login functionality`
- `bug(api): fix user data not loading`
- `chore(deps): update angular to v17`

## WIP Commits
- Format: `WIP: [descriptive context of current work]`
- Use during development for incremental progress
- Provide enough context for future reference
- Examples:
  - `WIP: add user authentication logic`
  - `WIP: implement map zoom controls`

## Git Operations Policy
- NEVER stage files unless explicitly asked
- NEVER create commits unless explicitly asked
- NEVER push commits unless explicitly asked
- User will manage git operations manually except when specifically requested
