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

*Last crawled: 2026-02-03*

### By Feature Domain

When implementing features related to:

- **Traffic Signs (CRUD, search, management)** → Read `backend/traffic-sign-backend.md`
  - Create, read, update, delete traffic signs (`traffic-signs/**`)
  - Traffic sign search, filtering, map updates
  - Traffic sign history (`traffic-signs/{id}/history`)
  - Traffic sign validation, road-section assignment, removal
  - RVV code upload (`traffic-signs/rvv-codes`)
  - Traffic sign import (`traffic-signs/import`)
  - Main signs CRUD (`rest/static-road-data/traffic-signs/v5/main-signs`)

- **Mutation Proposals** → Read `backend/traffic-sign-backend.md`
  - Mutation listing and road-section authority jobs (`mutations/**`)

- **Findings / Inspection Results** → Read `backend/traffic-sign-backend.md`
  - Traffic sign findings CRUD, map view, per-sign findings (`findings/**`)

- **Organizations & Users** → Read `backend/traffic-sign-backend.md`
  - Organization CRUD, search, authority checks, logo upload (`organizations/**`)
  - User CRUD, current user, password reset (`users/**`)
  - Road authority listing and search (`road-authorities/**`)

- **Images & Blobs** → Read `backend/traffic-sign-backend.md`
  - Traffic sign images upload and retrieval (`images/**`)
  - Generic blob/PDF retrieval (`blobs/**`)

- **Black Codes** → Read `backend/traffic-sign-backend.md`
  - Black code file upload and upload history (`black-codes/**`)

- **Connected Roads** → Read `backend/traffic-sign-backend.md`
  - Road connectivity lookup (`connected-roads/**`)

- **Opening Hours** → Read `backend/traffic-sign-backend.md`
  - Opening hours validation (`opening-hours/check-valid`)

- **Info Messages** → Read `backend/traffic-sign-backend.md`
  - System info messages CRUD (`info-messages/**`)

- **Usage Statistics** → Read `backend/traffic-sign-backend.md`
  - Usage stats JSON and CSV export (`usage-statistics`)

- **Current State / Static Road Data** → Read `backend/traffic-sign-backend.md`
  - Public read-only current state exports v4/v5 (`rest/static-road-data/traffic-signs/v4/current-state`, `v5/current-state`, `v5/main-signs`)
  - Event history v4 (`rest/static-road-data/traffic-signs/v4/events`)

- **OGC Features** → Read `backend/traffic-sign-backend.md`
  - OGC collections endpoint (`rest/static-road-data/traffic-signs/v4/collections`, `v5/collections`)

- **Event Processors (Admin)** → Read `backend/traffic-sign-backend.md`
  - Start/stop/replay event processors, state (`event-processors/**`)

- **Current State Admin** → Read `backend/traffic-sign-backend.md`
  - Trigger current-state rebuild (`traffic-signs/current-state/v4`)

- **GraphQL** → Read `backend/traffic-sign-backend.md`
  - GraphQL query endpoint (`/graphql`)

- **Environmental Zones / Emission Zones** → Read `backend/traffic-sign-area-backend.md`
  - Zone CRUD, GeoJSON map, shapefile upload (`environmental-zones/**` / `emission-zones/**`)

- **Areas** → Read `backend/traffic-sign-area-backend.md`
  - Generic area CRUD (`areas/**`)

- **Parking Bans** → Read `backend/traffic-sign-area-backend.md`
  - Parking ban CRUD, GeoJSON map (`parking-bans/**`)

- **Traffic Regulations** → Read `backend/traffic-sign-area-backend.md`
  - Traffic regulation CRUD (`traffic-regulations/**`)

- **Hazardous Substance Routes** → Read `backend/traffic-sign-area-backend.md`
  - Hazardous substance route CRUD, map (`routes/hazardous-substances/**`)

- **Counties & Towns** → Read `backend/traffic-sign-area-backend.md`
  - County listing (`counties/**`)
  - Town listing (`towns`)

- **IBBM Emission Zones** → Read `backend/traffic-sign-area-backend.md`
  - IBBM emission zones v1 read (`ibbm/emission-zones/v1`)

- **Coordinate Conversion** → Read `backend/traffic-sign-area-backend.md`
  - WGS84 coordinate conversion (`conversion/wgs84`)

