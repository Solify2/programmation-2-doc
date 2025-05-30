# Séance 7: algorithmes de tri

Dans cette séance, nous allons implémenter différents algorithmes de tri en progressant du plus naïf au plus sophistiqué. L'objectif principal sera d'analyser la complexité temporelle de chaque algorithme en mesurant le nombre d'opérations nécessaires selon la taille des données à traiter.

Nous étudierons le comportement asymptotique de chaque algorithme dans le pire des cas : comment un tri de complexité O(n²) nécessite 100 opérations pour 10 éléments mais 2500 pour 50 éléments, révélant une croissance quadratique dramatique. En comparant concrètement des algorithmes O(n²), O(n log n) et même O(n×n!) avec notre Bogosort, nous observerons l'impact direct de la complexité sur les performances réelles.

Cette approche comparative nous permettra de comprendre pourquoi certains algorithmes deviennent impraticables avec l'augmentation des données, et comment choisir l'algorithme optimal selon les contraintes du problème. À travers cette exploration pratique, nous développerons une intuition solide pour anticiper le comportement de nos programmes face à la croissance des volumes de données.


## Algorithme de Tri à Gobelets


Le tri à gobelets,est un algorithme de tri basé sur une approche purement aléatoire. le principe consiste à vérifier si le tableau est trié, et si ce n'est pas le cas, on va effectuer des échanges aléatoires entre 2 éléments jusqu'à obtenir, par pure chance, la configuration triée.

### Implémentation

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int is_array_sorted(int *array, int size) {
    for (int i = 0; i < size - 1; i++) {
        if (array[i] > array[i + 1]) {
            return 0;  // Non trié
        }
    }
    return 1; 
}

int tri_gobelets (int array[], int size) {
    int nb_permutation = 0;
    srand(time(NULL));
    
    while (!is_array_sorted(array, size)) {

        int index1 = rand() % size;
        int index2 = rand() % size;
        
        int temp = array[index1];
        array[index1] = array[index2];
        array[index2] = temp;
        
        nb_permutation++;
    }
    
    return nb_permutation;
}

void print_array(int array[], int size) {
    for (int i = 0; i < size; i++) {
        printf("%d ", array[i]);
    }
    printf("\n");
}

int main() {
    int size = 7;
    int data[] = {2, 10, 4, 1, 7, 22, 6};
    
    printf("=== TRI À GOBELETS === \n");
    
    printf("Tableau initial : \n ");
    print_array(data, size);
    
    printf("Tri en cours...\n");
    int nb_permutation = tri_gobelets(data, size);
    
    printf("\n Tableau trié : ");
    print_array(data, size);
    
    printf("Nombre d'échanges effectués : %d\n", nb_permutation);
    
    return 0;
}
```

```txt
=== TRI À GOBELETS === 
Tableau initial : 
 2 10 4 1 7 22 6 
Tri en cours...
Tableau trié : 1 2 4 6 7 10 22 
Nombre d'échanges effectués : 10140
```


```txt
=== TRI À GOBELETS === 
Tableau initial : 
 2 10 4 1 7 22 6 44 18 36 
Tri en cours...

Tableau trié : 
1 2 4 6 7 10 18 22 36 44 

