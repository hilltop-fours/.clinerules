# MIGRATION PROMPT FOR GRG PROJECT

**Copy and paste this entire message into Claude Code when working in the GRG project directory.**

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

The global files are already done. Now I need you to create the GRG-specific files.

## CURRENT WORKING DIRECTORY

You are in: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden`

## GRG PROJECT SPECIFICS

### Repository Locations:
- **Frontend**: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-frontend/`
- **Git operations directory**: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-frontend`
- **Design system**: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/ndw-design/`

### Backend Services (8 total):
1. **traffic-sign-backend** (`/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-backend/`)
   - Main traffic sign CRUD, search, management
   - 30+ endpoints for traffic signs

2. **traffic-sign-area-backend** (`/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-area-backend/`)
   - Environmental zones, emission zones
   - 30+ endpoints for area management

3. **traffic-sign-wkd-backend** (`/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-wkd-backend/`)
   - Vehicle access restrictions (WKD = Wegkenmerken Data)
   - 40+ endpoints for restrictions

4. **traffic-sign-inspection-backend** (`/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-inspection-backend/`)
   - Automated inspections
   - 12 endpoints for inspection management

5. **traffic-sign-wkd-derivation-backend** (`/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-wkd-derivation-backend/`)
   - Speed mutation proposals
   - 3 endpoints for derivation

6. **traffic-sign-feedback-backend** (`/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-feedback-backend/`)
   - User correction submissions
   - 3 endpoints for feedback

7. **traffic-sign-hgv-charge-backend** (`/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-hgv-charge-backend/`)
   - TSV export for heavy goods vehicles
   - 1 endpoint

8. **traffic-sign-profile-backend** (`/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-profile-backend/`)
   - Organization/user management
   - 8 endpoints for profiles

### Translation System:
**GRG has NO translation system** - UI text is hardcoded in Dutch (no ngx-translate, no translation files)

## TASKS

### Task 1: Create `projects/GRG-Wegkenmerken-verkeersborden/project-instructions.md`

Read the existing file:
`.clinerules/projects/GRG-Wegkenmerken-verkeersborden/README.md`

Then create a new file:
`.clinerules/projects/GRG-Wegkenmerken-verkeersborden/project-instructions.md`

**Content should include**:

1. **PROJECT IDENTIFICATION** section:
   - Working directory path contains: `/GRG-Wegkenmerken-verkeersborden`

2. **REPOSITORY LOCATIONS** section:
   - Frontend path and git operations directory
   - All 8 backend service paths (listed above)
   - Design system path
   - Note that project root is NOT a git repository

3. **BACKEND API MAPPING** section:
   - By Feature Domain (e.g., "Traffic Signs (CRUD)" → traffic-sign-backend.md)
   - By Endpoint Pattern (e.g., `/api/traffic-signs/**` → traffic-sign-backend.md)
   - List ALL 8 backend services with their feature areas
   - Note: Can be incomplete, user can expand later with backend crawl

4. **MONOREPO STRUCTURE** section:
   - Show directory tree
   - Mark what's editable vs reference only

5. **WHEN TO READ OTHER FILES** section:
   - Frontend coding rules → frontend/traffic-sign-frontend.md
   - Backend API details → appropriate backend file
   - Update backend docs → global/update-backend-api-instructions.md

**Use this as a reference** (modify for GRG):
`.clinerules/projects/NTM-Publicatie-overzicht/project-instructions.md`

### Task 2: Update `projects/GRG-Wegkenmerken-verkeersborden/frontend/GRG-Wegkenmerken-verkeersborden.md`

Rename it to:
`projects/GRG-Wegkenmerken-verkeersborden/frontend/traffic-sign-frontend.md`

**Content should include**:

1. **FILE TYPE RESTRICTIONS** section:
   - Only .ts, .html, .scss files
   - Glob pattern: `**/*.{ts,html,scss}`

2. **DESIGN SYSTEM** section:
   - GRG design system location
   - Component usage rules

3. **LANGUAGE RULES** section:
   - Code & comments: English
   - Commit messages: English
   - UI text: Dutch (hardcoded, NO translation system)
   - **CRITICAL**: GRG has NO ngx-translate, NO translation files
   - UI text is hardcoded Dutch strings directly in templates

4. **WORKING DIRECTORY** section:
   - Primary codebase: `/traffic-sign-frontend/src/`
   - What's editable vs not editable
   - Config files read-only unless explicitly asked

**Use this as a reference** (modify for GRG - remove translation system parts):
`.clinerules/projects/NTM-Publicatie-overzicht/frontend/ntm-frontend.md`

### Task 3: Delete Old README

After creating the files above, delete:
`.clinerules/projects/GRG-Wegkenmerken-verkeersborden/README.md`

## VERIFICATION

After completing the tasks:

1. Verify the structure matches:
```
.clinerules/projects/GRG-Wegkenmerken-verkeersborden/
├── project-instructions.md    (NEW)
├── frontend/
│   └── traffic-sign-frontend.md    (RENAMED & UPDATED)
└── backend/
    └── (8 backend files, unchanged)
```

2. Read the new `project-instructions.md` to confirm it has:
   - All 8 backend paths
   - Backend API mapping (feature domain + endpoint patterns)
   - Git execution directory
   - Correct frontend repo name

3. Read the new `traffic-sign-frontend.md` to confirm:
   - File type restrictions are clear
   - Language rules specify NO translation system
   - UI text is hardcoded Dutch

## IMPORTANT NOTES

- **Don't modify backend markdown files** - only create/update project-instructions.md and frontend file
- **The global/ folder is already done** - don't touch it
- **Backend API mapping can be incomplete** - list major features, user can expand later
- **GRG has NO translation system** - this is different from NTM, be explicit about it

---

Execute the tasks above. After completion, show me the final structure and let me verify the content.
