# Traffic Sign Profile Backend - API Reference

Complete reference for the traffic-sign-profile-backend API. This service manages user profiles, organizations, and road authority management, including user access control and organization geographic data (GeoJSON location uploads).

---

## COMMIT TRACKING

| Item | Value | Date |
|------|-------|------|
| Last Verified Commit | 01a2f1e | 2026-01-30 |
| Commit Message | feat(area) # | 2024-03-19 |
| Swagger Version | latest | 2026-01-30 |

**Status**: ✓ Up to date as of 2026-01-30
**Next Review**: Check commits after 01a2f1e

---

## QUICK REFERENCE BY FEATURE

| Feature | Main Endpoints | HTTP Methods | Auth Required | Purpose |
|---------|----------------|--------------|---------------|---------|
| **Organizations (CRUD)** | `/organizations`, `/organizations/{id}` | GET, POST, PUT, DELETE | ROLE_ADMIN | Create and manage organizations |
| **Organization Search** | `/organizations/search` | GET | ROLE_ADMIN | Search organizations by name |
| **Organization Locations** | `/organizations/{id}/upload`, `/organizations/{id}/location` | POST (upload), DELETE | ROLE_ADMIN, ROLE_USER_MANAGER | Upload and manage GeoJSON location data |
| **Users (CRUD)** | `/users`, `/users/{id}` | POST, GET, PUT, DELETE | ROLE_ADMIN, ROLE_USER_MANAGER | Create and manage user accounts |
| **User Organization Query** | `/users/organization/{id}` | GET | ROLE_ADMIN, ROLE_USER_MANAGER | Get all users in an organization |
| **Current User** | `/users/current` | GET | ROLE_ADMIN, ROLE_USER_MANAGER | Get authenticated user details |
| **Password Reset** | `/users/{id}/reset-password` | POST | ROLE_ADMIN, ROLE_USER_MANAGER | Trigger password reset for user |
| **Road Authorities** | `/road-authorities` | GET | ROLE_ADMIN | List all road authorities |

---

## DETAILED ENDPOINTS

### Organizations

#### GET /organizations

**Authentication**: ROLE_ADMIN

**Description**: Retrieve all organizations in the system.

**Response** (HTTP 200): Array of OrganizationDto

**Example Response**:
```json
[
  {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Municipality of Utrecht",
    "hasGlobalMutationPermissions": true,
    "hasRvmMutationPermissions": false,
    "roadAuthorityIds": ["a1b2c3d4-e5f6-7890-abcd-ef1234567890"]
  }
]
```

**Possible Status Codes**:
- 200 OK - Organizations retrieved
- 401 Unauthorized - Missing/invalid auth

**Notes**:
- Returns basic organization information
- Use GET `/{id}` for full details including GeoJSON location data

---

#### GET /organizations/search

**Authentication**: ROLE_ADMIN

**Description**: Search organizations by name.

**Query Parameters**:
- `name` (string, optional) - Organization name to search for (partial match)

**Response** (HTTP 200): Array of OrganizationDto

**Example Response**:
```json
[
  {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Municipality of Utrecht",
    "hasGlobalMutationPermissions": true,
    "hasRvmMutationPermissions": false
  }
]
```

**Possible Status Codes**:
- 200 OK - Search completed
- 401 Unauthorized - Missing/invalid auth

**Notes**:
- Returns all organizations if no search term provided
- Search is case-insensitive partial matching

---

#### GET /organizations/{id}

**Authentication**: ROLE_USER (or any authenticated user)

**Description**: Retrieve a specific organization with full details including GeoJSON location data.

**Path Parameters**:
- `{id}` (UUID) - Organization identifier

**Response** (HTTP 200): OrganizationDto (with full details including location)

