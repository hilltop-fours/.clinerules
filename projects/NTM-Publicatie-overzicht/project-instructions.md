# NTM PUBLICATIE OVERZICHT - PROJECT INSTRUCTIONS

**Project**: NTM Publicatie Overzicht
**Working directory path contains**: `/NTM-Publicatie-overzicht`
**Frontend repository**: `ntm-frontend/` (edit in `/src/**` only)
**Backend services**: `ntm-backend`, `ntm-tracker-backend` (reference only)
**Design system**: `ndw-design/` (reference only)

**For repository locations and structure overview**: Read `README.md` in this folder

---

## BACKEND API MAPPING

### By Feature Domain

When implementing features related to:

- **Data Publications** → Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
  - CRUD operations on data publications
  - Search, filtering, pagination
  - Approve, reject, hold, transfer ownership
  - Favorites on data publications
  - Import via DCAT dataset (`/data-publications/import`)

- **Datasets & Imports** → Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-tracker-backend.md`
  - Dataset listing, retrieval, rejection
  - Dataset import (`/datasets/import`)

- **Datasets (listing only)** → Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
  - List all datasets (`/datasets`)

- **Organizations** → Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
  - CRUD operations on organizations
  - Role requests (create, approve, deny)
  - User listing per organization
  - Logo management
  - Permissions / moderation settings
  - Organization repair utilities (`/repair/organizations`)

- **Users** → Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
  - CRUD operations on users
  - Current user info (`/users/current`)
  - Password reset

- **Notifications & Subscriptions** → Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
  - Notification listing and count (`/notifications`)
  - Email subscription management (`/subscriptions/mine`)

- **Favorites** → Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
  - List, add, delete favorites (`/favorites`)

- **Regions** → Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
  - Region lookup by code
  - Municipality listing (`/regions/municipalities`)

- **Standards** → Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
  - CRUD operations on standards

- **Data Regulations** → Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
  - List all regulations and get by ID

- **Statistics** → Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
  - User, organization, and data publication counts

- **Export** → Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
  - Export publications and users (`/export`)

- **Info Messages** → Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
  - CRUD operations on info messages
  - Current active messages and count

- **Blobs / Images** → Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
  - Upload and retrieve images (`/blobs/image`)

- **Contact Requests** → Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
  - Submit contact request (`/contact-request`)

- **Translations** → Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
  - Translation usage info (`/translations/usage`)

- **DCAT** → Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
  - DCAT-AP dataset endpoints (`/v1`)

- **External Organizations** → Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-tracker-backend.md`
  - List and get external organizations

- **External Categories** → Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-tracker-backend.md`
  - List and get external categories

### By Endpoint Pattern

When working with specific endpoints:

- `/data-publications/**` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
- `/data-publications/import` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
- `/datasets` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md` (listing only)
- `/datasets/**` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-tracker-backend.md` (import, reject, detail)
- `/organizations/**` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
- `/repair/organizations/**` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
- `/users/**` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
- `/notifications/**` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
- `/subscriptions/**` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
- `/favorites/**` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
- `/regions/**` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
- `/standards/**` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
- `/data-regulations/**` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
- `/statistics/**` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
- `/export/**` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
- `/info-messages/**` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
- `/blobs/**` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
- `/contact-request` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
- `/translations/**` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md`
- `/v1/**` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-backend.md` (DCAT-AP)
- `/external-organizations/**` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-tracker-backend.md`
- `/external-categories/**` → `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/backend/ntm-tracker-backend.md`


## PATTERN DOCUMENTATION

NTM uses specific patterns for recurring implementation scenarios. Each pattern is documented in its own focused file:

**WHEN working with accessible forms (WCAG 3.3.1 compliance)**:
→ Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/patterns/form-accessibility.md`
- Error message formatting
- ARIA live regions & aria-invalid attributes
- Focus management
- Required field indicators
- Visual differentiation (non-color based)
- Form validation implementation
- Testing checklists

**WHEN working with query parameters for routing, filtering, or deep linking**:
→ Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/patterns/query-params.md`
- Feature-specific deep linking
- Navigation state preservation
- Pagination patterns
- Filtering & search
- Security considerations
- Testing approaches

---

## WHEN TO READ OTHER FILES

**For frontend coding rules**: Read `$CLINERULES_ROOT/projects/NTM-Publicatie-overzicht/frontend/ntm-frontend.md`

**For backend API details**: Read the appropriate backend markdown file based on the mapping above

**For updating backend API docs**: Read `$CLINERULES_ROOT/global/update-backend-api-instructions.md`
