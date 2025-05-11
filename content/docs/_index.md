---
title: Documentation
next: first-page
---

---
title: Documentation
next: first-page
---

## Le but de ce portfolio

Dans le cadre du cours de programmation 2, j'ai réalisé ce travail qui explore en profondeur les structures de données en programmation. Le cours s'articule autour de **plusieurs séances**, chacune dédiée à la découverte d'un nouveau type de structure, L'objectif principal n'est pas simplement d'apprendre à implémenter une structure de données, mais surtout de savoir choisir la structure adéquate pour résoudre un problème de manière optimale. Ce cours m'a offert l'opportunité de m'immerger véritablement dans l'univers de l'algorithmique et des structures de données.


## Les compétences acquises pendant ce travail

Dans cette section, je vais parcourir les différentes séances et présenter les compétences essentielles que j'ai pu développer lors de chacune d'entre elles.


### Compétences générales développées tout au long du projet


1. **Consolidation des connaissances en C et découverte de concepts avancés:**
Ces séances m'ont permis de revisiter les concepts fondamentaux abordés dans le cours de programmation 1, tout en approfondissant ma compréhension du fonctionnement de la mémoire en C. J'ai pu faire la distinction entre les deux types d'allocation mémoire (dynamique et statique), notamment à travers l'utilisation de la fonction `malloc()` et l'importance de libérer la mémoire avec `free()` pour éviter ce que nous appelons dans le jargon informatique "les fuites de mémoire" (memory leaks).

2. **Utilisation des fichiers d'en-tête (Header files)**
J'ai réalisé qu'un des éléments fondamentaux du développement d'applications est d'assurer la maintenabilité du code. Une structure de projet propre garantit cet aspect, particulièrement lorsqu'un programme comporte plusieurs millions de lignes de code et de fonctions dans une application à grande échelle. En C, le fichier d'en-tête est un moyen efficace de découper notre programme en plusieurs modules, chacun ayant une responsabilité précise. Cette approche reflète les concepts architecturaux modernes comme les microservices ou le principe de responsabilité unique, dont l'idée centrale est de structurer notre programme pour faciliter sa gestion, assurer sa maintenabilité et offrir un environnement clair, même si le projet devait être repris par une nouvelle équipe ou un nouveau développeur. Tout ceci s'articule naturellement avec les fichiers .c correspondants.

3. **La notation Big O et le choix d'une structure appropriée:**
Une question que je me posais régulièrement durant mon apprentissage était : "Pourquoi avons-nous besoin de toutes ces structures de données (arbres, listes, graphes, etc.) si je peux résoudre un problème avec n'importe laquelle d'entre elles, et que mon programme fonctionne correctement et rapidement?" Bien que je soupçonnais des différences de performance, ce concept restait assez abstrait pour moi. Je ne percevais pas clairement la différence entre un programme implémenté avec un tableau classique et un autre utilisant un arbre binaire. C'est ce questionnement qui m'a conduit à découvrir les concepts de la notation Big O (worst case scenario, temps constant et linéaire) et plusieurs autres terminologies qui m'ont permis de développer une vision claire et de comprendre l'importance cruciale de la performance dans un programme. L'exemple de la recherche binaire a été particulièrement révélateur : j'ai découvert qu'un algorithme de recherche peut nécessiter jusqu'à **1 million de comparaisons**, tandis qu'un autre, accomplissant exactement la même tâche, n'en requiert que **log₂(1 million) ≈ 20**. Cette illustration m'a fait comprendre que la notion de performance ne se limite pas à un simple test dans mon éditeur avec quelques dizaines ou centaines d'éléments.



### Compétences spécifiques acquises par séance


## **Séance 1**


## La Liste Chaînée

Je souhaitrais commencer par expliquer la concept d'une listé chiané car elle serait utiliser dans l'implémentation des autres strucutres...

Une liste chaînée est une structure de données qui permet de stocker des valeurs de même type, avec l'avantage majeur d'avoir une taille variable, contrairement aux tableaux. une liste chaînée est constituée d'une chaîne composée de maillons, chaque maillon étant relié au suivant à travers un pointeur généralement appelé "next". Ainsi, chaque maillon contient deux éléments : une valeur et un pointeur vers le maillon suivant.

