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




## La File d'Attente (Queue)

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

### Conclusion de la Séance 1

Au terme de cette première séance consacrée aux structures de données fondamentales, j'ai exploré en profondeur les files d'attente (queues) et les piles (stacks), ainsi que leur implémentation optimale à l'aide de listes chaînées.

Cette séance m'a permis de comprendre l'importance cruciale du choix d'une structure de données appropriée en fonction des opérations à privilégier. J'ai pu constater comment les caractéristiques fondamentales de chaque structure (FIFO pour les files, LIFO pour les piles) déterminent leur utilisation dans différents contextes algorithmiques.

L'implémentation de ces structures à l'aide de listes chaînées m'a offert une perspective concrète sur l'optimisation des performances : les opérations d'ajout et de retrait s'effectuent en temps constant O(1), indépendamment du nombre d'éléments stockés. Cette efficacité temporelle représente un avantage considérable par rapport à d'autres implémentations qui nécessiteraient des décalages d'éléments coûteux.

### Takeaways de la Séance 1

1. **Maîtrise des structures fondamentales** : J'ai acquis une compréhension approfondie des piles, des files et des listes chaînées, ainsi que de leurs propriétés et comportements spécifiques.

2. **Analyse de complexité algorithmique** : J'ai appris à évaluer la performance des opérations sur ces structures à l'aide de la notation Big O, me permettant de faire des choix éclairés entre différentes implémentations.

3. **Gestion dynamique de la mémoire** : J'ai renforcé ma compréhension de l'allocation et de la libération de mémoire en C, notamment à travers les fonctions `malloc()` et `free()` utilisées dans la création et la suppression des maillons de listes chaînées.

4. **Terminologie précise** : J'ai intégré le vocabulaire spécifique associé à ces structures (enfiler/défiler pour les files, empiler/dépiler pour les piles), essentiel pour communiquer efficacement sur ces concepts.

5. **Conception modulaire** : J'ai mis en pratique la séparation des responsabilités à travers l'utilisation de fichiers d'en-tête (.h) et leurs implémentations (.c), renforçant ainsi la maintenabilité de mon code.

Cette première séance constitue une base solide pour les concepts plus avancés qui seront abordés dans les séances suivantes, particulièrement en ce qui concerne l'optimisation des algorithmes et la sélection judicieuse des structures de données en fonction des problèmes à résoudre.



## **Séance 2**

## La File d'Attente à Priorité (Priority Queue)

Une file de priorité (Priority Queue) est une structure de données abstraite (ADT) qui permet de stocker des éléments en fonction d'une valeur de priorité. La particularité de cette structure réside dans le fait que chaque élément possède une priorité qui détermine l'ordre dans lequel les éléments seront traités.

Par exemple, dans une file de priorité contenant des entiers, on peut définir que l'élément le plus grand est celui avec la plus haute priorité. Il est également possible de créer des implémentations plus sophistiquées où la priorité dépend de multiples critères. Le principe fondamental reste que les éléments avec la priorité la plus élevée sont retirés (défilés) en premier de la file.

Au cours de cette séance, j'ai exploré différentes méthodes d'implémentation de cette structure. Mon objectif était de partir d'une implémentation basique, peu optimale d'un point de vue algorithmique, pour progressivement aboutir à une solution beaucoup plus efficace.

### La file de priorité implémentée avec un tableau

Ma première approche, la plus intuitive, a consisté à utiliser un simple tableau (Array). Dans cette implémentation, à chaque insertion d'un élément, je dois déterminer sa position exacte en fonction de sa priorité, puis décaler les autres éléments en cas d'insertion au début ou au milieu de la liste. Cette opération implique de parcourir tout ou partie du tableau, rendant l'insertion particulièrement coûteuse en temps de calcul.

