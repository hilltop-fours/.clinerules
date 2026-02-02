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

## SPECIAL CASE - .CLINERULES REPO

When committing to the .clinerules repository itself:

Format: `type(scope): description`

Rules:
- Single line only (no body, no line breaks)
- NO story/task IDs (not tracked in Azure DevOps)
- Description: lowercase, can be longer than work commits
- English language

Scope naming:
- Single file edits: Use `folder/filename` pattern
  - Examples: `global/git-instructions`, `global/angular-instructions`, `global/backend-api-format`
  - For project files: `projects/PROJECT-NAME/filename`
  - For backend docs: `projects/PROJECT-NAME/backend/BACKEND-NAME`

- Multiple file edits: Use parent folder as scope
  - Examples: `global`, `projects/NTM-Publicatie-overzicht`, `projects/GRG-Wegkenmerken-verkeersborden`

- Root level files: Use filename only
  - Examples: `claude`, `readme`

Commit types (only these 3 for .clinerules repo):
- `docs` - Documentation updates (most common for .clinerules)
- `feat` - New instruction sets or major additions
- `chore` - Restructuring, file moves, cleanup

Examples:
- `docs(global/git-instructions): add wip squashing rules for duplicate commit prevention`
- `docs(projects/NTM-Publicatie-overzicht/backend/ntm-tracker-backend): update external-organizations endpoints`
- `docs(global): update git workflow and main instructions`
- `chore(projects): reorganize backend documentation structure`
- `docs(claude): add execution flow section for on-demand rule reading`

**Multiple commits strategy:**

When changes span multiple folders in `.clinerules`:
- Create SEPARATE commits for each folder/scope
- Commit order: Root files first, then alphabetical by scope
- Each commit focuses on one logical change area

Example with multiple changes:
```
Changes:
- claude.md (root)
- global/git-instructions.md
- projects/NTM/backend/ntm-tracker-backend.md

Commit order:
1. docs(claude): [changes to claude.md]
2. docs(global/git-instructions): [changes to git-instructions]
3. docs(projects/NTM/backend/ntm-tracker-backend): [changes to backend docs]
```

This keeps git history clean and changes logically separated.

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

## WIP SQUASHING - CRITICAL RULES

When user requests to "squash WIP commits" or similar:

**Step 1: Check for existing commit**
- Run `git log --oneline -10` to view recent commits (last 10 commits)
- Look for existing commit with the SAME story-id and task-id as WIP commits
- CRITICAL: Only consider commits that appear DIRECTLY BELOW ALL WIP commits with NO gaps
- Example: If WIP commits are for #12345 #67890, search for commit containing "#12345 #67890" immediately below the last WIP commit

**Step 2: Verify consecutive commits (no gaps)**
- Check that ALL WIP commits are consecutive (no other commits between them)
- Check that matching commit is IMMEDIATELY after the last WIP commit
- If ANY gaps exist (between WIPs or between WIPs and matching commit), create NEW commit instead

**Step 3: Choose squashing strategy**

**If existing commit FOUND directly below consecutive WIP commits:**
- Squash WIP commits INTO that existing commit using `git reset --soft` + `git commit --amend`
- Commands:
  ```bash
  git reset --soft HEAD~N  # N = number of WIP commits only (NOT including existing commit)
  git commit --amend --no-edit  # Amend into existing commit below, keep its message
  ```
- Result: Single commit with original message preserved
- This PREVENTS duplicate commits with identical messages

**If NO existing commit found OR gaps exist:**
- Create new properly formatted commit by squashing all WIP commits
- Use proper commit format: `type(scope): #[story-id] #[task-id] description`
- Commands:
  ```bash
  git reset --soft HEAD~N  # N = number of WIP commits only
  git commit -m "proper message here"
  ```

**Critical constraints:**
- ONLY squash into existing commit if ALL WIP commits are consecutive AND matching commit is immediately below
- If there are ANY commits between WIP commits themselves, create NEW commit
- If there are ANY commits between last WIP and matching story/task commit, create NEW commit
- NEVER use interactive rebase unless absolutely necessary (prefer reset + amend/commit)
- ALWAYS check git log before squashing
- User will notice immediately if duplicate commits are created

