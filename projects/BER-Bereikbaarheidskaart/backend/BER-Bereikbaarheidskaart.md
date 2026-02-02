# BER BEREIKBAARHEIDSKAART - BACKEND API MAPPING

This section documents which backend services handle specific API endpoints used by the accessibility-map-frontend. Use this when working on API-related features to know which backend service to reference.

## COMMIT TRACKING - Last Verified Changes

Use this section to track which backend commits have been reviewed for API changes.

| Backend Service | Last Verified Commit | Commit Message | Date |
|-----------------|---------------------|-----------------|------|
| traffic-sign-backend | [TO BE VERIFIED] | [TO BE VERIFIED] | [TO BE VERIFIED] |
| traffic-sign-wkd-backend | [TO BE VERIFIED] | [TO BE VERIFIED] | [TO BE VERIFIED] |
| traffic-sign-area-backend | [TO BE VERIFIED] | [TO BE VERIFIED] | [TO BE VERIFIED] |
| traffic-sign-feedback-backend | [TO BE VERIFIED] | [TO BE VERIFIED] | [TO BE VERIFIED] |

**How to use**: When you check a backend repo for changes, update the commit hash and message. Next time, only new commits after this one need to be reviewed.

---

## PRIMARY BACKEND: traffic-sign-backend

**Location:** `/traffic-sign-backend/`

This is the main backend service for the accessibility-map-frontend. It handles:

### Traffic Signs (`/api/traffic-signs`)
- `GET /traffic-signs` - Get all traffic signs with pagination
- `GET /traffic-signs/{id}` - Get specific traffic sign details
- `GET /traffic-signs/{id}/history` - Get traffic sign change history
- `GET /traffic-signs/map/updates` - Get map updates for traffic signs
- `POST /traffic-signs` - Create new traffic sign
- `PUT /traffic-signs/{id}` - Update traffic sign
- `PUT /traffic-signs/{id}/validate` - Update validation state
- `PUT /traffic-signs/{id}/remove` - Mark traffic sign as removed
- `PATCH /traffic-signs/{id}` - Partial updates (JSON Patch format)

### Organizations (`/api/organizations`)
- `GET /organizations` - Get all organizations
- `GET /organizations/{id}` - Get organization details
- `POST /organizations` - Create organization
- `PUT /organizations/{id}` - Update organization
- `DELETE /organizations/{id}` - Delete organization
- `DELETE /organizations/{id}/location` - Remove organization location

### Users (`/api/users`)
- `GET /users/organization/{organizationId}` - Get users by organization
- `GET /users/{id}` - Get user details
- `POST /users` - Create user
- `PUT /users/{id}` - Update user
- `DELETE /users/{id}` - Delete user
- `POST /users/{id}/reset-password` - Reset user password

### Findings/Issues (`/api/findings`)
- `GET /findings` - Get all findings with pagination/filtering
- `GET /findings?id={id}` - Get specific finding
- `GET /findings/map` - Get findings as map features
- `GET /findings/traffic-sign/{trafficSignId}` - Get findings for specific traffic sign
- `PUT /findings/{id}/status` - Update finding status

### Feedback & Corrections (`/api/feedback/corrections`)
- `GET /feedback/corrections` - Get all feedback
- `GET /feedback/corrections/map` - Get feedback as map features
- `POST /feedback/corrections` - Submit feedback

**Backend Structure (traffic-sign-backend modules):**
- `ts-projection` - Query/read operations
- `ts-command` - Write/create operations
- `ts-findings` - Findings/issues management
- `ts-common` - Common utilities and info messages

---

## SECONDARY BACKEND: traffic-sign-wkd-backend

**Location:** `/traffic-sign-wkd-backend/`

Handles Vehicle Access Restrictions (WKD - Wegverkeerswet Voertuigbeperkingen):

### HGV Charge (`/api/hgv-charge`)
- `GET /hgv-charge/mutations/{version}` - Get HGV mutations for specific version

### WKD Features (`/api/wkd`)
- `POST /wkd/{featureUrl}/{version}/road-sections/{roadSectionId}` - Create feature mutations
- `POST /wkd/joint-mutations/{version}/road-sections/{roadSectionId}` - Create joint mutations
- `GET /wkd/nls/nwb/map/{version}/roadSection/{roadSectionId}` - Get road section
- `GET /wkd/nls/nwb/map/{version}/roadSection?bagPublicSpaceLabel={label}` - Get road sections by label

