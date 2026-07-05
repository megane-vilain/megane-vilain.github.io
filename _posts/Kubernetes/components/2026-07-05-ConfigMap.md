---
layout: post
title : Config Map
author: Mégane Vilain
category: Kubernetes
subcategory: Components
---


# ConfigMaps

## Qu'est-ce qu'une ConfigMap ?

Une **ConfigMap** est une ressource Kubernetes qui permet de stocker des données de configuration sous forme de paires **clé/valeur**.

Elle permet de séparer la configuration d'une application de son image de conteneur.

Ainsi, une même image peut être utilisée dans plusieurs environnements (développement, test, production) avec des configurations différentes.

---

## Pourquoi utiliser une ConfigMap ?

Sans ConfigMap, il est courant de placer la configuration directement dans l'image du conteneur.

Cela présente plusieurs inconvénients :

- il faut reconstruire l'image à chaque modification de configuration ;
- la même image ne peut pas être facilement réutilisée dans différents environnements ;
- la configuration est mélangée avec l'application.

Les ConfigMaps permettent de conserver une image générique et de fournir la configuration au moment du déploiement.

---

## Que peut contenir une ConfigMap ?

Une ConfigMap peut contenir :

- des paramètres de configuration ;
- des adresses de services ;
- des ports ;
- des options de démarrage ;
- des fichiers de configuration.

En revanche, **elle ne doit pas contenir de données sensibles**.

Les mots de passe, certificats, clés API ou tokens doivent être stockés dans des **Secrets**.

---

## Exemple minimal

```yaml
apiVersion: v1
kind: ConfigMap

metadata:
  name: app-config

data:
  APP_NAME: MonApplication
  LOG_LEVEL: INFO
  SERVER_PORT: "8080"
```

Cette ConfigMap contient trois paramètres qui pourront être utilisés par une application.

---

## Utiliser une ConfigMap comme variables d'environnement

Le moyen le plus courant consiste à injecter les valeurs sous forme de variables d'environnement.

```yaml
containers:
  - name: app
    image: my-app

    envFrom:
      - configMapRef:
          name: app-config
```

Toutes les clés de la ConfigMap deviennent alors des variables d'environnement du conteneur.

Par exemple :

```
APP_NAME=MonApplication
LOG_LEVEL=INFO
SERVER_PORT=8080
```

---

## Utiliser une seule valeur

Il est également possible de récupérer uniquement une clé.

```yaml
env:
  - name: LOG_LEVEL
    valueFrom:
      configMapKeyRef:
        name: app-config
        key: LOG_LEVEL
```

Dans ce cas, seule la variable `LOG_LEVEL` est créée.

---

## Monter une ConfigMap comme volume

Une ConfigMap peut également être montée dans un volume.

Chaque clé devient alors un fichier.

```yaml
volumes:
  - name: config
    configMap:
      name: app-config

containers:
  - name: app
    volumeMounts:
      - name: config
        mountPath: /etc/config
```

Si la ConfigMap contient :

```
config.yaml
logging.conf
```

on retrouvera les fichiers :

```
/etc/config/config.yaml
/etc/config/logging.conf
```

Cette méthode est particulièrement utile pour les applications qui lisent leur configuration depuis des fichiers.

---

## Modifier une ConfigMap

Une ConfigMap peut être modifiée sans recréer la ressource.

Cependant, le comportement des applications dépend de la manière dont la configuration est utilisée.

Par exemple :

- une variable d'environnement est généralement lue au démarrage du conteneur ;
- un fichier monté depuis une ConfigMap peut être mis à jour automatiquement selon l'application et le mécanisme utilisé.

Dans de nombreux cas, il est nécessaire de redémarrer les Pods pour prendre en compte une nouvelle configuration.

---

## Quand utiliser une ConfigMap ?

Une ConfigMap est adaptée pour stocker :

- des paramètres d'application ;
- des fichiers de configuration ;
- des URL de services ;
- des niveaux de logs ;
- des options de démarrage.

---

## Quand ne pas utiliser une ConfigMap ?

Ne pas utiliser une ConfigMap pour stocker :

- des mots de passe ;
- des clés API ;
- des certificats ;
- des clés privées ;
- des tokens d'authentification.

Ces données doivent être stockées dans un **Secret**.

---

## Commandes utiles

Lister les ConfigMaps :

```bash
kubectl get configmaps
```

Afficher une ConfigMap :

```bash
kubectl describe configmap <nom>
```

Afficher son contenu :

```bash
kubectl get configmap <nom> -o yaml
```

Supprimer une ConfigMap :

```bash
kubectl delete configmap <nom>
```

---

## Bonnes pratiques

- Séparer la configuration de l'application.
- Ne jamais stocker d'informations sensibles dans une ConfigMap.
- Donner des noms explicites aux clés.
- Utiliser des ConfigMaps différentes lorsque les configurations concernent des applications distinctes.
- Préférer les fichiers montés lorsque l'application utilise déjà des fichiers de configuration.

---

## Ce qu'il faut retenir

- Une ConfigMap stocke de la configuration non sensible.
- Elle permet de séparer la configuration de l'image du conteneur.
- Les données peuvent être injectées comme variables d'environnement ou montées comme fichiers.
- Une même ConfigMap peut être utilisée par plusieurs Pods.
- Les données sensibles doivent être stockées dans un Secret.