Nombre d'échanges effectués : 2270129
```

### Détermine si cet algorithme se termine dans tous les cas.

Il peut théoriquement s'exécuter indéfiniment, même si la probabilité d'une exécution infinie est nulle, Chaque échange a une petite chance de créer le bon arrangement

Chaque échange a une petite chance de créer le bon arrangement, et cette chance existe toujours, donc statistiquement ça finira par marcher. Mais pour une exécution donnée, on pourrait avoir une malchance infinie et ne jamais tomber sur la bonne combinaison.

### Détermine l’évolution du nombre de permutations nécessaires pour trier ce tableau par rapport à la taille du tableau fourni en entrée.

Le nombre moyen de permutations nécessaires est d'environ n!/2, ce qui donne une croissance explosive (3 éléments → ~3 permutations, 10 éléments → ~1,8 million de permutations).



### Que conclure sur l’efficacité de cet algorithme ?

Dès 10-12 éléments, nécessitant des millions d'opérations là où les algorithmes efficaces en font quelques centaines.


## 2.Complexité d’un algorithme

### Trouve des algorithmes simples qui correspondent aux classes de complexités usuelles suivantes :

O(1), O(log(n)), O(n), O(n log(n)), O(n²), O(2n)

- `O(1)` - Constant : Accès à un élément d'un tableau par son index
- `O(log n)` - Logarithmique : Recherche dichotomique dans un tableau trié
- `O(n)` - Linéaire : Recherche linéaire dans un tableau
- `O(n²)` - Tri à bulles 
- `O(2^n)` - Exponentielle : Fibonacci


## Tri par sélection

Ce tri consiste à trouver le plus petit éléments du tableau et à le mettre au début, puis à trouver le
deuxième plus petit et à le mettre en deuxième position etc, jusqu’à la fin.
-Implémente cet algorithme de tri sous la forme d’une fonction qui prend en arguments un tableau
et sa taille et qui renvoie le nombre de comparaisons qui ont été effectuées

### Détermine si cet algorithme se termine dans tous les cas

**Oui, l'algorithme se termine dans tous les cas.** Le tri par sélection possède une structure déterministe avec une boucle principale qui s'exécute exactement n-1 fois, où i progresse de 0 à n-2 par incréments fixes de 1. À chaque itération, la fonction min parcourt un nombre déterminé d'éléments pour trouver le minimum dans la partie non triée du tableau. Contrairement au tri à gobelets qui dépend du hasard, aucune condition aléatoire ou imprévisible n'influence le déroulement de l'algorithme. Le nombre total d'opérations est parfaitement prévisible garantissant une terminaison certaine en un temps fini et calculable à l'avance.


### Détermine la complexité algorithmique de cet algorithme

**La complexité du tri par sélection est `O(n²)` dans tous les cas.** L'algorithme effectue une boucle principale de n-1 itérations, et à chaque itération i, la fonction min doit parcourir les éléments restants pour trouver le minimum, soit n-i-1 comparaisons. Le nombre total de comparaisons est donc (n-1) + (n-2) + (n-3) + ... + 1, ce qui correspond à la somme arithmétique n(n-1)/2, donnant asymptotiquement `O(n²)`. Cette complexité reste identique que le tableau soit déjà trié, partiellement ordonné ou complètement désorganisé, car l'algorithme doit systématiquement examiner tous les éléments restants pour garantir qu'il a trouvé le minimum. La complexité spatiale est O(1) puisque le tri s'effectue en place avec seulement quelques variables temporaires supplémentaires.

### Évalue expérimentalement sa complexité moyenne (en nombre de comparaisons effectuées)

**Le tri par sélection effectue toujours exactement n(n-1)/2 comparaisons, peu importe l'organisation initiale du tableau.** Cette constance confirme expérimentalement la complexité théorique `O(n²)` et distingue cet algorithme des autres tris dont les performances varient selon les données d'entrée.


```c
#include <stdio.h>

// Fonction qui trouve l'index du minimum à partir de la position start
int min(int tab[], int start, int taille) {
    int min_index = start;
    for (int j = start + 1; j < taille; j++) {
        if (tab[j] < tab[min_index]) {
            min_index = j;
        }
    }
    return min_index;
}

int tri_selection(int tab[], int taille) {
    int i = 0;
    int nb_op = 0;

    while (i < taille - 1) {  // Correction: pas besoin de traiter le dernier élément
        // n + (n-1) + (n-2) + (n-3) etc.....
        int minimum = min(tab, i, taille); // o(n)

        // Échange seulement si nécessaire
        if (minimum != i) {
            int tmp = tab[i];
            tab[i] = tab[minimum];
            tab[minimum] = tmp;
        }

        i++;
        nb_op += (taille - i);  // Correction: nombre réel de comparaisons
    }

    return nb_op;
}

