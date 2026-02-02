# GRG WEGKENMERKEN VERKEERSBORDEN - BACKEND API MAPPING

Complete reference guide for all backend services and their API endpoints. Use this when working on the traffic-sign-frontend and need to understand API contracts, data formats, DTOs, and where to find specific functionality.

---

## COMMIT TRACKING - Last Verified Changes

Use this section to track which backend commits have been reviewed for API changes.

| Backend Service | Last Verified Commit | Commit Message | Date |
|-----------------|---------------------|-----------------|------|
| traffic-sign-inspection-backend | 96cc5c8 | Merged PR 115364: Feature #108675 Add missing properties for frontend | 2026-01-28 |
| traffic-sign-backend | [TO BE VERIFIED] | [TO BE VERIFIED] | [TO BE VERIFIED] |
| traffic-sign-area-backend | [TO BE VERIFIED] | [TO BE VERIFIED] | [TO BE VERIFIED] |
| traffic-sign-feedback-backend | [TO BE VERIFIED] | [TO BE VERIFIED] | [TO BE VERIFIED] |
| traffic-sign-wkd-backend | [TO BE VERIFIED] | [TO BE VERIFIED] | [TO BE VERIFIED] |
| traffic-sign-wkd-derivation-backend | [TO BE VERIFIED] | [TO BE VERIFIED] | [TO BE VERIFIED] |
| traffic-sign-hgv-charge-backend | [TO BE VERIFIED] | [TO BE VERIFIED] | [TO BE VERIFIED] |
| traffic-sign-profile-backend | [TO BE VERIFIED] | [TO BE VERIFIED] | [TO BE VERIFIED] |

**How to use**: When you check a backend repo for changes, update the commit hash and message. Next time, only new commits after this one need to be reviewed.

---

## QUICK REFERENCE BY FEATURE

Use this table to quickly find which backend service handles your feature:

| Feature | Backend Service | Key Endpoints | Module |
|---------|-----------------|---------------|--------|
| **Traffic Signs (CRUD)** | traffic-sign-backend | `/traffic-signs`, `/black-codes`, `/opening-hours` | ts-command, ts-projection |
| **Organizations & Users** | traffic-sign-backend | `/organizations`, `/users` | ts-command |
| **Traffic Sign Findings** | traffic-sign-backend | `/findings` | ts-findings |
| **User Feedback** | traffic-sign-feedback-backend | `/corrections` | N/A |
| **Environmental Zones** | traffic-sign-area-backend | `/environmental-zones` | N/A |
| **Vehicle Restrictions (WKD)** | traffic-sign-wkd-backend | `/speeds`, `/axle-load-restrictions`, etc. | N/A |
| **HGV Charging Data** | traffic-sign-hgv-charge-backend | `/mutations` | N/A |
| **Data Validation Jobs** | traffic-sign-inspection-backend | `/jobs/inspections/*` | N/A |
| **WKD Derivation/Proposals** | traffic-sign-wkd-derivation-backend | `/speeds` (proposals) | N/A |
| **Traffic Sign Images** | traffic-sign-backend | `/images` | ts-command |
| **Traffic Sign History** | traffic-sign-backend | `/traffic-signs/{id}/history` | ts-command |

---

## 1. TRAFFIC SIGN BACKEND

**Location**: `/traffic-sign-backend/`
**Purpose**: Core traffic sign management, organizations, users, findings, images, and history
**Modules**: ts-command (write), ts-projection (read), ts-findings, ts-common, ts-graphql

### 1.1 TRAFFIC SIGNS - CRUD OPERATIONS

#### Create & Update Traffic Signs
```
POST /traffic-signs
  - Requires: ROLE_TRAFFIC_SIGN_WRITER
  - Request: TrafficSignRequestDto (JSON)
  - Response: TrafficSignRequestDto (JSON)

PUT /traffic-signs/{id}
  - Requires: ROLE_TRAFFIC_SIGN_WRITER
  - Path: {id} = UUID
  - Request: TrafficSignRequestDto (JSON)
  - Response: TrafficSignRequestDto (JSON)

PATCH /traffic-signs/{id}
  - Requires: ROLE_TRAFFIC_SIGN_WRITER
  - Path: {id} = UUID
  - Request: List<PatchOperationDto> (JSON Patch, max 1 operation)
  - Response: HTTP 204 No Content
```

#### Read Traffic Signs
```
GET /traffic-signs
  - Query: TrafficSignCriteria, Pageable
  - Response: Page<TrafficSignResponseDto> (JSON)
  - No authentication required

GET /traffic-signs/{id}
  - Path: {id} = UUID
  - Response: TrafficSignResponseDto (JSON with full details)
  - No authentication required

GET /traffic-signs/map/updates
  - Query: timestamp (Instant)
  - Response: FeatureCollection (GeoJSON)
```

#### Special Traffic Sign Operations
```
PUT /traffic-signs/{id}/remove
  - Requires: ROLE_TRAFFIC_SIGN_WRITER
  - Path: {id} = UUID
  - Request Body (optional): TrafficSignRemoveDto { removedOn: date }
  - Response: HTTP 202 Accepted

PUT /traffic-signs/{id}/validate
  - Requires: ROLE_TRAFFIC_SIGN_WRITER
  - Path: {id} = UUID
  - Query: validated (boolean)
  - Response: HTTP 204 No Content

PUT /traffic-signs/{id}/road-section
  - Requires: ROLE_NWB_UPDATER
  - Path: {id} = UUID
  - Request: Integer (roadSectionId, @Positive)
  - Response: HTTP 204 No Content

PUT /traffic-signs/{id}/road-section-lost
  - Requires: ROLE_NWB_UPDATER
  - Path: {id} = UUID
  - Request: RoadSectionLostDto (JSON)
  - Response: HTTP 204 No Content

GET /traffic-signs/{id}/authorize/organization/{organizationId}
  - Requires: ROLE_TRAFFIC_SIGN_AUTHORIZER
  - Paths: {id} = UUID, {organizationId} = UUID
  - Response: HasAuthorizationResponseDto (JSON with boolean)

GET /traffic-signs/{id}/state-before
  - Paths: {id} = UUID
  - Query: eventId (String)
  - Response: TrafficSignSnapshot (JSON, optional)
```

