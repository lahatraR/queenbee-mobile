# Contribuer à queenbee-mobile

## L'essentiel en 10 lignes

1. On ne pousse **jamais** directement sur `main`, `preprod` ou `develop` : tout passe par une Pull Request.
2. On part toujours de `develop` à jour : `git switch develop && git pull`.
3. Une branche = une carte Trello : `feature/QB-019-login-jwt`.
4. Code, noms de branches, commits et titres de PR : **en anglais**.
5. Titre de PR au format Conventional Commits : `feat(auth): add JWT refresh token`.
6. 1 relecture approuvée + CI verte avant de merger.
7. Merge d'une branche de travail dans `develop` : **Squash and merge**.
8. Promotion `develop` → `preprod` → `main` : **Create a merge commit**.
9. Aucun secret dans le dépôt (`.env`, clés, mots de passe) : seulement `.env.example`.
10. En cas de doute : demande dans le canal d'équipe avant de forcer quoi que ce soit.

---

## 1. Les branches

| Branche | Rôle | Environnement | Qui écrit dedans |
|---|---|---|---|
| `main` | Production : ce qui est livré au client | prod | PR depuis `preprod` ou `hotfix/*` uniquement |
| `preprod` | Préproduction : recette avant livraison | preprod | PR depuis `develop`, `main` ou `hotfix/*` |
| `develop` | Intégration : le travail du sprint en cours | dev | PR depuis les branches de travail, `main` ou `preprod` |
| `feature/*`, `bugfix/*`… | Travail sur une carte Trello | local | Toi |
| `hotfix/*` | Correction urgente de la production | — | Créée depuis `main` |

`develop` est la branche par défaut : une nouvelle PR la cible automatiquement.

Ces règles sont vérifiées automatiquement par le workflow **PR conventions** : une PR qui ne les respecte pas ne peut pas être mergée.

```
feature/QB-019-login-jwt ─┐
bugfix/QB-045-sync-dup  ──┼─(squash)─▶ develop ─(merge)─▶ preprod ─(merge)─▶ main ──▶ tag v0.2.0
                          │                                                   │
                          │                       hotfix/QB-110-crash ◀───────┘
                          │                                 │
                          └────── back-merge main ─▶ preprod ─▶ develop ◀──────┘
```

## 2. Nommer une branche

```
<type>/<carte>-<description-courte>
```

- **type** : `feature`, `bugfix`, `hotfix`, `refactor`, `docs`, `test`, `ci`, `chore`, `perf`
- **carte** : identifiant Trello, `QB-019` ou `R-02`
- **description** : 2 à 5 mots en anglais, minuscules, séparés par des tirets, sans accents

✅ `feature/QB-019-login-jwt` · `bugfix/QB-045-sync-duplicates` · `docs/R-04-use-case-mapping`
❌ `Feature/login` · `clement-test` · `feature/QB-19-Connexion_JWT`

## 3. Écrire un commit

