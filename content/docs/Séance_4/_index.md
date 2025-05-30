---
title: Séance_4
type: docs
prev: docs/first-page
next: docs/folder/leaf
sidebar:
  open: true
---

## 1.1.Opérations sur un tableau associatif
Définissez en français les 4 opérations qui peuvent être réalisées sur un tableau associatif : ajouter
un élément (et une clé), rechercher un élément (par sa clé), supprimer un élément (par sa clé),
modifier un élément (par sa clé). Définissez aussi des conditions qui devront toujours être
respectées.


Un tableau associatif (dictionnaire) supporte les quatre opérations suivantes :

### 1. **Ajouter un élément (add)**
- **Description** : Insérer une nouvelle paire clé-valeur dans le dictionnaire
- **Comportement** : L'opération réussit si la clé n'existe pas déjà, sinon elle échoue
- **Préconditions** : Le dictionnaire doit être valide (non NULL), la clé doit être unique

### 2. **Rechercher un élément (get)**
- **Description** : Récupérer la valeur associée à une clé donnée
- **Comportement** : Retourne la valeur si la clé existe, NULL sinon
- **Préconditions** : Le dictionnaire doit être valide (non NULL)

### 3. **Supprimer un élément (remove)**
- **Description** : Retirer une paire clé-valeur du dictionnaire en utilisant la clé
- **Comportement** : L'opération réussit si la clé existe, sinon elle échoue
- **Préconditions** : Le dictionnaire doit être valide (non NULL), la clé doit exister

### 4. **Modifier un élément (modify)**
- **Description** : Mettre à jour la valeur associée à une clé existante
- **Comportement** : L'opération réussit si la clé existe, sinon elle échoue
- **Préconditions** : Le dictionnaire doit être valide (non NULL), la clé doit exister


## 1.2. Définition formelle de l'ADT tableau associatif

Un **tableau associatif** (dictionnaire) est un type de données abstrait qui peut être défini formellement comme :

**Un ensemble fini de couples (clé, valeur)** où chaque clé est unique et permet d'accéder directement à sa valeur associée.


## 2.2.Complexité


## 2.2. Complexité

### **Complexité idéale pour un arbre binaire de recherche complet et parfaitement équilibré :**

Pour un arbre binaire de recherche avec **n** nœuds qui est complet et parfaitement équilibré, la hauteur de l'arbre est **h = ⌊log₂(n)⌋**.

**Complexité dans le pire des cas pour chaque opération :**

| Opération | Complexité | Explication |
|-----------|------------|-------------|
| **add** | **O(log n)** | Parcours de la racine jusqu'à la position d'insertion |
| **get** | **O(log n)** | Parcours de la racine jusqu'au nœud recherché |
| **remove (Pas implémenté)** | **O(log n)** | Recherche du nœud + recherche du successeur si nécessaire |
| **modify(** | **O(log n)** | Parcours de la racine jusqu'au nœud à modifier |