#### File Upload/Import
```
POST /traffic-signs (multipart upload)
  - Requires: ROLE_TRAFFIC_SIGN_WRITER
  - Request: Multipart file (CSV or APPLICATION_GEO_JSON)
  - Header: Content-Crs (optional, OGC CRS URI)
  - Response: FileUploadDto (JSON)

POST /traffic-signs/v5 (GeoJSON v5 upload)
  - Requires: ROLE_TRAFFIC_SIGN_WRITER
  - Request: Multipart file (GeoJSON)
  - Header: Content-Crs (optional)
  - Response: FileUploadDto (JSON)

PUT /traffic-signs/import
  - Requires: ROLE_TRAFFIC_SIGN_PROVIDER
  - Query: publicationTimestamp (Instant)
  - Request: TrafficSignImportDto (JSON)
  - Response: CompletableFuture<UUID> (async)

GET /traffic-signs/uploads
  - Requires: ROLE_TRAFFIC_SIGN_WRITER
  - Query: FileUploadCriteria, Pageable
  - Response: Page<FileUploadDto> (JSON)

GET /traffic-signs/uploads/{id}
  - Requires: ROLE_TRAFFIC_SIGN_WRITER
  - Path: {id} = UUID
  - Response: FileUploadDto (JSON)
```

#### OGC API Compliance
```
GET /rest/static-road-data/traffic-signs/v4/collections
GET /rest/static-road-data/traffic-signs/v5/collections
  - Response: OgcCollections (JSON with CRS URIs)
```

#### Main Sign API (GeoJSON v5)
```
POST /rest/static-road-data/traffic-signs/v5/main-signs
  - Requires: ROLE_TRAFFIC_SIGN_API
  - Header: Content-Crs (optional, OGC CRS URI, default WGS84)
  - Request: Raw GeoJSON string
  - Response: TrafficSignGeoJsonDtoV5 (GeoJSON, HTTP 200 with Location header)

PUT /rest/static-road-data/traffic-signs/v5/main-signs/{id}
  - Requires: ROLE_TRAFFIC_SIGN_API
  - Path: {id} = UUID
  - Header: Content-Crs (optional)
  - Request: Raw GeoJSON string
  - Response: TrafficSignGeoJsonDtoV5 (GeoJSON)
```

#### RVV Codes Management
```
POST /traffic-signs/rvv-codes
  - Requires: ROLE_ADMIN
  - Request: Multipart file
  - Response: HTTP 204 No Content
```

---

### 1.2 TRAFFIC SIGN HISTORY

```
GET /traffic-signs/{id}/history (non-paginated)
  - Path: {id} = UUID
  - Response: List<TrafficSignHistoryDto> (JSON)

GET /traffic-signs/{id}/history (paginated)
  - Path: {id} = UUID
  - Query: Pageable
  - Response: PagedModel<TrafficSignHistoryDto> (JSON)
```

**DTO**: TrafficSignHistoryDto - Contains event history and changes over time

---

### 1.3 TRAFFIC SIGN IMAGES

```
GET /images
  - Requires: ROLE_TRAFFIC_SIGN_IMAGE_READER
  - Query: url (String, @NotBlank)
  - Response: List<TrafficSignImageDto> (JSON)

GET /images/{id}/metadata
  - Requires: ROLE_TRAFFIC_SIGN_IMAGE_READER
  - Path: {id} = UUID
  - Response: TrafficSignImageDto (JSON)

GET /images/{id}
  - Path: {id} = UUID
  - Response: Streamed image (BMP, JPEG, or PNG)

POST /images
  - Requires: ROLE_TRAFFIC_SIGN_WRITER
  - Request: Multipart file (BMP, JPEG, PNG)
  - Response: TrafficSignImageDto (JSON)
```

**DTO**: TrafficSignImageDto - Image metadata and references

---

### 1.4 BLACK CODES MANAGEMENT

```
POST /black-codes
  - Requires: ROLE_TRAFFIC_SIGN_WRITER
  - Request: Multipart file (TEXT_TSV_VALUE - Tab Separated Values)
  - Response: FileUploadDto (JSON)

GET /black-codes/uploads
  - Requires: ROLE_TRAFFIC_SIGN_WRITER
  - Query: FileUploadCriteria, Pageable
  - Response: Page<FileUploadDto> (JSON)

GET /black-codes/uploads/{id}
  - Requires: ROLE_TRAFFIC_SIGN_WRITER
  - Path: {id} = UUID
  - Response: FileUploadDto (JSON)
```

---

### 1.5 ORGANIZATIONS

```
GET /organizations
  - Requires: ROLE_ADMIN
  - Response: List<OrganizationDto> (JSON)

GET /organizations/search
  - Requires: ROLE_ADMIN
  - Query: name (optional, String)
  - Response: List<OrganizationDto> (JSON)

GET /organizations/{id}
  - Requires: ROLE_USER or ROLE_ORGANIZATION_READER
  - Path: {id} = UUID
  - Response: OrganizationDto (JSON with full details)

GET /organizations/{id}/has-authority/{roadSectionId}
  - Requires: ROLE_ORGANIZATION_READER
  - Paths: {id} = UUID, {roadSectionId} = Integer
  - Response: Boolean (JSON)

POST /organizations
  - Requires: ROLE_ADMIN
  - Request: OrganizationDto (JSON)
  - Response: OrganizationDto (JSON)

PUT /organizations/{id}
  - Requires: ROLE_ADMIN
  - Path: {id} = UUID
  - Request: OrganizationDto (JSON)
  - Response: OrganizationDto (JSON)

DELETE /organizations/{id}
  - Requires: ROLE_ADMIN
  - Path: {id} = UUID
  - Response: HTTP 200 OK

POST /organizations/{id}/upload
  - Requires: ROLE_ADMIN or ROLE_USER_MANAGER
  - Path: {id} = UUID
  - Request: Multipart file (GeoJSON, Feature or FeatureCollection)
  - Response: HTTP 200 OK

DELETE /organizations/{id}/location
  - Requires: ROLE_ADMIN or ROLE_USER_MANAGER
  - Path: {id} = UUID
  - Response: HTTP 200 OK
```