Format [Conventional Commits](https://www.conventionalcommits.org/) :

```
<type>(<scope>): <description à l'impératif, en anglais, 72 caractères max>
```

| Type | Quand |
|---|---|
| `feat` | Nouvelle fonctionnalité |
| `fix` | Correction de bug |
| `refactor` | Changement de code sans changement de comportement |
| `perf` | Amélioration de performance |
| `test` | Ajout ou correction de tests |
| `docs` | Documentation uniquement |
| `build` | Dépendances, build |
| `ci` | Pipelines CI/CD |
| `chore` | Maintenance diverse |
| `revert` | Annulation d'un commit |

Scopes conseillés pour ce dépôt : `ui`, `auth`, `hives`, `inspections`, `photos`, `harvests`, `alerts`, `offline`, `sync`, `storage`, `push`, `i18n`, `deps`.

```
feat(auth): add JWT refresh token rotation
fix(sync): ignore already applied offline batches
docs(readme): explain local setup
```

Changement cassant (API incompatible…) : ajoute `!` → `feat(api)!: rename hive endpoints`, et explique-le dans la description de la PR.

## 4. Le cycle de travail au quotidien

Configuration à faire **une seule fois** :

```bash
git config --global pull.rebase true          # pull sans commits de merge inutiles
git config --global fetch.prune true          # supprime les branches distantes effacées
git config --global push.autoSetupRemote true # premier push sans --set-upstream
git config --global init.defaultBranch main
```

Démarrer une carte :

```bash
git switch develop
git pull
git switch -c feature/QB-019-login-jwt
```

Pendant le travail :

```bash
git add -p                         # relis ce que tu ajoutes
git commit -m "feat(auth): add login endpoint"
git push                           # pousse ta branche, au moins une fois par jour
```

Récupérer les nouveautés de `develop` sur ta branche :

```bash
git fetch
git rebase origin/develop
git push --force-with-lease        # uniquement sur TA branche, jamais sur une branche partagée
```

Ouvrir la PR : sur GitHub, base `develop`, remplis le modèle, ajoute le lien de la carte Trello, déplace la carte dans **👀 À revoir**.

Après le merge :

```bash
git switch develop
git pull
git branch -d feature/QB-019-login-jwt
```

### Interdits

- `git push --force` sur `main`, `preprod` ou `develop` (bloqué de toute façon).
- Commiter des fichiers générés, des builds ou des secrets.
- Une PR de plus de ~400 lignes : découpe-la.
- Merger sa propre PR sans relecture.

## 5. La Pull Request

- **Titre** : au format Conventional Commits. Il devient le message du commit sur `develop`.
- **Description** : remplis le modèle (lien Trello, ce qui change, comment tester).
- **Relecture** : par le relecteur indiqué sur la carte Trello. Il approuve ou demande des changements dans les 24 h ouvrées.
- **Conversations** : toutes résolues avant le merge.
- **Merge** : Squash and merge. La branche est supprimée automatiquement.

## 6. Livrer une version (fin de sprint)

1. **develop → preprod** : PR intitulée `chore(release): v0.2.0`, merge commit. Le déploiement en préproduction suit automatiquement.
2. **Recette** sur preprod : parcours de démo et tests manuels. Un bug ? Corrige-le sur `develop` puis refais une PR vers `preprod`.
3. **Mise à jour de `CHANGELOG.md`** dans cette même PR ou juste avant.
4. **preprod → main** : PR intitulée `chore(release): v0.2.0`, merge commit.
5. **Tag** sur `main` :
   ```bash
   git switch main && git pull
   git tag -a v0.2.0 -m "v0.2.0"
   git push origin v0.2.0
   ```
   Le workflow **Release** crée la release GitHub avec les notes générées.

Versionnage [SemVer](https://semver.org/lang/fr/) : `MAJEUR.MINEUR.CORRECTIF`.
- Avant la soutenance : `v0.x.y` (un MINEUR par sprint).
- Soutenance : `v1.0.0`.
- Hotfix : on incrémente le CORRECTIF (`v0.2.1`).

## 7. Corriger la production en urgence (hotfix)

```bash
git switch main && git pull
git switch -c hotfix/QB-110-crash-on-login
# … correction + test …
git push
```

1. PR `hotfix/…` → `main`, relue, merge commit, tag `v0.2.1`.
2. Report : PR `main` → `preprod`, puis `main` → `develop`, pour que la correction ne soit pas perdue.

## 8. Secrets et données

- Variables locales dans `.env` (ignoré par Git), modèle dans `.env.example`.
- Secrets de CI dans **Settings → Secrets and variables → Actions**, ou dans l'environnement GitHub concerné.
- Jamais de vraie donnée de bénéficiaire (apicultrices, coopératives, positions de ruchers) dans le dépôt ou les jeux de test.
- Un secret a fuité ? Préviens l'équipe, révoque-le immédiatement : le supprimer du dépôt ne suffit pas.

## 9. Definition of Done

Une carte est terminée quand :
- le code est relu et mergé dans `develop` ;
- la CI est verte (build, tests, analyse, scan de sécurité) ;
- les tests correspondants sont écrits ;
- la documentation est à jour si besoin ;
- les critères d'acceptation de la carte Trello sont cochés.
