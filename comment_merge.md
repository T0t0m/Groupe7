# Guide de Merge entre Branches Git

## Prérequis

Avant de commencer un merge, assurez-vous d'avoir :
- Git installé sur votre machine
- Un dépôt Git initialisé
- Au moins deux branches existantes

## Méthode 1 : Merge Standard

### Étapes pour merger une branche dans une autre

1. **Vérifier l'état de votre dépôt**
   ```bash
   git status
   ```

2. **Se positionner sur la branche de destination**
   
   Par exemple, pour merger `feature` dans `main` :
   ```bash
   git checkout main
   ```

3. **Mettre à jour la branche locale**
   ```bash
   git pull origin main
   ```

4. **Effectuer le merge**
   ```bash
   git merge feature
   ```

5. **Pousser les modifications**
   ```bash
   git push origin main
   ```

## Méthode 2 : Merge avec Fast-Forward

Si aucun commit n'a été fait sur la branche de destination depuis la création de la branche source, Git effectue un "fast-forward" :

```bash
git checkout main
git merge feature --ff
```

## Méthode 3 : Merge sans Fast-Forward

Pour conserver l'historique des branches et créer un commit de merge :

```bash
git checkout main
git merge feature --no-ff -m "Merge de la branche feature dans main"
```

## Gestion des Conflits

### Identifier les conflits

Après un `git merge`, si des conflits apparaissent :
```bash
git status
```

Les fichiers en conflit sont marqués comme "both modified".

### Résoudre les conflits

1. **Ouvrir les fichiers en conflit**
   
   Git marque les conflits avec des marqueurs :
   ```
   <<<<<<< HEAD
   Code de la branche actuelle
   =======
   Code de la branche à merger
   >>>>>>> feature
   ```

2. **Éditer manuellement les fichiers**
   
   Supprimez les marqueurs et conservez le code souhaité.

3. **Ajouter les fichiers résolus**
   ```bash
   git add fichier-resolu.txt
   ```

4. **Finaliser le merge**
   ```bash
   git commit -m "Résolution des conflits de merge"
   ```

5. **Pousser les modifications**
   ```bash
   git push origin main
   ```

## Annuler un Merge

### Avant le commit de merge

Si vous n'avez pas encore commité :
```bash
git merge --abort
```

### Après le commit de merge

Pour annuler le dernier commit de merge :
```bash
git reset --hard HEAD~1
```

**Attention** : Cette commande supprime définitivement les modifications.

## Bonnes Pratiques

1. **Toujours travailler sur une branche à jour**
   ```bash
   git pull origin main
   ```

2. **Tester avant de merger**
   
   Assurez-vous que votre code fonctionne correctement sur votre branche avant de merger.

3. **Créer une Pull Request (sur GitHub/GitLab)**
   
   Permet une revue de code avant le merge définitif.

4. **Supprimer les branches mergées**
   ```bash
   git branch -d feature
   git push origin --delete feature
   ```

5. **Utiliser des messages de commit descriptifs**
   
   Facilitent la compréhension de l'historique.

## Commandes Utiles

| Commande | Description |
|----------|-------------|
| `git branch` | Liste toutes les branches locales |
| `git branch -a` | Liste toutes les branches (locales et distantes) |
| `git log --oneline --graph` | Visualise l'historique des commits sous forme de graphe |
| `git merge --squash feature` | Combine tous les commits de la branche en un seul |
| `git diff main..feature` | Affiche les différences entre deux branches |

## Exemple Complet

```bash
# 1. Se positionner sur main
git checkout main

# 2. Mettre à jour
git pull origin main

# 3. Merger la branche feature
git merge feature

# 4. En cas de conflit, résoudre puis :
git add .
git commit -m "Merge feature dans main avec résolution de conflits"

# 5. Pousser
git push origin main

# 6. Supprimer la branche feature (optionnel)
git branch -d feature
git push origin --delete feature
```