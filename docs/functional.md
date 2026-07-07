# Spécification fonctionnelle — test atelier

## Contexte et objectifs
Le projet « Sinistres Demo » est une application de démonstration ultra simple destinée à illustrer la gestion de demandes sinistres, notamment les arrêts de travail et les pièces jointes, dans un contexte d'assurance complémentaire santé.

Objectifs du projet :
- Proposer une interface web CRUD pour consulter, créer, modifier et supprimer des demandes.
- Afficher un dashboard de répartition statistique des demandes selon les issues de traitement, l’urgence et le sentiment.
- Fonctionner localement via Docker.
- Simuler un back-office minimal de traitement de mails sinistres.

Chaque demande représente le résultat d’une analyse comprenant :
- une synthèse de mail,
- une urgence,
- un sentiment,
- des métadonnées de pièces jointes,
- des informations d’identité assuré,
- un statut d’issue de traitement.

## Utilisateurs et rôles
| Rôle | Description |
|---|---|
| Utilisateur de démonstration back-office | Consulte, filtre, crée, modifie et supprime des demandes sinistres ; consulte les statistiques du dashboard |

Aucune authentification ni gestion de rôles n’est prévue.

## Périmètre
### Inclus
- Interface web CRUD.
- Consultation de la liste des demandes.
- Création de demandes.
- Modification de demandes.
- Suppression de demandes.
- Dashboard statistique.
- API REST JSON.
- Recherche multicritères.
- Filtres métier.
- Initialisation automatique de la base.
- Chargement de données de démonstration.
- Exécution locale via Docker Compose.

### Exclus
- Authentification.
- Autorisation.
- Upload réel de pièces jointes.
- Workflow automatisé.
- Envoi d’e-mails.
- Transmission vers un système de gestion.
- Reconnaissance adhérent.
- Intégration CRM.
- Intégration SI de gestion.
- Intégration avec des systèmes tiers.
- Déploiement cloud ou production.

## Fonctionnalités principales
### Gestion des demandes
- Consultation de la liste des demandes.
- Recherche texte multicritères.
- Filtres temps réel.
- Consultation du détail d’une demande.
- Création via formulaire complet.
- Modification partielle des demandes.
- Suppression des demandes.

### Dashboard statistique
Affichage des KPI :
- Total des demandes.
- Compteur par issue de traitement hors statut `nouvelle`.

Graphiques :
1. Donut de répartition par issue de traitement.
2. Histogramme de répartition par urgence.
3. Histogramme de répartition par sentiment.

### Health check
- Vérification de disponibilité PostgreSQL.
- Exposition d’un endpoint de santé.

### Initialisation automatique
- Création idempotente de la table.
- Chargement automatique du seed si la table est vide et que `RUN_SEED` n’est pas égal à `false`.

## Parcours utilisateurs clés
### Consultation et filtrage des demandes
1. L’utilisateur ouvre la page liste.
2. Les demandes sont triées par date décroissante.
3. L’utilisateur applique des filtres :
   - recherche texte,
   - statut,
   - urgence,
   - traitement,
   - présence de pièce jointe.
4. Les résultats sont mis à jour en temps réel.

### Création d’une demande
1. L’utilisateur ouvre la modal CRUD.
2. Il renseigne les champs disponibles.
3. Si aucun statut n’est fourni, le statut `nouvelle` est appliqué.
4. La demande est enregistrée.

### Modification d’une demande
1. L’utilisateur ouvre une demande existante.
2. Il modifie uniquement certains champs.
3. Seuls les champs envoyés sont modifiés.

### Suppression d’une demande
1. L’utilisateur déclenche la suppression.
2. La demande est supprimée définitivement.

### Consultation du dashboard
1. L’utilisateur ouvre le dashboard.
2. Les KPI sont calculés.
3. Les graphiques affichent les répartitions statistiques.

