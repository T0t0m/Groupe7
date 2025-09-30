<<<<<<< HEAD
# Guide interne (factice) : Utilisation de Git chez **ExampleCorp**

> **But** : fournir une documentation claire et opérationnelle pour l'utilisation de Git au sein de l'entreprise.

---

## 1. Introduction

Cette documentation factice décrit les conventions, le workflow et les bonnes pratiques Git utilisées chez ExampleCorp. Elle sert d'exemple et peut être adaptée facilement pour refléter les règles réelles de votre équipe.

Public visé : développeurs, devops, chefs de projet et toute personne travaillant avec le code source.

---

## 2. Principes généraux

* **Traces claires** : chaque commit doit répondre à une intention unique et lisible.
* **Branches courtes** : les branches de travail (feature/bugfix) doivent être courtes-lived — fusion après revue et tests.
* **Revue obligatoire** : toute modification significative passe par une Pull Request (PR) et une revue par au moins un pair.
* **CI obligatoire** : la branche cible doit être protégée par la CI — les builds/tests doivent réussir avant merge.

---

## 3. Workflow recommandé (GitFlow simplifié)

1. **`main`** : branche de production. Toujours stable et déployable.
2. **`develop`** : branche d'intégration pour les fonctionnalités prêtes à être testées ensemble (optionnel si vous déployez directement depuis `main`).
3. **`feature/<ticket>-short-description`** : branches pour nouvelles fonctionnalités.
4. **`bugfix/<ticket>-short-description`** : branches pour corrections de bugs.
5. **`hotfix/<ticket>`** : corrections critiques appliquées directement sur `main` puis mergées vers `develop`.

Cycle :

* Créer une `feature` depuis `develop` (ou `main` si pas de develop).
* Push régulièrement et ouvrir une PR quand prête.
* Après revue et CI verte, merger via `merge commit` ou `squash` selon la politique (voir section Merge).

---

## 4. Convention de nommage des branches

* `feature/1234-login-social` – ajout d'un login social (ticket 1234)
* `bugfix/1412-fix-typo` – correction d'une faute
* `hotfix/2025-urgent-patch` – patch urgent en production

Règles : tout en minuscules, utiliser des `-` pour séparer, préfixer par type.

---

## 5. Convention de messages de commit

Les messages de commit doivent être clairs et structurés :

```
<type>(<scope>): <court résumé (50 char max)>

Description optionnelle plus détaillée, si besoin. Références : #<ticket>
```

Types usuels : `feat`, `fix`, `chore`, `docs`, `refactor`, `test`, `perf`.

Exemple :

```
feat(auth): add OAuth2 login with Google

Adds sign-in with Google using OAuth2. Related to #1234.
```

Pour des petits commits locaux et expérimentaux, utilisez des messages temporaires, mais nettoyez avant la PR (rebase/squash si nécessaire).

---

## 6. Pull Request (PR) — processus

* **Titre** : court et descriptif (`[FEATURE] Login social (Google)`)
* **Description** : objectif, capture d'écran si UI, étapes pour tester, ticket lié.
* **Assignation** : assigner un reviewer.
* **Labels** : `feature`, `bug`, `urgent`, `needs-review`, `ready`.
* **Checklist (dans la description)** :

  * [ ] Tests unitaires ajoutés/ok
  * [ ] Tests d'intégration ajoutés/ok
  * [ ] Build CI réussi
  * [ ] Documentation mise à jour

Minimum : 1 approbation + CI passed.

---

## 7. Merge strategy

* **Squash merge** (par défaut) : garder un historique propre sur `main` (1 PR = 1 commit).
* **Merge commit** (si on veut conserver le détail) : pour features complexes avec plusieurs commits pertinents.
* **Rebase and merge** : éviter sur les PR publiques si d'autres branches en dépendent.

Protection des branches :

* `main` protected : pas de push direct, PR obligatoire, CI verte, review requise.

---

## 8. Gestion des conflits

