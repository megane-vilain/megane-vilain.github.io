---
layout:  post
title:   Types primitifs
author:  Mégane Vilain
category:  Java
---

## Nombres entiers

### Integer

|||
|---|---|
|Width | **32**|
|Minimum value | **-2,147,483,648**|
|Maximum value | **2,147,483,648**|

### Byte

```
Pour effectuer des opération sur un byte il faut caster l'opération.
byte myBYteValue = -128;
byte myNewBite = (byte) (myByteValue/2);
```

|||
|---|---|
|Width | **8**|
|Minimum value | **-128**|
|Maximum value | **127**|

### Short

```
Pour effectuer des opération sur un byte il faut caster l'opération.
```

|||
|---|---|
|Minimum value | **-32768**|
|Maximum value |**32767**|

### Long

```
Il faut écrire un L à la fin de la valeur.
```


|||
|---|---|
|Minimum value | **-2^63**|
|Maximum value |**2^63**|


## Nombres décimaux

### Float

```
Précision simple - 7 chiffres après la virgule.
Il est recommandé d'écrire un f à la fin de la valeur.
float myFloatValue = 5f;
```

|||
|---|---|
|Minimum value | **-3.4^38**|
|Maximum value | **3.4^38**|

### Double 

```
Précision double - 16 chiffres après la virgule.
Il est recommandé d'écrire un d à la fin de la valeur.
```

|||
|---|---|
|Minimum value | **-1,7^308**|
|Maximum value | **1,7^308**|

## Autres types

### Char 

```
Peut stocker un lettre ou un nombre ou unicode.
char myCharValue = '\u00A9';
```

### Boolean

```
Peut être égal à true ou false.
```


