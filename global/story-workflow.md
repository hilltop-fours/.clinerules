# STORY WORKFLOW

Standard workflow when user starts a new story/task. Follow these steps in order.

---

## TRIGGER PHRASES

Activate this workflow when user says:
- "new story", "start story", "here's the story"
- "new task", "start task", "here's the task"
- "new bug", "start bug", "here's the bug"
- Or posts Azure DevOps screenshots of a story/task/bug at the start of a conversation

---

## STEP 1: EXTRACT & PRESERVE ORIGINAL TEXT

**When user posts story/task screenshots, IMMEDIATELY extract and save the exact text.**

This is the most critical step — the original text gets lost when conversation context is compacted.

### What to extract

**From Story (parent):**
- Description (exact text, preserve Dutch/English, formatting, lists)
- Acceptance Criteria (exact text)
- Discussion (exact text, or "None" if empty)

**From Task or Bug (child):**
- Description (exact text, or "See story" if empty/redundant)
- Discussion (exact text, or "None" if empty)

### Where to save

Create a markdown file in the **project root** (not in `.clinerules/`).

**Filename convention:**
- Derived from what the PR title would be
- Format: `type(scope)-storyId-taskId-description.md`
- Keep `()` for scope, strip `#` and `:`, spaces become `-`, all lowercase
- Type: `feat` for features, `bug` for bugs, `chore` for chores (matches git-instructions.md)

Examples:
- PR title: `feat(map): #12345 #67890 add map zoom controls`
- Filename: `feat(map)-12345-67890-add-map-zoom-controls.md`

**One file per task/bug** — if a story has multiple children, each gets its own file with the shared story context repeated at the top.

### File structure

```markdown
# [PR Title]

**Story:** #[id] / **Task:** #[id]
**Branch:** `type/[story-id]/[task-id]/description`
**Date:** YYYY-MM-DD

---

## Story — Original Text

### Description
[Exact text from screenshot — preserve original language and formatting]

### Acceptance Criteria
[Exact text from screenshot — preserve original language and formatting]

### Discussion
[Exact text from screenshot, or "None"]

---

## Task — Original Text

### Description
[Exact text from screenshot, or "See story"]

### Discussion
[Exact text from screenshot, or "None"]

---

## Analysis

[Filled in during Step 2]

---

## Implementation Plan

[Filled in during Step 3]
```

**IMPORTANT:** Save this file immediately after extracting text. Do not wait until later steps.

---

## STEP 2: ANALYZE & DISCUSS

After saving the text, explain back to the user what needs to be done.

**Rules:**
- Conversational only — NO code examples, NO implementation details yet
- Explain in plain language what the story/task is asking for
- Identify any ambiguities or open questions
- Confirm understanding with the user before moving to planning
- Goal: both Claude and user agree on WHAT needs to be done before discussing HOW

**After discussion, update the Analysis section** in the story file with a summary of what was agreed.

---

## STEP 3: CREATE PHASED PLAN

Break the work into phases that can be independently verified and committed.

**Rules:**
- Each phase should be small enough that the user can check the code works
- Each phase should produce a visible, testable result where possible
- Suggest WIP commits between phases (per git-instructions.md)
- Phases will later be squashed into a single commit on main via PR

**Update the Implementation Plan section** in the story file with the phases.

**Phase structure:**
```markdown
### Phase 1: [Name]
[What this phase covers — deliverables, not implementation details]

### Phase 2: [Name]
[What this phase covers]

### Phase 3: [Name]
[What this phase covers]
```

---

## STEP 4: DEV TOOLS (IF NEEDED)

Sometimes development requires mock data or special tooling because:
- Backend is not merged yet
- Need to test with different account types/roles
- Need to simulate specific data states

**When needed:**
- Create a small dev tools component for the development period
- Keep it simple — just enough to unblock development
- User decides when to clean it up (no automatic cleanup phase)

---

## STEP 5: EXECUTE PHASES

Work through phases one at a time.

**During implementation:**
- Follow `angular-instructions.md` for all Angular code
- Follow `code-simplicity.md` for keeping code simple
- Focus on making the result work — do NOT run validation during development
- Suggest WIP commits at natural breakpoints between phases
- Wait for user confirmation before moving to the next phase

**Do NOT:**
- Implement everything in one go
- Run validation checks during development (only when explicitly asked)
- Move to the next phase without user confirming the current one works

---

## STEP 6: VALIDATE (WHEN ASKED)

Only run the full validation workflow when the user explicitly asks to validate.

This follows the existing validation rules in `claude.md` — build, lint, prettier, code review.

During development phases, focus entirely on making the feature work correctly.