**Example Response**:
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "Municipality of Utrecht",
  "hasGlobalMutationPermissions": true,
  "hasRvmMutationPermissions": false,
  "roadAuthorities": [
    {
      "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
      "name": "Utrecht Road Authority",
      "type": "G",
      "code": "UT"
    }
  ],
  "location": {
    "type": "Polygon",
    "coordinates": [[[5.1204, 52.0704], [5.1305, 52.0805], [5.1204, 52.0704]]]
  }
}
```

**Possible Status Codes**:
- 200 OK - Organization found
- 401 Unauthorized - Missing/invalid auth
- 404 Not Found - Organization not found

**Notes**:
- Returns full organization details including GeoJSON location
- Location field is optional (only present if uploaded)
- Road authorities are nested within organization data

---

#### POST /organizations

**Authentication**: ROLE_ADMIN

**Description**: Create a new organization.

**Request Body**: OrganizationDto

**Example Request**:
```json
{
  "name": "New Municipality",
  "hasGlobalMutationPermissions": true,
  "hasRvmMutationPermissions": false,
  "roadAuthorityIds": ["a1b2c3d4-e5f6-7890-abcd-ef1234567890"]
}
```

**Response** (HTTP 200): OrganizationDto (created organization with full details)

**Possible Status Codes**:
- 200 OK - Organization created
- 400 Bad Request - Invalid organization data
- 401 Unauthorized - Missing/invalid auth

**Notes**:
- `name` field is required and must not be blank
- Both permission fields are required (boolean)
- `roadAuthorityIds` is optional

---

#### PUT /organizations/{id}

**Authentication**: ROLE_ADMIN

**Description**: Update an existing organization.

**Path Parameters**:
- `{id}` (UUID) - Organization identifier

**Request Body**: OrganizationDto

**Example Request**:
```json
{
  "name": "Updated Municipality Name",
  "hasGlobalMutationPermissions": false,
  "hasRvmMutationPermissions": true,
  "roadAuthorityIds": ["a1b2c3d4-e5f6-7890-abcd-ef1234567890", "b2c3d4e5-f6a7-8901-bcde-f12345678901"]
}
```

**Response** (HTTP 200): OrganizationDto (updated organization)

**Possible Status Codes**:
- 200 OK - Organization updated
- 400 Bad Request - Invalid organization data
- 401 Unauthorized - Missing/invalid auth
- 404 Not Found - Organization not found

**Notes**:
- All fields in the request body will be updated
- Location data is not updated via this endpoint (use upload endpoint)

---

#### DELETE /organizations/{id}

**Authentication**: ROLE_ADMIN

**Description**: Delete an organization.

**Path Parameters**:
- `{id}` (UUID) - Organization identifier

**Response** (HTTP 200): OK (no response body)

**Possible Status Codes**:
- 200 OK - Organization deleted
- 401 Unauthorized - Missing/invalid auth
- 404 Not Found - Organization not found

**Notes**:
- Deleting an organization may affect associated road authorities and users
- This action cannot be undone

---

### Organization Locations

#### POST /organizations/{id}/upload

**Authentication**: ROLE_ADMIN, ROLE_USER_MANAGER

**Description**: Upload GeoJSON location data for an organization. Supports Feature, FeatureCollection, and direct geometry objects.

**Path Parameters**:
- `{id}` (UUID) - Organization identifier

**Request Body**: Multipart form data
- `file` (MultipartFile) - GeoJSON file in one of these formats:
  - `application/geo+json`
  - `application/json`
  - `application/vnd.geo+json`

**Response** (HTTP 200): OK (no response body)

**Possible Status Codes**:
- 200 OK - Location uploaded successfully
- 400 Bad Request - Invalid file format or empty geometry
- 401 Unauthorized - Missing/invalid auth
- 404 Not Found - Organization not found

**Notes**:
- Accepts GeoJSON Feature with geometry
- Accepts GeoJSON FeatureCollection (uses first feature's geometry)
- Accepts raw GeoJSON geometry objects (Polygon, Point, etc.)
- Feature geometry must not be empty
- FeatureCollection features must not be empty
- Replaces any previously uploaded location

---

#### DELETE /organizations/{id}/location

**Authentication**: ROLE_ADMIN, ROLE_USER_MANAGER

**Description**: Delete the GeoJSON location data for an organization.

**Path Parameters**:
- `{id}` (UUID) - Organization identifier

**Response** (HTTP 200): OK (no response body)

**Possible Status Codes**:
- 200 OK - Location deleted
- 401 Unauthorized - Missing/invalid auth
- 404 Not Found - Organization not found

**Notes**:
- Removes the geographic location from the organization
- Organization continues to exist without location data

---

### Users

#### GET /users/organization/{id}

**Authentication**: ROLE_ADMIN, ROLE_USER_MANAGER

**Description**: Get all users belonging to a specific organization.

**Path Parameters**:
- `{id}` (UUID) - Organization identifier

**Response** (HTTP 200): Array of KeycloakUserResponseDto

**Example Response**:
```json
[
  {
    "id": "f1e2d3c4-b5a6-7890-1234-567890abcdef",
    "firstName": "John",
    "lastName": "Doe",
    "email": "john.doe@example.com",
    "organization": {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "name": "Municipality of Utrecht"
    },
    "active": true,
    "createdOn": "2024-03-15T10:30:00Z"
  }
]
```

**Possible Status Codes**:
- 200 OK - Users retrieved
- 401 Unauthorized - Missing/invalid auth
- 404 Not Found - Organization not found

**Notes**:
- Returns all users in the specified organization
- Does not include roleGroups in basic response

---

#### GET /users/current

**Authentication**: ROLE_ADMIN, ROLE_USER_MANAGER

**Description**: Get details of the currently authenticated user.

**Response** (HTTP 200): KeycloakUserResponseDto

**Example Response**:
```json
{
  "id": "f1e2d3c4-b5a6-7890-1234-567890abcdef",
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com",
  "organization": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Municipality of Utrecht"
  },
  "active": true,
  "createdOn": "2024-03-15T10:30:00Z"
}
```

**Possible Status Codes**:
- 200 OK - Current user retrieved
- 401 Unauthorized - Not authenticated

**Notes**:
- Returns the user making the request
- Does not include roleGroups in basic response

---

#### GET /users/{id}

**Authentication**: ROLE_ADMIN, ROLE_USER_MANAGER

**Description**: Get details of a specific user with full information including role groups.

**Path Parameters**:
- `{id}` (UUID) - User identifier

**Response** (HTTP 200): KeycloakUserResponseDto (with roleGroups)

**Example Response**:
```json
{
  "id": "f1e2d3c4-b5a6-7890-1234-567890abcdef",
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com",
  "organization": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Municipality of Utrecht"
  },
  "roleGroups": ["USER", "ORGANIZATION_ADMIN"],
  "active": true,
  "createdOn": "2024-03-15T10:30:00Z"
}
```

**Possible Status Codes**:
- 200 OK - User found
- 401 Unauthorized - Missing/invalid auth
- 404 Not Found - User not found

**Notes**:
- Includes roleGroups (sorted) in the response
- More detailed than other user endpoints

---

#### POST /users

**Authentication**: ROLE_ADMIN, ROLE_USER_MANAGER

**Description**: Create a new user in Keycloak.

**Request Body**: KeycloakUserRequestDto

**Example Request**:
```json
{
  "firstName": "Jane",
  "lastName": "Smith",
  "email": "jane.smith@example.com",
  "organizationId": "550e8400-e29b-41d4-a716-446655440000",
  "roleGroups": ["USER", "TRAFFIC_SIGN_EDITOR"],
  "active": true
}
```

**Response** (HTTP 200 or 409): KeycloakUserResponseDto (with roleGroups)

**Possible Status Codes**:
- 200 OK - User created successfully
- 409 Conflict - User already exists in Keycloak (still created in local system)
- 400 Bad Request - Invalid user data (email required for creation)
- 401 Unauthorized - Missing/invalid auth

**Notes**:
- Email is required for user creation
- Returns 409 if user already exists in Keycloak but is still created locally
- roleGroups are optional but recommended
- active field is required

---

#### PUT /users/{id}

**Authentication**: ROLE_ADMIN, ROLE_USER_MANAGER

**Description**: Update an existing user.

**Path Parameters**:
- `{id}` (UUID) - User identifier

**Request Body**: KeycloakUserRequestDto

**Example Request**:
```json
{
  "firstName": "Jane",
  "lastName": "Smith",
  "email": "jane.smith@example.com",
  "organizationId": "550e8400-e29b-41d4-a716-446655440000",
  "roleGroups": ["USER", "ORGANIZATION_ADMIN", "TRAFFIC_SIGN_EDITOR"],
  "active": true
}
```

**Response** (HTTP 200): KeycloakUserResponseDto (with roleGroups)

**Possible Status Codes**:
- 200 OK - User updated
- 400 Bad Request - Invalid user data
- 401 Unauthorized - Missing/invalid auth
- 404 Not Found - User not found

**Notes**:
- Email is optional for updates (not required like in creation)
- All provided fields are updated
- roleGroups can be modified via this endpoint

---

#### DELETE /users/{id}

**Authentication**: ROLE_ADMIN, ROLE_USER_MANAGER

**Description**: Delete a user from the system.

**Path Parameters**:
- `{id}` (UUID) - User identifier

**Response** (HTTP 200): OK (no response body)

**Possible Status Codes**:
- 200 OK - User deleted
- 401 Unauthorized - Missing/invalid auth
- 404 Not Found - User not found

**Notes**:
- Deletes the user from Keycloak and local system
- Cannot be undone without recreating the user

---

#### POST /users/{id}/reset-password

**Authentication**: ROLE_ADMIN, ROLE_USER_MANAGER

**Description**: Trigger a password reset for a user in Keycloak.

**Path Parameters**:
- `{id}` (UUID) - User identifier

**Response** (HTTP 200): OK (no response body)

**Possible Status Codes**:
- 200 OK - Password reset triggered
- 401 Unauthorized - Missing/invalid auth
- 404 Not Found - User not found

**Notes**:
- Sends a password reset email to the user
- User must follow the email link to set a new password

---

### Road Authorities

#### GET /road-authorities

**Authentication**: ROLE_ADMIN

**Description**: Retrieve all road authorities in the system.

**Response** (HTTP 200): Array of RoadAuthorityDto

**Example Response**:
```json
[
  {
    "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    "name": "Utrecht Road Authority",
    "type": "G",
    "code": "UT"
  },
  {
    "id": "b2c3d4e5-f6a7-8901-bcde-f12345678901",
    "name": "North Holland Provincial Authority",
    "type": "P",
    "code": "NH"
  }
]
```

**Possible Status Codes**:
- 200 OK - Road authorities retrieved
- 401 Unauthorized - Missing/invalid auth

**Notes**:
- Returns all road authorities in the system
- Road authorities are typically associated with organizations

---

## DATA TYPES (DTOs)

### OrganizationDto

**Used in**: GET/POST/PUT `/organizations` endpoints, GET `/organizations/{id}`, nested in KeycloakUserResponseDto

| Field | Type | Required | Constraints | Description |
|-------|------|----------|-------------|-------------|
| `id` | UUID | Yes (response) | - | Unique organization identifier |
| `name` | String | Yes | min 1 char, not blank | Organization name |
| `hasGlobalMutationPermissions` | Boolean | Yes | - | Whether organization has global mutation rights |
| `hasRvmMutationPermissions` | Boolean | Yes | - | Whether organization has RVM mutation rights |
| `roadAuthorityIds` | Array[UUID] | No | - | IDs of associated road authorities |
| `roadAuthorities` | Array[RoadAuthorityDto] | No (response only) | - | Full road authority objects (nested) |
| `location` | GeoJsonObject | No | Valid GeoJSON geometry | Geographic location data (Polygon, Point, etc.) |

**Example**:
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "Municipality of Utrecht",
  "hasGlobalMutationPermissions": true,
  "hasRvmMutationPermissions": false,
  "roadAuthorityIds": ["a1b2c3d4-e5f6-7890-abcd-ef1234567890"],
  "roadAuthorities": [
    {
      "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
      "name": "Utrecht Road Authority",
      "type": "G",
      "code": "UT"
    }
  ],
  "location": {
    "type": "Polygon",
    "coordinates": [[[5.1204, 52.0704], [5.1305, 52.0805], [5.1204, 52.0704]]]
  }
}
```