**DTO**: OrganizationDto - Organization details including name, location, contact info

---

### 1.6 USERS

```
GET /users/organization/{id}
  - Requires: ROLE_ADMIN or ROLE_USER_MANAGER
  - Path: {id} = UUID
  - Response: List<KeycloakUserResponseDto> (JSON)

GET /users/current
  - Requires: ROLE_ADMIN or ROLE_USER_MANAGER
  - Response: KeycloakUserResponseDto (JSON)

GET /users/{id}?applicationScope=false
  - Requires: ROLE_USER_ADMIN_READER
  - Path: {id} = UUID
  - Query: applicationScope (optional boolean)
  - Response: KeycloakUserResponseDto (JSON with full details)

GET /users/{id}
  - Requires: ROLE_ADMIN or ROLE_USER_ADMIN_READER or ROLE_USER_MANAGER
  - Path: {id} = UUID
  - Response: KeycloakUserResponseDto (JSON with full details)

POST /users
  - Requires: ROLE_ADMIN or ROLE_USER_MANAGER
  - Request: KeycloakUserRequestDto (JSON)
  - Response: KeycloakUserResponseDto (JSON)
  - Status: HTTP 200 or 409 Conflict (if user exists)

PUT /users/{id}
  - Requires: ROLE_ADMIN or ROLE_USER_MANAGER
  - Path: {id} = UUID
  - Request: KeycloakUserRequestDto (JSON)
  - Response: KeycloakUserResponseDto (JSON)

DELETE /users/{id}
  - Requires: ROLE_ADMIN or ROLE_USER_MANAGER
  - Path: {id} = UUID
  - Response: HTTP 200 OK

POST /users/{id}/reset-password
  - Requires: ROLE_ADMIN or ROLE_USER_MANAGER
  - Path: {id} = UUID
  - Response: HTTP 200 OK
```

**DTOs**:
- KeycloakUserRequestDto - User creation/update data
- KeycloakUserResponseDto - Full user details including email, roles, organization

---

### 1.7 FINDINGS (Issues/Problems)

```
GET /findings
  - Requires: ROLE_TRAFFIC_SIGN_WRITER or ROLE_TRAFFIC_SIGN_FINDING_WRITER
  - Query: FindingCriteria, Pageable
  - Response: Page<TrafficSignFindingDto> (JSON)

GET /findings/map
  - Requires: ROLE_TRAFFIC_SIGN_WRITER or ROLE_TRAFFIC_SIGN_FINDING_WRITER
  - Query: roadAuthority (String, @NotNull)
  - Response: List<TrafficSignFindingDto> (JSON)

GET /findings/traffic-signs/{trafficSignId}
  - Requires: ROLE_TRAFFIC_SIGN_WRITER or ROLE_TRAFFIC_SIGN_FINDING_WRITER
  - Path: {trafficSignId} = UUID
  - Response: List<TrafficSignFindingDto> (JSON)

GET /findings/traffic-signs/{trafficSignId}/reasons/{reason}
  - Requires: ROLE_TRAFFIC_SIGN_WRITER or ROLE_TRAFFIC_SIGN_FINDING_WRITER
  - Paths: {trafficSignId} = UUID, {reason} = FindingReason enum
  - Response: TrafficSignFindingDto (JSON)

POST /findings
  - Requires: ROLE_TRAFFIC_SIGN_FINDING_WRITER
  - Request: TrafficSignFindingDto (JSON)
  - Response: HTTP 200 OK

DELETE /findings/{id}
  - Requires: ROLE_TRAFFIC_SIGN_FINDING_WRITER
  - Path: {id} = UUID
  - Response: HTTP 200 OK

PUT /findings/{id}/status
  - Requires: ROLE_ADMIN or ROLE_TRAFFIC_SIGN_WRITER or ROLE_TRAFFIC_SIGN_FINDING_WRITER
  - Path: {id} = UUID
  - Request: TrafficSignFindingDto (JSON)
  - Response: TrafficSignFindingDto (JSON)
```

**DTO**: TrafficSignFindingDto - Finding details including:
- `trafficSign`: TrafficSignDto (nested object with traffic sign ID reference)
- `reason`: FindingReason enum (category of finding)
- `status`: FindingStatus enum (current status: NEW, CLOSED, RESOLVED, etc.)
- `nlsId`: UUID (optional NLS issue reference)
- `feedbackText`: String (optional user feedback)

**Note**: The `trafficSign` field is a nested `TrafficSignDto` object. Access the traffic sign ID via `trafficSign.id`, not directly from the finding.

**Enum**: FindingReason - Categories of findings (e.g., INVALID_BLACK_CODE, NO_COUNTERPART, UNKNOWN_TRAFFIC_ORDER, WKD_INCONSISTENCY_SPEED, MISSING_DIRECTION)

---

### 1.8 ROAD AUTHORITIES

```
GET /road-authorities
  - Response: List<RoadAuthorityDto> (JSON)

GET /road-authorities/search
  - Query: type (RoadAuthorityType), code (String)
  - Response: RoadAuthorityDto (JSON, optional)
```

**DTO**: RoadAuthorityDto - Authority details
**Enum**: RoadAuthorityType - Types of road authorities

---

### 1.9 OTHER UTILITIES

#### Connected Roads
```
GET /connected-roads/{roadId}
  - Requires: ROLE_TRAFFIC_SIGN_WRITER
  - Path: {roadId} = Integer
  - Response: List<RoadSectionDto> (JSON)
```

#### Opening Hours Validation
```
POST /opening-hours/check-valid
  - Request: OpeningHoursCheckDto (JSON)
  - Response: OpeningHoursResultDto (JSON with valid: boolean)
```

#### Blobs (File Storage)
```
GET /blobs/{id}
  - Path: {id} = UUID
  - Response: Streamed blob file

GET /blobs/pdf/{id} [DEPRECATED since 2025-04-16]
  - Path: {id} = UUID
  - Response: Streamed PDF file
```