**Features available:**
- Axle load restrictions
- Carriageway types
- Driving directions
- Height restrictions
- Length restrictions
- Load restrictions
- Road authorities
- Road categories
- Road narrowings
- RVM types
- Speeds
- Traffic types

---

## TERTIARY BACKEND: traffic-sign-area-backend

**Location:** `/traffic-sign-area-backend/`

Handles area-related data and regulations:

### Environmental Zones (`/api/area/environmental-zones`)
- `GET /environmental-zones` - Get all environmental zones
- `GET /environmental-zones/{id}` - Get zone details
- `GET /environmental-zones/map` - Get zones as map features
- `GET /environmental-zones?countyCode={code}` - Get zones by county
- `POST /environmental-zones` - Create zone
- `POST /environmental-zones/{id}` - Update zone
- `DELETE /environmental-zones/{id}` - Delete zone

### Other Area Features
- Parking bans
- Traffic regulations
- Hazardous substance routes
- Town/area management

---

## QUATERNARY BACKEND: traffic-sign-feedback-backend

**Location:** `/traffic-sign-feedback-backend/`

Handles user feedback and corrections:

### Corrections (`/feedback/corrections`)
- `GET /corrections` - Get all corrections
- `POST /corrections` - Create correction
- `GET /corrections/map` - Get corrections as map features

---

## EXTERNAL API DEPENDENCIES

The frontend also integrates with external third-party services (READ ONLY for reference):

### NDW/NLS Services
- **NLS Routing:** `https://nls.ndw.nu/api/gis/routing`
- **NLS Maps:** `https://maps.ndw.nu/api/v1/`
- **Traffic Signs TileServer:** `https://wegkenmerken.staging.ndw.nu/api/tileserv/`
- **HGV Charge API:** `https://nls.staging.ndw.nu/api/rest/static-road-data/road-attributes/v1/hgv-charge`
- **Location Issues:** `https://nls.staging.ndw.nu/api/rest/static-road-data/location-data-issues/v1/issues`

### Vector Tile Endpoints (Map Data)
- Speed limits, height/length/axle load restrictions
- Road categories, traffic types, road narrowings
- School zones, traffic signs, RVM data

### PDOK (Dutch Spatial Data Infrastructure)
- **Location Server:** `https://api.pdok.nl/bzk/locatieserver/search/v3_1`
- **BAG/BGT:** `https://api.pdok.nl/`
- **Administrative divisions and map tiles**

### Map Styling & Basemaps
- PDOK BRT Achtergrondkaart
- PDOK Aerial Photos
- NDW Map Styles

### Authentication
- **Keycloak:** `https://iam.ndw.nu/auth/realms/ndw`
- Client ID: `traffic-sign-frontend-dev`

---

## API COMMUNICATION PATTERNS

### Base URL Configuration
- Local API calls: `/api` (proxied to backend services)
- Environment configuration: `src/environments/environment.ts`

### HTTP Methods
- `GET` - Retrieve data
- `POST` - Create new resources
- `PUT` - Full update
- `PATCH` - Partial update (JSON Patch format)
- `DELETE` - Remove resources

### Response Formats
- JSON for data endpoints
- GeoJSON for map/geographic features
- CSV/TSV for exports

### Pagination & Filtering
- Uses AgGrid server-side pagination parameters
- Query string parameters for filtering and searching
- Request body for POST/PUT/PATCH operations

### Error Handling
- Base service with centralized error handling: `src/app/core/services/base.service.ts`
- RxJS observables for async operations

---

## WHEN TO ASK FOR BACKEND HELP

When working on features that interact with APIs, identify which backend service is involved:

1. **Traffic sign data (CRUD operations)** → Ask about `traffic-sign-backend`
2. **User/organization management** → Ask about `traffic-sign-backend` (ts-command module)
3. **Vehicle access restrictions (WKD)** → Ask about `traffic-sign-wkd-backend`
4. **Area-based data (zones, regulations)** → Ask about `traffic-sign-area-backend`
5. **User feedback/corrections** → Ask about `traffic-sign-feedback-backend`
6. **Map tile data or routing** → Ask about external NDW/NLS/PDOK services

Mention the specific backend name when asking about API behavior, data format, validation rules, or expected payloads.