**Validation Notes**:
- In POST/PUT requests, `location` should not be set directly (use upload endpoint)
- Response includes location data from previous uploads
- roadAuthorities field is ignored in requests (use roadAuthorityIds instead)

---

### KeycloakUserRequestDto

**Used in**: POST/PUT `/users` endpoints

| Field | Type | Required | Constraints | Description |
|-------|------|----------|-------------|-------------|
| `firstName` | String | Yes | min 1 char, not blank | User's first name |
| `lastName` | String | Yes | min 1 char, not blank | User's last name |
| `email` | String | Yes (POST only) | Valid email format | User's email address |
| `organizationId` | UUID | Yes | Valid organization UUID | Organization the user belongs to |
| `roleGroups` | Array[KeycloakRoleGroup] | No | - | List of role groups to assign |
| `active` | Boolean | Yes | - | Whether the user account is active |

**Example**:
```json
{
  "firstName": "Jane",
  "lastName": "Smith",
  "email": "jane.smith@example.com",
  "organizationId": "550e8400-e29b-41d4-a716-446655440000",
  "roleGroups": ["USER", "TRAFFIC_SIGN_EDITOR"],
  "active": true
}
```

**Validation Notes**:
- Email is required for POST (create) but optional for PUT (update)
- firstName and lastName are always required
- organizationId must be a valid existing organization
- active is a required boolean field