#### Info Messages
```
GET /info-messages
  - Requires: ROLE_ADMIN
  - Response: List<InfoMessageDto> (JSON)

GET /info-messages/current
  - Requires: ROLE_USER
  - Response: List<InfoMessageDto> (JSON)

POST /info-messages
  - Requires: ROLE_ADMIN
  - Request: InfoMessageDto (JSON)
  - Response: InfoMessageDto (JSON)

PUT /info-messages/{id}
  - Requires: ROLE_ADMIN
  - Path: {id} = UUID
  - Request: InfoMessageDto (JSON)
  - Response: InfoMessageDto (JSON)

DELETE /info-messages/{id}
  - Requires: ROLE_ADMIN
  - Path: {id} = UUID
  - Response: HTTP 200 OK
```

#### Traffic Sign Mutations (Query Operations)
```
GET /mutations
  - Query: TrafficSignMutationCriteria, Pageable
  - Response: Page<TrafficSignMutationResponseDto> (JSON)

POST /mutations/road-section-authority-job
  - Requires: ROLE_ADMIN
  - Response: String message
```

#### Usage Statistics
```
GET /usage-statistics (JSON)
  - Requires: ROLE_ADMIN
  - Query: date-start (LocalDate, optional), date-end (LocalDate, optional), organization-id (List<UUID>, optional)
  - Response: List<UsageStatisticsBucketDto> (JSON)

GET /usage-statistics (CSV)
  - Requires: ROLE_ADMIN
  - Query: (same as above)
  - Response: Streamed CSV (TEXT_CSV_VALUE)
```

#### Event Processor Control (Admin)
```
POST /event-processors/start
  - Requires: ROLE_ADMIN
  - Query: processing-group (ProcessingGroupType)
  - Response: String message

POST /event-processors/stop
  - Requires: ROLE_ADMIN
  - Query: processing-group (ProcessingGroupType)
  - Response: String message

POST /event-processors/replay
  - Requires: ROLE_ADMIN
  - Query: processing-group (ProcessingGroupType)
  - Response: String message

POST /event-processors/replay/{aggregateId}
  - Requires: ROLE_ADMIN
  - Path: {aggregateId} = UUID
  - Query: useSnapshot (optional, default true)
  - Response: String message

GET /event-processors/state
  - Response: List<ProcessorState> (JSON)
```

#### Current State Admin
```
POST /traffic-signs/current-state/v4
  - Requires: ROLE_ADMIN
  - Response: HTTP 200 OK
```

### 1.10 GRAPHQL API (Non-REST)

**Endpoint**: GraphQL query endpoint
**Type**: Query-only (read operations)

**Available Queries**:
```
findTrafficSigns(rvvCodes: [String], roadSectionIds: [Int], status: TrafficSignStatus)
findTrafficSignsPaged(rvvCodes: [String], status: TrafficSignStatus, trafficOrderIds: [String], page: Int, size: Int, sort: [String])
getTrafficSignsByIds(ids: [UUID])
getTrafficSignById(trafficSignId: UUID)
getTrafficSignsWithoutRoadSection(page: Int, size: Int)
getTrafficSignsWithoutRoadSectionPaged(page: Int, size: Int, sort: [String])
getTrafficSignsWithInvalidBlackCode(page: Int, size: Int, sort: [String])
getTrafficSignsWithoutDirection(page: Int, size: Int, sort: [String], exceptRvvCodes: [String])
```

---

## 2. TRAFFIC SIGN AREA BACKEND

**Location**: `/traffic-sign-area-backend/`
**Purpose**: Area management, environmental zones, parking bans, traffic regulations, hazardous substance routes

### 2.1 ENVIRONMENTAL ZONES (also accessible as `/emission-zones`)

```
GET /environmental-zones
  - Query: EmissionZoneCriteria
  - Response: List<EnvironmentalZoneDto> (JSON)

GET /environmental-zones/{id}
  - Path: {id} = UUID
  - Response: EnvironmentalZoneDto (JSON)

POST /environmental-zones
  - Requires: ROLE_EDITOR
  - Request: EnvironmentalZoneDto (JSON)
  - Response: EnvironmentalZoneDto (JSON)

POST /environmental-zones/{id}
  - Requires: ROLE_EDITOR
  - Path: {id} = UUID
  - Request: EnvironmentalZoneDto (JSON)
  - Response: EnvironmentalZoneDto (JSON)

POST /environmental-zones/upload
  - Requires: ROLE_EDITOR
  - Request: Multipart file (GeoJSON, JSON)
  - Response: FeatureCollection (GeoJSON)

DELETE /environmental-zones/{id}
  - Requires: ROLE_EDITOR
  - Path: {id} = UUID
  - Response: HTTP 204 No Content

GET /environmental-zones/map
  - Response: FeatureCollection (GeoJSON)
```

**DTO**: EnvironmentalZoneDto - Zone boundaries, names, regulations

---

### 2.2 AREAS

```
GET /areas
  - Response: List<AreaDto> (JSON)

GET /areas/{id}
  - Path: {id} = UUID
  - Response: AreaDto (JSON with details)

POST /areas
  - Requires: ROLE_EDITOR
  - Request: AreaDto (JSON)
  - Response: AreaDto (JSON with details)

POST /areas/{id}
  - Requires: ROLE_EDITOR
  - Path: {id} = UUID
  - Request: AreaDto (JSON)
  - Response: AreaDto (JSON with details)

DELETE /areas/{id}
  - Requires: ROLE_EDITOR
  - Path: {id} = UUID
  - Response: HTTP 204 No Content
```

**DTO**: AreaDto - Area definition and metadata

---

### 2.3 PARKING BANS

