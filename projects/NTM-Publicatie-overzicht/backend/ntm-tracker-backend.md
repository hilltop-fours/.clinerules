# NTM Tracker Backend API - API Reference

Minimal, focused API for dataset tracking and import management. This service handles the ingestion and tracking of external datasets from various public sources (Data.overheid, NGR, RDW, RWS, CBS) before they are processed into NTM publications.

---

## COMMIT TRACKING

| Item | Value | Date |
|------|-------|------|
| Last Verified Commit | N/A | 2026-01-30 |
| Commit Message | From Swagger JSON | |
| Swagger Version | latest | 2026-01-30 |

**Status**: ✓ Documented from OpenAPI spec as of 2026-01-30
**Next Review**: Check repository commits

---

## QUICK REFERENCE BY FEATURE

| Feature | Main Endpoints | HTTP Methods | Auth Required | Purpose |
|---------|----------------|--------------|---------------|---------|
| **Dataset Queries** | `/datasets`, `/datasets/{id}` | GET | Keycloak NTM | Retrieve and search datasets |
| **Dataset Import** | `/datasets/import` | POST | Keycloak NTM | Bulk import datasets into tracker |
| **Dataset Rejection** | `/datasets/{id}/reject` | POST | Keycloak NTM | Reject imported datasets with reason |

---

## DETAILED ENDPOINTS

### Dataset Management

#### GET /datasets

**Authentication**: Keycloak NTM (OAuth2)

**Description**: Retrieve all datasets with filtering by status and deletion state. Results are paginated.

**Query Parameters**:
- `status` (array of enum, optional) - Filter by dataset status. Values: `NEW`, `IMPORT_STARTED`, `IMPORT_FAILED`, `IMPORTED`, `REJECTED`
- `isDeleted` (boolean, optional) - Filter by deletion state (true/false)
- `pageable` (Pageable, required) - Pagination parameters
  - `page` (int, 0-indexed) - Page number
  - `size` (int, min 1) - Items per page
  - `sort` (array, optional) - Sort criteria (e.g., `["id,desc"]`)

**Response** (HTTP 200): PagedModelDatasetDto
```json
{
  "content": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "site": "DATA_OVERHEID",
      "externalId": "ext-12345",
      "title": "Public Transport Routes",
      "description": "Real-time public transport routes for the Netherlands",
      "url": "https://data.overheid.nl/datasets/transport",
      "owner": "Ministry of Infrastructure",
      "organization": {
        "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
        "identifier": "org-123",
        "name": "Ministry of Infrastructure",
        "abbreviation": "MoI",
        "cbsCode": "CBS-123",
        "tags": ["government", "transport"]
      },
      "externalModifiedOn": "2026-01-20T10:00:00Z",
      "dataModifiedOn": "2026-01-25T15:30:00Z",
      "tags": ["public-transport", "realtime"],
      "categories": ["TRAFFIC_MANAGEMENT"],
      "status": "IMPORTED",
      "rejectedReason": null,
      "ntmPublicationId": "pub-uuid-001",
      "createdOn": "2026-01-20T10:00:00Z",
      "updatedOn": "2026-01-25T15:30:00Z",
      "isDeleted": false
    }
  ],
  "page": {
    "size": 20,
    "number": 0,
    "totalElements": 142,
    "totalPages": 8
  }
}
```

**Possible Status Codes**:
- 200 OK - Datasets retrieved successfully
- 400 Bad Request - Invalid pagination or filter parameters
- 401 Unauthorized - Missing or invalid authentication token

**Notes**:
- Pagination is required (pageable parameter must be provided)
- Status filter allows multiple values for OR queries
- Results can be very large; use pagination to avoid timeout

---

#### GET /datasets/{id}

**Authentication**: Keycloak NTM (OAuth2)

**Description**: Retrieve a single dataset by its unique identifier.

**Path Parameters**:
- `{id}` (UUID) - Dataset identifier

