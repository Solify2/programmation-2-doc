---
title: Questions
type: docs
prev: docs/folder/
---

## Exercice 1.1  de la séance 1, 

### 1.1.Concret ou abstrait ?
Jetez un œil sur le fichier queue.h reproduit ici. S’agit-il d’un type de donnée abstrait ou concret ?
Justifiez vos réponses en vous basant sur les définitions de ces types de données.

**En analysant le fichier queue.h, nous pouvons déduire qu'il s'agit d'un type de donnée abstrait (ADT). Cela se base sur la définition d'un type abstrait, qui consiste à décrire le comportement et les propriétés d'un type sans préciser comment il est implémenté. Dans notre exemple de "queue", qui est une ADT, nous définissons un type de donnée qui prend en charge l'ajout (empiler) et la suppression (dépiler) en utilisant le principe LIFO (Last In First Out). Cela se fait sans indiquer comment le programme va précisément fonctionner. En résumé, un ADT décrit ce que notre programme doit accomplir sans spécifier les détails de son fonctionnement en coulisses**

 




## Exercice 1.2  de la séance 1

### 1.2.Utilisation de la Queue

Est-ce que plusieurs implémentations concrètes correctes de la Queue sont possibles ? Cela va-t-il
influencer l’expérience de l’utilisateur du programme "manipulation_queue.c" ? Si oui, en quoi ?
Sinon, pourquoi ?

**Résolution: Oui, plusieurs implémentations concrètes d'une queue sont possibles, par exemple en utilisant un tableau classique ou une liste chaînée (simple ou double)**

**Cela pourrait effectivement influencer l’expérience de l’utilisateur, car chaque implémentation a ses propres avantages et inconvénients, et cela dépend de plusieurs facteurs
Par exemple, si on implémente la queue avec un tableau et que celui-ci contient des milliers d'éléments, il faudra peut-être redimensionner le tableau en copiant ces éléments dans un nouveau tableau Cela entraînera un coût supplémentaire. De plus, avec un tableau, il y a une limitation si la taille est fixée, car à un moment donné, lorsqu’on tente d’ajouter un nouvel élément (enqueue), cela pourrait entraîner un message indiquant que la queue est remplie**
