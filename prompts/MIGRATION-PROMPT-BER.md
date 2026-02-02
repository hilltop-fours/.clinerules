# MIGRATION PROMPT FOR BER PROJECT

**Copy and paste this entire message into Claude Code when working in the BER project directory.**

---

I need to restructure the `.clinerules` folder to match the new optimized structure we created for the NTM project. This restructure reduces token usage by organizing files more efficiently and using on-demand loading instead of injecting everything upfront.

## CONTEXT: Why We're Doing This

The `.clinerules` folder is a git submodule shared across multiple projects (NTM, GRG, BER). We recently restructured it to:
1. Reduce token usage by slimming `claude.md` from 154 to 113 lines
2. Create a `global/` folder for shared rules (git, angular, backend update process)
3. Create project-specific `project-instructions.md` files that contain:
   - Repository locations (frontend, backends, design system)
   - Git execution directory
   - Backend API mapping (which backend file to read for which feature)
4. Update frontend-specific files with complete coding rules

The global files are already done. Now I need you to create the BER-specific files.

## CURRENT WORKING DIRECTORY

You are in: `/Users/daniel/Developer/BER-Bereikbaarheidskaart`

## BER PROJECT SPECIFICS

### Repository Locations:
- **Frontend**: `/Users/daniel/Developer/BER-Bereikbaarheidskaart/accessibility-map-frontend/`
- **Git operations directory**: `/Users/daniel/Developer/BER-Bereikbaarheidskaart/accessibility-map-frontend`
- **Design system**: `/Users/daniel/Developer/BER-Bereikbaarheidskaart/ndw-design/`

### Backend Services (4 total - shared from GRG):
1. **traffic-sign-backend** (`/Users/daniel/Developer/BER-Bereikbaarheidskaart/traffic-sign-backend/`)
   - Traffic sign data for accessibility mapping
   - Shared copy from GRG project

2. **traffic-sign-wkd-backend** (`/Users/daniel/Developer/BER-Bereikbaarheidskaart/traffic-sign-wkd-backend/`)
   - Vehicle restrictions for route planning
   - Shared copy from GRG project

3. **traffic-sign-area-backend** (`/Users/daniel/Developer/BER-Bereikbaarheidskaart/traffic-sign-area-backend/`)
   - Environmental/emission zones
   - Shared copy from GRG project

4. **traffic-sign-feedback-backend** (`/Users/daniel/Developer/BER-Bereikbaarheidskaart/traffic-sign-feedback-backend/`)
   - User feedback submissions
   - Shared copy from GRG project

### Translation System:
**BER has NO translation system** - UI text is hardcoded in Dutch (no ngx-translate, no translation files)

## TASKS

### Task 1: Create `projects/BER-Bereikbaarheidskaart/project-instructions.md`

Read the existing file:
`.clinerules/projects/BER-Bereikbaarheidskaart/README.md`

Then create a new file:
`.clinerules/projects/BER-Bereikbaarheidskaart/project-instructions.md`

**Content should include**:

1. **PROJECT IDENTIFICATION** section:
   - Working directory path contains: `/BER-Bereikbaarheidskaart`

2. **REPOSITORY LOCATIONS** section:
   - Frontend path and git operations directory
   - All 4 backend service paths (listed above)
   - Design system path (shared GRG design system)
   - Note that project root is NOT a git repository

3. **BACKEND API MAPPING** section:
   - By Feature Domain (e.g., "Traffic Signs (for maps)" → traffic-sign-backend.md)
   - By Endpoint Pattern (e.g., `/api/traffic-signs/**` → traffic-sign-backend.md)
   - List ALL 4 backend services with their feature areas
   - Note: These are shared copies from GRG, used for accessibility mapping
   - Note: Can be incomplete, user can expand later with backend crawl

4. **MONOREPO STRUCTURE** section:
   - Show directory tree
   - Mark what's editable vs reference only
   - Note that backends are shared from GRG

5. **WHEN TO READ OTHER FILES** section:
   - Frontend coding rules → frontend/accessibility-map-frontend.md
   - Backend API details → appropriate backend file
   - Update backend docs → global/update-backend-api-instructions.md

**Use this as a reference** (modify for BER):
`.clinerules/projects/NTM-Publicatie-overzicht/project-instructions.md`

### Task 2: Update `projects/BER-Bereikbaarheidskaart/frontend/BER-Bereikbaarheidskaart.md`

Rename it to:
`projects/BER-Bereikbaarheidskaart/frontend/accessibility-map-frontend.md`

**Content should include**:

1. **FILE TYPE RESTRICTIONS** section:
   - Only .ts, .html, .scss files
   - Glob pattern: `**/*.{ts,html,scss}`

2. **DESIGN SYSTEM** section:
   - GRG design system location (shared from GRG project)
   - Component usage rules

3. **LANGUAGE RULES** section:
   - Code & comments: English
   - Commit messages: English
   - UI text: Dutch (hardcoded, NO translation system)
   - **CRITICAL**: BER has NO ngx-translate, NO translation files
   - UI text is hardcoded Dutch strings directly in templates

4. **WORKING DIRECTORY** section:
   - Primary codebase: `/accessibility-map-frontend/src/`
   - What's editable vs not editable
   - Config files read-only unless explicitly asked

**Use this as a reference** (modify for BER - remove translation system parts):
`.clinerules/projects/NTM-Publicatie-overzicht/frontend/ntm-frontend.md`

### Task 3: Delete Old README

After creating the files above, delete:
`.clinerules/projects/BER-Bereikbaarheidskaart/README.md`

## VERIFICATION

After completing the tasks:

1. Verify the structure matches:
```
.clinerules/projects/BER-Bereikbaarheidskaart/
├── project-instructions.md    (NEW)
├── frontend/
│   └── accessibility-map-frontend.md    (RENAMED & UPDATED)
└── backend/
    └── (4 backend files, unchanged)
```

2. Read the new `project-instructions.md` to confirm it has:
   - All 4 backend paths
   - Backend API mapping (feature domain + endpoint patterns)
   - Git execution directory
   - Correct frontend repo name
   - Note that backends are shared from GRG

3. Read the new `accessibility-map-frontend.md` to confirm:
   - File type restrictions are clear
   - Language rules specify NO translation system
   - UI text is hardcoded Dutch
   - Design system is shared from GRG

## IMPORTANT NOTES

- **Don't modify backend markdown files** - only create/update project-instructions.md and frontend file
- **The global/ folder is already done** - don't touch it
- **Backend API mapping can be incomplete** - list major features, user can expand later
- **BER has NO translation system** - this is different from NTM, be explicit about it
- **BER uses shared backends from GRG** - these are copies, not separate implementations

---

Execute the tasks above. After completion, show me the final structure and let me verify the content.
