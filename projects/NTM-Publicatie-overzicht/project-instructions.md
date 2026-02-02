# NTM PUBLICATIE OVERZICHT - PROJECT INSTRUCTIONS

## PROJECT IDENTIFICATION

Working directory path contains: `/NTM-Publicatie-overzicht`

## REPOSITORY LOCATIONS

### Frontend
- **Path**: `/Users/daniel/Developer/NTM-Publicatie-overzicht/ntm-frontend/`
- **Git operations**: ALL git commands (checkout, add, commit, push, status, branch, etc.) MUST execute from this directory
- **Editable**: YES - Only edit files in `/ntm-frontend/src/**`

### Backend Services (Reference Only - Never Edit)
- **ntm-backend**: `/Users/daniel/Developer/NTM-Publicatie-overzicht/ntm-backend/`
- **ntm-tracker-backend**: `/Users/daniel/Developer/NTM-Publicatie-overzicht/ntm-tracker-backend/`

### Design System (Reference Only - Never Edit)
- **Path**: `/Users/daniel/Developer/NTM-Publicatie-overzicht/ndw-design/`

**CRITICAL**: Project root `/Users/daniel/Developer/NTM-Publicatie-overzicht` is NOT a git repository. Git commands will fail if executed there.

---

## BACKEND API MAPPING

### By Feature Domain

When implementing features related to:

- **Publications (CRUD)** → Read `backend/ntm-backend.md`
  - Create, read, update, delete publications
  - Publication metadata and details

- **Data Publications** → Read `backend/ntm-backend.md`
  - Data publication management
  - Data publication search and filtering

- **Organizations** → Read `backend/ntm-backend.md`
  - Organization management, users, roles, permissions

- **Notifications** → Read `backend/ntm-backend.md`
  - Subscriptions, alerts, preferences

- **Datasets & Imports** → Read `backend/ntm-tracker-backend.md`
  - Dataset import, tracking, ingestion, processing
  - Import status, history, errors

### By Endpoint Pattern

When working with specific endpoints:

- `/api/publications/**` → `backend/ntm-backend.md`
- `/api/data-publications/**` → `backend/ntm-backend.md`
- `/api/organizations/**` → `backend/ntm-backend.md`
- `/api/users/**` → `backend/ntm-backend.md`
- `/api/notifications/**` → `backend/ntm-backend.md`
- `/api/datasets/**` → `backend/ntm-tracker-backend.md`
- `/api/tracker/**` → `backend/ntm-tracker-backend.md`
- `/api/imports/**` → `backend/ntm-tracker-backend.md`

**Note**: This mapping is incomplete. If you need comprehensive endpoint mapping, use the backend crawl prompt provided separately.

---

## MONOREPO STRUCTURE

```
NTM-Publicatie-overzicht/
├── ntm-frontend/                   ← EDIT ONLY THIS (in /src/ subdirectory)
│   └── src/                        ← All edits happen here
├── ntm-backend/                    ← Reference only
├── ntm-tracker-backend/            ← Reference only
├── ndw-design/                     ← Reference only
└── .clinerules/                    ← Editable (documentation only)
    └── projects/NTM-Publicatie-overzicht/
        ├── project-instructions.md ← You are here
        ├── frontend/
        │   └── ntm-frontend.md
        └── backend/
            ├── ntm-backend.md
            └── ntm-tracker-backend.md
```

---

## WHEN TO READ OTHER FILES

**For frontend coding rules**: Read `frontend/ntm-frontend.md`

**For backend API details**: Read the appropriate backend markdown file based on the mapping above

**For updating backend API docs**: Read `.clinerules/global/update-backend-api-instructions.md`