```
GET /parking-bans
  - Response: List<ParkingBanDto> (JSON)

GET /parking-bans/map
  - Query: ParkingBanCriteria
  - Response: Streamed GeoJSON (APPLICATION_GEO_JSON)

GET /parking-bans/{id}
  - Path: {id} = UUID
  - Response: ParkingBanDto (JSON with details)

POST /parking-bans
  - Requires: ROLE_EDITOR
  - Request: ParkingBanDto (JSON)
  - Response: ParkingBanDto (JSON with details)

POST /parking-bans/{id}
  - Requires: ROLE_EDITOR
  - Path: {id} = UUID
  - Request: ParkingBanDto (JSON)
  - Response: ParkingBanDto (JSON with details)

DELETE /parking-bans/{id}
  - Requires: ROLE_EDITOR
  - Path: {id} = UUID
  - Response: HTTP 204 No Content
```

**DTO**: ParkingBanDto - Parking restriction details

---

### 2.4 TRAFFIC REGULATIONS

```
GET /traffic-regulations
  - Response: List<TrafficRegulationDto> (JSON)

GET /traffic-regulations/{id}
  - Path: {id} = UUID
  - Response: TrafficRegulationDto (JSON with details)

POST /traffic-regulations
  - Requires: ROLE_EDITOR
  - Request: TrafficRegulationDto (JSON)
  - Response: TrafficRegulationDto (JSON with details)

POST /traffic-regulations/{id}
  - Requires: ROLE_EDITOR
  - Path: {id} = UUID
  - Request: TrafficRegulationDto (JSON)
  - Response: TrafficRegulationDto (JSON with details)

DELETE /traffic-regulations/{id}
  - Requires: ROLE_EDITOR
  - Path: {id} = UUID
  - Response: HTTP 204 No Content
```

**DTO**: TrafficRegulationDto - Traffic rule definitions

---

### 2.5 HAZARDOUS SUBSTANCE ROUTES

```
GET /routes/hazardous-substances/{id}
  - Path: {id} = UUID
  - Response: HazardousSubstanceRouteDto (JSON)

POST /routes/hazardous-substances
  - Requires: ROLE_EDITOR
  - Request: HazardousSubstanceRouteDto (JSON)
  - Response: HazardousSubstanceRouteDto (JSON)

PUT /routes/hazardous-substances/{id}
  - Requires: ROLE_EDITOR
  - Path: {id} = UUID
  - Request: HazardousSubstanceRouteDto (JSON)
  - Response: HazardousSubstanceRouteDto (JSON)

DELETE /routes/hazardous-substances/{id}
  - Requires: ROLE_EDITOR
  - Path: {id} = UUID
  - Response: HTTP 204 No Content

GET /routes/hazardous-substances/map
  - Response: FeatureCollection (GeoJSON)
```

**DTO**: HazardousSubstanceRouteDto - Route details and restrictions

---

### 2.6 COUNTIES & TOWNS

```
GET /counties
  - Response: List<CountyDto> (JSON)

GET /counties/{code}
  - Requires: ROLE_COUNTY_READER
  - Path: {code} = String (@ValidCountyCode)
  - Response: CountyDto (JSON)

GET /towns
  - Requires: ROLE_COUNTY_READER
  - Query: townName (optional), municipalityName (optional)
  - Response: List<TownDto> (JSON)
```

**DTOs**: CountyDto, TownDto - Geographic location data

---

### 2.7 COORDINATE CONVERSION

```
POST /conversion/wgs84
  - Requires: ROLE_EDITOR
  - Request: GeoJsonObject (Feature, FeatureCollection, or Geometry)
  - Response: GeoJsonObject (JSON, converted to WGS84)
```

---

### 2.8 IBBM EMISSION ZONES API

```
GET /ibbm/emission-zones/v1
  - Requires: ROLE_EMISSION_ZONE_API
  - Response: JSON string (serialized emission zones)
```

---

## 3. TRAFFIC SIGN FEEDBACK BACKEND

**Location**: `/traffic-sign-feedback-backend/`
**Purpose**: User feedback and corrections

```
GET /corrections
  - Requires: ROLE_USER
  - Response: List<CorrectionDto> (JSON)

POST /corrections
  - Requires: ROLE_USER
  - Request: CorrectionDto (JSON)
  - Response: Feature (GeoJSON Feature)

GET /corrections/map
  - Requires: ROLE_USER
  - Response: FeatureCollection (GeoJSON)
```

**DTO**: CorrectionDto - User-submitted corrections with location and description

---

## 4. TRAFFIC SIGN HGV CHARGE BACKEND

**Location**: `/traffic-sign-hgv-charge-backend/`
**Purpose**: Heavy Goods Vehicle charging network data

```
GET /mutations/{version}
  - Requires: ROLE_USER_MANAGER AND ROLE_HGV_CHARGE_EDITOR
  - Path: {version} = LocalDate (ISO date format YYYY-MM-DD)
  - Response: Streamed TSV file (TEXT_TSV_VALUE)
  - Header: Content-Disposition with filename
```

**Format**: Tab-Separated Values (TSV) export format

---

## 5. TRAFFIC SIGN INSPECTION BACKEND

**Location**: `/traffic-sign-inspection-backend/`
**Purpose**: Data validation and quality inspection jobs

### 5.1 INSPECTION JOBS

All inspection endpoints:
- **Path**: `/jobs/inspections/*`
- **Method**: POST
- **Authentication**: Requires `ROLE_ADMIN`
- **Response**: String message (TEXT_PLAIN)

```
POST /jobs/inspections/invalid-black-codes
  - Runs inspection for invalid black codes

POST /jobs/inspections/road-sections
  - Runs inspection for road section references

POST /jobs/inspections/unknown-traffic-orders
  - Runs inspection for unknown traffic orders

POST /jobs/inspections/wkd/axle-load
  - Runs WKD axle load data inspection

POST /jobs/inspections/wkd/height
  - Runs WKD height restriction inspection

POST /jobs/inspections/wkd/length
  - Runs WKD length restriction inspection

POST /jobs/inspections/wkd/load
  - Runs WKD load restriction inspection

POST /jobs/inspections/wkd/width
  - Runs WKD width restriction inspection

POST /jobs/inspections/wkd/speed
  - Runs WKD speed limit inspection

POST /jobs/inspections/missing-direction
  - Runs inspection for missing direction data

POST /jobs/inspections/nls-issues
  - Runs inspection for NLS data issues
```