De plus, lorsque je retire l'élément avec la plus haute priorité, je dois décaler tous les éléments qui le suivaient. J'ai constaté que ces deux opérations (insertion et suppression) peuvent significativement dégrader les performances lorsque le nombre d'éléments devient important, car elles nécessitent un temps linéaire O(n). J'ai également noté que l'utilisation d'un tableau impose certaines limitations concernant la taille et la gestion de la mémoire.

### La file de priorité implémentée avec une liste chaînée

Ma deuxième implémentation a fait appel à une liste chaînée. Là encore, l'insertion d'un nouvel élément m'oblige à parcourir la liste pour trouver la position appropriée selon sa priorité, ce qui reste une opération en temps linéaire O(n).

Cependant, j'ai découvert un avantage significatif : retirer l'élément prioritaire devient beaucoup plus rapide, car celui-ci se trouve toujours en tête (head) de la liste selon l'ordre de tri, sans nécessiter de décalage des autres éléments. Je peux donc le retirer en temps constant O(1). Cette solution s'avère plus avantageuse que le tableau si j'ai besoin de retirer fréquemment des éléments prioritaires, mais elle demeure limitée pour les opérations d'insertion.

### La file de priorité implémentée avec un tas binaire

Finalement, j'ai implémenté la solution la plus efficace : le tas binaire (binary heap). Cette structure, basée sur un arbre binaire mais implémentée astucieusement avec un tableau, m'a permis d'obtenir des performances bien supérieures.

Le principe fondamental du tas est que chaque nœud respecte une propriété d'ordre spécifique : par exemple, dans un tas de type max-priority, chaque parent est toujours plus grand que ses enfants. Grâce à cette organisation hiérarchique, j'ai constaté que les opérations d'insertion comme de suppression s'effectuent en temps logarithmique O(log n), car il suffit de réorganiser une seule branche de l'arbre à chaque opération.

Le tas binaire représente donc une structure idéale pour manipuler des files de priorité contenant un grand nombre d'éléments, offrant des performances équilibrées pour toutes les opérations essentielles.

### Analyse algorithmique comparative

En analysant mes différentes implémentations, j'ai pu tirer plusieurs conclusions importantes :

1. Avec le tableau simple, j'ai observé que les opérations manipulant la file de priorité nécessitent un temps linéaire O(n), ce qui devient problématique lorsque la taille des données augmente.

2. En passant à la liste chaînée, j'ai constaté une amélioration notable pour l'opération de défilement (dequeue). Grâce à la flexibilité de cette structure, j'ai pu défiler l'élément le plus prioritaire (en le plaçant à la tête de la liste) en temps constant O(1), sans avoir besoin de décaler les autres éléments. Cette solution est donc plus efficace que le tableau lorsqu'on doit retirer fréquemment des éléments prioritaires, mais elle reste limitée concernant les insertions.

3. L'implémentation utilisant un tas binaire, qui s'appuie sur le concept d'arbre binaire, m'a permis d'atteindre une complexité logarithmique O(log n) pour toutes les opérations principales. Le tas binaire organise les éléments de façon à maintenir constamment une propriété d'ordre entre les nœuds parents et enfants. Lors de l'insertion ou de la suppression d'un élément, le tas s'auto-réorganise en "percolant" l'élément vers le haut ou le bas de l'arbre, ce qui garantit que l'élément le plus prioritaire reste toujours accessible rapidement.


## Concepts importants supplémentaires

### 1. Les fonctions d'ordre supérieur (Higher Order Functions)

A travers de cette séance, j'ai également approfondi le concept des fonctions d'ordre supérieur, un principe fondamental de la programmation fonctionnelle. Ces fonctions sont caractérisées par leur capacité à prendre d'autres fonctions comme arguments ou à renvoyer des fonctions comme résultats.

Ce concept m'était déjà familier grâce à mes connaissances préalable en JavaScript, particulièrement dans le contexte de la programmation fonctionnelle.

Dans le cadre de ces séances en C, j'ai pu transposer ce concept en implémentant des fonctions qui acceptent des pointeurs de fonction comme paramètres. Cela m'a permis de créer des structures de données plus flexibles et génériques, notamment pour la file de priorité où la fonction de comparaison peut être passée en argument pour déterminer l'ordre de priorité selon différents critères.

