
# Introduction - file de priorité (priority queue)


Une file de priorité (Priority Queue) est une structure de données (ADT) qui permet de stocker des éléments **en fonction d’une valeur de priorité**. Chaque élément a une priorité, et c’est cette priorité qui détermine l’ordre dans lequel les éléments sont traités.

Par exemple, si on a une file de priorité contenant des entiers, on peut décider que l’élément le plus grand est celui avec la plus haute priorité. Mais on peut aussi aller plus loin et créer des implémentations plus complexes, où la priorité dépend de plusieurs critères.



> [!IMPORTANT]
> En C, on peut représenter ça avec une struct dans laquelle on définit plusieurs propriétés. On verra justement un exemple concret de ça dans l’exercice sur la gestion de livres, où la priorité sera basée sur plusieurs champs.

L’idée principale, c’est que les éléments avec la priorité la plus élevée sont retirés (défilés) en premier.

Il existe plusieurs façons d’implémenter cette structure. Une des plus courantes, c’est le tas binaire (binary heap), mais avant d’en arriver là, on va d’abord l’implémenter avec un simple tableau (array), ensuite, on améliore ça avec une liste chaînée et enfin on passera au tas binaire.

> [!NOTE]
> L’objectif de ces différentes versions, c’est de voir comment améliorer notre solution petit à petit, en réfléchissant à des critères comme la complexité en temps.
Par exemple, est-ce qu’une certaine implémentation reste efficace si on a une file contenant 100 millions d’éléments ? C’est ce genre de questions qu’on va explorer au fur et à mesure.


### Implémentations de la file de priorité:

* *[Implémentation d'une file de priorité à l'aide d'un tableau (Array)](https://solify2.github.io/programmation-2-doc/docs/s%C3%A9ance_2/pq/pq_array/)*.
* *[Implémentation d’une file de priorité basée sur une liste chaînée](https://solify2.github.io/programmation-2-doc/docs/s%C3%A9ance_2/pq/4_pq_list/)*.
* *[Implémentation d’une file de priorité avec un tas binaire (Binary Heap)](https://solify2.github.io/programmation-2-doc/docs/s%C3%A9ance_2/pq/5_pq_tree/)*.
* *[Exercice : Gestion des livres avec une file de priorité basée sur un tas binaire](https://solify2.github.io/programmation-2-doc/docs/s%C3%A9ance_2/pq/6_pq_books/)*.