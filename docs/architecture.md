# Architecture — test atelier

## Type de dépôt
- Monorepo applicatif simple.
- Architecture monolithique.

## Structure cible
```text
sinistres-demo/
├── app/
│   ├── db/
│   ├── public/
│   ├── Dockerfile
│   └── docker-compose.yml
├── docs/
│   └── decisions/
├── tests/
├── .github/
│   └── workflows/
└── README.md
```

## Rôle des composants
| Chemin | Responsabilité |
|---|---|
| `app/` | Application Node.js principale |
| `app/db/` | Scripts SQL d’initialisation et de données de démonstration |
| `app/public/` | Frontend statique HTML/CSS/JS |
| `app/Dockerfile` | Construction du conteneur applicatif |
| `app/docker-compose.yml` | Orchestration locale application + PostgreSQL |
| `docs/` | Documentation projet |
| `docs/decisions/` | Documentation des décisions d’architecture |
| `tests/` | Emplacement réservé aux tests |
| `.github/workflows/` | Pipelines d’automatisation et workflows |
| `README.md` | Documentation d’utilisation du projet |

## Conventions
- Application organisée autour d’un backend Express unique.
- Frontend statique servi par l’application backend.
- Absence de séparation microservices.
- Aucun package frontend dédié.
- Aucun package partagé.
- Architecture volontairement simple pour démonstration locale.
- Base PostgreSQL isolée dans un service Docker dédié.

## Références
- `docs/decisions/`
- `docs/technical.md`