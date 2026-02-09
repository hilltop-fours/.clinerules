# NTM Publicatie Overzicht - Project Overview

Quick reference guide for the NTM project structure and repository locations.

## Project Identification

Working directory path contains: `/NTM-Publicatie-overzicht`

## Repository Locations

### Frontend (Editable)
- **Path**: `/Users/daniel/Developer/NTM-Publicatie-overzicht/ntm-frontend/`
- **Git operations**: Execute all git commands (checkout, add, commit, push, status, branch, etc.) from this directory
- **Editing allowed**: YES - Only edit files in `/ntm-frontend/src/**`

### Backend Services (Reference Only - Never Edit)
- **ntm-backend**: `/Users/daniel/Developer/NTM-Publicatie-overzicht/ntm-backend/`
- **ntm-tracker-backend**: `/Users/daniel/Developer/NTM-Publicatie-overzicht/ntm-tracker-backend/`

### Design System (Reference Only - Never Edit)
- **ndw-design**: `/Users/daniel/Developer/NTM-Publicatie-overzicht/ndw-design/`

### Rules & Documentation (Editable)
- **`.clinerules/`**: `/Users/daniel/Developer/NTM-Publicatie-overzicht/.clinerules/`

---

## Monorepo Structure

```
NTM-Publicatie-overzicht/
├── ntm-frontend/                   ← EDIT ONLY THIS (in /src/ subdirectory)
│   └── src/                        ← All edits happen here
├── ntm-backend/                    ← Reference only
├── ntm-tracker-backend/            ← Reference only
├── ndw-design/                     ← Reference only
└── .clinerules/                    ← Editable (documentation only)
    └── projects/NTM-Publicatie-overzicht/
        ├── README.md               ← You are here (project overview)
        ├── project-instructions.md ← Task-based rules
        ├── frontend/
        │   └── ntm-frontend.md     ← Frontend coding rules
        ├── backend/
        │   ├── ntm-backend.md      ← API reference
        │   └── ntm-tracker-backend.md ← API reference
        └── patterns/               ← Implementation patterns
            ├── form-accessibility.md
            └── query-params.md
```

---

## Critical Notes

- **Project root** `/Users/daniel/Developer/NTM-Publicatie-overzicht` is NOT a git repository—git commands will fail if executed there
- **Git operations** must execute from `ntm-frontend/` directory
- **All editing** happens in `ntm-frontend/src/` only
- For task-based rules and backend API mapping, see `project-instructions.md`
