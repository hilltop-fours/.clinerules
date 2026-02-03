# BER BEREIKBAARHEIDSKAART - PROJECT INSTRUCTIONS

## PROJECT IDENTIFICATION

Working directory path contains: `/BER-Bereikbaarheidskaart`

## REPOSITORY LOCATIONS

### Frontend
- **Path**: `/Users/daniel/Developer/BER-Bereikbaarheidskaart/accessibility-map-frontend/`
- **Git operations**: ALL git commands (checkout, add, commit, push, status, branch, etc.) MUST execute from this directory
- **Editable**: YES - Only edit files in `/accessibility-map-frontend/src/**`

### Backend Services (Reference Only - Never Edit)
All 4 backends are shared copies from the GRG project, used here for accessibility mapping.

- **traffic-sign-backend**: `/Users/daniel/Developer/BER-Bereikbaarheidskaart/traffic-sign-backend/`
- **traffic-sign-wkd-backend**: `/Users/daniel/Developer/BER-Bereikbaarheidskaart/traffic-sign-wkd-backend/`
- **traffic-sign-area-backend**: `/Users/daniel/Developer/BER-Bereikbaarheidskaart/traffic-sign-area-backend/`
- **traffic-sign-feedback-backend**: `/Users/daniel/Developer/BER-Bereikbaarheidskaart/traffic-sign-feedback-backend/`

### Design System (Reference Only - Never Edit)
- **Path**: `/Users/daniel/Developer/BER-Bereikbaarheidskaart/ndw-design/`
- Shared GRG design system

**CRITICAL**: Project root `/Users/daniel/Developer/BER-Bereikbaarheidskaart` is NOT a git repository. Git commands will fail if executed there.

---

## BACKEND API MAPPING

### By Feature Domain

When implementing features related to:

- **Traffic Signs (for maps)** → Read `backend/traffic-sign-backend.md`
  - Traffic sign CRUD, validation, removal
  - Traffic sign images and history
  - Map updates (GeoJSON)

- **Organizations & Users** → Read `backend/traffic-sign-backend.md`
  - Organization management
  - User management and roles (Keycloak)
  - Road authority lookups

- **Findings / Issues** → Read `backend/traffic-sign-backend.md`
  - Traffic sign findings and issue tracking
  - Black code uploads and validation

- **Vehicle Restrictions (WKD)** → Read `backend/traffic-sign-wkd-backend.md`
  - Speed limits, height/length/load/axle-load restrictions
  - Traffic type and driving direction mutations
  - School zones, road narrowings, road categories
  - NWB version-based mutations and exports

- **Environmental & Emission Zones** → Read `backend/traffic-sign-area-backend.md`
  - Environmental zones and emission zones
  - Parking bans and traffic regulations
  - Hazardous substance routes
  - Geographic areas, counties, towns

- **User Feedback / Corrections** → Read `backend/traffic-sign-feedback-backend.md`
  - User-submitted corrections about road features
  - Location-based feedback (GeoJSON points)

### By Endpoint Pattern

When working with specific endpoints:

- `/traffic-signs/**` → `backend/traffic-sign-backend.md`
- `/images/**` → `backend/traffic-sign-backend.md`
- `/organizations/**` → `backend/traffic-sign-backend.md`
- `/users/**` → `backend/traffic-sign-backend.md`
- `/findings/**` → `backend/traffic-sign-backend.md`
- `/black-codes/**` → `backend/traffic-sign-backend.md`
- `/info-messages/**` → `backend/traffic-sign-backend.md`
- `/road-authorities/**` → `backend/traffic-sign-backend.md`
- `/speeds/**` → `backend/traffic-sign-wkd-backend.md`
- `/traffic-types/**` → `backend/traffic-sign-wkd-backend.md`
- `/height-restrictions/**` → `backend/traffic-sign-wkd-backend.md`
- `/length-restrictions/**` → `backend/traffic-sign-wkd-backend.md`
- `/load-restrictions/**` → `backend/traffic-sign-wkd-backend.md`
- `/axle-load-restrictions/**` → `backend/traffic-sign-wkd-backend.md`
- `/schoolzones/**` → `backend/traffic-sign-wkd-backend.md`
- `/road-narrowings/**` → `backend/traffic-sign-wkd-backend.md`
- `/driving-directions/**` → `backend/traffic-sign-wkd-backend.md`
- `/export/**` → `backend/traffic-sign-wkd-backend.md`
- `/environmental-zones/**` → `backend/traffic-sign-area-backend.md`
- `/emission-zones/**` → `backend/traffic-sign-area-backend.md`
- `/parking-bans/**` → `backend/traffic-sign-area-backend.md`
- `/traffic-regulations/**` → `backend/traffic-sign-area-backend.md`
- `/routes/hazardous-substances/**` → `backend/traffic-sign-area-backend.md`
- `/areas/**` → `backend/traffic-sign-area-backend.md`
- `/counties/**` → `backend/traffic-sign-area-backend.md`
- `/corrections/**` → `backend/traffic-sign-feedback-backend.md`

**Note**: This mapping is incomplete. If you need comprehensive endpoint mapping, use the backend crawl prompt provided separately.

---

## MONOREPO STRUCTURE

```
BER-Bereikbaarheidskaart/
├── accessibility-map-frontend/       ← EDIT ONLY THIS (in /src/ subdirectory)
│   └── src/                          ← All edits happen here
├── traffic-sign-backend/             ← Reference only (shared from GRG)
├── traffic-sign-wkd-backend/         ← Reference only (shared from GRG)
├── traffic-sign-area-backend/        ← Reference only (shared from GRG)
├── traffic-sign-feedback-backend/    ← Reference only (shared from GRG)
├── ndw-design/                       ← Reference only (shared GRG design system)
└── .clinerules/                      ← Editable (documentation only)
    └── projects/BER-Bereikbaarheidskaart/
        ├── project-instructions.md   ← You are here
        ├── frontend/
        │   └── accessibility-map-frontend.md
        └── backend/
            ├── traffic-sign-backend.md
            ├── traffic-sign-wkd-backend.md
            ├── traffic-sign-area-backend.md
            └── traffic-sign-feedback-backend.md
```

---

## WHEN TO READ OTHER FILES

**For frontend coding rules**: Read `frontend/accessibility-map-frontend.md`

**For backend API details**: Read the appropriate backend markdown file based on the mapping above

**For updating backend API docs**: Read `.clinerules/global/update-backend-api-instructions.md`
