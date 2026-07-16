# Architecture Decision Record

## Image Storage and Format Strategy

**Date:** 2026-07-17

**Context:** 
We need a robust way to store radiologic images for teaching cases. Initially, Cloudflare R2 was considered for its zero egress bandwidth fees. We also debated whether to support raw DICOM files (`.dcm`) requiring a complex browser viewer like Cornerstone.js, or rely on exported JPEGs/PNGs.

**Decision:**
1. **File Provider:** We chose to use **UploadThing** instead of Cloudflare R2.
2. **File Format:** We chose to strictly use standard web image formats (**JPEG/PNG**) for V1, rejecting raw DICOM files.

**Why:**
- **Cost vs Velocity:** Because the platform will be used by a very small cohort of students concurrently (3-5 users), bandwidth constraints are non-existent. We traded the scalability of Cloudflare R2 for the sheer developer velocity and maintainability of UploadThing's Next.js integration.
- **Simplicity:** Building a DICOM viewer with window/leveling in React is an immense undertaking that adds extreme complexity to the frontend. By relying on teachers to export specific annotated slices as JPEGs from their PACS, we can leverage standard `<img>` tags and keep the application lightweight and fast.
