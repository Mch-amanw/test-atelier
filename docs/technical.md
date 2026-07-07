# Spécification technique — test atelier

## Stack technique
| Couche | Technologie |
|---|---|
| Backend | Node.js 20 |
| Framework backend | Express 4.x |
| Base de données | PostgreSQL 16 |
| Frontend | HTML/CSS/JS vanilla |
| Librairie graphique | Chart.js 4.4 (CDN) |
| Conteneurisation | Docker |
| Orchestration locale | Docker Compose |

## Modèle de données
### Entité `demande`
| Champ | Type | Nullable | Défaut | Description métier |
|---|---|---|---|---|
| `id` | SERIAL | NON | auto | Identifiant unique |
| `created_at` | TIMESTAMPTZ | NON | `now()` | Date de création |
| `statut` | VARCHAR(50) | NON | `'nouvelle'` | Issue / état de traitement |
| `concerne_arret_travail` | BOOLEAN | OUI | — | La demande concerne un arrêt de travail |
| `urgence` | VARCHAR(20) | OUI | — | `basse`, `normale`, `haute` |
| `sentiment` | VARCHAR(20) | OUI | — | `neutre`, `inquiet`, `urgent`, `confus` |
| `synthese_mail` | TEXT | OUI | — | Synthèse du mail reçu |
| `nom` | VARCHAR(150) | OUI | — | Nom assuré |
| `prenom` | VARCHAR(150) | OUI | — | Prénom assuré |
| `numero_adherent` | VARCHAR(50) | OUI | — | Numéro adhérent |
| `nir` | VARCHAR(20) | OUI | — | Numéro de sécurité sociale |
| `date_naissance` | DATE | OUI | — | Date de naissance |
| `date_arret_travail` | DATE | OUI | — | Date d’arrêt de travail |
| `email` | VARCHAR(255) | OUI | — | Email |
| `telephone` | VARCHAR(30) | OUI | — | Téléphone |
| `pj_presente` | BOOLEAN | OUI | — | Une PJ est-elle jointe au mail |
| `pj_est_arret_travail` | BOOLEAN | OUI | — | La PJ est-elle un certificat d'arrêt |
| `pj_type_document` | VARCHAR(255) | OUI | — | Exemple : « Certificat médical », « Facture » |
| `pj_reference_cerfa` | VARCHAR(30) | OUI | — | Exemple : `CERFA-10170` |
| `pj_medecin` | VARCHAR(150) | OUI | — | Nom du médecin |
| `pj_employeur` | VARCHAR(150) | OUI | — | Nom employeur |

## API / contrats REST
### Général
- Base URL : `/api`
- Format : JSON
- Authentification : aucune

### GET `/api/meta`
Retourne les métadonnées des énumérations.

Réponse :
```json
{
  "statuts": [{ "value": "nouvelle", "label": "Nouvelle" }],
  "urgences": ["basse", "normale", "haute"],
  "sentiments": ["neutre", "inquiet", "urgent", "confus"]
}
```

Codes HTTP :
| Code | Description |
|---|---|
| `200` | Succès |

### GET `/health`
Health check PostgreSQL.

Codes HTTP :
| Code | Description | Réponse |
|---|---|---|
| `200` | PostgreSQL joignable | `{ "status": "ok" }` |
| `503` | Erreur PostgreSQL | `{ "status": "error", "message": "..." }` |

### GET `/api/stats`
Retourne les statistiques du dashboard.

Réponse :
```json
{
  "total": 5,
  "par_statut": [
    { "statut": "demande_complete_traitee", "label": "...", "count": 1 }
  ],
  "par_urgence": [{ "urgence": "haute", "count": 2 }],
  "par_sentiment": [{ "sentiment": "neutre", "count": 2 }]
}
```

Règles :
- `par_statut` exclut le statut `nouvelle`.
- Les statuts absents doivent avoir `count = 0`.
- `par_urgence` et `par_sentiment` incluent toutes les demandes.
- Les valeurs nulles sont regroupées sous « non renseignée » et « non renseigné ».

Codes HTTP :
| Code | Description |
|---|---|
| `200` | Succès |

### GET `/api/demandes`
Liste des demandes.

Tri :
- `created_at DESC`
- `id DESC`

Codes HTTP :
| Code | Description |
|---|---|
| `200` | Succès |

### GET `/api/demandes/:id`
Retourne le détail d’une demande.

Codes HTTP :
| Code | Description | Réponse |
|---|---|---|
| `200` | Succès | Objet demande |
| `404` | Introuvable | `{ "error": "Demande introuvable." }` |

### POST `/api/demandes`
Création d’une demande.

Headers :
| Nom | Valeur |
|---|---|
| `Content-Type` | `application/json` |

Codes HTTP :
| Code | Description |
|---|---|
| `201` | Demande créée |
| `400` | Statut invalide ou erreur de validation |