Le dernier maillon de la liste ne pointe vers aucun autre élément, et pour représenter cette absence de valeur suivante, on utilise la valeur NULL. Cela est particulièrement utile lorsque nous parcourons une liste, car elle nous permet de déterminer précisément où se termine cette liste.

### Avantages de la liste chaînée

1. **Gestion dynamique de la mémoire** : La liste chaînée s'adapte automatiquement à la quantité de données stockées.
2. **Insertion et suppression rapides** : Ces opérations ne nécessitent pas de décalage des éléments, contrairement aux tableaux.

### Inconvénients de la liste chaînée

1. **Accès indexé impossible** : Contrairement aux tableaux, on ne peut pas accéder directement au[x] élément.
2. **Consommation mémoire supérieure** : Une liste chaînée occupe plus de mémoire car elle doit stocker, en plus de la valeur, un pointeur (voire deux pointeurs dans le cas d'une liste doublement chaînée).
3. **Accès séquentiel obligatoire** : Pour accéder à un élément spécifique d'une liste chaînée, je suis obligé de parcourir tous les maillons jusqu'au maillon recherché.

### Analyse algorithmique

Durant mes travaux pratiques, j'ai pu comparer les performances des listes chaînées et des tableaux :

* **Accès direct** : Un tableau permet d'avoir un accès indexé en temps constant O(1), tandis qu'avec une liste chaînée, je dois parcourir la liste élément par élément pour trouver la bonne valeur, ce qui donne un temps linéaire O(n).
* **Recherche** : Pour chercher un élément dans les deux structures, il faut généralement examiner élément par élément. Dans le pire des cas (si l'élément se trouve à la fin de la liste ou n'existe pas), la complexité est O(n). Cependant, avec un tableau trié, j'ai découvert qu'il est possible d'obtenir un temps logarithmique O(log n) en appliquant la technique de recherche binaire.
* **Suppression et insertion** : Dans une liste chaînée, ces opérations au début et à la fin se font en temps constant O(1), car elles ne nécessitent pas de décalage des éléments, contrairement aux tableaux.

Cette analyse m'a permis de comprendre que le choix entre liste chaînée et tableau dépend essentiellement du type d'opérations que je prévois d'effectuer le plus fréquemment sur mes données.




### La File d'Attente (Queue)

En informatique, une file d'attente (en anglais "queue") est un type abstrait de données basé sur le principe « premier entré, premier sorti » ou PEPS, désigné en anglais par l'acronyme FIFO ("first in, first out"). Concrètement, cela signifie que les premiers éléments ajoutés à la file seront les premiers à en être retirés.

### Implémentation et complexité algorithmique (Big O)

| Opération | Array | Liste Chaînée |
|-----------|----------------|---------------|
| enqueue  | O(1) | O(1) |
| dequeue  | O(n) | O(1) |


Il existe plusieurs possibilités pour implémenter une file d'attente. Pour éviter les opérations coûteuses de décalage des éléments lors des opérations d'enfilement ou de défilement, j'ai adopté une solution basée sur les listes chaînées.
L'avantage majeur de cette approche est qu'une liste chaînée permet d'accéder facilement et en temps constant à la fois à la tête (dequeue) et à la queue de la file (enqueue) via des pointeurs. Cette structure sera d'ailleurs approfondie lors de la séance dédiée aux listes chaînées.
Grâce à cette implémentation, les deux opérations principales d'une file d'attente peuvent être exécutées en temps constant (O(1)), indépendamment du nombre d'éléments stockés dans la file. J'ai ainsi pu confirmer que l'implémentation d'une file d'attente avec une liste chaînée représente la solution la plus optimale d'un point de vue algorithmique.




## La Pile (Stack)

Une pile (en anglais "stack") est un type abstrait de données basé sur le principe « dernier entré, premier sorti », désigné en anglais par l'acronyme LIFO ("last in, first out"). Concrètement, cela signifie que le dernier élément ajouté à la pile sera le premier à en être retiré.

### Implémentation avec une liste chaînée et complexité algorithmique (Big O)

J'ai constaté qu'il existe plusieurs approches pour implémenter une pile, mais l'utilisation d'une liste chaînée offre des avantages significatifs. En implémentant une pile à l'aide d'une liste chaînée, j'ai pu optimiser les opérations fondamentales de cette structure.

La beauté de cette implémentation réside dans sa simplicité : il suffit d'ajouter et de supprimer des éléments uniquement au début de la liste chaînée. Ainsi, les deux opérations principales d'une pile - l'empilement (push) et le dépilement (pop) - peuvent être exécutées en temps constant O(1), quelle que soit la taille de la pile.

Pour implémenter une pile avec une liste chaînée, j'ai simplement besoin de maintenir un pointeur vers le sommet de la pile (l'élément le plus récemment ajouté). Lors d'un empilement, je crée un nouveau maillon, je stocke la valeur dedans, puis je fais pointer ce nouveau maillon vers l'ancien sommet avant de mettre à jour le pointeur du sommet. Pour le dépilement, je récupère la valeur du sommet actuel, je mets à jour le pointeur du sommet pour qu'il pointe vers le maillon suivant, puis je libère la mémoire du maillon retiré.