- **WKD Restrictions (per road section)** → Read `backend/traffic-sign-wkd-backend.md`
  - Axle load restrictions (`axle-load-restrictions/**`)
  - Height restrictions (`height-restrictions/**`)
  - Length restrictions (`length-restrictions/**`)
  - Load restrictions (`load-restrictions/**`)
  - Speed limits (`speeds/**`)
  - Driving directions (`driving-directions/**`)
  - Carriageway types (`carriageway-types/**`)
  - Road categories (`road-categories/**`)
  - Road narrowings (`road-narrowings/**`)
  - RVM types (`rvm-types/**`)
  - Traffic types (`traffic-types/**`)
  - School zones (`schoolzones/**`)
  - Road authorities in WKD context (`road-authorities/**`)

- **WKD Joint Mutations** → Read `backend/traffic-sign-wkd-backend.md`
  - Joint mutation listing and creation per road section (`joint-mutations/**`)

- **WKD Export** → Read `backend/traffic-sign-wkd-backend.md`
  - Export start, file listing, reprocess, migrate (`export/**`)

- **WKD File Uploads** → Read `backend/traffic-sign-wkd-backend.md`
  - File upload listing and retrieval (`file-uploads/**`)

- **WKD Blob Files** → Read `backend/traffic-sign-wkd-backend.md`
  - Blob file retrieval (`blob-files/**`)

- **WKD SFTP** → Read `backend/traffic-sign-wkd-backend.md`
  - SFTP list, test, write (`sftp/**`)

- **WKD NLS Proxies** → Read `backend/traffic-sign-wkd-backend.md`
  - NLS NWB road section data (`nls/nwb/**`)
  - NLS routing nearest-point (`nls/routing/**`)
  - NLS WKD restriction exports (`nls/wkd/**`)

- **WKD Usage Statistics** → Read `backend/traffic-sign-wkd-backend.md`
  - WKD usage stats JSON and CSV (`usage-statistics`)

- **Speed Mutation Proposals (WKD Derivation)** → Read `backend/traffic-sign-wkd-derivation-backend.md`
  - Speed mutation proposals per road section, CRUD (`speeds/road-section/**`)

- **Automated Inspections** → Read `backend/traffic-sign-inspection-backend.md`
  - Trigger inspection jobs: invalid black codes, road sections, unknown traffic orders, WKD checks (axle-load, height, length, load, width, speed), missing direction, NLS issues (`jobs/inspections/**`)

- **Inspection Feedback** → Read `backend/traffic-sign-inspection-backend.md`
  - Submit inspection feedback (`feedback`)

- **Feedback / Corrections** → Read `backend/traffic-sign-feedback-backend.md`
  - Correction listing, creation, GeoJSON map (`corrections/**`)

- **HGV Charges (TSV Export)** → Read `backend/traffic-sign-hgv-charge-backend.md`
  - HGV mutation TSV export by version (`mutations/{version}`)

- **Profile Backend** → Read `backend/traffic-sign-profile-backend.md`
  - No controllers found in source — may be a config-only or proxy service

### By Endpoint Pattern

When working with specific endpoints:

**traffic-sign-backend:**
- `/traffic-signs/**` → `backend/traffic-sign-backend.md`
- `/traffic-signs/import` → `backend/traffic-sign-backend.md`
- `/traffic-signs/{id}/history` → `backend/traffic-sign-backend.md`
- `/traffic-signs/current-state/**` → `backend/traffic-sign-backend.md`
- `/mutations/**` → `backend/traffic-sign-backend.md`
- `/findings/**` → `backend/traffic-sign-backend.md`
- `/organizations/**` → `backend/traffic-sign-backend.md`
- `/users/**` → `backend/traffic-sign-backend.md`
- `/road-authorities/**` → `backend/traffic-sign-backend.md`
- `/images/**` → `backend/traffic-sign-backend.md`
- `/blobs/**` → `backend/traffic-sign-backend.md`
- `/black-codes/**` → `backend/traffic-sign-backend.md`
- `/connected-roads/**` → `backend/traffic-sign-backend.md`
- `/opening-hours/**` → `backend/traffic-sign-backend.md`
- `/info-messages/**` → `backend/traffic-sign-backend.md`
- `/usage-statistics` → `backend/traffic-sign-backend.md`
- `/event-processors/**` → `backend/traffic-sign-backend.md`
- `/rest/static-road-data/traffic-signs/v4/**` → `backend/traffic-sign-backend.md`
- `/rest/static-road-data/traffic-signs/v5/**` → `backend/traffic-sign-backend.md`
- `/graphql` → `backend/traffic-sign-backend.md`

