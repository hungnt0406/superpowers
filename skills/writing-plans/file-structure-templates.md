# File Structure Templates

Illustrative starting points for **greenfield** projects. They exist to help
you propose a sane top-level layout when there's no existing structure to
follow.

**How to use these:**
- Pick the closest archetype, then **adapt** — rename, drop, or add directories
  to fit the actual language, framework, and domain.
- Never copy a tree verbatim into a plan. A layout you don't use is clutter.
- Prefer grouping by feature/responsibility over technical layer. Files that
  change together should live together.
- Keep each unit small and reviewable. If a directory is accumulating
  unrelated files, split it.
- For an **existing** codebase, ignore these and follow established patterns.

Common to all: `README.md`, `LICENSE`, `.gitignore`, `.env.example`,
`CHANGELOG.md` at the root as appropriate.

---

## Data Science / ML

For experimentation, pipelines, and analysis work.

```
project-name/
├── data/
│   ├── raw/            # immutable source data
│   ├── processed/      # cleaned/derived data
│   └── exports/        # outputs for downstream use
├── notebooks/          # exploratory analysis (numbered, one concern each)
├── src/
│   ├── data/           # loading, cleaning, feature engineering
│   ├── models/         # training, evaluation, inference
│   └── viz/            # plotting/reporting helpers
├── pipelines/          # orchestrated end-to-end runs
├── config/             # experiment/config files (params, paths)
├── tests/
├── reports/            # generated figures, metrics, writeups
└── models/             # serialized/trained artifacts
```

Notes: keep `data/raw` read-only. Notebooks are for exploration; promote
reusable logic into `src/`.

---

## Backend / Service

For APIs and server-side applications. Group by feature module, not by
technical layer.

```
project-name/
├── src/
│   ├── modules/            # feature modules — each self-contained
│   │   └── <feature>/      # router + service + model + schema together
│   ├── core/               # app bootstrap, DI, cross-cutting concerns
│   ├── middleware/         # auth, logging, error handling
│   ├── db/                 # migrations, connection, base models
│   └── config/             # settings, env loading
├── tests/
│   ├── unit/
│   └── integration/
├── scripts/                # ops/dev scripts (seed, migrate helpers)
└── docs/
    ├── architecture/
    └── decisions/          # ADRs
```

Notes: a feature's route, business logic, and data access live together under
`modules/<feature>/`, so a change touches one directory. `core/` holds only
what every module depends on.

---

## Frontend / Client

For SPA / component-based web apps. Colocate components with their styles,
tests, and hooks.

```
project-name/
├── src/
│   ├── features/           # feature-based: each owns its UI + state + logic
│   │   └── <feature>/      # components, hooks, api, tests together
│   ├── components/         # shared, reusable presentational components
│   ├── hooks/              # shared hooks
│   ├── lib/                # api client, utils, formatters
│   ├── routes/             # route/page definitions
│   ├── styles/             # global styles, theme/tokens
│   └── assets/             # images, fonts, icons
├── tests/
│   ├── unit/
│   └── e2e/
└── public/                 # static served files
```

Notes: `features/<feature>/` is the default home for code; only promote to
`components/`, `hooks/`, or `lib/` once something is genuinely shared.

---

## Full-Stack (Monorepo)

For a combined client + server codebase with shared code.

```
project-name/
├── apps/
│   ├── web/                # frontend app (see Frontend template)
│   └── api/                # backend service (see Backend template)
├── packages/
│   ├── shared/             # types, validation schemas shared client+server
│   ├── ui/                 # shared component library
│   └── config/             # shared lint/tsconfig/build config
├── scripts/
├── docs/
│   ├── architecture/
│   └── decisions/
└── tests/                  # cross-app / e2e
```

Notes: each `apps/*` follows its own template above. `packages/shared` is the
key win — a type or contract defined once, used on both sides, so client and
server can't drift.
