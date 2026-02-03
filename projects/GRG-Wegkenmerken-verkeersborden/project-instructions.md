# GRG WEGKENMERKEN VERKEERSBORDEN - PROJECT INSTRUCTIONS

## PROJECT IDENTIFICATION

Working directory path contains: `/GRG-Wegkenmerken-verkeersborden`

## REPOSITORY LOCATIONS

### Frontend
- **Path**: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-frontend/`
- **Git operations**: ALL git commands (checkout, add, commit, push, status, branch, etc.) MUST execute from this directory
- **Editable**: YES - Only edit files in `/traffic-sign-frontend/src/**`

### Backend Services (Reference Only - Never Edit)
- **traffic-sign-backend**: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-backend/`
- **traffic-sign-area-backend**: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-area-backend/`
- **traffic-sign-wkd-backend**: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-wkd-backend/`
- **traffic-sign-inspection-backend**: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-inspection-backend/`
- **traffic-sign-wkd-derivation-backend**: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-wkd-derivation-backend/`
- **traffic-sign-feedback-backend**: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-feedback-backend/`
- **traffic-sign-hgv-charge-backend**: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-hgv-charge-backend/`
- **traffic-sign-profile-backend**: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/traffic-sign-profile-backend/`

### Design System (Reference Only - Never Edit)
- **Path**: `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden/ndw-design/`

**CRITICAL**: Project root `/Users/daniel/Developer/GRG-Wegkenmerken-verkeersborden` is NOT a git repository. Git commands will fail if executed there.

---

## BACKEND API MAPPING

### By Feature Domain

When implementing features related to:

- **Traffic Signs (CRUD, search, management)** → Read `backend/traffic-sign-backend.md`
  - Create, read, update, delete traffic signs
  - Traffic sign search and filtering
  - Mutation proposals and status management

- **Environmental Zones / Emission Zones** → Read `backend/traffic-sign-area-backend.md`
  - Zone creation, editing, deletion
  - Area-based traffic restrictions

- **Vehicle Access Restrictions (WKD)** → Read `backend/traffic-sign-wkd-backend.md`
  - Wegkenmerken data restrictions
  - Access control rules per vehicle type

- **Speed Mutation Proposals (WKD Derivation)** → Read `backend/traffic-sign-wkd-derivation-backend.md`
  - Derived speed limit proposals

- **Automated Inspections** → Read `backend/traffic-sign-inspection-backend.md`
  - Inspection management and results

- **Feedback / Correction Submissions** → Read `backend/traffic-sign-feedback-backend.md`
  - User-submitted corrections for traffic signs

- **HGV Charges (TSV Export)** → Read `backend/traffic-sign-hgv-charge-backend.md`
  - Heavy goods vehicle charge export

- **Organizations / User Management** → Read `backend/traffic-sign-profile-backend.md`
  - Organization and user profiles
  - Roles and permissions

### By Endpoint Pattern

When working with specific endpoints:

- `/api/traffic-signs/**` → `backend/traffic-sign-backend.md`
- `/api/mutations/**` → `backend/traffic-sign-backend.md`
- `/api/areas/**` → `backend/traffic-sign-area-backend.md`
- `/api/zones/**` → `backend/traffic-sign-area-backend.md`
- `/api/wkd/**` → `backend/traffic-sign-wkd-backend.md`
- `/api/derivations/**` → `backend/traffic-sign-wkd-derivation-backend.md`
- `/api/inspections/**` → `backend/traffic-sign-inspection-backend.md`
- `/api/feedback/**` → `backend/traffic-sign-feedback-backend.md`
- `/api/hgv/**` → `backend/traffic-sign-hgv-charge-backend.md`
- `/api/profiles/**` → `backend/traffic-sign-profile-backend.md`
- `/api/organizations/**` → `backend/traffic-sign-profile-backend.md`

**Note**: This mapping is incomplete. If you need comprehensive endpoint mapping, use the backend crawl prompt provided separately.

---

## MONOREPO STRUCTURE

```
GRG-Wegkenmerken-verkeersborden/
├── traffic-sign-frontend/             ← EDIT ONLY THIS (in /src/ subdirectory)
│   └── src/                           ← All edits happen here
├── traffic-sign-backend/              ← Reference only
├── traffic-sign-area-backend/         ← Reference only
├── traffic-sign-wkd-backend/          ← Reference only
├── traffic-sign-inspection-backend/   ← Reference only
├── traffic-sign-wkd-derivation-backend/ ← Reference only
├── traffic-sign-feedback-backend/     ← Reference only
├── traffic-sign-hgv-charge-backend/   ← Reference only
├── traffic-sign-profile-backend/      ← Reference only
├── ndw-design/                        ← Reference only
└── .clinerules/                       ← Editable (documentation only)
    └── projects/GRG-Wegkenmerken-verkeersborden/
        ├── project-instructions.md    ← You are here
        ├── frontend/
        │   └── traffic-sign-frontend.md
        └── backend/
            ├── traffic-sign-backend.md
            ├── traffic-sign-area-backend.md
            ├── traffic-sign-wkd-backend.md
            ├── traffic-sign-inspection-backend.md
            ├── traffic-sign-wkd-derivation-backend.md
            ├── traffic-sign-feedback-backend.md
            ├── traffic-sign-hgv-charge-backend.md
            └── traffic-sign-profile-backend.md
```

---

## WHEN TO READ OTHER FILES

**For frontend coding rules**: Read `frontend/traffic-sign-frontend.md`

**For backend API details**: Read the appropriate backend markdown file based on the mapping above

**For updating backend API docs**: Read `.clinerules/global/update-backend-api-instructions.md`