**traffic-sign-area-backend:**
- `/environmental-zones/**` → `backend/traffic-sign-area-backend.md`
- `/emission-zones/**` → `backend/traffic-sign-area-backend.md`
- `/areas/**` → `backend/traffic-sign-area-backend.md`
- `/parking-bans/**` → `backend/traffic-sign-area-backend.md`
- `/traffic-regulations/**` → `backend/traffic-sign-area-backend.md`
- `/routes/hazardous-substances/**` → `backend/traffic-sign-area-backend.md`
- `/counties/**` → `backend/traffic-sign-area-backend.md`
- `/towns` → `backend/traffic-sign-area-backend.md`
- `/ibbm/**` → `backend/traffic-sign-area-backend.md`
- `/conversion/**` → `backend/traffic-sign-area-backend.md`

**traffic-sign-wkd-backend:**
- `/axle-load-restrictions/**` → `backend/traffic-sign-wkd-backend.md`
- `/height-restrictions/**` → `backend/traffic-sign-wkd-backend.md`
- `/length-restrictions/**` → `backend/traffic-sign-wkd-backend.md`
- `/load-restrictions/**` → `backend/traffic-sign-wkd-backend.md`
- `/speeds/**` → `backend/traffic-sign-wkd-backend.md`
- `/driving-directions/**` → `backend/traffic-sign-wkd-backend.md`
- `/carriageway-types/**` → `backend/traffic-sign-wkd-backend.md`
- `/road-categories/**` → `backend/traffic-sign-wkd-backend.md`
- `/road-narrowings/**` → `backend/traffic-sign-wkd-backend.md`
- `/rvm-types/**` → `backend/traffic-sign-wkd-backend.md`
- `/traffic-types/**` → `backend/traffic-sign-wkd-backend.md`
- `/schoolzones/**` → `backend/traffic-sign-wkd-backend.md`
- `/road-authorities/**` (WKD context) → `backend/traffic-sign-wkd-backend.md`
- `/joint-mutations/**` → `backend/traffic-sign-wkd-backend.md`
- `/export/**` → `backend/traffic-sign-wkd-backend.md`
- `/file-uploads/**` → `backend/traffic-sign-wkd-backend.md`
- `/blob-files/**` → `backend/traffic-sign-wkd-backend.md`
- `/sftp/**` → `backend/traffic-sign-wkd-backend.md`
- `/nls/nwb/**` → `backend/traffic-sign-wkd-backend.md`
- `/nls/routing/**` → `backend/traffic-sign-wkd-backend.md`
- `/nls/wkd/**` → `backend/traffic-sign-wkd-backend.md`
- `/usage-statistics` (WKD context) → `backend/traffic-sign-wkd-backend.md`

**traffic-sign-wkd-derivation-backend:**
- `/speeds/road-section/**` → `backend/traffic-sign-wkd-derivation-backend.md`

**traffic-sign-inspection-backend:**
- `/jobs/inspections/**` → `backend/traffic-sign-inspection-backend.md`
- `/feedback` → `backend/traffic-sign-inspection-backend.md`

**traffic-sign-feedback-backend:**
- `/corrections/**` → `backend/traffic-sign-feedback-backend.md`

**traffic-sign-hgv-charge-backend:**
- `/mutations/{version}` (TSV) → `backend/traffic-sign-hgv-charge-backend.md`

**Ambiguous paths**: `/road-authorities/**` exists in both traffic-sign-backend and traffic-sign-wkd-backend. `/speeds/**` exists in both traffic-sign-wkd-backend and traffic-sign-wkd-derivation-backend (derivation uses `/speeds/road-section/`). `/usage-statistics` exists in both traffic-sign-backend and traffic-sign-wkd-backend — context determines which service is called.

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
