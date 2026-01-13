---
layout: post
title : Commits et branches
author: Mégane Vilain
category: Git
---

### Commits

|Commande  | Description |
|---|---|
|`git status`|Affiche la liste de modifications|
|`git log`|Liste l'historique des commits de la branche|
|`git add "file_name"`|Ajoute un fichier au futur commit |
|`git add -A`|Ajoute tous les fichiers modifiés et suivis au futur commit|
|`git add .`|Ajoute tous les fichiers modifiés au futur commit|
|`git reset HEAD "file_name"`|Enlève un fichier du futur commit|
|`git checkout -- "file_name"`|Supprime les modifications apportées à un fichier|
|`git commit -m "message"`|Effectue un commit en local|
|`git reset --hard HEAD~1`|Supprime les commits local non publiés|
|`git push`|Envoie en amont les commits effectués|

### Modifier un commit **local**

**Ne fonctionne que pour le dernier commit**

Modifier le message du commit - `git commit --amend -m "an updated commit message"`

Modifier les fichiers du commit : 
* Effectuer les modifications (modifier, ajouter ou supprimer un des fichier du commit)   
* `git commit --amend --no-edit` (le flag --no-edit indique qu'il ne faut pas modifier le message du commit original


### Supprimer un commit local

```bash
git reset --soft HEAD~1 # Supprime le dernier commit en gardant les modifications du commit 
```

```bash
git reset --hard HEAD~1 # Supprime le dernier commit en supprime les modifications
```

### Infos sur les branches 

***git branch*** - Liste toutes les branches disponible localement et indique la branche actuelle

|Options  | Description |
|---|---|
|`-a`|Liste toutes les branches locales et en amont|
|`-r`|Liste toutes les branches en amont|
|`-v`|Liste toutes les branches et affiche le dernier commit|
|`-vv`|Liste toutes les branches et affiche la branche en amont|


### Manipulation de branches

|Commande  | Description |
|---|---|
|`git checkout "branch_name"`|Change de branche|
|`git branch "branch_name"`|Crée une nouvelle branche à partir de la branche actuelle|
|`git checkout -b "branch_name"`|Crée une nouvelle branche à partir de la branche actuelle et se positionne dessus|
|`git push -u "remote_name" "branch_name"`|Crée la branche local sur la remote. Les noms de branche doivent être identiques|
|`git checkout -b "branch_name" "remote_name"/"branch_name"`|Crée une nouvelle branche qui est une copie de la branche en amont et se place dessus|
|`git branch -u "remote_name"/"branch_name"`|Traque une branche en amont - **Les noms de de branche en local et en amont doivent être identiques**|
|`git branch -d "branch_name"`|Supprime la branche locale|
|`git branch -d "origin_name"/"branch_name"`|Suprrime la branche en amont|
|`git push "remote_name" --delete "branch_name"`|Supprime la branche en amont et en local|

### Renommer une branche

1.  Se positionner sur la branche à renommer - `git chekout "old_name"`
2.  Renommer la branche - `git branch -m "new_name"`

**Si la branche est déjà présente sur Gitlab, il faut exécuter les deux étapes suivantes:**
1.  Supprimer la branche en question - `git push origin --delete "old_name"`
2.  Pousser la branche renommée - `git push origin -u "new_name"`

### Récupérer les modifications en amont

#### Git Fetch

```
Récupère les modifications effectués en amont mais ne les fusione pas avec la version en local
```

|Commande  | Description |
|---|---|
|`git fetch "remote_name"`|Récupére les modifications de toutes les branches en amont|
|`git fetch "remote_name" "branch"`|Récupére les modifications de la branche en amont|

#### Git Pull

```
Récupère les modifications effectués en amont et tente de les fusioner avec la version en local - Effectue un git fetch suivi d'un git merge
```

|Commande  | Description |
|---|---|
|`git pull "remote_name"`|Récupère et merge les modifications de la branche|
|`git pull "remote_name" --rebase`|Récupère et rebase les modifications de la branche|


### Clean local

***git clean*** - Cleans the working tree by recursively removing files that are not under version control, starting from the current directory


|Options  | Description |
|---|---|
|-d |Specify -d to have it recurse into untracked directories as well. |
|-f, --force|If the Git configuration variable clean.requireForce is not set to false, git clean will refuse to delete files or directories unless given -f |
|-i, --interacive|
|-n, --dry-run| Show what files will be deleted|
