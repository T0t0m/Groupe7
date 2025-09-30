# Documentation Développement – Intissar

## 1. Commandes Git utiles
Voici les commandes de base que j’utilise dans ce projet :

- `git clone <url>` : cloner un dépôt distant.
- `git checkout -b <branche>` : créer et passer sur une nouvelle branche.
- `git status` : voir l’état du dépôt local.
- `git add <fichier>` : ajouter un fichier au suivi.
- `git commit -m "message"` : valider les modifications.
- `git push origin <branche>` : envoyer les changements sur GitHub.
- `git pull` : récupérer les dernières mises à jour.

---

## 2. Workflow choisi
Nous travaillons en équipe avec **plusieurs branches** :
- Chaque personne crée une branche à son nom (ex : `intissar`, `amine`, `Gaspar`, `Tom`, `Georges`).
- On ajoute nos fichiers ou nos parties de documentation.
- modification du fichier par Gaspar : 
on peux aussi merge avec d'autre branch que la branch main pour modifier les travaux des collaborateurs
- Ensuite, on ouvre une **Pull Request** pour fusionner avec la branche `main`.

---

## 3. Tutos rapides
- **Créer une branche** :  
  ```bash
  git checkout -b ma-branche