**Response** (HTTP 200): DatasetDto
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "site": "DATA_OVERHEID",
  "externalId": "ext-12345",
  "title": "Public Transport Routes",
  "description": "Real-time public transport routes for the Netherlands",
  "url": "https://data.overheid.nl/datasets/transport",
  "owner": "Ministry of Infrastructure",
  "organization": {
    "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    "identifier": "org-123",
    "name": "Ministry of Infrastructure",
    "abbreviation": "MoI",
    "cbsCode": "CBS-123",
    "tags": ["government", "transport"]
  },
  "externalModifiedOn": "2026-01-20T10:00:00Z",
  "dataModifiedOn": "2026-01-25T15:30:00Z",
  "tags": ["public-transport", "realtime"],
  "categories": ["TRAFFIC_MANAGEMENT"],
  "status": "IMPORTED",
  "rejectedReason": null,
  "ntmPublicationId": "pub-uuid-001",
  "createdOn": "2026-01-20T10:00:00Z",
  "updatedOn": "2026-01-25T15:30:00Z",
  "isDeleted": false
}
```

**Possible Status Codes**:
- 200 OK - Dataset found
- 401 Unauthorized - Missing/invalid authentication
- 404 Not Found - Dataset with given ID does not exist

---

#### POST /datasets/import

**Authentication**: Keycloak NTM (OAuth2)

**Description**: Import multiple datasets into the tracker from external sources. Initiates the import workflow for bulk operations.

**Request Body**: DatasetImportDto
```json
{
  "organizationId": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "ownerId": "c3d4e5f6-7890-abcd-ef1234567890ab",
  "datasetIds": [
    "550e8400-e29b-41d4-a716-446655440000",
    "550e8400-e29b-41d4-a716-446655440001"
  ],
  "datasets": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "site": "DATA_OVERHEID",
      "externalId": "ext-12345",
      "title": "Public Transport Routes",
      "description": "Real-time public transport routes",
      "url": "https://data.overheid.nl/datasets/transport",
      "owner": "Ministry of Infrastructure",
      "organization": {
        "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
        "identifier": "org-123",
        "name": "Ministry of Infrastructure",
        "abbreviation": "MoI",
        "cbsCode": "CBS-123",
        "tags": ["government"]
      },
      "externalModifiedOn": "2026-01-20T10:00:00Z",
      "dataModifiedOn": "2026-01-25T15:30:00Z",
      "tags": ["public-transport"],
      "categories": ["TRAFFIC_MANAGEMENT"],
      "status": "NEW",
      "createdOn": "2026-01-20T10:00:00Z",
      "updatedOn": "2026-01-20T10:00:00Z",
      "isDeleted": false
    }
  ]
}
```

**Response** (HTTP 200): DatasetImportDto (echo of request with import status)

**Possible Status Codes**:
- 200 OK - Import initiated successfully
- 400 Bad Request - Invalid organization/owner IDs or dataset list empty
- 401 Unauthorized - Missing/invalid authentication token
- 404 Not Found - Organization or owner user not found

**Notes**:
- At least one dataset ID must be provided in `datasetIds` array
- Either `datasetIds` (array of existing dataset UUIDs) or `datasets` (full objects) can be provided
- Import process runs asynchronously; check status via GET `/datasets/{id}`
- Status changes from NEW → IMPORT_STARTED → IMPORTED or IMPORT_FAILED

---

#### POST /datasets/{id}/reject

**Authentication**: Keycloak NTM (OAuth2)

**Description**: Reject a dataset that has been imported or is pending review. Sets rejection reason for audit trail.

**Path Parameters**:
- `{id}` (UUID) - Dataset identifier to reject

**Request Body**: RejectDto
```json
{
  "reason": "DUPLICATE"
}
```

**Response** (HTTP 200): DatasetDto (updated dataset with REJECTED status)
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "site": "DATA_OVERHEID",
  "externalId": "ext-12345",
  "title": "Public Transport Routes",
  "description": "Real-time public transport routes for the Netherlands",
  "url": "https://data.overheid.nl/datasets/transport",
  "owner": "Ministry of Infrastructure",
  "organization": {
    "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    "identifier": "org-123",
    "name": "Ministry of Infrastructure",
    "abbreviation": "MoI",
    "cbsCode": "CBS-123",
    "tags": ["government"]
  },
  "externalModifiedOn": "2026-01-20T10:00:00Z",
  "dataModifiedOn": "2026-01-25T15:30:00Z",
  "tags": ["public-transport"],
  "categories": ["TRAFFIC_MANAGEMENT"],
  "status": "REJECTED",
  "rejectedReason": "DUPLICATE",
  "ntmPublicationId": null,
  "createdOn": "2026-01-20T10:00:00Z",
  "updatedOn": "2026-01-30T14:20:00Z",
  "isDeleted": false
}
```

