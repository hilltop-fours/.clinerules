# CLAUDE CODE - INSTRUCTIONS

## ⚠️ PURPOSE - READ FIRST

**Everything in this `.clinerules/` repository is written EXCLUSIVELY for Claude Code.**

This is not human documentation. Every file, every structure, every detail exists for one purpose only:
- **For Claude to read and understand**
- **For Claude to make better decisions**
- **For Claude to write better code**
- **For Claude to locate information quickly**

When creating or editing files in `.clinerules/`:
- Optimize for Claude's comprehension, not human readability
- Structure for Claude's navigation patterns
- Write explanations for Claude's task-based reading
- Format files so Claude can find and apply the rules efficiently

The user should never need to read this. If the user needs information, Claude reads it and explains. If the user needs to change something, they ask Claude to edit it.

---

## FOLDER STRUCTURE REFERENCE

Quick overview of `.clinerules/` organization:

- `global/` - Shared rules for all projects
  - `git-instructions.md` - Git workflow, commit messages, branch naming, pushing
  - `angular-instructions.md` - Angular coding standards and patterns
  - `update-backend-api-instructions.md` - How to update backend documentation
  - `backend-api-format.md` - Format reference for backend API documentation

- `validation/` - Detailed validation and style enforcement rules
  - `angular-*.md` - Specific Angular style and validation rules

- `projects/` - Project-specific configurations
  - `{PROJECT-NAME}/`
    - `README.md` - Project structure, repository locations, editing boundaries (reference)
    - `project-instructions.md` - Task-based rules, backend API mapping, patterns
    - `patterns/` - Project-specific code patterns and conventions (optional)
    - `frontend/` - Frontend-specific coding rules for this project
      - `{repo-name}.md` - Component structure, naming, style patterns
    - `backend/` - Backend API documentation for this project
      - `{backend-name}.md` - API endpoints, models, request/response formats

- `claude.md` - This file (Claude Code instructions and task rules)

---

## PROJECT IDENTIFICATION

Check your current working directory path and match it against these patterns:

**IF path contains `/NTM-Publicatie-overzicht`**:
- Project: NTM Publicatie Overzicht
- $CLINERULES_ROOT = `/Users/daniel/Developer/NTM-Publicatie-overzicht/.clinerules`
- Read: `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/project-instructions.md`

**IF path contains `/GRG-Wegkenmerken-verkeersborden`**:
- Project: GRG Wegkenmerken Verkeersborden
- $CLINERULES_ROOT = `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/.clinerules`
- Read: `$CLINERULES_ROOT/projects/GRG-Wegkenmerken-verkeersborden/project-instructions.md`

**IF path contains `/BER-Bereikbaarheidskaart`**:
- Project: BER Bereikbaarheidskaart
- $CLINERULES_ROOT = `/Users/daniel/Developer/BER-Bereikbaarheidskaart/.clinerules`
- Read: `$CLINERULES_ROOT/projects/BER-Bereikbaarheidskaart/project-instructions.md`

---

## PATH VARIABLES

For file references in `.clinerules/` documentation, use the `$CLINERULES_ROOT` variable:

**$CLINERULES_ROOT** = Absolute path to `.clinerules/` directory for the current project
- NTM: `/Users/daniel/Developer/NTM-Publicatie-overzicht/.clinerules`
- GRG: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/.clinerules`
- BER: `/Users/daniel/Developer/BER-Bereikbaarheidskaart/.clinerules`

All file references use this pattern: `$CLINERULES_ROOT/path/to/file.md`

---

## READ RULES ON-DEMAND

**BEFORE any git operation** (commit, branch, push, pull, status, checkout):
→ Read `$CLINERULES_ROOT/global/git-instructions.md`
→ **CRITICAL FOR .CLINERULES COMMITS**: Single line only, `type(scope): description` format, separate commits per scope (see git-instructions.md SPECIAL CASE section)

**BEFORE editing ANY code** (components, services, templates, styles):
→ Read `$CLINERULES_ROOT/global/angular-instructions.md`
→ Reference `$CLINERULES_ROOT/validation/angular-*.md` files for detailed style and validation rules

**WHEN user asks to update backend API docs**:
→ Read `$CLINERULES_ROOT/global/update-backend-api-instructions.md`

**WHEN implementing features or need project context**:
→ Read project-specific `project-instructions.md` (found via project identification above)

**WHEN need frontend-specific coding rules**:
→ Read appropriate `frontend/{repo-name}.md` from `$CLINERULES_ROOT/projects/{PROJECT}/frontend/`

**WHEN implementing features that use backend APIs**:
→ Project's `project-instructions.md` has backend mapping with `$CLINERULES_ROOT` paths

**IF confused about backend API documentation format**:
→ Read `$CLINERULES_ROOT/global/backend-api-format.md` (reference only)

---

## FILE TYPE RESTRICTIONS - FRONTEND ONLY

All **frontend projects** use ONLY these file types:
- TypeScript: `.ts` files
- HTML templates: `.html` files
- SCSS styles: `.scss` files

**Glob patterns**: Always use `**/*.{ts,html,scss}` when searching frontend code

**NEVER** search for or work with in frontend:
- `.js`, `.jsx`, `.tsx` (not used in frontend)
- `.css`, `.less` (frontend uses only SCSS)
- `.vue` (Angular projects only)

**Note**: Backend uses Java (`.java`, `.xml`, etc.) but you NEVER edit backend code, so backend file types are irrelevant.

---

## EDITING RESTRICTIONS - CRITICAL

**ONLY edit files in these locations**:
- Frontend source code: `/{frontend-repo}/src/**`
  - Example: `/ntm-frontend/src/**` (all subdirectories)
- Rules/docs: `/.clinerules/**` (any file in .clinerules folder)

**NEVER edit**:
- Backend code: `/*-backend/**` (any backend directory)
- Design system: `/ndw-design/**`, `/design/**`
- Node modules: `/**/node_modules/**`
- Build artifacts: `/**/dist/**`, `/**/build/**`, `/**/.angular/**`

**You CAN read (but not edit) outside `/src/`**:
- Configuration files: `package.json`, `angular.json`, `tsconfig.json`, etc.
- When user explicitly asks to check/read specific files
- When needed for understanding project setup

**Exception for editing**: Only edit config files if user explicitly says "edit", "update", or "change"

**Example**:
- User: "Check if package abc is installed" → ✅ Read `package.json`
- User: "Add package abc" → ✅ Edit `package.json`
- Without explicit instruction → ❌ Don't edit config files

---

## EXECUTION FLOW

1. **Identify project** using working directory path
2. **Define $CLINERULES_ROOT** based on project (see PROJECT IDENTIFICATION section)
3. **Read `project-instructions.md`** at `$CLINERULES_ROOT/projects/{PROJECT}/project-instructions.md`
4. **Based on task**, read additional files:
   - Git operation → Read `$CLINERULES_ROOT/global/git-instructions.md`
   - Code editing → Read `$CLINERULES_ROOT/global/angular-instructions.md` + `$CLINERULES_ROOT/validation/angular-*.md` + frontend rules
   - Backend API work → Read appropriate backend `.md` file (path in project-instructions.md)
   - Update backend docs → Read `$CLINERULES_ROOT/global/update-backend-api-instructions.md`
5. **Apply all rules** from the files you read to every action

---

## DEBUGGING & BUILD ERRORS

Frontend projects use build commands to identify issues:
- Run: `npm run build` or `npx ng build` (from the frontend directory)
- Build output shows all errors/warnings clearly
- No persistent log files—errors are caught via build command
- Debug by: running build, adding console.log statements, re-running build
- (Chrome DevTools connection via Claude Code is future work, not currently available)

---

## GIT EXECUTION CONTEXT

**Git commands execute from these subdirectories ONLY** (project roots are NOT git repos):

1. **For `.clinerules/**` changes** (rules, docs, API documentation):
   - Execute git from: `{project-root}/.clinerules/` directory
   - Example: `/Users/daniel/Developer/NTM-Publicatie-overzicht/.clinerules`
   - Example: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/.clinerules`

2. **For frontend code changes** (components, services, styles):
   - Execute git from: `{project-root}/{project-prefix}-frontend/` directory
   - Example: `/Users/daniel/Developer/NTM-Publicatie-overzicht/ntm-frontend`
   - Example: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-frontend`

**Critical**: Never attempt git commands from project root directories (e.g., `/NTM-Publicatie-overzicht/` or `/GRG-Wegkenmerken-verkeersborden/`) as they are NOT git repositories.

---

## CRITICAL REMINDERS

- Backend is **reference only** - Never edit backend source code
- Design system is **reference only** - Never edit design system code
- Project root directories are **NOT git repositories** - Git commands will fail there
- Git operations MUST execute from specified subdirectories (see Git Execution Context above)
- When in doubt, refer to the project's `project-instructions.md`
