# ADR-0001 : Architecture monolithique Node.js et Express

## Contexte
Le projet Sinistres Demo est une application de démonstration destinée à illustrer la gestion de demandes sinistres avec un périmètre fonctionnel limité et un fonctionnement local.

## Décision
Le projet adopte une architecture monolithique basée sur Node.js 20 et Express 4.x avec un frontend statique HTML/CSS/JS servi par le backend.

## Conséquences
- Simplification du développement et de l’exécution locale.
- Réduction de la complexité d’infrastructure.
- Absence de séparation microservices.
- Dépendance unique à une application backend Express.