**Possible Status Codes**:
- 200 OK - Dataset rejected successfully
- 400 Bad Request - Invalid rejection reason
- 401 Unauthorized - Missing/invalid authentication
- 404 Not Found - Dataset not found
- 409 Conflict - Cannot reject dataset in current status

**Notes**:
- Only datasets in certain statuses can be rejected (NEW, IMPORT_STARTED, IMPORT_FAILED, IMPORTED)
- Rejection reason is stored in `rejectedReason` field
- Once rejected, dataset cannot be un-rejected; must be re-imported
- Rejection breaks link to NTM publication if one exists

---

## DATA TYPES (DTOs)

### DatasetDto

**Used in**: GET `/datasets`, GET `/datasets/{id}`, POST `/datasets/import`, POST `/datasets/{id}/reject`

| Field | Type | Required | Constraints | Description |
|-------|------|----------|-------------|-------------|
| `id` | UUID | Yes (response) | - | Unique dataset identifier |
| `site` | Enum | Yes | DATA_OVERHEID, NGR, RDW, RWS, CBS | Source catalog site |
| `externalId` | String | Yes | - | External system's dataset ID |
| `title` | String | Yes | - | Human-readable dataset name |
| `description` | String | No | - | Detailed description of dataset contents |
| `url` | String | Yes | valid URI | Direct link to dataset or catalog entry |
| `owner` | String | No | - | Organization or person owning the data |
| `organization` | ExternalOrganizationDto | Yes (response) | nested object | Organization metadata |
| `externalModifiedOn` | DateTime | No | ISO 8601 | Last modification timestamp from external source |
| `dataModifiedOn` | DateTime | No | ISO 8601 | When the actual dataset content was last updated |
| `tags` | Array<String> | No | - | Searchable tags/keywords |
| `categories` | Array<String> | No | - | Data categorization (e.g., TRAFFIC_MANAGEMENT) |
| `status` | Enum | Yes | NEW, IMPORT_STARTED, IMPORT_FAILED, IMPORTED, REJECTED | Current processing status |
| `rejectedReason` | Enum | No | DUPLICATE, INCOMPLETE, OUTDATED, UNSUITABLE | Why dataset was rejected (null if not rejected) |
| `ntmPublicationId` | UUID | No | - | Link to NTM publication if imported successfully |
| `createdOn` | DateTime | Yes (response) | ISO 8601 | When dataset was created in tracker |
| `updatedOn` | DateTime | Yes (response) | ISO 8601 | Last update timestamp |
| `isDeleted` | Boolean | No | - | Soft delete flag (true = archived/hidden) |

