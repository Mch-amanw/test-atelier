# ADR-0003 : Exécution locale via Docker Compose

## Contexte
Le projet doit être facilement exécutable dans un contexte de démonstration locale.

## Décision
L’application et PostgreSQL sont orchestrés via Docker et Docker Compose.

## Conséquences
- Démarrage simplifié du projet.
- Uniformisation de l’environnement local.
- Absence de déploiement cloud ou Kubernetes prévu.