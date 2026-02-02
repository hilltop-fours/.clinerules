# BER BEREIKBAARHEIDSKAART - PROJECT GUIDE

Welcome to the BER Bereikbaarheidskaart project. This guide explains the project structure and where to find relevant documentation.

## PROJECT OVERVIEW

This is a monorepo containing:
- **Frontend**: `/accessibility-map-frontend/` - The main web application for accessibility mapping
- **Backend Services**: Multiple specialized backend services (traffic-sign-backend, traffic-sign-wkd-backend, traffic-sign-area-backend, traffic-sign-feedback-backend)
- **Design System**: `/ndw-design/` - Shared GRG design system

## WHICH PART ARE YOU WORKING ON?

### 🎨 FRONTEND DEVELOPMENT

If you're working on the **accessibility-map-frontend** application:

1. **Start here**: [./frontend/BER-Bereikbaarheidskaart.md](./frontend/BER-Bereikbaarheidskaart.md)
   - Frontend-specific rules and conventions
   - Working directory and git repository configuration
   - Design system usage
   - Language rules and UI text guidelines

### 🔧 BACKEND API REFERENCE

If you need to understand the **backend APIs** used by this frontend:

1. **API Documentation**: [./backend/BER-Bereikbaarheidskaart.md](./backend/BER-Bereikbaarheidskaart.md)
   - Overview of all backend services used by this project
   - Which backend handles which features
   - External API dependencies and integration patterns
   - When to ask for backend help

### 🎨 DESIGN SYSTEM REFERENCE

The frontend uses the **GRG design system** located in `/ndw-design/`:
- Reference only - understand available components, styles, and patterns
- Never edit design system code
- If you need new components or modifications → Contact the design system team

## ⚠️ CRITICAL RULE: EDIT ONLY THE FRONTEND

**ALL code edits happen in `/accessibility-map-frontend/` ONLY.**

Backend services and the design system are **reference only** - never edit them. If you need changes to:
- Backend APIs → Contact the backend team
- Design system components → Contact the design system team

Use them to understand data structures, API contracts, and available components.

## GENERAL RULES

- **Language**: English for code and commits, Dutch for UI text
- **No translation system**: UI text is hardcoded in Dutch

## MONOREPO STRUCTURE

```
GRG-Wegkenmerken-verkeersborden/
├── accessibility-map-frontend/         ← EDIT ONLY THIS
├── traffic-sign-backend/               ← Reference only
├── traffic-sign-wkd-backend/           ← Reference only
├── traffic-sign-area-backend/          ← Reference only
├── traffic-sign-feedback-backend/      ← Reference only
├── ndw-design/                         ← Reference only
└── .clinerules/
    └── projects/BER-Bereikbaarheidskaart/
        ├── README.md                   ← You are here
        ├── frontend/
        │   └── BER-Bereikbaarheidskaart.md
        └── backend/
            └── BER-Bereikbaarheidskaart.md
```
