# CLAUDE CODE - INSTRUCTIONS

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
    - `project-instructions.md` - Project setup, paths, and backend mapping
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
- Read: `.clinerules/projects/NTM-Publicatie-overzicht/project-instructions.md`

**IF path contains `/GRG-Wegkenmerken-verkeersborden`**:
- Project: GRG Wegkenmerken Verkeersborden
- Read: `.clinerules/projects/GRG-Wegkenmerken-verkeersborden/project-instructions.md`

**IF path contains `/BER-Bereikbaarheidskaart`**:
- Project: BER Bereikbaarheidskaart
- Read: `.clinerules/projects/BER-Bereikbaarheidskaart/project-instructions.md`

---

## READ RULES ON-DEMAND

**BEFORE any git operation** (commit, branch, push, pull, status, checkout):
→ Read `.clinerules/global/git-instructions.md`

**BEFORE editing ANY code** (components, services, templates, styles):
→ Read `.clinerules/global/angular-instructions.md`
→ Reference `.clinerules/validation/angular-*.md` files for detailed style and validation rules

**WHEN user asks to update backend API docs**:
→ Read `.clinerules/global/update-backend-api-instructions.md`

**WHEN implementing features or need project context**:
→ Read project-specific `project-instructions.md` (found via project identification above)

**WHEN need frontend-specific coding rules**:
→ Read `frontend/{repo-name}.md` from the project directory
→ Examples: `frontend/ntm-frontend.md`, `frontend/traffic-sign-frontend.md`

**WHEN implementing features that use backend APIs**:
→ Project's `project-instructions.md` has backend mapping
→ Backend mapping tells you which backend `.md` file to read
→ Example: NTM project → read `.clinerules/projects/NTM-Publicatie-overzicht/backend-api.md`

**IF confused about backend API documentation format**:
→ Read `.clinerules/global/backend-api-format.md` (reference only)

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
2. **Read `project-instructions.md`** for that project
3. **Based on task**, read additional files:
   - Git operation → Read `global/git-instructions.md`
   - Code editing → Read `global/angular-instructions.md` + validation files (`validation/angular-*.md`) + `frontend/{repo}.md`
   - Backend API work → Read appropriate backend `.md` file
   - Update backend docs → Read `global/update-backend-api-instructions.md`
4. **Apply all rules** from the files you read to every action

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
