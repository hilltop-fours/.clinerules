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

- **Data Publications** → Read `backend/ntm-backend.md`
  - CRUD operations on data publications
  - Search, filtering, pagination
  - Approve, reject, hold, transfer ownership
  - Favorites on data publications
  - Import via DCAT dataset (`/data-publications/import`)

- **Datasets & Imports** → Read `backend/ntm-tracker-backend.md`
  - Dataset listing, retrieval, rejection
  - Dataset import (`/datasets/import`)

- **Datasets (listing only)** → Read `backend/ntm-backend.md`
  - List all datasets (`/datasets`)

- **Organizations** → Read `backend/ntm-backend.md`
  - CRUD operations on organizations
  - Role requests (create, approve, deny)
  - User listing per organization
  - Logo management
  - Permissions / moderation settings
  - Organization repair utilities (`/repair/organizations`)

- **Users** → Read `backend/ntm-backend.md`
  - CRUD operations on users
  - Current user info (`/users/current`)
  - Password reset

- **Notifications & Subscriptions** → Read `backend/ntm-backend.md`
  - Notification listing and count (`/notifications`)
  - Email subscription management (`/subscriptions/mine`)

- **Favorites** → Read `backend/ntm-backend.md`
  - List, add, delete favorites (`/favorites`)

- **Regions** → Read `backend/ntm-backend.md`
  - Region lookup by code
  - Municipality listing (`/regions/municipalities`)

- **Standards** → Read `backend/ntm-backend.md`
  - CRUD operations on standards

- **Data Regulations** → Read `backend/ntm-backend.md`
  - List all regulations and get by ID

- **Statistics** → Read `backend/ntm-backend.md`
  - User, organization, and data publication counts

- **Export** → Read `backend/ntm-backend.md`
  - Export publications and users (`/export`)

- **Info Messages** → Read `backend/ntm-backend.md`
  - CRUD operations on info messages
  - Current active messages and count

- **Blobs / Images** → Read `backend/ntm-backend.md`
  - Upload and retrieve images (`/blobs/image`)

- **Contact Requests** → Read `backend/ntm-backend.md`
  - Submit contact request (`/contact-request`)

- **Translations** → Read `backend/ntm-backend.md`
  - Translation usage info (`/translations/usage`)

- **DCAT** → Read `backend/ntm-backend.md`
  - DCAT-AP dataset endpoints (`/v1`)

- **External Organizations** → Read `backend/ntm-tracker-backend.md`
  - List and get external organizations

- **External Categories** → Read `backend/ntm-tracker-backend.md`
  - List and get external categories

### By Endpoint Pattern

When working with specific endpoints:

- `/data-publications/**` → `backend/ntm-backend.md`
- `/data-publications/import` → `backend/ntm-backend.md`
- `/datasets` → `backend/ntm-backend.md` (listing only)
- `/datasets/**` → `backend/ntm-tracker-backend.md` (import, reject, detail)
- `/organizations/**` → `backend/ntm-backend.md`
- `/repair/organizations/**` → `backend/ntm-backend.md`
- `/users/**` → `backend/ntm-backend.md`
- `/notifications/**` → `backend/ntm-backend.md`
- `/subscriptions/**` → `backend/ntm-backend.md`
- `/favorites/**` → `backend/ntm-backend.md`
- `/regions/**` → `backend/ntm-backend.md`
- `/standards/**` → `backend/ntm-backend.md`
- `/data-regulations/**` → `backend/ntm-backend.md`
- `/statistics/**` → `backend/ntm-backend.md`
- `/export/**` → `backend/ntm-backend.md`
- `/info-messages/**` → `backend/ntm-backend.md`
- `/blobs/**` → `backend/ntm-backend.md`
- `/contact-request` → `backend/ntm-backend.md`
- `/translations/**` → `backend/ntm-backend.md`
- `/v1/**` → `backend/ntm-backend.md` (DCAT-AP)
- `/external-organizations/**` → `backend/ntm-tracker-backend.md`
- `/external-categories/**` → `backend/ntm-tracker-backend.md`

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
