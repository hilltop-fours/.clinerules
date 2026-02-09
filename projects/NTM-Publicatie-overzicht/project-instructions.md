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


## PATTERN DOCUMENTATION

NTM uses specific patterns for recurring implementation scenarios. Each pattern is documented in its own focused file:

**WHEN working with accessible forms (WCAG 3.3.1 compliance)**:
→ Read `patterns/form-accessibility.md`
- Error message formatting
- ARIA live regions & aria-invalid attributes
- Focus management
- Required field indicators
- Visual differentiation (non-color based)
- Form validation implementation
- Testing checklists

**WHEN working with query parameters for routing, filtering, or deep linking**:
→ Read `patterns/query-params.md`
- Feature-specific deep linking
- Navigation state preservation
- Pagination patterns
- Filtering & search
- Security considerations
- Testing approaches

---

## WHEN TO READ OTHER FILES

**For frontend coding rules**: Read `frontend/ntm-frontend.md`

**For backend API details**: Read the appropriate backend markdown file based on the mapping above

**For updating backend API docs**: Read `.clinerules/global/update-backend-api-instructions.md`
