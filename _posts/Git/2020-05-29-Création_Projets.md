---
layout: post
title : Création de projet
author: Mégane Vilain
category: Git
---

### Création d'un projet

|Commande  | Description |
|---|---|
|`git init`|Transform the current directory into a Git repository. |
|`git init "folder_name"`|Create an empty Git repository in the specified directory. Running this command will create a new subdirectory called ＜directory＞ containing nothing but the .git subdirectory. |
|`git clone "project_url"`|Crée une version locale d'un projet déjà existant|

### Lier le projet local à la remote

|Commande  | Description |
|---|---|
|`git remote`|Affiche la liste des reomte|
|`git remote -v`|Affiche la liste des remotes avec les url|
|`git remote add "remote_name" "url"`|Ajoute une url|
|`git remote delete "remote_name"`|Supprime une url|
