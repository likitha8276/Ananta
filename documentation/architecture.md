# ANANTA — System Architecture

## 1. Overview

ANANTA is an interactive, source-grounded knowledge exploration platform for Hindu scriptures, mythology, cosmology, philosophy and traditions.

Knowledge is represented as an interconnected network of entities, relationships, events, narratives and scriptural sources. Every published claim is traceable to a passage in a stated edition of a text.

## 2. Architectural Vision

The system is a modular monolith with six major components:

1. **Frontend:** interactive interface for exploring the ANANTA universe.
2. **Backend:** REST APIs and business logic.
3. **Database:** PostgreSQL storage for entities, relationships, events and sources.
4. **Knowledge graph layer:** relationship traversal (lineage, rebirth chains, connection paths) implemented in the backend on top of PostgreSQL. There is no separate graph database in the initial architecture.
5. **Source management:** scriptures, editions, passages and citations.
6. **Editorial system:** content creation, review and verification.

## 3. Technology Stack

| Component | Technology |
|---|---|
| Frontend | React, TypeScript |
| Styling | Tailwind CSS |
| Graph visualization | React Flow (trees, ego networks); D3 only where needed |
| Backend | Java 21, Spring Boot |
| Persistence | Spring Data JPA for CRUD; jOOQ or native SQL for recursive graph queries |
| Database | PostgreSQL (extensions: `pg_trgm`, `unaccent`, `ltree`) |
| Migrations | Flyway |
| Search | PostgreSQL full-text search with a transliteration-normalised key |
| Authentication | Spring Security (editorial/admin access) |
| Backend tests | JUnit, Testcontainers |
| Frontend tests | Vitest, React Testing Library, Playwright |
| CI | GitHub Actions |
| Containers | Docker, Docker Compose |
| Version control | Git, GitHub |
| Deployment | Docker on a managed host (Render, Railway or Fly.io; to be chosen) |
| Media storage | S3-compatible storage (later, when illustrations are added) |

Not used initially: Neo4j, Elasticsearch, Kafka, microservices, GraphQL, Kubernetes.

## 4. Core Architecture Principles

- Modular monolith for initial development.
- PostgreSQL is the single source of truth.
- The system stores **claims made by sources**, not unqualified facts.
- Every published assertion must have at least one verified citation.
- Conflicting scriptural accounts are preserved independently and never merged into one chronology.
- Entities and relationship types are data, so new ones need no code changes.
- Roles (for example "Indra of a Manvantara") are modelled separately from the persons who hold them.
- Content history is traceable through revisions and superseded assertions.
- Graph visualization is progressive: expand-on-demand, never the whole graph at once.

## 5. Core Data Domains

The **sourced assertion** is the central record. Relationships, birth chains, event outcomes and category-of-being statements are all assertions, and each attaches to citations, a tradition, a source class and a verification status.

- Knowledge entities
- Entity names and aliases
- Relationship types (controlled vocabulary)
- **Sourced assertions** and citations
- Scriptures, editions, divisions and passages
- Translations
- Events and participants
- Traditions and interpretations
- Time spans and placements
- Editorial verification and revision history

## 6. Development Strategy

ANANTA is developed incrementally, one working slice at a time.

**Phase 1:** Repository and development environment setup.

**Phase 1.5:** Hand-model the first content slice (Jaya and Vijaya's births) in a spreadsheet to validate the data model before writing SQL.

**Phase 2:** Database schema (entities, assertions, citations, scriptures, editions, passages) with Flyway migrations and a data importer.

**Phase 3:** Spring Boot backend and REST APIs (profile, relations, life-chain, lineage, search).

**Phase 4:** Editorial workflow: add, review and verify claims; admin authentication.

**Phase 5:** React frontend and entity exploration.

**Phase 6:** Interactive knowledge graph and narrative exploration.

**Phase 7:** Testing, deployment and continuous content expansion.

## 7. MVP Scope

**In scope:** the vertical slice from creation accounts through Brahma, the Prajapatis and Kashyapa's family to Jaya and Vijaya's three births; entity profiles; life-chain and lineage views; story reader; search; a claim-entry form that requires a citation.

**Out of scope for the MVP:** universe map, full timeline, user accounts and bookmarks, native apps, additional Puranas beyond the slice.

## 8. Long-Term Direction

ANANTA aims to become a comprehensive digital knowledge universe where every entity, event, relationship and narrative can be explored through its scriptural, philosophical and traditional context. Scale is reached through bulk import and a review queue, with an optional Neo4j read-side projection added only if graph queries measurably outgrow PostgreSQL.

*ANANTA — Explore the Infinite Universe.*