---

### 5.2 FEEDBACK SUBMISSION

```
POST /feedback
  - Request: FeedbackRequestDto (JSON with trafficSignId and feedback text)
  - Response: HTTP 204 No Content
```

**DTO**: FeedbackRequestDto - User feedback on specific traffic signs

---

## 6. TRAFFIC SIGN WKD BACKEND

**Location**: `/traffic-sign-wkd-backend/`
**Purpose**: Vehicle Access Restrictions (WKD - Wegverkeerswet Voertuigbeperkingen)

### 6.1 SPEED LIMITS

```
GET /speeds
  - Requires: ROLE_WKD_EDITOR
  - Query: SpeedMutationCriteria, Pageable
  - Response: Page<SpeedMutationDto> (JSON)

GET /speeds/{nwbVersion}
  - Requires: ROLE_WKD_EDITOR
  - Path: {nwbVersion} = LocalDate
  - Query: NwbRoadSectionCriteria
  - Response: FeatureCollection (GeoJSON)

GET /speeds/{nwbVersion}/road-sections/{roadSectionId}
  - Requires: ROLE_WKD_EDITOR
  - Paths: {nwbVersion} = LocalDate, {roadSectionId} = Integer (@Positive)
  - Response: List<SpeedMutationDto> (JSON)

POST /speeds/{nwbVersion}/road-sections/{roadSectionId}
  - Requires: ROLE_WKD_EDITOR
  - Paths: {nwbVersion} = LocalDate, {roadSectionId} = Integer (@Positive)
  - Request: List<SpeedMutationDto> (JSON, @NotEmpty)
  - Response: FeatureCollection (GeoJSON)

DELETE /speeds/{nwbVersion}/road-sections/{roadSectionId}
  - Requires: ROLE_WKD_EDITOR
  - Paths: {nwbVersion} = LocalDate, {roadSectionId} = Integer (@Positive)
  - Response: HTTP 204 No Content

POST /speeds/upload
  - Requires: ROLE_WKD_EDITOR
  - Request: Multipart file (CSV)
  - Response: FileUploadDto (JSON)
```

**DTO**: SpeedMutationDto - Speed restriction details

---

### 6.2 AXLE LOAD RESTRICTIONS

```
GET /axle-load-restrictions
  - Requires: ROLE_WKD_EDITOR
  - Query: AxleLoadRestrictionMutationCriteria, Pageable
  - Response: Page<AxleLoadRestrictionMutationDto> (JSON)

GET /axle-load-restrictions/{nwbVersion}
  - Requires: ROLE_WKD_EDITOR
  - Path: {nwbVersion} = LocalDate
  - Query: NwbRoadSectionCriteria
  - Response: FeatureCollection (GeoJSON)

GET /axle-load-restrictions/{nwbVersion}/road-sections/{roadSectionId}
  - Requires: ROLE_WKD_EDITOR
  - Paths: {nwbVersion} = LocalDate, {roadSectionId} = Integer (@Positive)
  - Response: List<AxleLoadRestrictionMutationDto> (JSON)

POST /axle-load-restrictions/{nwbVersion}/road-sections/{roadSectionId}
  - Requires: ROLE_WKD_EDITOR
  - Paths: {nwbVersion} = LocalDate, {roadSectionId} = Integer (@Positive)
  - Request: List<AxleLoadRestrictionMutationDto> (JSON)
  - Response: FeatureCollection (GeoJSON)

DELETE /axle-load-restrictions/{nwbVersion}/road-sections/{roadSectionId}
  - Requires: ROLE_WKD_EDITOR
  - Paths: {nwbVersion} = LocalDate, {roadSectionId} = Integer (@Positive)
  - Response: HTTP 204 No Content
```

**DTO**: AxleLoadRestrictionMutationDto - Axle weight restrictions

---

### 6.3 HEIGHT RESTRICTIONS

```
GET /height-restrictions
  - Requires: ROLE_WKD_EDITOR
  - Query: Pageable
  - Response: Page<HeightRestrictionMutationDto> (JSON)

GET /height-restrictions/{nwbVersion}
  - Requires: ROLE_WKD_EDITOR
  - Path: {nwbVersion} = LocalDate
  - Query: NwbRoadSectionCriteria
  - Response: FeatureCollection (GeoJSON)

GET /height-restrictions/{nwbVersion}/road-sections/{roadSectionId}
  - Requires: ROLE_WKD_EDITOR
  - Paths: {nwbVersion} = LocalDate, {roadSectionId} = Integer (@Positive)
  - Response: List<HeightRestrictionMutationDto> (JSON)

POST /height-restrictions/{nwbVersion}/road-sections/{roadSectionId}
  - Requires: ROLE_WKD_EDITOR
  - Paths: {nwbVersion} = LocalDate, {roadSectionId} = Integer (@Positive)
  - Request: List<HeightRestrictionMutationDto> (JSON)
  - Response: FeatureCollection (GeoJSON)

DELETE /height-restrictions/{nwbVersion}/road-sections/{roadSectionId}
  - Requires: ROLE_WKD_EDITOR
  - Paths: {nwbVersion} = LocalDate, {roadSectionId} = Integer (@Positive)
  - Response: HTTP 204 No Content
```

**DTO**: HeightRestrictionMutationDto - Height clearance restrictions

---

### 6.4 LENGTH RESTRICTIONS

```
GET /length-restrictions
GET /length-restrictions/{nwbVersion}
GET /length-restrictions/{nwbVersion}/road-sections/{roadSectionId}
POST /length-restrictions/{nwbVersion}/road-sections/{roadSectionId}
DELETE /length-restrictions/{nwbVersion}/road-sections/{roadSectionId}
```

**DTO**: LengthRestrictionMutationDto - Maximum vehicle length restrictions

---

### 6.5 LOAD RESTRICTIONS

