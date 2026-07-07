# test atelier

## Contexte et objectifs

Le projet « Sinistres Demo » est une application de démonstration locale destinée à illustrer la gestion simplifiée de demandes sinistres dans un contexte d’assurance complémentaire santé.

L’application permet :
- la consultation et la gestion CRUD de demandes sinistres ;
- la recherche multicritères et le filtrage métier ;
- l’affichage d’un dashboard statistique ;
- l’exposition d’une API REST JSON ;
- l’exécution locale via Docker Compose.

Chaque demande contient des informations métier liées :
- à une synthèse de mail ;
- à des données assuré ;
- à des métadonnées de pièces jointes ;
- à des indicateurs d’urgence et de sentiment ;
- à un statut de traitement.

Le périmètre vise un environnement de démonstration simple :
- sans authentification ;
- sans intégration SI ;
- sans gestion documentaire réelle ;
- sans workflow automatisé.

La stack technique repose sur :
- Node.js 20 ;
- Express 4.x ;
- PostgreSQL 16 ;
- HTML/CSS/JavaScript vanilla ;
- Chart.js 4.4 ;
- Docker et Docker Compose.

## Architecture globale du système

L’architecture du système est organisée autour :
- d’un backend Express exposant une API REST JSON ;
- d’une base PostgreSQL assurant la persistance ;
- d’un frontend web vanilla consommant les endpoints API ;
- d’un environnement Docker local orchestré par Docker Compose.

Vue macro des responsabilités :
- Le frontend back-office fournit les écrans de consultation, formulaire CRUD et dashboard.
- L’API REST centralise les échanges de données et les contrats JSON.
- Le module de validation garantit la cohérence métier des données entrantes.
- Le module de persistance exécute les opérations SQL et les requêtes statistiques.
- Le module de recherche applique les filtres métier et la recherche texte.
- Le dashboard statistique expose les indicateurs agrégés nécessaires aux graphiques.
- Les modules d’initialisation et Docker assurent un démarrage local reproductible.
- Le module de santé permet la supervision minimale de l’application.

Les spécifications détaillées de chaque module seront produites ultérieurement dans des documents dédiés.

## Inventaire des modules validés

| Module | Description | Statut conception |
|---|---|---|
| M-01 — Métadonnées et référentiels | Centralisation des statuts, labels UI et énumérations métier exposés via l’API. | En attente de détail |
| M-02 — Configuration et exécution Docker | Configuration Docker et Docker Compose pour l’environnement local. | En attente de détail |
| M-03 — Persistance PostgreSQL | Gestion PostgreSQL, requêtes SQL CRUD et statistiques sans ORM. | En attente de détail |
| M-04 — Initialisation et seed de démonstration | Initialisation automatique de la base et chargement des données de démonstration. | En attente de détail |
| M-05 — Validation métier et normalisation des données | Validation applicative et normalisation des données entrantes. | En attente de détail |
| M-06 — API REST JSON | Exposition des endpoints REST et gestion des réponses JSON. | En attente de détail |
| M-07 — Santé applicative et observabilité | Endpoint de health check et logs applicatifs. | En attente de détail |
| M-08 — Gestion des demandes sinistres | Gestion CRUD des demandes sinistres. | En attente de détail |
| M-09 — Recherche et filtrage métier | Recherche texte et filtres métier temps réel. | En attente de détail |
| M-10 — Dashboard statistique | Calcul et affichage des statistiques et KPI. | En attente de détail |
| M-11 — Interface web back-office | Frontend HTML/CSS/JS pour les écrans utilisateurs. | En attente de détail |

## État d'avancement de la conception

- Modules détaillés : 0 / 11
- Modules validés au niveau squelette : 11 / 11

État actuel :
- Les modules fonctionnels et techniques ont été identifiés et validés.
- Les responsabilités macro sont définies.
- Les spécifications détaillées par module restent à produire.

Prochaines actions :
1. Produire les spécifications détaillées module par module.
2. Définir les contrats internes et dépendances techniques.
3. Détailler les flux frontend/backend.
4. Structurer les futurs tickets et lots de développement.
5. Finaliser les règles métier de validation et de filtrage.

## Ordre de développement préliminaire

| # | Réf | Module | Chemin | Pourquoi cet ordre |
|---|---|---|---|---|
| 1 | M-02 | Configuration et exécution Docker | `/docs/modules/configuration-et-execution-docker.md` | Mettre en place l’environnement d’exécution local dès le départ. |
| 2 | M-03 | Persistance PostgreSQL | `/docs/modules/persistance-postgresql.md` | Fournir la base de données et les primitives SQL nécessaires aux autres modules. |
| 3 | M-04 | Initialisation et seed de démonstration | `/docs/modules/initialisation-et-seed-de-demonstration.md` | Garantir un environnement de données exploitable immédiatement. |
| 4 | M-01 | Métadonnées et référentiels | `/docs/modules/metadonnees-et-referentiels.md` | Centraliser les valeurs métier utilisées dans toute l’application. |
| 5 | M-05 | Validation métier et normalisation des données | `/docs/modules/validation-metier-et-normalisation-des-donnees.md` | Sécuriser les flux de données avant exposition API. |
| 6 | M-06 | API REST JSON | `/docs/modules/api-rest-json.md` | Exposer les contrats backend nécessaires au frontend. |
| 7 | M-07 | Santé applicative et observabilité | `/docs/modules/sante-applicative-et-observabilite.md` | Ajouter la supervision minimale et les contrôles techniques. |
| 8 | M-08 | Gestion des demandes sinistres | `/docs/modules/gestion-des-demandes-sinistres.md` | Implémenter les opérations métier CRUD principales. |
| 9 | M-09 | Recherche et filtrage métier | `/docs/modules/recherche-et-filtrage-metier.md` | Ajouter les capacités avancées de consultation et filtrage. |
| 10 | M-10 | Dashboard statistique | `/docs/modules/dashboard-statistique.md` | Construire les indicateurs et statistiques après disponibilité des données métier. |
| 11 | M-11 | Interface web back-office | `/docs/modules/interface-web-back-office.md` | Finaliser l’expérience utilisateur complète sur une base API stabilisée. |

## Comment lire cette documentation

Cette documentation est organisée :
- par module fonctionnel et technique ;
- par ordre de dépendance de développement ;
- par responsabilités métier clairement séparées.

La colonne `#` du tableau d’ordre de développement indique la priorité préliminaire d’implémentation :
- `1` = premier module à développer ;
- les numéros suivants représentent l’ordre recommandé de construction du système.

Les identifiants `M-XX` sont des références stables de modules indépendantes de l’ordre de développement.

Les documents de spécification détaillés des modules seront ajoutés progressivement dans le répertoire `/docs/modules/`.

---

Document vivant de conception et de pilotage projet, enrichi à chaque validation de module et mise à jour des spécifications.