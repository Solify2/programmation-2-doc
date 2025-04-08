# Implémentation d’une file d’attente (Queue) basée sur une liste chaînée

Dans l'implémentation ci-dessous, la queue est implémentée en utilisant une liste chaînée. La queue est représentée par deux structures : `struct Node` et `struct Queue`.

- **Struct Node** : Représente un élément d'une liste contenant deux valeurs :
  1. La valeur de l'élément de la liste quel que soit son type (`int`, `char`, `double`, `array`, etc...).
  2. Un pointeur vers l'élément suivant de la liste, ayant le même type que la structure elle-meme (`struct Node*`) et cela qui va permettre de créer un système de chaînage.

- **Struct Queue** : Représente la queue dans sa totalité (un ensemble de noeuds) et contient les éléments suivants :
  1. **Head** : La tête de la liste, représentant le premier élément.
  2. **Tail** : Le dernier élément de la liste.
  3. **Size** : La longueur de la liste chaînée.

| Avantages | Inconvénients |
|-----------|--------------|
| Allocation dynamique de la mémoire | Moins facile à implémenter par rapport à l'implémentation avec un tableau |
| L'insertion et la suppression sont efficaces et ne nécessitent pas de décalage | Nécessite de parcourir toute la liste pour trouver un élément situé à la fin |
| Certaines opérations peuvent être réalisées en temps constant (`enqueue`, `dequeue`) | Gestion manuelle de la mémoire (`malloc`, `free`) |

```c
#include <stdlib.h>
#include "../list/list.h"
#include "queue.h"
#define MAX_SIZE 100

typedef struct Node {
    int value;
    struct Node *next;
} Node;

typedef struct Queue {
    Node *head;
    Node *tail;
    int size;
} Queue;

Queue_t new_queue() {
    Queue_t queue = malloc(sizeof(Queue));
    queue->head = NULL;
    queue->tail = NULL;
    queue->size = 0;
    return  queue;
}

int size(Queue_t queue) {
    if (queue == NULL || queue->size == 0) {
        return 0;
    }
    return queue->size;
}

void enqueue(Queue_t queue, int element) {
    Node_t new_node = malloc(sizeof(Node));
    new_node->value = element;

    if (queue == NULL) {
        printf("Queue invalide\n");
        return;
    }

    if(queue->size==0) {
        queue->head = new_node;
        queue->tail = new_node;
    }else {
        queue->tail->next = new_node;
        queue->tail = new_node;
    }
    queue->size++;
}

int dequeue(Queue_t queue) {
    if(queue == NULL || queue->size==0) {
        printf("Queue invalide (vide/null)\n");
        return 0;
    }
    int element = queue->head->value;
    Node_t old_head = queue->head;

    if (queue->size==1) {
        queue->head = NULL;
        queue->tail = NULL;
    }else {
        queue->head = queue->head->next;
    }
    free(old_head);
    queue->size--;
    return element;
}


int main(){
    Queue_t q = new_queue();
    printf("Taille:%d\n",size(q));
    enqueue(q,3);
    printf("Enqueue 3 \n");
    printf("3 -> NULL\n");
    printf("Taille:%d\n",size(q));
    printf("-------------\n");

    enqueue(q,5);
    printf("Enqueue 5 \n");
    printf("3 -> 5 -> NULL\n");
    printf("Taille:%d\n",size(q));
    printf("-------------\n");

    enqueue(q,1);
    printf("Enqueue 1 \n");
    printf("3 -> 5 -> 1 -> NULL\n");
    printf("Taille:%d\n",size(q));
    printf("-------------\n");

    int a = dequeue(q);
    printf("Dequeue \n");
    printf("Élement enlevé:%d\n",a);
    printf("5 -> 1 -> NULL\n");
    printf("-------------\n");

    int b = dequeue(q);
    printf("Dequeue \n");
    printf("Élement enlevé:%d\n",b);
    printf("1 -> NULL\n");
    printf("-------------\n");

    printf("Taille:%d\n",size(q));
    printf("Enqueue 6 \n");
    enqueue(q,6);
    printf("1 -> 6 -> NULL\n");
    printf("-------------\n");

    int c = dequeue(q);

    printf("Dequeue \n");
    printf("Élement enlevé:%d\n",c);
    printf("6 -> NULL\n");
    printf("-------------\n");

    int d = dequeue(q);
    printf("Élement enlevé:%d\n",d);
    printf("Taille:%d\n",size(q));
    printf("-------------\n");

    printf("Queue vide...\n");
    printf("Dequeue depuis une queue vide:\n");
    dequeue(q);
    printf("Taille:%d\n",size(q));

    free(q);
    return 0;
}
```

### Voici un exemple d'exécution d’une file d’attente (Queue) basée sur une liste chainée

```txt

Taille:0
Enqueue 3 
3 -> NULL
Taille:1
-------------
Enqueue 5 
3 -> 5 -> NULL
Taille:2
-------------
Enqueue 1 
3 -> 5 -> 1 -> NULL
Taille:3
-------------
Dequeue 
Élement enlevé:3
5 -> 1 -> NULL
-------------
Dequeue 
Élement enlevé:5
1 -> NULL
-------------
Taille:1
Enqueue 6 
1 -> 6 -> NULL
-------------
Dequeue 
Élement enlevé:1
6 -> NULL
-------------
Élement enlevé:6
Taille:0
-------------
Queue vide...
Dequeue depuis une queue vide:
Queue invalide (vide/null)
Taille:0
```