```
GET /load-restrictions
GET /load-restrictions/{nwbVersion}
GET /load-restrictions/{nwbVersion}/road-sections/{roadSectionId}
POST /load-restrictions/{nwbVersion}/road-sections/{roadSectionId}
DELETE /load-restrictions/{nwbVersion}/road-sections/{roadSectionId}
```

**DTO**: LoadRestrictionMutationDto - Total load weight restrictions

---

### 6.6 ROAD NARROWINGS

```
GET /road-narrowings
GET /road-narrowings/{nwbVersion}
GET /road-narrowings/{nwbVersion}/road-sections/{roadSectionId}
POST /road-narrowings/{nwbVersion}/road-sections/{roadSectionId}
DELETE /road-narrowings/{nwbVersion}/road-sections/{roadSectionId}
```

**DTO**: RoadNarrowingMutationDto - Road narrowing locations

---

### 6.7 DRIVING DIRECTIONS

```
GET /driving-directions
GET /driving-directions/{nwbVersion}
GET /driving-directions/{nwbVersion}/road-sections/{roadSectionId}
POST /driving-directions/{nwbVersion}/road-sections/{roadSectionId}
DELETE /driving-directions/{nwbVersion}/road-sections/{roadSectionId}
POST /driving-directions/upload
  - Request: Multipart file
```

**DTO**: DrivingDirectionMutationDto - Directional driving restrictions

---

### 6.8 ROAD CATEGORIES

```
GET /road-categories
GET /road-categories/{nwbVersion}
GET /road-categories/{nwbVersion}/road-sections/{roadSectionId}
POST /road-categories/{nwbVersion}/road-sections/{roadSectionId}
DELETE /road-categories/{nwbVersion}/road-sections/{roadSectionId}
```

**DTO**: RoadCategoryMutationDto - Road classification types

---

### 6.9 TRAFFIC TYPES

```
GET /traffic-types
GET /traffic-types/{nwbVersion}
GET /traffic-types/{nwbVersion}/road-sections/{roadSectionId}
POST /traffic-types/{nwbVersion}/road-sections/{roadSectionId}
DELETE /traffic-types/{nwbVersion}/road-sections/{roadSectionId}
```

**DTO**: TrafficTypeMutationDto - Traffic composition categories

---

### 6.10 CARRIAGEWAY TYPES

```
GET /carriageway-types
GET /carriageway-types/{nwbVersion}
```

**DTO**: CarriagewayTypeDto - Road surface and structure types

---

### 6.11 SCHOOL ZONES

```
GET /school-zones
GET /school-zones/{nwbVersion}
GET /school-zones/{nwbVersion}/road-sections/{roadSectionId}
POST /school-zones/{nwbVersion}/road-sections/{roadSectionId}
DELETE /school-zones/{nwbVersion}/road-sections/{roadSectionId}
```

**DTO**: SchoolZoneMutationDto - School zone locations

---

### 6.12 RVM TYPES (Restricted Vehicle Management)

```
GET /rvm-types
GET /rvm-types/{nwbVersion}
GET /rvm-types/{nwbVersion}/road-sections/{roadSectionId}
POST /rvm-types/{nwbVersion}/road-sections/{roadSectionId}
DELETE /rvm-types/{nwbVersion}/road-sections/{roadSectionId}
```

**DTO**: RvmTypeDto - Restricted vehicle categories

---

### 6.13 JOINT MUTATIONS

```
GET /joint-mutations/{nwbVersion}/road-sections/{roadSectionId}
  - Requires: ROLE_WKD_EDITOR
  - Paths: {nwbVersion} = LocalDate, {roadSectionId} = Integer (@Positive)
  - Response: JointMutationDto (JSON)

POST /joint-mutations/{nwbVersion}/road-sections/{roadSectionId}
  - Requires: ROLE_WKD_EDITOR
  - Paths: {nwbVersion} = LocalDate, {roadSectionId} = Integer (@Positive)
  - Request: JointMutationDto (JSON)
  - Response: JointMutationDto (JSON)
```

**DTO**: JointMutationDto - Combined restriction mutations

---

### 6.14 NLS PROXY ENDPOINTS (External Data)

```
GET /nls/nwb/map/{mapVersion}/roadSection
  - Path: {mapVersion} = LocalDate
  - Query: bagPublicSpaceLabel (optional), page (optional), size (optional)
  - Response: Dynamic/Raw (proxied from NLS service)

GET /nls/nwb/map/{mapVersion}/roadSection/{roadSectionId}
  - Path: {mapVersion} = LocalDate, {roadSectionId} = Long (@Positive)
  - Response: Dynamic/Raw

GET /nls/nwb/map/{mapVersion}/roadSection/{roadSectionId}/street
  - Path: {mapVersion} = LocalDate, {roadSectionId} = Long (@Positive)
  - Response: Dynamic/Raw (Street/BAG data)
```

These endpoints proxy to external NLS (Network Layout Service) for:
- Road section geometries
- Street/address (BAG) lookup
- Map version handling

---

### 6.15 FILE & EXPORT OPERATIONS

```
GET /file-uploads
  - Requires: ROLE_WKD_EDITOR
  - Query: FileUploadCriteria, Pageable
  - Response: Page<FileUploadDto> (JSON)

GET /file-uploads/{id}
  - Requires: ROLE_WKD_EDITOR
  - Path: {id} = UUID
  - Response: FileUploadDto (JSON)

GET /blob-files/{id}
  - Requires: ROLE_WKD_EDITOR
  - Path: {id} = UUID
  - Response: Streamed file
```

#### Export Operations
```
POST /export/start
  - Requires: ROLE_ADMIN
  - Query: nwbVersion (LocalDate, optional), type (ExportType)
  - Response: String message

GET /export/files
  - Requires: ROLE_ADMIN
  - Query: nwbVersion (LocalDate)
  - Response: Streamed ZIP file

POST /export/run-processed-job/nwb
  - Requires: ROLE_ADMIN
  - Response: String message

POST /export/run-processed-job/wkd
  - Requires: ROLE_ADMIN
  - Query: wkdType (ExportType)
  - Response: String message

POST /export/reprocess
  - Requires: ROLE_ADMIN
  - Query: nwbVersion (LocalDate), type (ExportType)
  - Response: String message

POST /export/migrate
  - Requires: ROLE_ADMIN
  - Query: fromNwbVersion (LocalDate), toNwbVersion (LocalDate), type (ExportType)
  - Response: String message

POST /export/remigrate
  - Requires: ROLE_ADMIN
  - Query: fromNwbVersion (LocalDate), toNwbVersion (LocalDate), type (ExportType)
  - Response: String message
```