**Example**:
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "site": "DATA_OVERHEID",
  "externalId": "ext-12345",
  "title": "Public Transport Routes",
  "description": "Real-time public transport routes",
  "url": "https://data.overheid.nl/datasets/transport",
  "owner": "Ministry of Infrastructure",
  "organization": {
    "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    "identifier": "org-123",
    "name": "Ministry of Infrastructure",
    "abbreviation": "MoI",
    "cbsCode": "CBS-123",
    "tags": ["government"]
  },
  "externalModifiedOn": "2026-01-20T10:00:00Z",
  "dataModifiedOn": "2026-01-25T15:30:00Z",
  "tags": ["public-transport", "realtime"],
  "categories": ["TRAFFIC_MANAGEMENT"],
  "status": "IMPORTED",
  "rejectedReason": null,
  "ntmPublicationId": "pub-uuid-001",
  "createdOn": "2026-01-20T10:00:00Z",
  "updatedOn": "2026-01-25T15:30:00Z",
  "isDeleted": false
}
```

---

### ExternalOrganizationDto

**Used in**: DatasetDto (nested)

| Field | Type | Required | Constraints | Description |
|-------|------|----------|-------------|-------------|
| `id` | UUID | No | - | Organization's NTM system identifier |
| `identifier` | String | No | - | External system identifier (unique per source) |
| `name` | String | Yes | - | Full organization name |
| `abbreviation` | String | No | - | Organization abbreviation (e.g., MoI, RWS) |
| `cbsCode` | String | No | - | CBS (Statistics Netherlands) organization code |
| `tags` | Array<String> | No | - | Organization classification tags |

**Example**:
```json
{
  "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "identifier": "org-123",
  "name": "Ministry of Infrastructure",
  "abbreviation": "MoI",
  "cbsCode": "CBS-123",
  "tags": ["government", "ministry"]
}
```

---

### DatasetImportDto

**Used in**: POST `/datasets/import`

| Field | Type | Required | Constraints | Description |
|-------|------|----------|-------------|-------------|
| `organizationId` | UUID | Yes | - | NTM organization to own imported datasets |
| `ownerId` | UUID | Yes | - | User ID who initiated the import |
| `datasetIds` | Array<UUID> | Yes (conditional) | minItems: 1 | Existing dataset UUIDs to import |
| `datasets` | Array<DatasetDto> | Yes (conditional) | - | Full dataset objects to create and import |

**Notes**:
- Either `datasetIds` OR `datasets` must be provided, not both
- `datasetIds` refers to existing datasets already in the tracker
- `datasets` creates new dataset entries during import
- At least one dataset must be imported per request

**Example**:
```json
{
  "organizationId": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "ownerId": "c3d4e5f6-7890-abcd-ef1234567890ab",
  "datasetIds": [
    "550e8400-e29b-41d4-a716-446655440000",
    "550e8400-e29b-41d4-a716-446655440001"
  ],
  "datasets": []
}
```

---

### RejectDto

**Used in**: POST `/datasets/{id}/reject`

| Field | Type | Required | Constraints | Description |
|-------|------|----------|-------------|-------------|
| `reason` | Enum | Yes | DUPLICATE, INCOMPLETE, OUTDATED, UNSUITABLE | Why the dataset is being rejected |

**Example**:
```json
{
  "reason": "DUPLICATE"
}
```

---

### Pageable

**Used in**: Query parameters for paginated endpoints

| Field | Type | Required | Constraints | Description |
|-------|------|----------|-------------|-------------|
| `page` | Integer | Yes | minimum: 0 | Page number (0-indexed) |
| `size` | Integer | Yes | minimum: 1 | Records per page |
| `sort` | Array<String> | No | - | Sort criteria (e.g., ["id,desc", "createdOn,asc"]) |

**Example Query String**:
```
?pageable.page=0&pageable.size=20&pageable.sort=createdOn,desc
```

---

### PageMetadata

**Used in**: PagedModelDatasetDto (nested)

| Field | Type | Description |
|-------|------|-------------|
| `size` | Long | Page size (records returned) |
| `number` | Long | Current page number (0-indexed) |
| `totalElements` | Long | Total number of records across all pages |
| `totalPages` | Long | Total number of pages |

---

### PagedModelDatasetDto

**Used in**: GET `/datasets` response

| Field | Type | Description |
|-------|------|-------------|
| `content` | Array<DatasetDto> | Array of datasets for this page |
| `page` | PageMetadata | Pagination metadata |

---

## ENUMS

### DatasetStatus

**Used in**: DatasetDto.status, GET `/datasets` status filter

| Value | Description |
|-------|-------------|
| `NEW` | Dataset discovered but import not yet started |
| `IMPORT_STARTED` | Import process has begun (async processing) |
| `IMPORT_FAILED` | Import process encountered an error |
| `IMPORTED` | Dataset successfully imported and ready for publication |
| `REJECTED` | Dataset was explicitly rejected (see rejectedReason) |

---

### RejectionReason

**Used in**: DatasetDto.rejectedReason, RejectDto.reason

| Value | Description |
|-------|-------------|
| `DUPLICATE` | Dataset duplicates existing content in NTM |
| `INCOMPLETE` | Dataset is missing required metadata or fields |
| `OUTDATED` | Dataset content is stale or no longer relevant |
| `UNSUITABLE` | Dataset doesn't meet NTM quality or scope requirements |

---

### DataSource

**Used in**: DatasetDto.site

| Value | Description |
|-------|-------------|
| `DATA_OVERHEID` | Dutch government open data portal |
| `NGR` | National Geo Register (geographic data) |
| `RDW` | Dutch Vehicle Authority (vehicle registration data) |
| `RWS` | Dutch Ministry of Public Works & Water Management |
| `CBS` | Central Bureau for Statistics (Netherlands) |

---

## AUTHENTICATION & AUTHORIZATION

**Base URL**: `https://ntm.staging.ndw.nu/api/tracker`