---

### KeycloakUserResponseDto

**Used in**: GET `/users/*` endpoints, POST/PUT `/users` responses

| Field | Type | Required | Constraints | Description |
|-------|------|----------|-------------|-------------|
| `id` | UUID | Yes (response) | - | Unique user identifier |
| `firstName` | String | Yes | - | User's first name |
| `lastName` | String | Yes | - | User's last name |
| `email` | String | Yes | - | User's email address |
| `organization` | OrganizationDto | No | - | Organization the user belongs to |
| `roleGroups` | Array[KeycloakRoleGroup] | No | - | Assigned role groups (only in detailed response) |
| `active` | Boolean | Yes | - | Whether the user account is active |
| `createdOn` | Instant | Yes (response) | ISO 8601 timestamp | Account creation timestamp |

**Example**:
```json
{
  "id": "f1e2d3c4-b5a6-7890-1234-567890abcdef",
  "firstName": "Jane",
  "lastName": "Smith",
  "email": "jane.smith@example.com",
  "organization": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Municipality of Utrecht"
  },
  "roleGroups": ["USER", "TRAFFIC_SIGN_EDITOR"],
  "active": true,
  "createdOn": "2024-03-15T10:30:00Z"
}
```

---

### RoadAuthorityDto

**Used in**: GET `/road-authorities`, nested in OrganizationDto