### PUT `/api/demandes/:id`
Modification partielle d’une demande.

Headers :
| Nom | Valeur |
|---|---|
| `Content-Type` | `application/json` |

Règles :
- Seuls les champs envoyés sont modifiés.

Codes HTTP :
| Code | Description |
|---|---|
| `200` | Mise à jour effectuée |
| `400` | Erreur validation |
| `404` | Introuvable |

### DELETE `/api/demandes/:id`
Suppression d’une demande.

Codes HTTP :
| Code | Description |
|---|---|
| `204` | Supprimée |
| `404` | Introuvable |

## Filtres et requêtes
### Paramètres GET `/api/demandes`
| Paramètre | Type | Description |
|---|---|---|
| `statut` | string | Filtre exact sur le statut |
| `q` | string | Recherche ILIKE sur nom, prénom, numéro adhérent, email, synthèse mail |
| `urgence` | string | Filtre exact |
| `sentiment` | string | Filtre exact ; non exposé dans l’UI liste |
| `traite` | `true`/`false` | Traitées vs en attente |
| `pj_presente` | `true`/`false` | Présence de pièce jointe |
| `concerne_arret_travail` | `true`/`false` | Filtre arrêt travail ; non exposé dans l’UI |

### Champs modifiables via API
| Champ |
|---|
| `statut` |
| `concerne_arret_travail` |
| `urgence` |
| `sentiment` |
| `synthese_mail` |
| `nom` |
| `prenom` |
| `numero_adherent` |
| `nir` |
| `date_naissance` |
| `date_arret_travail` |
| `email` |
| `telephone` |
| `pj_presente` |
| `pj_est_arret_travail` |
| `pj_type_document` |
| `pj_reference_cerfa` |
| `pj_medecin` |
| `pj_employeur` |

## Authentification et sécurité
- Aucune authentification.
- Aucune autorisation.
- Usage démonstration uniquement.
- Validation applicative des statuts.
- Validation applicative des énumérations.
- Les booléens acceptent :
  - `true`
  - `false`
  - `"true"`
  - `"false"`
  - `"1"`
  - `"0"`
- Les chaînes vides sont converties en `NULL`.
- Aucune contrainte base de données sur les valeurs d’énumération.

## Intégrations externes
| Intégration | Description |
|---|---|
| Chart.js CDN | Chargement des graphiques dashboard via CDN Internet |

## Configuration et déploiement
### Variables d’environnement
| Variable | Valeur par défaut | Description |
|---|---|---|
| `PORT` | `3080` | Port HTTP de l’application |
| `DATABASE_URL` | — | Connection string PostgreSQL prioritaire sur les variables individuelles |
| `POSTGRES_HOST` | `localhost` | Hôte PostgreSQL |
| `POSTGRES_PORT` | `5432` | Port PostgreSQL |
| `POSTGRES_USER` | `postgres` | Utilisateur PostgreSQL |
| `POSTGRES_PASSWORD` | `postgres` | Mot de passe PostgreSQL |
| `POSTGRES_DB` | `sinistres` | Nom base PostgreSQL |
| `POSTGRES_SSL` | `false` | Activation SSL pour base distante |
| `RUN_SEED` | `true` | Exécuter le seed si la table est vide |

### Ports
| Service | Port |
|---|---|
| Application HTTP | `3080` |
| PostgreSQL exposé hôte | `5436` |
| PostgreSQL interne | `5432` |

### Initialisation base
1. Exécution de `db/init.sql`.
2. Création de table idempotente.
3. Migration idempotente.
4. Exécution conditionnelle du seed.

### Commandes disponibles
| Commande | Description |
|---|---|
| `make up` | Démarrer application et PostgreSQL |
| `make down` | Arrêter les conteneurs |
| `make reset` | Supprimer volumes et redémarrer |
| `make db` | Ouvrir shell `psql` |
| `make logs` | Afficher logs conteneurs |
| `make shell` | Ouvrir shell dans conteneur application |
| `make local` | Délégation racine vers application |

### URLs locales
| URL | Description |
|---|---|
| `http://localhost:3080` | Application |
| `http://localhost:3080/dashboard` | Dashboard |

## Performance, volumétrie et contraintes non fonctionnelles
| Aspect | Comportement |
|---|---|
| Architecture | Monolithique |
| ORM | Aucun ORM |
| Frontend | Aucun build frontend |
| Pagination | Non supportée |
| Déploiement | Local Docker uniquement |
| Idempotence DB | `CREATE TABLE IF NOT EXISTS` et seed conditionnel |

## Observabilité
| Élément | Description |
|---|---|
| Logs | Logs stdout du conteneur application |
| Endpoint santé | `GET /health` |
| Monitoring avancé | Non prévu |

## Sauvegarde, PRA et continuité
Aucune exigence de sauvegarde, PRA ou continuité de service n’est prévue.