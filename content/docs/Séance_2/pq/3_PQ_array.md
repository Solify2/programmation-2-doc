# 1. Implémentation d'une file de priorité à l'aide d'un tableau (Array)

La première implémentation, la plus simple, consiste à utiliser un tableau (Array). Dans ce cas, à chaque fois que l'on insère un élément, on doit  trouver directement la bonne position pour l’insérer. Dans ce cas là, cela implique de parcourir tout ou partie du tableau, ce qui rend l'insertion assez coûteuse. De plus, lorsqu’on retire l’élément avec la plus haute priorité, il faut décaler tous les éléments qui le suivaient. Ces deux opérations **insertion et suppression** peuvent rendre cette approche peu performante lorsque le nombre d’éléments devient important.


```c
#include <stdlib.h>
#include <stdio.h>
#include "PQ.h"

#define MAX_SIZE 100
struct PQ {
    int *elements;
    int size;
    int maxsize;
    int (*leq)(int, int);
};

PQ_t create(int (*leq)(int,int)){
  PQ_t pq = malloc(sizeof(struct PQ));
  pq->elements = malloc(MAX_SIZE * sizeof(int));

  if (pq->elements == NULL) {
        free(pq);
        return NULL;
  }

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

    int i = pq->size - 1;
    while (i >= 0 && pq->leq(element, pq->elements[i])) {
        pq->elements[i + 1] = pq->elements[i];
        i--;
    }
    pq->elements[i + 1] = element;
    pq->size++;
}

int serve(PQ_t pq) {
    if (pq==NULL || pq->size==0) {
        printf("PQ invalide (empty/null)\n");
        return 0;
    }
    int element_pq = pq->elements[0];
    for (int i = 1; i <= pq->size; i++) {
        pq->elements[i-1] = pq->elements[i];
    }
    pq->size--;
    return element_pq;
}

int peek(PQ_t pq) {
    if (pq==NULL || pq->size==0) {
        printf("PQ invalide (empty/null)\n");
        return 0;
    }

    int element_pq = pq->elements[0];
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
    printf("Priority Queue: ");
    for (int i = 0; i < pq->size; i++) {
        printf("%d ", pq->elements[i]);
    }
    printf("\n");
}

int main() {
    PQ_t pq = create(min_priority);
    printf("Enqueu elements: \n");
    enqueue(pq, 10);
    print(pq);
    enqueue(pq, 2);
    print(pq);
    enqueue(pq, 6);
    print(pq);
    enqueue(pq, 5);
    print(pq);
    enqueue(pq, 4);
    print(pq);

    printf("peek(): %d\n", peek(pq));
    printf("serve(): %d\n", serve(pq));
    print(pq);
    printf("peek(): %d\n", peek(pq));
    printf("serve(): %d\n", serve(pq));
    print(pq);
    printf("peek(): %d\n", peek(pq));
    printf("serve(): %d\n", serve(pq));
    print(pq);
    printf("peek(): %d\n", peek(pq));
    printf("serve(): %d\n", serve(pq));
    print(pq);
    printf("peek(): %d\n", peek(pq));
    printf("serve(): %d\n", serve(pq));
    print(pq);
    printf("peek(): %d\n", peek(pq));
    printf("serve(): %d\n", serve(pq));

    free(pq);
}
```

### Voici un exemple d'exécution d’une PQ basée sur un tableau (array)

```txt
Enqueu elements: 
Priority Queue: 10 
Priority Queue: 2 10 
Priority Queue: 2 6 10 
Priority Queue: 2 5 6 10 
Priority Queue: 2 4 5 6 10 
peek(): 2
serve(): 2
Priority Queue: 4 5 6 10 
peek(): 4
serve(): 4
Priority Queue: 5 6 10 
peek(): 5
serve(): 5
Priority Queue: 6 10 
peek(): 6
serve(): 6
Priority Queue: 10 
peek(): 10
serve(): 10
Priority Queue: 
PQ invalide (empty/null)
peek(): 0
PQ invalide (empty/null)
serve(): 0
```