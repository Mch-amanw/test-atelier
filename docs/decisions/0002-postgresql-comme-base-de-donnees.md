# ADR-0002 : Utilisation de PostgreSQL 16

## Contexte
L’application nécessite une persistance simple des demandes sinistres et des statistiques associées.

## Décision
La persistance des données repose sur PostgreSQL 16.

## Conséquences
- Compatibilité avec les requêtes SQL relationnelles et les filtres métiers.
- Possibilité d’exécuter localement la base via Docker Compose.
- Initialisation idempotente de la structure de données.