void print_array(int tab[], int taille) {
    for (int i = 0; i < taille; i++) {
        printf("%d ", tab[i]);
    }
    printf("\n");
}

int main() {
    int data[] = {64, 25, 12, 22, 11, 90, 5};
    int size = sizeof(data) / sizeof(data[0]);

    printf("=== TRI PAR SÉLECTION ===\n\n");

    printf("Tableau initial : ");
    print_array(data, size);

    int operations = tri_selection(data, size);

    printf("Tableau trié : ");
    print_array(data, size);

    printf("Nombre d'opérations : %d\n", operations);

    return 0;
}
```

```txt
=== TRI PAR SÉLECTION ===

Tableau initial : 64 25 12 22 11 90 5 
Tableau trié : 5 11 12 22 25 64 90 
Nombre d'opérations : 21
```


## Quick sort

CCe tri récursif consiste, si le tableau a une taille de 1, à renvoyer le tableau. Sinon, choisir un
élément "p" qui sera le pivot. Placer tous les éléments du tableau plus petit que "p" à gauche de "p"
et tous les éléments plus grands à droite. Puis réappliquer l’algorithme sur le sous-tableau à gauche
de "p" et puis sur le sous-tableau à droite de "p".

### Détermine la complexité algorithmique de cet algorithme


* **Complexité du pire cas [Big-O]** : `O(n²)` Elle survient lorsque l'élément pivot choisi est soit le plus grand soit le plus petit élément. Cette condition conduit au cas où l'élément pivot se trouve à une extrémité du tableau trié. Un sous-tableau est toujours vide et l'autre sous-tableau contient `n - 1` éléments. Ainsi, quicksort n'est appelé que sur ce sous-tableau. Cependant, l'algorithme quicksort a de meilleures performances avec des pivots dispersés.

* **Complexité du meilleur cas [Big-omega]** : `O(n*log n)` Elle survient lorsque l'élément pivot est toujours l'élément du milieu ou proche de l'élément du milieu.

* **Complexité du cas moyen [Big-theta]** : `O(n*log n)` Elle survient lorsque les conditions ci-dessus ne se produisent pas.


### Évalue expérimentalement sa complexité moyenne (en nombre de comparaisons effectuées)

**Quick Sort effectue en moyenne un nombre de comparaisons proportionnel à n log n.** Cette performance le rend très efficace en pratique, largement supérieur aux algorithmes O(n²), malgré son pire cas quadratique qui reste rare avec un bon choix de pivot.


```c
#include <stdio.h>

void swap(int tab[], int a, int b) {
    int temp = tab[a];
    tab[a] = tab[b];
    tab[b] = temp;
}

void QuickSort(int tab[], int i, int j) {
    if (i >= j){
        return;
    } 
    
    int max = j;
    int min = i;
    
    int pivot = tab[i];
    int left = i + 1;
    int right = j;
    
    while (left <= right) {
        
        if (tab[left] <= pivot) { 
            left++;
        } else {
            swap(tab, left, right);
            right--;
        }
    }
    
    swap(tab, min, right);
    
    QuickSort(tab, min, right - 1);
    QuickSort(tab, right + 1, max);
}

void print_array(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");
}

int main() {
    int data[] = {64, 34, 25, 12, 22, 11, 90};
    int size = sizeof(data) / sizeof(data[0]);
    
    printf("=== TRI RAPIDE (QUICK SORT) ===\n\n");
    
    printf("Tableau initial : ");
    print_array(data, size);
    
    QuickSort(data, 0, size - 1);
    
    printf("Tableau trié : ");
    print_array(data, size);
    
    return 0;
}
```

```txt
=== TRI RAPIDE (QUICK SORT) ===

Tableau initial : 64 34 25 12 22 11 90 
Tableau trié : 11 12 22 25 34 64 90 
```




