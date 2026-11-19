# 🐝 QueenBee — Mobile

Application mobile Flutter de QueenBee pour les apiculteurs et les coordinateurs : état des ruches, inspections, photos, récoltes et alertes, utilisable sans réseau avec synchronisation automatique au retour de la connexion.

Fait partie de la plateforme **QueenBee**, qui aide les coopératives apicoles d'Afrique de l'Ouest et centrale à suivre 400 ruches connectées sur 8 sites, même sans réseau.

| Dépôt | Contenu |
|---|---|
| [queenbee-backend](https://github.com/lahatraR/queenbee-backend) | API Spring Boot, base PostgreSQL/PostGIS, simulateur, infrastructure et déploiement |
| [queenbee-frontend](https://github.com/lahatraR/queenbee-frontend) | Application web Angular |
| [queenbee-mobile](https://github.com/lahatraR/queenbee-mobile) | Application mobile Flutter hors ligne |

📋 Backlog : [Trello QueenBee — Dev & DevOps](https://trello.com/b/lfFjpVWK)

## Stack

- Flutter, Dart
- Base locale chiffrée (choix dans l'ADR, Trello QB-011)
- Notifications Firebase Cloud Messaging
- Client d'API généré depuis le contrat OpenAPI de `queenbee-backend`

## Structure

```
queenbee-mobile/
├── lib/                 # Code Flutter (créé par QB-013)
├── test/                # Tests
├── docs/
│   └── adr/             # Choix techniques propres au mobile
└── .github/workflows/   # CI, sécurité, conventions de PR, release
```

## Démarrer en local

> À compléter par la carte QB-013. L'objectif : cloner, puis lancer le projet en une ou deux commandes.

```bash
git clone https://github.com/lahatraR/queenbee-mobile.git
cd queenbee-mobile
git switch develop
flutter pub get
flutter run
```

## Branches et environnements

| Branche | Environnement | Déploiement |
|---|---|---|
| `main` | Production | À chaque tag `vX.Y.Z` |
| `preprod` | Préproduction (recette) | À chaque merge |
| `develop` | Développement (intégration) | À chaque merge |

Tout changement passe par une Pull Request relue et une CI verte. Les règles complètes sont dans **[CONTRIBUTING.md](CONTRIBUTING.md)** : à lire avant ta première PR.

## Équipe

Clément · Brice · Aimé-Césaire · Riantsoa — Master 1, INGETIS / ITIS, projet fil rouge 2026-2027.

## Licence

Code publié pour consultation uniquement, tous droits réservés : voir [LICENSE](LICENSE).
