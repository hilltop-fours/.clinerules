# INSTRUCTIONS FOR CLAUDE CODE

## META: How to Write These Files
These configuration files are written FOR CLAUDE CODE TO READ, not for humans.
- Use explicit, unambiguous language that leaves no room for interpretation
- State requirements as direct commands/rules, not suggestions
- Be specific about file paths, working directories, and command execution locations
- Write assuming the reader (Claude) might be Haiku 4.5 which needs maximum clarity
- Avoid human-friendly explanations - focus on machine-parseable rules

---

## BACKEND API DOCUMENTATION SYSTEM

All three projects use a centralized backend API documentation system in `.clinerules/projects/*/backend/`.

**Key Rules**:
- Backend documentation is markdown-based, NOT raw Swagger JSON
- Format specification: Read `backend-api-format.md` for standards
- All backend APIs are REFERENCE ONLY - never edit backend source code
- Backend documentation is kept in sync with Swagger verification dates and git commits

**Backend Files by Project**:

| Project | Backends Documented | Location |
|---------|-------------------|----------|
| GRG Wegkenmerken-Verkeersborden | 8 | `.clinerules/projects/GRG-Wegkenmerken-verkeersborden/backend/` |
| NTM Publicatie-Overzicht | 2 | `.clinerules/projects/NTM-Publicatie-overzicht/backend/` |
| BER Bereikbaarheidskaart | 4 (shared from GRG) | `.clinerules/projects/BER-Bereikbaarheidskaart/backend/` |

**GRG Backend Services** (8 total):
1. `traffic-sign-backend.md` - Main traffic sign CRUD (30+ endpoints)
2. `traffic-sign-area-backend.md` - Environmental zones, emission zones (30+ endpoints)
3. `traffic-sign-inspection-backend.md` - Automated inspections (12 endpoints)
4. `traffic-sign-wkd-derivation-backend.md` - Speed mutation proposals (3 endpoints)
5. `traffic-sign-wkd-backend.md` - Vehicle access restrictions (40+ endpoints)
6. `traffic-sign-feedback-backend.md` - Correction submissions (3 endpoints)
7. `traffic-sign-hgv-charge-backend.md` - TSV export (1 endpoint)
8. `traffic-sign-profile-backend.md` - Organization/user management (8 endpoints)

**NTM Backend Services** (2 total):
1. `ntm-backend.md` - Publication management (40+ endpoints)
2. `ntm-tracker-backend.md` - Dataset ingestion tracking (4 endpoints)

**BER Backend Services** (4 shared from GRG):
1. `traffic-sign-backend.md` (shared copy)
2. `traffic-sign-wkd-backend.md` (shared copy)
3. `traffic-sign-area-backend.md` (shared copy)
4. `traffic-sign-feedback-backend.md` (shared copy)

---

## PROJECT IDENTIFICATION - EXECUTE FIRST

Read your current working directory path. Match it against these patterns:

**IF path contains `/GRG-Wegkenmerken-verkeersborden`**:
  - Project: GRG Wegkenmerken Verkeersborden
  - PROJECT GUIDE: `.clinerules/projects/GRG-Wegkenmerken-verkeersborden/README.md`
  - FRONTEND RULES: `.clinerules/projects/GRG-Wegkenmerken-verkeersborden/frontend/GRG-Wegkenmerken-verkeersborden.md`
  - BACKEND REFERENCE: `.clinerules/projects/GRG-Wegkenmerken-verkeersborden/backend/`
  - GIT COMMAND EXECUTION DIRECTORY: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-frontend`
  - ALL git commands (checkout, add, commit, push, status, branch, etc.) MUST execute from `traffic-sign-frontend`
  - NEVER run git commands from project root (NOT a git repository)
  - CRITICAL RULE: Only edit `/traffic-sign-frontend/` - backend is reference only

**IF path contains `/NTM-Publicatie-overzicht`**:
  - Project: NTM Publicatie Overzicht
  - PROJECT GUIDE: `.clinerules/projects/NTM-Publicatie-overzicht/README.md`
  - FRONTEND RULES: `.clinerules/projects/NTM-Publicatie-overzicht/frontend/NTM-Publicatie-overzicht.md`
  - BACKEND REFERENCE: `.clinerules/projects/NTM-Publicatie-overzicht/backend/`
  - GIT COMMAND EXECUTION DIRECTORY: `/Users/daniel/Developer/NTM-Publicatie-overzicht/ntm-frontend`
  - ALL git commands (checkout, add, commit, push, status, branch, etc.) MUST execute from `ntm-frontend`
  - NEVER run git commands from project root (NOT a git repository)
  - CRITICAL RULE: Only edit `/ntm-frontend/` - backend is reference only

**IF path contains `/BER-Bereikbaarheidskaart`**:
  - Project: BER Bereikbaarheidskaart
  - PROJECT GUIDE: `.clinerules/projects/BER-Bereikbaarheidskaart/README.md`
  - FRONTEND RULES: `.clinerules/projects/BER-Bereikbaarheidskaart/frontend/BER-Bereikbaarheidskaart.md`
  - BACKEND REFERENCE: `.clinerules/projects/BER-Bereikbaarheidskaart/backend/`
  - GIT COMMAND EXECUTION DIRECTORY: `/Users/daniel/Developer/BER-Bereikbaarheidskaart/accessibility-map-frontend`
  - ALL git commands (checkout, add, commit, push, status, branch, etc.) MUST execute from `accessibility-map-frontend`
  - NEVER run git commands from project root (NOT a git repository)
  - CRITICAL RULE: Only edit `/accessibility-map-frontend/` - backend is reference only

---

## EXECUTION ORDER

1. **Identify project** using rules above
2. **Read project guide** (README.md in `.clinerules/projects/{project}/`) - contains monorepo structure and what is editable
3. **Read frontend rules** (in `.clinerules/projects/{project}/frontend/`) if working on frontend code
4. **Read backend documentation** (in `.clinerules/projects/{project}/backend/`) if implementing features that use APIs
5. **Read shared rule files**:
   - `angular.md` (Angular coding standards)
   - `git-commits.md` (commit message format)
   - `backend-api-format.md` (how backend API documentation is structured)
6. Apply all rules from steps 2-5 to every action taken in this session

---

## CRITICAL RULES

- **Backend is reference only** - Never edit backend source code. All edits happen in frontend only.
- Project guides (README.md) contain ALL context needed for the project
- Git operations MUST be executed from the frontend subdirectory specified above, NOT from project root
- Project root directories are NOT git repositories and git commands will fail there
- When in doubt about any rule, refer back to the project's README.md file
- Backend documentation is markdown-based and follows the format in `backend-api-format.md`
- Swagger verification dates in backend docs indicate when the API was last verified against source

---

## FILE STRUCTURE

```
.clinerules/
├── claude.md                          ← You are here
├── backend-api-format.md              ← Format specification for all backend docs
├── angular.md                         ← Angular coding standards
├── git-commits.md                     ← Commit message format rules
└── projects/
    ├── GRG-Wegkenmerken-verkeersborden/
    │   ├── README.md                  ← Read this for project overview
    │   ├── frontend/
    │   │   └── GRG-Wegkenmerken-verkeersborden.md
    │   └── backend/                   ← 8 backend API docs
    │       ├── traffic-sign-backend.md
    │       ├── traffic-sign-area-backend.md
    │       ├── traffic-sign-inspection-backend.md
    │       ├── traffic-sign-wkd-derivation-backend.md
    │       ├── traffic-sign-wkd-backend.md
    │       ├── traffic-sign-feedback-backend.md
    │       ├── traffic-sign-hgv-charge-backend.md
    │       └── traffic-sign-profile-backend.md
    ├── NTM-Publicatie-overzicht/
    │   ├── README.md                  ← Read this for project overview
    │   ├── frontend/
    │   │   └── NTM-Publicatie-overzicht.md
    │   └── backend/                   ← 2 backend API docs
    │       ├── ntm-backend.md
    │       └── ntm-tracker-backend.md
    └── BER-Bereikbaarheidskaart/
        ├── README.md                  ← Read this for project overview
        ├── frontend/
        │   └── BER-Bereikbaarheidskaart.md
        └── backend/                   ← 4 backend API docs (shared from GRG)
            ├── traffic-sign-backend.md
            ├── traffic-sign-wkd-backend.md
            ├── traffic-sign-area-backend.md
            └── traffic-sign-feedback-backend.md
```