**Scenario 1: Squash INTO existing (CORRECT)**
```
Current log (newest first):
abc123 WIP: add zoom buttons
def456 WIP: implement zoom logic
ghi789 feat(map): #12345 #67890 add map zoom controls  ← First commit below WIPs, matches IDs
jkl012 bug(api): #11111 #22222 fix loading issue

Action: Squash WIP commits
Verification: ✅ WIP commits are consecutive (abc123, def456), ✅ matching commit directly below
Result: Squash abc123 and def456 INTO ghi789 (same story/task IDs, directly below, no gaps)
Commands: git reset --soft HEAD~2, then git commit --amend --no-edit
Final: Single commit "feat(map): #12345 #67890 add map zoom controls" with all changes
```

**Scenario 2: Create NEW commit (CORRECT - other commits in between WIP and matching)**
```
Current log (newest first):
abc123 WIP: add zoom buttons
def456 chore(deps): update angular version  ← Different commit in between!
ghi789 feat(map): #12345 #67890 add map zoom controls  ← Matching IDs but NOT directly below
jkl012 bug(api): #11111 #22222 fix loading issue

Action: Squash WIP commits
Result: Create NEW commit (because def456 is between WIP and matching commit - gap exists)
Commands: git reset --soft HEAD~1, then git commit -m "new message"
Final: "feat(map): #12345 #67890 add zoom buttons to map controls" (new commit with WIP changes)
```

**Scenario 2b: Create NEW commit (CORRECT - gaps between WIP commits)**
```
Current log (newest first):
abc123 WIP: add zoom buttons
def456 chore(deps): update angular version  ← Gap between WIP commits!
ghi789 WIP: implement zoom logic
jkl012 feat(map): #12345 #67890 add map zoom controls  ← Matching IDs exist

Action: Squash WIP commits
Result: Create NEW commit (because gap exists BETWEEN WIP commits themselves)
Reasoning: WIP commits must be consecutive. Any gaps = create new commit.
Final: New commit squashing abc123 and ghi789 (skip def456)
```

**Scenario 3: Create NEW commit (CORRECT - no matching commit exists)**
```
Current log (newest first):
abc123 WIP: add authentication
def456 WIP: implement login form
ghi789 bug(api): #11111 #22222 fix loading issue  ← No matching story/task IDs

Action: Squash WIP commits
Result: Create NEW commit (no existing commit with #12345 #67890 found)
Final: "feat(auth): #12345 #67890 add user authentication with login form"
```

**Scenario 4: DO NOT squash into old commit (WRONG)**
```
Current log (newest first):
abc123 WIP: add zoom buttons
def456 bug(api): #11111 #22222 fix loading issue
ghi789 chore(deps): update packages
... (10 more commits)
xyz789 feat(map): #12345 #67890 add map zoom controls  ← Too far back! From different day

Action: Squash WIP commits
Result: Create NEW commit (matching commit is too old, not directly below)
Reasoning: Squashing into old commit could cause unexpected merge/conflict issues
```

## GIT OPERATIONS - CRITICAL RULES

NEVER stage files unless user explicitly requests it
NEVER create commits unless user explicitly requests it
NEVER push commits unless user explicitly requests it

**CRITICAL: Each operation must be separate**
- NEVER combine `git add` and `git commit` in a single command (e.g., `git add . && git commit`)
- NEVER combine `git commit` and `git push` in a single command (e.g., `git commit && git push`)
- Each operation (add, commit, push) must be executed in SEPARATE tool calls
- This ensures user permission prompts work correctly for each operation

**CRITICAL: Each request is ONE-TIME ONLY**
- If user says "stage these files", run ONLY `git add` then STOP
- If user says "commit this", run ONLY `git commit` then STOP
- If user says "push", run ONLY `git push` then STOP
- DO NOT continue staging/committing/pushing in subsequent operations unless explicitly asked again
- Each git operation requires fresh explicit permission
- "Always allow" in settings does NOT mean "do this automatically forever"
- You must be explicitly instructed for EACH separate git operation

Exception: WIP commits when user requests them during development
Exception: Squashing WIP commits when user explicitly requests it (see WIP SQUASHING section above)

User manages git operations manually. Only execute git commands when specifically instructed.