| Field | Type | Required | Constraints | Description |
|-------|------|----------|-------------|-------------|
| `id` | UUID | Yes | - | Unique road authority identifier |
| `name` | String | Yes | - | Road authority name |
| `type` | RoadAuthorityType | Yes | R, P, G, W, T | Authority type code |
| `code` | String | No | - | Short code for the authority |

**Example**:
```json
{
  "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "name": "Municipality of Utrecht",
  "type": "G",
  "code": "UT"
}
```

---

## ENUMS

### RoadAuthorityType

**Used in**: RoadAuthorityDto

| Value | Description |
|-------|-------------|
| R | Rijk (National/Central Government) |
| P | Provincie (Provincial Authority) |
| G | Gemeente (Municipality) |
| W | Waterschap (Water Board) |
| T | Overige (Other) |

---

### KeycloakRoleGroup

**Used in**: KeycloakUserRequestDto, KeycloakUserResponseDto

| Value | Description |
|-------|-------------|
| USER | Basic user role - default for all users |
| ORGANIZATION_ADMIN | Organization administrator - can manage users in organization |
| TRAFFIC_SIGN_EDITOR | Traffic sign editor - can create and edit traffic signs |
| ROAD_FEATURE_EDITOR | Road feature editor - can edit road features (WKD, etc.) |
| ADMIN | System administrator - full system access |

**Notes**:
- Users typically have at least USER role
- Multiple roles can be assigned to a single user
- Roles are sorted alphabetically in responses

---

## AUTHENTICATION & AUTHORIZATION

**Base URL**: `https://wegkenmerken.staging.ndw.nu/api/profile`
**Security Scheme**: OAuth2 (Keycloak/NDW IAM)