#### Other Utilities
```
GET /usage-statistics
  - Requires: ROLE_ADMIN
  - Query: Pageable
  - Response: JSON or CSV

GET /sftp/list
  - List SFTP files

GET /sftp/test
  - Test SFTP connection

POST /sftp/write
  - Write to SFTP
```

---

## 7. TRAFFIC SIGN WKD DERIVATION BACKEND

**Location**: `/traffic-sign-wkd-derivation-backend/`
**Purpose**: Generates and manages proposed WKD mutations (suggestions/derivations)

### 7.1 SPEED MUTATION PROPOSALS

```
GET /speeds/road-section/{roadSectionId}
  - Requires: ROLE_TRAFFIC_SIGN_WRITER
  - Path: {roadSectionId} = int
  - Response: SpeedMutationProposalDto (JSON)

POST /speeds/road-section/{roadSectionId}
  - Requires: ROLE_TRAFFIC_SIGN_WRITER
  - Path: {roadSectionId} = int
  - Response: SpeedMutationProposalDto (JSON)
  - Action: Get or create proposal

DELETE /speeds/{id}
  - Requires: ROLE_TRAFFIC_SIGN_WRITER
  - Path: {id} = UUID
  - Response: HTTP 200 OK
```

**DTO**: SpeedMutationProposalDto - AI-generated or system-suggested speed restrictions

---

## 8. TRAFFIC SIGN PROFILE BACKEND

**Location**: `/traffic-sign-profile-backend/`
**Status**: Configuration/profile backend (minimal endpoints)

---

## AUTHENTICATION & AUTHORIZATION REFERENCE

### Common Roles Used

| Role | Purpose | Access Level |
|------|---------|--------------|
| `ROLE_ADMIN` | Full system access | All endpoints |
| `ROLE_TRAFFIC_SIGN_WRITER` | Modify traffic signs | CRUD on traffic signs, WKD derivation proposals |
| `ROLE_TRAFFIC_SIGN_API` | External API client | GeoJSON API access |
| `ROLE_WKD_EDITOR` | Edit WKD restrictions | All WKD endpoints |
| `ROLE_EDITOR` | Edit areas and zones | Area backend |
| `ROLE_USER_MANAGER` | Manage users | User CRUD, password reset |
| `ROLE_TRAFFIC_SIGN_AUTHORIZER` | Check authorization | Authorization checks |
| `ROLE_TRAFFIC_SIGN_FINDING_WRITER` | Manage findings | Findings CRUD |
| `ROLE_TRAFFIC_SIGN_PROVIDER` | External provider | Traffic sign import |
| `ROLE_ORGANIZATION_READER` | Read organizations | Organization reads |
| `ROLE_TRAFFIC_SIGN_IMAGE_READER` | View images | Image metadata/download |
| `ROLE_NWB_UPDATER` | Update NWB data | Road section updates |
| `ROLE_HGV_CHARGE_EDITOR` | Edit HGV data | HGV mutations |
| `ROLE_COUNTY_READER` | Read county data | County/town reads |
| `ROLE_EMISSION_ZONE_API` | Emission zone API | IBBM access |
| `ROLE_USER` | User feedback submission | Submit feedback and corrections (used in feedback-backend) |

---

## DATA FORMAT REFERENCE

### Standard Formats
- **JSON**: All REST API responses and requests
- **GeoJSON**: Geographic features (FeatureCollection, Feature, Geometry)
- **CSV**: Bulk data import/export
- **TSV**: Tab-separated values (HGV exports)
- **Multipart**: File uploads (images, bulk data)
- **Streamed**: Large file downloads with Content-Disposition

### Common Date/Time Formats
- **LocalDate**: `YYYY-MM-DD` (e.g., `2025-01-30`)
- **Instant**: ISO 8601 (e.g., `2025-01-30T10:30:00Z`)
- **UUID**: Standard UUID format (e.g., `550e8400-e29b-41d4-a716-446655440000`)

### Query & Path Parameters
- **Pageable**: Spring Data pagination (page, size, sort)
- **Criteria Objects**: Custom filter DTOs (TrafficSignCriteria, etc.)
- **Enum Values**: String representations of enums

---

## QUICK TIPS FOR FRONTEND DEVELOPERS

1. **When you need to work with traffic signs**: Use `traffic-sign-backend` `/traffic-signs` endpoints
2. **When you need user/org data**: Use `traffic-sign-backend` `/users` and `/organizations`
3. **When you need vehicle restrictions**: Use `traffic-sign-wkd-backend` (speeds, axle-load, height, etc.)
4. **When you need area/zone data**: Use `traffic-sign-area-backend` (environmental zones, areas)
5. **When you need user feedback**: Use `traffic-sign-feedback-backend` `/corrections`
6. **When you need to validate data**: Use `traffic-sign-inspection-backend` `/jobs/inspections`
7. **When you need geographic conversions**: Use `traffic-sign-area-backend` `/conversion/wgs84`
8. **When you need file uploads**: Check the specific endpoint (images, black codes, WKD files, etc.)
9. **When you need to check authorization**: Use `traffic-sign-backend` authorization endpoints
10. **When you get a date-based parameter** (e.g., nwbVersion): Use ISO format `YYYY-MM-DD`

---

## CONTACT FOR BACKEND CHANGES

When implementing features, reference this document by the backend service name:
- "I'm working with the **traffic-sign-backend** on the `/traffic-signs` endpoint"
- "I need the **DTO format** for creating a new speed mutation in **traffic-sign-wkd-backend**"
- "I need to understand the **authorization flow** for the **traffic-sign-backend**"

This allows for efficient communication about API requirements, data formats, and validation rules.
