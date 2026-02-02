# GIT COMMIT MESSAGE RULES

## FORMAT - MANDATORY

Single line only. NO body. NO co-authored-by.

Standard format: `type(scope): description`

Project-specific format (if project config specifies story/task IDs): `type(scope): #[story-id] #[task-id] description`

## LANGUAGE

ALL commit messages: English
Description: lowercase only
Description: concise action (what was done)

## COMMIT TYPES

Use these types:
- `feat` - New feature
- `bug` - Bug fix
- `chore` - Maintenance (refactoring, dependencies, config)
- `docs` - Documentation
- `test` - Tests

Project config may override available types. Check project-specific config file.

## SCOPE

Scope = main component/feature affected
Examples: `auth`, `api`, `ui`, `map-controls`, `user-profile`
Check project-specific config for scope conventions.

## EXAMPLES

Standard commits:
- `feat(auth): add login functionality`
- `bug(api): fix user data not loading`
- `chore(deps): update angular to v17`

Project-specific with IDs:
- `feat(auth): #12345 #67890 add login functionality`
- `bug(api): #12345 #67890 fix user data not loading`

WIP commits:
- `WIP: add user authentication logic`
- `WIP: implement map zoom controls`

## BRANCH NAMING

Format: `type/[story-id]/[task-id]/[description]`

Types:
- `bug` - Bug fixes
- `feature` - New features

Rules:
- Always create branches from `main`
- Description: kebab-case
- Story ID and Task ID: Obtained from project workflow (see WORKFLOW section)

Example: `bug/106187/108922/fix-verkeersbesluit-id-uppercase-validation`

## WORKFLOW - AZURE DEVOPS INTEGRATION

User starts tasks by providing Azure DevOps screenshots containing:
- Story ID and Task ID
- Task description and acceptance criteria
- Comments (if any)

Screenshots provide context for:
- Branch naming (story-id, task-id, description)
- Commit type (bug, feat, chore)
- Implementation details

Development process:
- Work done incrementally
- WIP commits used between stages when requested

## GIT OPERATIONS - CRITICAL RULES

NEVER stage files unless user explicitly requests it
NEVER create commits unless user explicitly requests it
NEVER push commits unless user explicitly requests it

Exception: WIP commits when user requests them during development

User manages git operations manually. Only execute git commands when specifically instructed.