### Required Roles by Feature

| Feature | Required Roles | Description |
|---------|----------------|-------------|
| View all organizations | ROLE_ADMIN | Admin-only |
| Search organizations | ROLE_ADMIN | Admin-only |
| View organization details | ROLE_USER (any authenticated) | Any authenticated user |
| Create organization | ROLE_ADMIN | Admin-only |
| Update organization | ROLE_ADMIN | Admin-only |
| Delete organization | ROLE_ADMIN | Admin-only |
| Upload organization location | ROLE_ADMIN, ROLE_USER_MANAGER | Upload GeoJSON data |
| Delete organization location | ROLE_ADMIN, ROLE_USER_MANAGER | Remove location data |
| View users in organization | ROLE_ADMIN, ROLE_USER_MANAGER | Manager-level access |
| View current user | ROLE_ADMIN, ROLE_USER_MANAGER | Self-service query |
| View specific user | ROLE_ADMIN, ROLE_USER_MANAGER | Manager-level access |
| Create user | ROLE_ADMIN, ROLE_USER_MANAGER | Manager-level access |
| Update user | ROLE_ADMIN, ROLE_USER_MANAGER | Manager-level access |
| Delete user | ROLE_ADMIN, ROLE_USER_MANAGER | Manager-level access |
| Reset user password | ROLE_ADMIN, ROLE_USER_MANAGER | Manager-level access |
| View road authorities | ROLE_ADMIN | Admin-only |

### Authentication Details

- **Provider**: Keycloak (NDW IAM)
- **Base Auth URL**: Keycloak instance managing NDW realms
- **User Management**: Integrated with Keycloak for user/password management
- **Organization Ownership**: Users are assigned to organizations, role groups determine permissions within the system

### Role Hierarchy

1. **ROLE_ADMIN** - Full access to all endpoints
2. **ROLE_USER_MANAGER** - Can manage users and upload organization data
3. **ROLE_USER** - Can only view their own organization

---

## TYPICAL WORKFLOWS

### Creating an Organization with Location

1. Create organization:
   ```
   POST /organizations
   {
     "name": "New Municipality",
     "hasGlobalMutationPermissions": true,
     "hasRvmMutationPermissions": false
   }
   ```

2. Upload location data:
   ```
   POST /organizations/{id}/upload
   (multipart file with GeoJSON)
   ```

3. Verify:
   ```
   GET /organizations/{id}
   ```

### Managing Users in an Organization

1. Create user:
   ```
   POST /users
   {
     "firstName": "John",
     "lastName": "Doe",
     "email": "john@example.com",
     "organizationId": "{org-id}",
     "roleGroups": ["USER", "TRAFFIC_SIGN_EDITOR"],
     "active": true
   }
   ```

2. View organization users:
   ```
   GET /users/organization/{org-id}
   ```

3. Update user roles:
   ```
   PUT /users/{user-id}
   {
     "firstName": "John",
     "lastName": "Doe",
     "email": "john@example.com",
     "organizationId": "{org-id}",
     "roleGroups": ["USER", "TRAFFIC_SIGN_EDITOR", "ROAD_FEATURE_EDITOR"],
     "active": true
   }
   ```

4. Reset password:
   ```
   POST /users/{user-id}/reset-password
   ```

---

## INTEGRATION WITH OTHER SERVICES

This backend provides the foundational user and organization management layer for the traffic sign system.

### Role in System Architecture

- **Central Authority Management**: Defines organizations and their permissions
- **User Access Control**: Manages who can access the system
- **Location Data Storage**: Maintains GeoJSON boundaries for organizations
- **Keycloak Integration**: Single sign-on and user credential management

---

## NOTES

- **GeoJSON Support**: Uses standard GeoJSON format for organization boundaries
- **Multi-role Users**: Users can have multiple role groups for flexible permission management
- **Organization-centric**: All users must belong to an organization
- **Keycloak Backend**: User credentials and basic authentication handled by Keycloak
- **Location Uploads**: GeoJSON geometry replaces previous upload, supporting Feature, FeatureCollection, and raw geometry formats

---
