# BACKEND ENDPOINT CRAWL PROMPT

**Copy and paste this message into a fresh Claude Code chat when you need to update the backend API endpoint mapping.**

---

I need you to analyze the NTM backend repositories to create a comprehensive endpoint mapping for our .clinerules documentation.

## CONTEXT

We have a `.clinerules` folder with documentation about our backend APIs. The file `.clinerules/projects/NTM-Publicatie-overzicht/project-instructions.md` contains a "BACKEND API MAPPING" section that helps developers know which backend documentation file to read based on the feature they're working on.

## CURRENT MAPPING (incomplete)

The mapping currently lists:
- Publications → backend/ntm-backend.md
- Data Publications → backend/ntm-backend.md
- Organizations → backend/ntm-backend.md
- Notifications → backend/ntm-backend.md
- Datasets → backend/ntm-tracker-backend.md

## TASK

### Step 1: Navigate to Backend Repositories

1. **ntm-backend**: `/Users/daniel/Developer/NTM-Publicatie-overzicht/ntm-backend/`
2. **ntm-tracker-backend**: `/Users/daniel/Developer/NTM-Publicatie-overzicht/ntm-tracker-backend/`

### Step 2: Search for Controller Files

Backend is Java Spring Boot. Search for controller files:

```bash
cd /Users/daniel/Developer/NTM-Publicatie-overzicht/ntm-backend
find . -name "*Controller.java" -type f
```

```bash
cd /Users/daniel/Developer/NTM-Publicatie-overzicht/ntm-tracker-backend
find . -name "*Controller.java" -type f
```

### Step 3: Extract Endpoint Patterns

For each controller file:

1. Look for `@RestController` annotation
2. Look for `@RequestMapping` annotations (class-level and method-level)
3. Extract the base path and endpoint patterns

Example:
```java
@RestController
@RequestMapping("/api/publications")
public class PublicationController {

    @GetMapping
    public List<Publication> getAll() { ... }

    @PostMapping
    public Publication create(@RequestBody PublicationDto dto) { ... }
}
```

This gives you endpoint pattern: `/api/publications/**` → ntm-backend.md

### Step 4: Group Endpoints by Feature Domain

Organize endpoints into feature categories:
- Publications (CRUD operations on publications)
- Data Publications (data publication management)
- Organizations (organization management)
- Users (user management)
- Notifications (notification subscriptions)
- Datasets (dataset import and management)
- Tracker (import tracking and status)
- etc.

### Step 5: Update the Mapping

Edit the file:
`.clinerules/projects/NTM-Publicatie-overzicht/project-instructions.md`

Update the **BACKEND API MAPPING** section with your findings.

## EXPECTED OUTPUT FORMAT

The updated section should look like this:

```markdown
## BACKEND API MAPPING

### By Feature Domain

When implementing features related to:

- **Publications (CRUD)** → Read `backend/ntm-backend.md`
  - Create, read, update, delete publications
  - Publication metadata and details
  - Publication search and filtering

- **Data Publications** → Read `backend/ntm-backend.md`
  - Data publication management
  - Data publication search and filtering

- **Organizations** → Read `backend/ntm-backend.md`
  - Organization management, users, roles, permissions

- **Users** → Read `backend/ntm-backend.md`
  - User management and authentication

- **Notifications** → Read `backend/ntm-backend.md`
  - Subscriptions, alerts, preferences

- **Datasets** → Read `backend/ntm-tracker-backend.md`
  - Dataset import, ingestion, processing

- **Import Tracking** → Read `backend/ntm-tracker-backend.md`
  - Import status, history, errors

[Add any other features you discover...]

### By Endpoint Pattern

When working with specific endpoints:

- `/api/publications/**` → `backend/ntm-backend.md`
- `/api/data-publications/**` → `backend/ntm-backend.md`
- `/api/organizations/**` → `backend/ntm-backend.md`
- `/api/users/**` → `backend/ntm-backend.md`
- `/api/notifications/**` → `backend/ntm-backend.md`
- `/api/datasets/**` → `backend/ntm-tracker-backend.md`
- `/api/tracker/**` → `backend/ntm-tracker-backend.md`
- `/api/imports/**` → `backend/ntm-tracker-backend.md`

[Add all other endpoint patterns you discover...]
```

## IMPORTANT GUIDELINES

1. **Map to correct backend file**:
   - If endpoint is in `ntm-backend` repository → `backend/ntm-backend.md`
   - If endpoint is in `ntm-tracker-backend` repository → `backend/ntm-tracker-backend.md`

2. **Be comprehensive**:
   - List ALL endpoint patterns you find
   - Don't skip obscure or less-used endpoints

3. **Use consistent naming**:
   - Feature names should be clear and descriptive
   - Use plural forms for collections (Publications, Organizations)

4. **Group related endpoints**:
   - If multiple controllers handle similar features, group them under one feature domain

5. **Remove the incomplete note**:
   - After updating, remove the line: "**Note**: This mapping is incomplete..."

## VERIFICATION

After updating:

1. Verify you've checked both backend repositories
2. Verify all controller files were examined
3. Verify the mapping is comprehensive
4. Read the updated file to confirm formatting is correct

---

Execute the task and show me the updated BACKEND API MAPPING section when done.