## Statuts, énumérations et libellés
### Statuts de traitement
| Code | Libellé UI | Rôle / description métier |
|---|---|---|
| `nouvelle` | Nouvelle | Demande en attente — visible en liste/CRUD uniquement |
| `demande_complete_traitee` | Demande complète traitée | Dossier complet, traité |
| `sans_piece` | Traitée sans pièce | Demande sans PJ (ex. question remboursement) |
| `mail_piece_complementaire` | Mail pièce complémentaire | PJ manquante, mail envoyé au client |
| `transmis_gestion` | Transmise à la gestion | Dossier transmis au back-office |
| `adherent_introuvable_gestion` | Adhérent introuvable — gestion | N° adhérent non reconnu, escalade gestion |

### Énumérations
| Champ | Valeurs autorisées |
|---|---|
| `urgence` | `basse`, `normale`, `haute` |
| `sentiment` | `neutre`, `inquiet`, `urgent`, `confus` |

## Règles de gestion
### Règle — Statut par défaut
- À la création, le statut par défaut est `nouvelle` si aucun statut n’est fourni.

### Règle — Validation des statuts
- Seuls les statuts listés dans l’énumération officielle sont acceptés.
- La validation est réalisée côté API.

### Règle — Filtre traité
- `traite=true` exclut les demandes avec statut `nouvelle`.
- `traite=false` retourne uniquement les demandes avec statut `nouvelle`.

### Règle — Statistiques par statut
- Les statistiques `par_statut` comptent uniquement les demandes hors statut `nouvelle`.
- Les statuts absents doivent apparaître avec un compteur `0`.

### Règle — Statistiques urgence et sentiment
- Les statistiques `par_urgence` et `par_sentiment` incluent toutes les demandes.
- Les valeurs nulles sont regroupées sous :
  - « non renseignée » pour l’urgence,
  - « non renseigné » pour le sentiment.

### Règle — Tri des demandes
- Tri par `created_at DESC, id DESC`.

### Règle — Booléens API
Les booléens acceptent :
- `true`
- `false`
- `"true"`
- `"false"`
- `"1"`
- `"0"`

### Règle — Conversion des chaînes vides
- Les chaînes vides sont converties en `NULL`.

### Règle — Seed conditionnel
Le seed est exécuté uniquement si :
- `RUN_SEED !== 'false'`,
- et la table est vide.

## Données de référence
### Données de démonstration
Le seed contient 5 enregistrements couvrant chaque statut de traitement sauf `nouvelle`.

| Nom | Description | Statut |
|---|---|---|
| Marie Dupont | arrêt maladie complet | `demande_complete_traitee` |
| Luc Bernard | question sans PJ | `sans_piece` |
| Sophie Moreau | prolongation arrêt, PJ manquante | `mail_piece_complementaire` |
| Jean Petit | adhérent inconnu | `adherent_introuvable_gestion` |
| Claire Lemaire | transmise à la gestion | `transmis_gestion` |

## Contraintes fonctionnelles
- Application de démonstration uniquement.
- Fonctionnement local via Docker.
- Aucune authentification.
- Aucun upload réel de fichiers.
- Dashboard dépendant de Chart.js via CDN.
- Architecture monolithique.
- Aucun ORM.
- Aucun build frontend.
- Pas de pagination des résultats.

## Critères d’acceptation globaux
- CRUD opérationnel sur les demandes.
- Dashboard fonctionnel.
- Statistiques cohérentes.
- Validation stricte des statuts.
- Validation des énumérations.
- Filtres temps réel fonctionnels.
- API REST conforme aux codes HTTP définis.
- Health check PostgreSQL opérationnel.
- Seed automatique exécuté dans les conditions prévues.
- Les statistiques `par_statut` excluent le statut `nouvelle`.
- Les valeurs nulles sont regroupées correctement dans les statistiques.

## Limites connues et hors périmètre
- Pas de pagination sur `GET /api/demandes`.
- Pas de contraintes base de données sur les énumérations.
- Validation des énumérations uniquement au niveau applicatif.
- Le KPI « Total demandes traitées » affiche en réalité le total de toutes les demandes (`stats.total`).
- Chart.js est chargé depuis un CDN et nécessite une connexion Internet pour le dashboard.
- Aucun système d’authentification.
- Aucun système d’autorisation.
- Aucun workflow automatisé.
- Aucun déploiement cloud prévu.