# GRG WEGKENMERKEN VERKEERSBORDEN - PROJECT GUIDE

Welcome to the GRG Wegkenmerken Verkeersborden project. This guide explains the project structure and where to find relevant documentation.

## PROJECT OVERVIEW

This is a monorepo containing:
- **Frontend**: `/traffic-sign-frontend/` - The main web application for traffic sign management
- **Backend Services**: Multiple specialized backend services (traffic-sign-backend, traffic-sign-area-backend, traffic-sign-wkd-backend, etc.)
- **Design System**: `/ndw-design/` - Shared GRG design system

## WHICH PART ARE YOU WORKING ON?

### 🎨 FRONTEND DEVELOPMENT

If you're working on the **traffic-sign-frontend** application:

1. **Start here**: [./frontend/GRG-Wegkenmerken-verkeersborden.md](./frontend/GRG-Wegkenmerken-verkeersborden.md)
   - Frontend-specific rules and conventions
   - Working directory and git repository configuration
   - Design system usage
   - Language rules and UI text guidelines

### 🔧 BACKEND API REFERENCE

If you need to understand the **backend APIs** used by this frontend:

1. **API Documentation**: [./backend/GRG-Wegkenmerken-verkeersborden.md](./backend/GRG-Wegkenmerken-verkeersborden.md)
   - Overview of all backend services
   - Commit tracking and version information

2. **Detailed Service APIs**: [./backend/traffic-sign-backend.md](./backend/traffic-sign-backend.md)
   - Complete API reference for the main traffic-sign-backend service
   - Endpoints, DTOs, enums, and authentication details
   - Use this for understanding data structures and API contracts

### 🎨 DESIGN SYSTEM REFERENCE

The frontend uses the **GRG design system** located in `/ndw-design/`:
- Reference only - understand available components, styles, and patterns
- Never edit design system code
- If you need new components or modifications → Contact the design system team

## ⚠️ CRITICAL RULE: EDIT ONLY THE FRONTEND

**ALL code edits happen in `/traffic-sign-frontend/` ONLY.**

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
├── traffic-sign-frontend/          ← EDIT ONLY THIS
├── traffic-sign-backend/           ← Reference only
├── traffic-sign-area-backend/      ← Reference only
├── traffic-sign-wkd-backend/       ← Reference only
├── traffic-sign-feedback-backend/  ← Reference only
├── traffic-sign-inspection-backend/← Reference only
├── traffic-sign-profile-backend/   ← Reference only
├── traffic-sign-hgv-charge-backend/← Reference only
├── ndw-design/                     ← Reference only
└── .clinerules/
    └── projects/GRG-Wegkenmerken-verkeersborden/
        ├── README.md               ← You are here
        ├── frontend/
        │   └── GRG-Wegkenmerken-verkeersborden.md
        └── backend/
            ├── GRG-Wegkenmerken-verkeersborden.md
            └── traffic-sign-backend.md
```
