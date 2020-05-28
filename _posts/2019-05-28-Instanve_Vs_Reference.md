---
layout:  post
title:   Instance VS Reference
author:  Mégane Vilain
category:  Java
---

## Instance
Une instance et ce qui est créee par l'opérateur `new`.

## Référence
Une référence à une isntance d'une classe.

```
House blueHouse = new House("Blue");
```
`blueHouse` est ce qui est renvoyé par l'opérateur `new` qui référence l'instance crée cet opérateur.

```
House greenHouse = blueHouse;
```
`greenHouse` et `blueHouse` font référence à la même instance. Changer un des attributs de greenHouse modifiera `greenHouse` et `blueHouse`
