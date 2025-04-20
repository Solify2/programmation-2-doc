# Implémentation d'une file de priorité basée sur une liste chaînée

Une deuxième implémentation est d’utiliser une liste chaînée, de nouveau, l’insertion d’un nouvel élément nécessite de parcourir la liste pour trouver la bonne position, par contre, *retirer l’élément prioritaire est beaucoup plus rapide*, car il se trouve toujours en tête (head) de la liste selon l’ordre de tri sans avoir besoin de déclaer les autres éléments de la list. On peut donc le retirer en temps constant. Cette solution est donc plus intéressante que le tableau si on a besoin de retirer souvent des éléments prioritaires, mais elle reste limitée en cas d'insertion.


Dans cette version, la file de priorité est implémentée à l’aide d’une **liste chaînée triée**. Chaque élément de la file est représenté par un nœud (`Node`) contenant une valeur entière (`value`) et un pointeur vers le nœud suivant (`next`).

La structure `PQ` représente la file elle-même. Elle contient :

- `head` : un pointeur vers le premier élément de la liste, c’est-à-dire l’élément avec la priorité la plus élevée (selon la fonction `leq`).
- `tail` : un pointeur vers le dernier élément de la file, utile pour optimiser l’insertion en fin de liste si besoin.
- `size` : le nombre actuel d’éléments dans la file.
- `maxsize` : le nombre maximal d’éléments que la file peut contenir.
- `leq` : un pointeur vers une fonction de comparaison utilisée pour maintenir l’ordre de priorité dans la liste à chaque insertion.

Grâce à cette structure, on peut facilement insérer un élément à la bonne position pour que la liste reste triée, et accéder rapidement à l’élément prioritaire en tête de liste.


```c
#include <stdlib.h>
#include <stdio.h>
#include "PQ.h"

#define MAX_SIZE 100

typedef struct Node {
    int value;
    struct Node *next;
} Node, *Node_t;

struct PQ {
    Node_t head;
    Node_t tail;
    int size;
    int maxsize;
    int (*leq)(int, int);
};

PQ_t create(int (*leq)(int,int)){
    PQ_t pq = malloc(sizeof(struct PQ));
    pq->head = NULL;
    pq->tail = NULL;
    pq->size = 0;
    pq->maxsize = MAX_SIZE;
    pq->leq = leq;

    return pq;
}

void enqueue(PQ_t pq, int element) {
    if (pq==NULL){
        printf("PQ invalide\n");
        return;
    }
    Node_t new_node = malloc(sizeof(Node));
    new_node->value = element;
    new_node->next = NULL;

    if (pq->head == NULL || pq->leq(element, pq->head->value)) {
        new_node->next = pq->head;
        pq->head = new_node;
        pq->size++;
        return;
    }

    Node_t current = pq->head;
    while (current->next && pq->leq(current->next->value, element)) {
        current = current->next;
    }

    new_node->next = current->next;
    current->next = new_node;
    pq->size++;
}

int serve(PQ_t pq) {
    if (pq==NULL || pq->size==0) {
        printf("PQ invalide (empty/null)\n");
        return 0;
    }
    int element = pq->head->value;
    Node_t old_head = pq->head;

    if (pq->size==1) {
        pq->head = NULL;
        pq->tail = NULL;
    }else {
        pq->head = pq->head->next;
    }
    free(old_head);
    pq->size--;
    return element;
}

int peek(PQ_t pq) {
    if (pq==NULL || pq->size==0) {
        printf("PQ invalide (empty/null)\n");
        return 0;
    }
    int element_pq = pq->head->value;
    return element_pq;
}

int min_priority(int a, int b) {
    return a <= b;
}

int max_priority(int a, int b) {
    return a >= b;
}

int empty(PQ_t pq) {
    if (pq->size == 0) {
        return 1;
    }
    return 0;
}

void print(PQ_t pq) {
    if (pq == NULL) {
        printf("Priority Queue is NULL.\n");
        return;
    }

    if (empty(pq)==1) {
        printf("Priority Queue is empty.\n");
        return;
    }

    printf("Priority Queue: ");
    Node_t current = pq->head;
    while(current!=NULL) {
        printf("%d ", current->value);
        current = current->next;
    }
    printf("\n");
}

void unit_test(int *elements, int size, int (*leq)(int, int)) {

    PQ_t pq = create(leq);

    for (int i=0; i<size; i++) {
        enqueue(pq,elements[i]);
    }

    print(pq);

    for (int i=0; i<size; i++) {
        printf("peek(): %d\n", peek(pq));
        printf("serve(): %d\n", serve(pq));
        print(pq);
    }
    free(pq);
}

int main() {
    // TEST avec max_priority
    printf("----------TEST avec max_priority----------------\n");
    int arr[] = {5,8,2,1,3,50,12};
    unit_test(arr, 7, max_priority);

    // TEST avec min_priority
    printf("----------TEST avec min_priority----------------\n");
    int arr2[] = {5,8,2,1,3,50,12};
    unit_test(arr2, 7, min_priority);

    printf("----------TEST array vide----------------\n");
    // TEST array vide
    int arr3[] = {};
    unit_test(arr3, 0, min_priority);
}
```





```txt
----------TEST avec max_priority----------------

Priority Queue: 50 12 8 5 3 2 1 
peek(): 50
serve(): 50
Priority Queue: 12 8 5 3 2 1 
peek(): 12
serve(): 12
Priority Queue: 8 5 3 2 1 
peek(): 8
serve(): 8
Priority Queue: 5 3 2 1 
peek(): 5
serve(): 5
Priority Queue: 3 2 1 
peek(): 3
serve(): 3
Priority Queue: 2 1 
peek(): 2
serve(): 2
Priority Queue: 1 
peek(): 1
serve(): 1
Priority Queue is empty.

----------TEST avec min_priority----------------

Priority Queue: 1 2 3 5 8 12 50 
peek(): 1
serve(): 1
Priority Queue: 2 3 5 8 12 50 
peek(): 2
serve(): 2
Priority Queue: 3 5 8 12 50 
peek(): 3
serve(): 3
Priority Queue: 5 8 12 50 
peek(): 5
serve(): 5
Priority Queue: 8 12 50 
peek(): 8
serve(): 8
Priority Queue: 12 50 
peek(): 12
serve(): 12
Priority Queue: 50 
peek(): 50
serve(): 50
Priority Queue is empty.

----------TEST array vide----------------

Priority Queue is empty.

Process finished with exit code 0
```