Cette approche m'a permis de réutiliser le même code pour diverses logiques métier sans avoir à modifier la structure sous-jacente. Par exemple, j'ai pu utiliser la même implémentation de file de priorité pour ordonner des éléments selon un critère numérique, alphabétique ou même selon des règles personnalisées complexes.

### 2. Le développement piloté par les tests (TDD) et les tests unitaires

Lors de l'une de nos séances, nous avons abordé le concept fondamental du développement piloté par les tests (Test-Driven Development ou TDD). Cette méthodologie inverse le flux de développement traditionnel en commençant par l'écriture des tests avant même d'implémenter le code fonctionnel.

Le principe du TDD repose sur un cycle en trois étapes : On commence par écrire un test qui échoue (puis on implémente le minimum de code nécessaire pour que le test réussisse, et enfin on améliore le code sans modifier son comportement.

Dans le cadre de mes projets, j'ai tenté de simuler cette approche en créant des fonctions de test qui échouaient initialement. Ces tests définissaient clairement les comportements attendus de mes structures de données. Par exemple, pour la file de priorité, j'ai d'abord écrit des tests vérifiant que l'élément défilé était bien celui avec la plus haute priorité.

Au fur et à mesure de mon implémentation, j'ai progressivement fait évoluer mon code pour satisfaire ces tests. Cette démarche m'a aidé à clarifier mes objectifs, à détecter rapidement les régressions et à m'assurer que chaque fonctionnalité répondait correctement aux spécifications. Cette expérience m'a confirmé l'intérêt majeur des tests unitaires pour garantir la robustesse et la fiabilité du code, particulièrement dans le contexte de structures de données complexes comme les tas binaires.

## Takeaways de la Séance 2

1. **Importance de l'analyse comparative** : J'ai appris à évaluer systématiquement différentes implémentations d'une même structure de données pour identifier celle qui répond le mieux aux exigences spécifiques du problème.

2. **Compromis performance/complexité** : J'ai compris que le choix d'une structure de données implique souvent un compromis entre la simplicité d'implémentation et l'efficacité algorithmique.

3. **Puissance des structures d'arbre** : J'ai découvert comment les structures de type arbre, comme le tas binaire, peuvent offrir des performances logarithmiques là où des structures linéaires imposent des complexités en O(n).

4. **Optimisation ciblée** : J'ai réalisé l'importance d'adapter l'implémentation en fonction des opérations les plus fréquemment utilisées dans un contexte donné. Par exemple, si mon application nécessite beaucoup d'insertions mais peu de suppressions, une liste chaînée triée pourrait être préférable. À l'inverse, si je dois principalement récupérer l'élément le plus prioritaire rapidement et fréquemment, un tas binaire sera bien plus efficace. Cette réflexion m'a appris à ne pas chercher une solution universelle, mais plutôt la structure qui correspond le mieux au profil d'utilisation spécifique du problème à résoudre.

5. **Application pratique de la notation Big O** : Cette séance m'a permis d'appliquer concrètement les concepts théoriques de complexité algorithmique pour guider mes choix d'implémentation.


6. **Flexibilité des fonctions d'ordre supérieur** : J'ai mis en pratique l'utilisation de fonctions prenant d'autres fonctions comme paramètres, ce qui m'a permis de créer des structures adaptables à différents critères de priorité sans modifier leur implémentation.

7. **Développement guidé par les tests** : En écrivant des tests avant le code fonctionnel, j'ai pu clarifier mes objectifs et vérifier systématiquement que mes implémentations répondaient correctement aux comportements attendus de mes structures de données.


## **séance 3**

La séance 3 était entièrement consacrée à la réalisation d'un projet pratique. Contrairement aux séances précédentes qui introduisaient de nouveaux concepts théoriques, cette séance m'a offert l'opportunité d'appliquer concrètement les connaissances acquises lors des séances 1 et 2.

## Séance 4 : Les Structures de Type Arbre

> [!NOTE]
> Les informations présentées ci-dessous sur les structures des arbres sont inspirées d'une excellente vidéo éducative disponible sur YouTube *[vidéo 1](https://youtu.be/Z1Znyx11pvM?si=gkfaeevGgOd2N1kz)* , *[vidéo 2](https://youtu.be/MayJowPcAwM?si=Pxr1Hp7AqbL3bQnT)*.



Après avoir implémenté un arbre binaire dans le contexte du tas binaire pour la file de priorité, cette séance est entièrement consacrée à l'exploration approfondie des structures de type arbre, et plus particulièrement de l'arbre binaire de recherche.

## Définition concise d'un arbre

Un arbre est une structure de données hiérarchique non linéaire composée de nœuds reliés entre eux par des liens, sans former de cycle. Il commence par un nœud spécial appelé racine (root), à partir duquel se développent des branches vers d'autres nœuds. Dans un arbre binaire, chaque nœud parent contient typiquement deux pointeurs dirigés vers ses nœuds enfants (généralement désignés comme enfant gauche et enfant droit). Ces pointeurs représentent les connexions ou arêtes de l'arbre et permettent de naviguer dans la structure. Cette organisation permet de représenter efficacement des relations hiérarchiques entre les données et offre des avantages significatifs pour le stockage et la recherche d'informations.


## Terminologie clé pour comprendre les structures d'arbre

Voici quelques termes fondamentaux qui m'ont été particulièrement utiles pour comprendre le concept et le fonctionnement des structures arborescentes :

* **Racine (Root)** : Le nœud supérieur de l'arbre, point de départ de toute la structure.
* **Parent / Enfant** : Un nœud parent est directement connecté à un ou plusieurs nœuds enfants situés au niveau inférieur.
* ** Leaf node ** : Nœud qui n'a pas d'enfants.
* **Nœud interne** : Tout nœud qui n'est ni un leaf node ni la racine.
* **Hauteur d'un arbre** : La longueur du plus long chemin de la racine jusqu'à une feuille.
* **Profondeur d'un nœud** : Distance (nombre d'arêtes) entre ce nœud et la racine.

