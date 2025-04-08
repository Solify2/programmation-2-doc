# Implémentation d’une file d’attente (Queue) basée sur un tableau (array)

Dans l'implémentation ci-dessous, la queue est implémentée en utilisant un tableau de taille fixe (`#define MAX_SIZE 100`). La queue est représentée par une structure contenant deux éléments :

1. Un tableau qui représente l'ensemble des éléments dans la file (des entiers dans cet exemple).
2. Un entier (`tail`), permettant de garder une trace du dernier élément inséré dans la file.

| Avantages | Inconvénients |
|-----------|--------------|
| Facile à implémenter | Capacité de stockage limitée |
| Ne demande pas de connaissances sur d'autres structures (ex: liste chaînée) | Allocation mémoire statique |
| Certaines opérations peuvent être réalisées en temps constant (`enqueue`) | Temps linéaire dû au décalage de tous les éléments restants |



Inconvinent:



```c
#include <stdlib.h>
#include "queue.h"
#define MAX_SIZE 100

struct Queue {
  int elements[MAX_SIZE];
  int tail;
};

Queue_t new_queue() {
  Queue_t queue = malloc(sizeof(struct Queue));
  queue->tail = -1;
  return queue;
}

int size(Queue_t queue) {
  if (queue == NULL || queue->tail == -1) {
    return 0;
  }
  return (queue->tail +1);
}


void enqueue(Queue_t queue, int element) {

  if (queue == NULL) {
    printf("Queue invalide\n");
    return;
  }

  if (queue->tail == MAX_SIZE - 1) {
    printf("Queue Rempli!\n");
  }

  queue->tail++;
  queue->elements[queue->tail] = element;
}

int dequeue(Queue_t queue) {
  if (queue == NULL || size(queue) ==0) {
    printf("Queue invalide (vide/null)\n");
    return 0;
  }

  int item = queue->elements[0];
  for (int i = 0; i < queue->tail ; i++) {
    queue->elements[i] = queue->elements[i + 1];
  }
  queue->tail--;
  return item;

}

int main(){

  printf("Enqueue dans une queue NULL \n");
  enqueue(NULL,1);
  printf("-------------------\n");

  printf("Initialisé une queue \n");
  Queue_t queue = new_queue();
  printf("Taille:%i\n",size(queue));

  enqueue(queue,3);
  printf("Enqueue : 3  -> size: %d \n", size(queue));

  enqueue(queue,5);
  printf("Enqueue : 5  -> size: %d \n", size(queue));

  enqueue(queue,2);
  printf("Enqueue : 2  -> size: %d \n", size(queue));
  printf("-------------------\n");

  int a = dequeue(queue);
  printf("dequeue : %d  -> size: %d \n", a,size(queue));
  int b = dequeue(queue);
  printf("dequeue : %d  -> size: %d \n", b,size(queue));
  int c = dequeue(queue);
  printf("dequeue : %d  -> size: %d \n", c,size(queue));
  printf("-------------------\n");

  enqueue(queue,1);
  printf("Enqueue : 1  -> size: %d \n", size(queue));
  int d = dequeue(queue);
  printf("dequeue : %d  -> size: %d \n", d,size(queue));
  printf("-------------------\n");

  printf("Dequeue depuis une queue vide:\n");
  dequeue(queue);

  free(queue);
  return 0;
}
```



### Voici un exemple d'exécution d’une file d’attente (Queue) basée sur un tableau (array)

```txt
Enqueue dans une queue NULL 
Queue invalide
-------------------
Initialisé une queue 
Taille:0
Enqueue : 3  -> size: 1 
Enqueue : 5  -> size: 2 
Enqueue : 2  -> size: 3 
-------------------
dequeue : 3  -> size: 2 
dequeue : 5  -> size: 1 
dequeue : 2  -> size: 0 
-------------------
Enqueue : 1  -> size: 1 
dequeue : 1  -> size: 0 
-------------------
Dequeue depuis une queue vide:
Queue invalide (vide/null)

Process finished with exit code 0
```