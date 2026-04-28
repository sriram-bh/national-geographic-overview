# Pristine Seas — National Geographic Society

**A full-stack archive platform built for NatGeo's ocean conservation program.**  
Built through a 180 Degrees Consulting engagement · University of Michigan

> The repository is private at the client's request. This page documents the scope, architecture, and decisions behind the project.

---

## What It Is

The National Geographic Pristine Seas program has spent 15 years exploring the most remote ocean environments on earth — generating a massive archive of marine species photography with no public-facing way to access it.

This project builds that interface from scratch: a searchable image archive with scientific taxonomy filtering, a photographer upload portal, and a production migration plan for NatGeo's engineering team to execute.

---

## Screenshots

### Home Page
![Home page with hero, search, and featured expedition showcase](.github/screenshots/home.png)

### Gallery with Filters
![Gallery page with cascading taxonomy filters and search results](.github/screenshots/gallery.png)

### Image Detail Modal
![Two-panel modal with species taxonomy, description, and carousel](.github/screenshots/modal.png)

### Photographer Upload Portal
![Three-step authenticated upload flow](.github/screenshots/upload.png)

*(Screenshots available on request)*

---

## Scope of Work

This was a full consulting engagement, not just a coding project. Deliverables included:

- **Figma design system** — high-fidelity mockups for every page and interaction state, built to NatGeo brand standards
- **PostgreSQL schema** — designed around WoRMS (World Register of Marine Species) taxonomy, with controlled vocabulary tables and a clean separation between species data and photo metadata
- **SQL query library** — search, filtering, and taxonomy dropdown queries with documented design rationale
- **Photographer Upload API specification** — full workflow design including authentication, WoRMS ID validation, S3 image storage, and metadata ingest
- **Working Next.js prototype** — fully functional frontend and API, running on mock data that maps 1:1 to the real schema
- **Production migration plan with financial modeling** — cost estimates, infrastructure recommendations, and phased rollout plan for NatGeo's engineering team

---

## Technical Highlights

**Taxonomy-first data model**  
Species data is stored once, keyed by WoRMS AphiaID — the globally recognized marine species identifier. Photos reference species through foreign keys. Taxonomy is never duplicated, never inconsistent.

**Combined search ranking**  
Search results are ranked using two signals simultaneously: linguistic trigram similarity (handles typos and partial matches) and biological taxonomic proximity (a search for "whale" surfaces other cetaceans before unrelated species). This mirrors PostgreSQL's `pg_trgm` extension behavior, implemented in TypeScript for the prototype.

**Cascading taxonomy filters**  
The filter system enforces valid biological relationships — selecting a Family only shows Genera that exist within it, selecting a Genus only shows Species within that Genus. Invalid combinations are structurally impossible. This was designed to match NatGeo's FAIR data principles.

**Pending vs applied filter state**  
The gallery only updates when a user explicitly clicks Apply — not on every dropdown change. This is intentional: with a large dataset, firing a query on every selection change would be expensive and disorienting. The state management layer maintains a `pendingFilters` object separate from `appliedFilters`.

**WoRMS API integration**  
The photographer upload system validates every WoRMS ID against the live WoRMS REST API if it isn't found in the local database. This means the system works with any valid marine species, not just the seeded ones — the database grows as photographers upload.

**JWT authentication with no external dependencies**  
The photographer portal uses a custom JWT implementation built on Node's native `crypto` module. No external auth libraries. Credentials are hashed and stored separately from the main application database.

---

## Stack

`Next.js 14` · `TypeScript` · `PostgreSQL` (schema) · `AWS S3` (storage) · `WoRMS REST API` · `JWT` · `styled-jsx`

---

## Team

Built by a 180 Degrees Consulting team at the University of Michigan. 180DC is the world's largest student-run consultancy, partnering with nonprofits and social enterprises globally.

The team included two members with development experience and several with no coding background — the non-technical members produced the Figma designs, financial modeling, migration planning, and API workflow documentation that made the technical implementation possible.
