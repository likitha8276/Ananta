# ANANTA

**Explore the Infinite Universe.**

ANANTA is a source-cited knowledge graph of Hindu cosmology and mythology. Instead of one "official" story, it stores **claims made by specific texts**, so you can follow a character across births, see their family, and compare what different scriptures say, with the verse behind every fact.

> **Status: early MVP (in progress).** See [What works today](#what-works-today).

<!-- Add a screenshot or GIF here once the first page works -->
<!-- ![ANANTA screenshot](docs/screenshot.png) -->

---

## Why this exists

Most mythology sites retell one blended version of a story with no sources. ANANTA does the opposite:

- Every fact is a **claim** tied to a scripture, edition and verse.
- When texts disagree, **both accounts are kept**, not merged.
- Every character, relationship and story is clickable and leads somewhere else.

Example the MVP is built around: Jaya and Vijaya, and their three births, traced from Vaikuntha to their deaths, with a citation on every step.

## Principles

1. **Claims, not facts.** The data model stores what a source says, not what is "true".
2. **No invented citations.** Every verse reference is checked against a real edition by a human.
3. **Plurality over harmony.** Differences between traditions are shown side by side.
4. **Data-driven.** New characters and stories are added through data, not code changes.
5. **Start small.** One fully working slice before expanding the universe.

## What works today

- [x] Architecture and data model designed
- [x] Claims spreadsheet template for hand-modelling the first slice
- [ ] Database schema (Flyway migration)
- [ ] Importer for the claims data
- [ ] Backend API (character, relations, life-chain, lineage)
- [ ] Frontend (profile, story reader, relationship graph)
- [ ] Admin form (adds a claim only with a citation)
- [ ] Deployed demo

## MVP scope

**The vertical slice:** Creation accounts, Brahma, the Prajapatis, Kashyapa's family, then Jaya and Vijaya's three births (Hiranyaksha and Hiranyakashipu, Ravana and Kumbhakarna, Shishupala and Dantavakra) with the avatar events connected to them.

**Out of scope for the MVP:** universe map, full timeline, user accounts, mobile app.

## Tech stack

| Layer | Choice |
|---|---|
| Frontend | React, TypeScript, Tailwind CSS, React Flow |
| Backend | Java 21, Spring Boot, Spring Data JPA |
| Database | PostgreSQL, Flyway migrations |
| Search | PostgreSQL full-text search |
| Tests | JUnit, Testcontainers, Vitest, Playwright |
| Deployment | Docker, GitHub Actions |

**Why PostgreSQL and not Neo4j?** At this scale, relationship tables with recursive queries are enough, and PostgreSQL gives better support for citations, versioning and review workflows. Neo4j may be added later as a read-only projection if graph queries become slow.

## Data model (short version)

```
Entity ──subject── Assertion ──object── Entity
                      │
                      ├── Citation → Passage → Scripture / Edition
                      ├── tradition, source class, confidence
                      └── status: DRAFT → SOURCED → VERIFIED
```

Source classes: direct text, traditional commentary, regional retelling, later literary work, modern academic interpretation.

## Getting started

```bash
git clone https://github.com/likitha8276/ananta.git
cd ananta
docker compose up
```

> Setup instructions will be completed as the project is built.

## Project structure (planned)

```
ananta/
├── backend/        Spring Boot application
├── frontend/       React + TypeScript app
├── data/           Claims CSV files and import scripts
├── docs/           Architecture notes and diagrams
├── CONTENT_POLICY.md
└── docker-compose.yml
```

## Content policy

- Use only passages that have been opened and read in a stated edition.
- Record edition and numbering system with every citation (verse numbers differ between editions).
- Paraphrase instead of copying copyrighted translations; prefer public-domain translations.
- Describe traditions neutrally and attribute accounts to their source.
- Entries stay `DRAFT` until a second review marks them `VERIFIED`.

## Roadmap

| Phase | Focus |
|---|---|
| 1 | MVP: Jaya/Vijaya slice, profiles, life-chain, story reader, admin form |
| 2 | Connection finder, compare versions, avatar and event explorers, timeline |
| 3 | Universe map, personal exploration paths, advanced search |
| Later | More Puranas and traditions, optional Neo4j projection, public API |

## Contributing

Not open yet. Once the MVP is stable, corrections with a source citation will be welcome through issues.

## Licence

To be decided. Source texts keep their own licences; see the edition table in the data directory once added.