**Security Scheme**: OAuth2 (Keycloak)

**IAM Server**: NDW Identity & Access Management (Keycloak realm: NTM)

### Authentication Flow

All endpoints require a valid Keycloak access token issued by the NDW IAM system. The token must be included as:
```
Authorization: Bearer {access_token}
```

### Authorization

No specific role restrictions are documented in the API spec. All authenticated users with valid Keycloak tokens can:
- Query all datasets (GET `/datasets`, GET `/datasets/{id}`)
- Import datasets (POST `/datasets/import`)
- Reject datasets (POST `/datasets/{id}/reject`)

> **Note**: Fine-grained role-based authorization may be enforced at the service level even if not explicit in the OpenAPI spec. Consult with the NTM team for role requirements.

### Common Use Cases

#### 1. Import External Datasets

```bash
POST /datasets/import
Authorization: Bearer {token}
Content-Type: application/json

{
  "organizationId": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "ownerId": "c3d4e5f6-7890-abcd-ef1234567890ab",
  "datasetIds": ["550e8400-e29b-41d4-a716-446655440000"]
}
```

Response: Datasets begin async import (status: IMPORT_STARTED)

#### 2. Check Import Status

```bash
GET /datasets/550e8400-e29b-41d4-a716-446655440000
Authorization: Bearer {token}
```

Monitor the `status` field until it reaches IMPORTED or IMPORT_FAILED.

#### 3. Reject a Dataset

```bash
POST /datasets/550e8400-e29b-41d4-a716-446655440000/reject
Authorization: Bearer {token}
Content-Type: application/json

{
  "reason": "DUPLICATE"
}
```

Response: Dataset status changes to REJECTED, ntmPublicationId is cleared.

#### 4. Query Datasets by Status

```bash
GET /datasets?status=IMPORTED&status=NEW&pageable.page=0&pageable.size=50
Authorization: Bearer {token}
```

Retrieves paginated results filtered by multiple status values.

---

## NOTES

- **Minimal API**: This is a focused, small API designed specifically for dataset ingestion tracking
- **Async Processing**: Import operations are asynchronous; use GET endpoints to poll for completion
- **Soft Deletes**: The `isDeleted` flag supports logical deletion without removing audit history
- **Pagination Required**: All list endpoints (GET `/datasets`) require pagination parameters
- **External Organization Metadata**: Organizations from external sources are stored as `ExternalOrganizationDto` for audit/reference
- **Keycloak Integration**: Tight integration with NDW's Keycloak instance; all users must be provisioned there