1. Mettre à jour votre branche locale : `git fetch && git rebase origin/develop` (ou `merge` si préférences locales).
2. Résoudre les conflits manuellement, tester localement.
3. `git add` les fichiers résolus puis `git rebase --continue` ou commit le merge.
4. Push force si rebase (`git push --force-with-lease`).

Conseils :

* Préférez `--force-with-lease` plutôt que `--force`.
* Si la PR est revue, informez les reviewers après un rebase important.

---

## 9. Tags et releases

* Utiliser des tags sémantiques : `v1.2.3`.
* Releasing : créer une release GitHub/GitLab avec changelog minimal (points saillants et tickets résolus).
* Pour les hotfixes, tagger la version patch et documenter l'urgence.

---

## 10. CI/CD et hooks locaux

* La CI exécute : lint, tests unitaires, build, et tests d'intégration si nécessaire.
* Hooks recommandés (pré-commit) :

  * formatage automatique (ex : `dotnet format`, `prettier`, `black`)
  * vérification des messages de commit (optionnel)
  * exécution rapide de tests unitaires critiques

Exemples : utilisez `husky` (JS) ou `pre-commit` (Python) pour installer des hooks partagés.

---

## 11. Bonnes pratiques

* Committez souvent, mais par petites unités logiques.
* Évitez les commits qui touchent plusieurs sujets non liés.
* Documentez les décisions non triviales dans la PR.
* Nettoyez l'historique avant de merger si nécessaire (squash, tidy commits).
* Ne pas committer d'informations sensibles (mot de passe, clefs). Utiliser des variables d'environnement et un vault.

---

## 12. Fiche de commandes rapide

* `git clone <url>` — cloner le repo
* `git checkout -b feature/1234-desc` — créer et switcher
* `git add . && git commit -m "feat: ..."` — commit
* `git push -u origin feature/1234-desc` — push
* `git fetch && git rebase origin/develop` — mettre à jour et rebaser
* `git merge origin/develop` — merger (si politique merge)
* `git pull --rebase` — récupérer et rebaser
* `git tag -a v1.2.0 -m "Release v1.2.0"` — tag

---

## 13. Scénarios courants

### A. Je veux récupérer la dernière version de `develop` dans ma feature

```
git fetch origin
git checkout feature/1234-desc
git rebase origin/develop
# résoudre les conflits
git push --force-with-lease
```

### B. J'ai fait une erreur dans le dernier commit (pas encore push)

```
git commit --amend --no-edit
# ou
git commit --amend -m "fix: corrected message"
```

### C. Annuler un commit déjà poussé (danger)

* Préférer faire un revert plutôt qu'un reset si le commit est public :

```
git revert <commit-hash>
```

---

## 14. Template de PR (à copier dans la description)

```
## But
Courte description du pourquoi de la PR.

## Changements
- Liste des changements importants.

## Comment tester
1. Etape 1
2. Etape 2

## Tickets liés
- #1234

## Checklist
- [ ] Tests ajoutés
- [ ] CI verte
- [ ] Documentation mise à jour
```

---

## 15. FAQ / Troubleshooting rapide

**Q : Comment retrouver un ancien fichier supprimé ?**
A : `git checkout <commit> -- path/to/file`

**Q : Quel est l'état de mes fichiers ?**
A : `git status` et `git diff`.

**Q : Je veux annuler tous les changements locaux non commités**
A : `git reset --hard` (attention : pertes de données locales)

---

## 16. Annexes

### Exemple `.gitignore` basique

```
# OS
.DS_Store
Thumbs.db

# Visual Studio
.vs/
*.user

# Node
node_modules/

# Python
__pycache__/
*.pyc

# Secrets
.env
```

### Modèle de message de commit (commitizen-friendly)

```
feat(scope): description

BREAKING CHANGE: description
```

---

## 17. Contacts

* Responsable Git / DevOps : `devops@examplecorp.com`
* Support équipe : `team-support@examplecorp.com`

---

*Document factice généré automatiquement — à adapter avant usage en production.*
=======
Coucou

Modification pour une fork
>>>>>>> gaspar
