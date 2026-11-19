# Politique de sécurité

## Signaler une vulnérabilité

Ne créez pas d'issue publique. Utilisez **Security → Report a vulnerability** sur ce dépôt (signalement privé GitHub), en décrivant :

- le composant concerné et la version (tag ou commit) ;
- les étapes pour reproduire ;
- l'impact estimé.

L'équipe accuse réception sous 72 h ouvrées.

## Versions maintenues

| Version | Maintenue |
|---|---|
| Dernière version `v0.x` | ✅ |
| Versions précédentes | ❌ |

## Bonnes pratiques appliquées

- Aucun secret dans le dépôt, scan automatique à chaque PR (gitleaks).
- Scan des dépendances (Snyk, Dependabot).
- Branches `main`, `preprod` et `develop` protégées : relecture et CI obligatoires.