### Avantages

En travaillant avec cette implémentation, j'ai identifié plusieurs avantages :

1. **Efficacité temporelle** : Les opérations d'empilement et de dépilement s'effectuent en temps constant O(1).

2. **Taille dynamique** : La pile peut croître ou diminuer selon les besoins, sans gaspillage de mémoire.
3. **Simplicité d'implémentation** : L'ajout et la suppression se font toujours au même endroit (le sommet).

## Conclusion de la Séance 1

Au terme de cette première séance consacrée aux structures de données fondamentales, j'ai exploré en profondeur les files d'attente (queues) et les piles (stacks), ainsi que leur implémentation optimale à l'aide de listes chaînées.

Cette séance m'a permis de comprendre l'importance cruciale du choix d'une structure de données appropriée en fonction des opérations à privilégier. J'ai pu constater comment les caractéristiques fondamentales de chaque structure (FIFO pour les files, LIFO pour les piles) déterminent leur utilisation dans différents contextes algorithmiques.

L'implémentation de ces structures à l'aide de listes chaînées m'a offert une perspective concrète sur l'optimisation des performances : les opérations d'ajout et de retrait s'effectuent en temps constant O(1), indépendamment du nombre d'éléments stockés. Cette efficacité temporelle représente un avantage considérable par rapport à d'autres implémentations qui nécessiteraient des décalages d'éléments coûteux.

### Takeaways essentiels

1. **Maîtrise des structures fondamentales** : J'ai acquis une compréhension approfondie des piles, des files et des listes chaînées, ainsi que de leurs propriétés et comportements spécifiques.

2. **Analyse de complexité algorithmique** : J'ai appris à évaluer la performance des opérations sur ces structures à l'aide de la notation Big O, me permettant de faire des choix éclairés entre différentes implémentations.

3. **Gestion dynamique de la mémoire** : J'ai renforcé ma compréhension de l'allocation et de la libération de mémoire en C, notamment à travers les fonctions `malloc()` et `free()` utilisées dans la création et la suppression des maillons de listes chaînées.

4. **Terminologie précise** : J'ai intégré le vocabulaire spécifique associé à ces structures (enfiler/défiler pour les files, empiler/dépiler pour les piles), essentiel pour communiquer efficacement sur ces concepts.

5. **Conception modulaire** : J'ai mis en pratique la séparation des responsabilités à travers l'utilisation de fichiers d'en-tête (.h) et leurs implémentations (.c), renforçant ainsi la maintenabilité de mon code.

Cette première séance constitue une base solide pour les concepts plus avancés qui seront abordés dans les séances suivantes, particulièrement en ce qui concerne l'optimisation des algorithmes et la sélection judicieuse des structures de données en fonction des problèmes à résoudre.


2. **Séance 2**
3. **Séance 3**
4. **Séance 4**

