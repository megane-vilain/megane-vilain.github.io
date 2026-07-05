---
layout: post
title : Deployment
author: Mégane Vilain
category: Kubernetes
subcategory: Components
---

# Deployments

## Qu'est-ce qu'un Deployment ?

Un **Deployment** est une ressource Kubernetes qui permet de gérer le cycle de vie d'une application.

Plutôt que de créer directement des Pods, on décrit l'état souhaité de l'application dans un Deployment. Kubernetes se charge ensuite de créer et de maintenir le nombre de Pods demandé.

Pour la majorité des applications (API, sites web, microservices...), le Deployment est la ressource utilisée.

---

## Pourquoi utiliser un Deployment ?

Créer un Pod directement fonctionne, mais présente une limitation importante : si le Pod est supprimé, il ne sera pas recréé.

Le Deployment permet notamment de :

- maintenir un nombre de Pods donné ;
- recréer automatiquement un Pod disparu ;
- mettre à jour une application ;
- revenir à une version précédente (Rollback) ;
- augmenter ou diminuer le nombre de Pods.

---

## Comment fonctionne un Deployment ?

Un Deployment ne crée pas directement les Pods.

Lorsqu'un Deployment est créé, Kubernetes crée automatiquement un **ReplicaSet**.

Le ReplicaSet est chargé de maintenir le nombre de Pods demandé.

Le fonctionnement est donc le suivant :

```
Deployment
      │
      ▼
ReplicaSet
      │
      ▼
Pods
```

Dans la plupart des cas, il n'est pas nécessaire d'interagir directement avec les ReplicaSets. Ils sont gérés automatiquement par le Deployment.

---

## Exemple minimal

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx

spec:
  replicas: 3

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
        - name: nginx
          image: nginx:1.29
          ports:
            - containerPort: 80
```

Ce manifeste demande à Kubernetes :

- de créer un Deployment nommé `nginx` ;
- d'exécuter **3 Pods** ;
- chaque Pod exécute un conteneur NGINX.

---

## Les principaux champs

### replicas

Détermine le nombre de Pods souhaités.

```yaml
replicas: 3
```

Si un Pod disparaît, Kubernetes en crée automatiquement un nouveau afin de conserver trois instances.

---

### selector

Le **selector** indique quels Pods appartiennent au Deployment.

Kubernetes identifie les Pods grâce à leurs **labels** (des paires clé/valeur associées aux ressources).

Par exemple, un Pod peut posséder le label :

```yaml
labels:
  app: nginx
```

Le Deployment utilise ensuite un selector pour dire :

> "Je gère tous les Pods qui possèdent le label `app: nginx`."

```yaml
selector:
  matchLabels:
    app: nginx
```

Les labels définis dans le `template` du Pod doivent correspondre au `selector`.

```yaml
spec:
  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx
```

### template

Le template décrit le Pod qui sera créé.

Il s'agit quasiment du même contenu qu'un manifeste de type `Pod`.

Chaque modification du template (image, variables d'environnement, ressources...) entraînera la création d'une nouvelle version des Pods.

---

## Mise à jour d'un Deployment

L'un des principaux avantages d'un Deployment est la possibilité de mettre à jour une application sans interruption de service.

Par exemple, si l'image passe de :

```yaml
image: nginx:1.29
```

à

```yaml
image: nginx:1.30
```

Kubernetes remplace progressivement les anciens Pods par de nouveaux.

Ce mécanisme est appelé **Rolling Update**.

Pendant cette opération, les anciens et les nouveaux Pods peuvent coexister afin d'assurer la continuité du service.

---

## Rollback

Si une mise à jour pose problème, Kubernetes permet de revenir à une version précédente du Deployment.

Les ReplicaSets des anciennes versions sont conservés pendant un certain temps afin de permettre ce retour arrière.

---

## Scaling

Le nombre de Pods peut être modifié simplement en changeant la valeur de `replicas`.

Par exemple :

```yaml
replicas: 5
```

Kubernetes créera automatiquement les Pods supplémentaires.

À l'inverse, diminuer cette valeur entraînera la suppression des Pods en trop.

---

## Ce que le Deployment ne fait pas

Un Deployment ne répartit pas le trafic réseau entre les Pods.

Pour cela, on utilise un **Service**.

De même, le Deployment ne stocke pas les données de l'application. Le stockage est assuré par les volumes utilisés par les Pods.

---

## Quand utiliser un Deployment ?

Le Deployment est adapté à la majorité des applications :

- API REST ;
- applications Web ;
- microservices ;
- backends ;
- frontends.

---

## Quand ne pas utiliser un Deployment ?

Le Deployment n'est pas adapté à toutes les situations.

Par exemple :

- une base de données nécessitant une identité stable utilisera généralement un **StatefulSet** ;
- une tâche ponctuelle utilisera un **Job** ;
- une tâche planifiée utilisera un **CronJob** ;
- un Pod devant être présent sur chaque Worker Node utilisera un **DaemonSet**.

Ces ressources sont détaillées dans leurs pages respectives.

---

## Commandes utiles

Lister les Deployments :

```bash
kubectl get deployments
```

Afficher les détails :

```bash
kubectl describe deployment <nom>
```

Modifier le nombre de réplicas :

```bash
kubectl scale deployment <nom> --replicas=5
```

Afficher l'état du déploiement :

```bash
kubectl rollout status deployment <nom>
```

Voir l'historique :

```bash
kubectl rollout history deployment <nom>
```

Revenir à la version précédente :

```bash
kubectl rollout undo deployment <nom>
```

---

## Bonnes pratiques

- Créer les applications via un Deployment plutôt qu'un Pod.
- Utiliser plusieurs réplicas pour les applications critiques.
- Utiliser des labels cohérents.
- Effectuer les mises à jour via un Rolling Update.
- Ne pas modifier directement les Pods créés par un Deployment.

---

## Ce qu'il faut retenir

- Un Deployment gère le cycle de vie d'une application.
- Il crée et gère automatiquement un ReplicaSet.
- Le ReplicaSet maintient le nombre de Pods souhaité.
- Les mises à jour sont réalisées progressivement grâce aux Rolling Updates.
- Un Deployment permet également le rollback et le scaling.