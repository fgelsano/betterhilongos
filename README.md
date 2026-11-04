# 🏛️ Better Hilongos

A community-led, open-source portal that makes the government of the **Municipality of Hilongos, Leyte** accessible, transparent, and user-friendly.

- **Website:** https://betterhilongos.org
- **Repository:** https://github.com/fgelsano/betterhilongos

> **Status:** early setup. The portal framework is in place; Hilongos-specific content (officials, barangays, departments, services, logos) is still being added. See [Project Status](#project-status).

---

## Portal Features

Better Hilongos provides residents with:

- **Public Services Directory**: Guide to municipal services with requirements, fees, and step-by-step processes
- **Legislative Portal**: Ordinances, resolutions, and executive orders of the Sangguniang Bayan
- **Transparency Dashboard**: Financial data, procurement bids, and infrastructure projects
- **Government Directory**: Contact information for municipal departments, barangays, and officials
- **Multi-language Support**: English and Filipino

Feature modules can be switched on or off in [`config/lgu.config.json`](./config/lgu.config.json) under `features`. Currently enabled: OpenLGU, transparency, and statistics. Tourism is disabled.

---

## Project Status

| Area                 | Status                                                   |
| -------------------- | -------------------------------------------------------- |
| Portal configuration | Done: Hilongos identity, domain, and filters are set     |
| Departments          | To do: `src/data/directory/departments.json` is empty   |
| Barangays            | To do: `src/data/directory/barangays.json` is empty     |
| Elected officials    | To do: `executive.json` and `legislative.json` are empty |
| Services & charter   | To do: still contains placeholder content from the template |
| Statistics, About    | To do: still contains placeholder content from the template |
| Logos & branding     | To do: temporary logos from the template are in use      |
| Legislative database | To do: Cloudflare D1 is not yet set up for Hilongos      |

---

## Technical Stack

- **Frontend**: React 19, Vite, TypeScript (strict mode)
- **Styling**: Tailwind CSS v4
- **Design System**: [@bettergov/kapwa](https://www.npmjs.com/package/@bettergov/kapwa)
- **Backend**: Cloudflare Pages Functions (TypeScript)
- **Data**: Structured JSON, organized by category
- **Search**: Meilisearch with Fuse.js fuzzy search
- **Localization**: i18next (English and Filipino)
- **Maps**: Leaflet
- **Testing**: Vitest (unit) and Playwright (E2E)
- **Code Quality**: ESLint, Prettier, Husky pre-commit hooks, commitlint

---

## Getting Started

### Prerequisites

- Node.js (see [`.nvmrc`](./.nvmrc))
- Python 3 (used by the data merge scripts)

### Run locally

```bash
git clone https://github.com/fgelsano/betterhilongos
cd betterhilongos
cp .env.example .env     # then fill in your Meilisearch settings
npm install
npm run dev
```

The portal is served at `http://localhost:5173`.

### Useful commands

```bash
npm run dev            # Start the development server
npm run build          # Merge data, type-check, and build for production
npm run test           # Unit tests (Vitest)
npm run test:e2e       # End-to-end tests (Playwright)
npm run lint           # ESLint (max warnings = 0)
npm run format         # Format with Prettier
```

---

## Configuration

All Hilongos-specific identity lives in [`config/lgu.config.json`](./config/lgu.config.json). The app reads it at runtime, so no LGU names are hardcoded in source.

| Field                | Value                              |
| -------------------- | ---------------------------------- |
| `lgu.name`           | Hilongos                           |
| `lgu.fullName`       | Municipality of Hilongos           |
| `lgu.province`       | Leyte                              |
| `lgu.region`         | Eastern Visayas (VIII)             |
| `lgu.type`           | municipality                       |
| `portal.name`        | BetterHilongos                     |
| `portal.baseUrl`     | https://betterhilongos.org         |

Other places that hold Hilongos content:

| File                                      | What to maintain                              |
| ----------------------------------------- | --------------------------------------------- |
| `src/data/directory/departments.json`     | Municipal departments and offices             |
| `src/data/directory/barangays.json`       | Barangay profiles and officials               |
| `src/data/directory/executive.json`       | Mayor, Vice Mayor, and executive officials    |
| `src/data/directory/legislative.json`     | Sangguniang Bayan members                     |
| `src/data/services/categories/*.json`     | Public services by category                   |
| `src/data/citizens-charter/`              | Service requirements, fees, and client steps  |
| `src/data/statistics/`                    | Municipal demographics and indicators         |
| `public/locales/{en,fil}/common.json`     | UI text strings                               |
| `public/logos/`                           | Logos and favicons                            |

The service directory is split into category files. Run `python3 scripts/merge_services.py` (or `npm run build`) to merge them before using the app.

---

## Project Structure

```
betterhilongos/
├── config/                  # lgu.config.json: Hilongos identity and feature flags
├── db/                      # D1 database migrations
├── docs/                    # Architecture, API, and integration guides
├── e2e/                     # End-to-end tests
├── functions/               # Cloudflare Pages Functions (API endpoints)
├── public/                  # Static assets (locales, logos)
├── scripts/                 # Automation and maintenance scripts
└── src/
    ├── components/          # Reusable UI components
    ├── data/                # Structured data (directory, services, statistics, ...)
    ├── hooks/               # Custom React hooks
    ├── i18n/                # Internationalization setup
    ├── lib/                 # Utilities, including the LGU config loader
    ├── pages/               # Route-level pages (government, services, legislation,
    │                        #   transparency, statistics, about, contribute, ...)
    └── types/               # TypeScript type definitions
```

See [`ARCHITECTURE.md`](./ARCHITECTURE.md) for system design details.

---

## Deployment

The portal is built for **Cloudflare Pages**:

- **Frontend**: Vite build output in `dist/`
- **Backend**: Cloudflare Pages Functions in `functions/`
- **Database**: Cloudflare D1 for legislative data
- **Search**: a Meilisearch instance

> **Heads up:** `wrangler.jsonc` and the deploy workflow still reference the BetterLB Cloudflare project and D1 database. Before the first deployment, create Hilongos' own Cloudflare Pages project and D1 database, and update those references. See [`ARCHITECTURE.md`](./ARCHITECTURE.md#deployment) and [`docs/`](./docs/).

---

## Contributing

You don't need to be a developer to help.

1. **Residents and non-developers**: suggest new services or fix outdated information by opening a [GitHub issue](https://github.com/fgelsano/betterhilongos/issues).
2. **Developers**: pick up an open issue. See [`CONTRIBUTING.md`](./CONTRIBUTING.md) for the workflow.
3. **Data auditors**: help verify submitted information against official sources.
4. **Translators**: improve the Filipino and other translations in `public/locales/`.

Development conventions:

- Follow [Conventional Commits](https://www.conventionalcommits.org/) (enforced by commitlint)
- ESLint, Prettier, and tests run on every pull request

Please read the [Code of Conduct](./CODE_OF_CONDUCT.md) before participating.

---

## Credits

Better Hilongos is built on the [BetterLB](https://github.com/BetterLosBanos/betterlb) portal template, which is itself part of the [BetterGov.ph](https://bettergov.ph) community effort.

## License and Data Sources

### Code License

Released under [Creative Commons CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/). The work is dedicated to the public domain and can be freely used, modified, and distributed.

### Data Attribution

| Data Source                                             | Type                                         |
| ------------------------------------------------------- | -------------------------------------------- |
| Municipality of Hilongos                                | Official government data, services directory |
| Philippine Government Electronic Procurement (PhilGEPS) | Procurement bids and awards                  |
| Department of Budget and Management (DBM)               | Financial releases                           |
| Department of Public Works and Highways (DPWH)         | Infrastructure projects                      |

Data is presented as-is and may not reflect the most current information. Always verify with official LGU sources.