## **séance 6**

> [!NOTE]
> Non réalisée

## **séance 7**

À travers cette séance, nous avons couvert multiple algorithmes de tri qui illustrent parfaitement l'évolution de la pensée algorithmique, du plus naïf au plus sophistiqué.

### Exploration Progressive

J'ai commencé par le **Tri à Gobelets **, qui nous a servi de référence pour comprendre ce qu'il ne faut jamais faire. Cette approche purement aléatoire nous a montré les dangers d'une méthode sans stratégie.

Le **Tri par Sélection** m'a ensuite introduit aux algorithmes déterministes simples. Bien qu'inefficace pour de grandes données, il représente une première approche méthodique et prévisible du problème du tri.

Enfin, le **Tri Rapide (Quick Sort)** m'a fait découvrir l'élégance des algorithmes utilisant la stratégie "diviser pour régner", montrant comment une approche intelligente peut transformer radicalement les performances.

Cette progression m'a permis de comprendre concrètement pourquoi certains algorithmes deviennent impraticables avec l'augmentation des données, tandis que d'autres restent efficaces même sur de gros volumes. Nous avons également exploré les différentes classes de complexité algorithmique, de la constante à l'exponentielle, pour développer une intuition sur le comportement des algorithmes.

## Takeaways de la Séance 7

### Leçons Fondamentales

**L'importance du choix algorithmique** : La différence entre un bogosort O(n!) et un quicksort O(n log n) peut représenter l'écart entre un algorithme totalement inutilisable et un algorithme utilisable en production pour le même problème.

**Comportement asymptotique** : La complexité révèle comment un algorithme "scale" - une différence négligeable sur 10 éléments devient dramatique sur 10 000 éléments.
