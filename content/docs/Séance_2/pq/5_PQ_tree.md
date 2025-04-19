# Implémentation d'une file de priorité avec un tas binaire (Binary Heap)
> Une solution optimisée pour les performances, utilisant une structure de tas pour gérer les priorités efficacement.


La solution la plus **efficace** est le tas binaire (binary heap). C’est une structure de type arbre binaire implémentée avec un tableau, Le principe du tas est que chaque nœud respecte une propriété d’ordre : par exemple, dans un max-priority, chaque parent est toujours plus grand que ses enfants. Grâce à cette organisation, l’insertion comme la suppression se font en **temps logarithmique  O (log n)**, car il suffit de réorganiser une seule branche de l’arbre à chaque opération. C’est donc une structure idéale pour manipuler des files de priorité contenant un grand nombre d’éléments, avec des performances équilibrées.

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#define INIT_SIZE 100

typedef struct {
    int* elements;
    int size;
    int capacity;
    int (*leq)(int, int);
} PQ, *PQ_t;

int get_left_child_index(int parent){
    return (2 * parent) + 1;
}

int get_right_child_index(int parent){
    return (2 * parent) + 2;
}

int get_parent_index(int child_index){
    return (child_index -1) / 2;
}

bool has_left_child(int index,int size){
    return get_left_child_index(index) < size;
}

bool has_right_child(int index,int size){
    return get_right_child_index(index) < size;
}

bool has_parent(int index){
    return get_parent_index(index) >=0;
}

int get_left_child(int *elements, int index){
    return elements[get_left_child_index(index)];
}

int get_right_child(int *elements, int index){
    return elements[get_right_child_index(index)];
}

int get_parent(int *elements,int index){
    return elements[get_parent_index(index)];
}

void swap(int *arr, int index_1, int index_2){
    int temp = arr[index_1];
    arr[index_1] = arr[index_2];
    arr[index_2] = temp;
}

PQ_t create(int (*leq)(int, int)) {
    PQ_t pq = malloc(sizeof(PQ_t));
    pq->elements = malloc(INIT_SIZE * sizeof(int));

    if (pq->elements == NULL) {
        free(pq);
        return NULL;
    }

    pq->size = 0;
    pq->capacity = INIT_SIZE;
    pq->leq = leq;

    return pq;
}

void heapify_up(PQ_t pq){
    int index = pq->size - 1;
    while(has_parent(index) &&  !(pq->leq(get_parent(pq->elements,index),pq->elements[index]))){
        swap(pq->elements,get_parent_index(index), index);
        index = get_parent_index(index);
    }
}

void heapify_down(PQ_t pq){
    int index = 0;
    while(has_left_child(index, pq->size)){
        int smaller_child_index = get_left_child_index(index);
        if(has_right_child(index, pq->size) && pq->leq(get_right_child(pq->elements,index), get_left_child(pq->elements,index))){
            smaller_child_index = get_right_child_index(index);
        }

        if(pq->leq(pq->elements[index],pq->elements[smaller_child_index])){
            break;
        }
        else{
            swap(pq->elements,index, smaller_child_index);
        }
        index = smaller_child_index;
    }

}

int serve(PQ_t pq){
    if (pq==NULL || pq->size==0) {
        printf("PQ invalide (empty/null)\n");
        return 0;
    }

    int item = pq->elements[0];
    pq->elements[0] = pq->elements[pq->size - 1];
    pq->size--;
    heapify_down(pq);
    return item;
}

int peek(PQ_t pq){
    if (pq==NULL || pq->size==0) {
        printf("PQ invalide (empty/null)\n");
        return 0;
    }
    return pq->elements[0];
}

void enqueue(PQ_t pq, int item){
    pq->elements[pq->size] = item;
    pq->size++;
    heapify_up(pq);
}

void print(PQ_t pq) {
    printf("Priority Queue: ");
    for (int i = 0; i < pq->size; i++) {
        printf("%d ", pq->elements[i]);
    }
    printf("\n");
}

int min_priority(int a, int b) {
    return a <= b;
}

int max_priority(int a, int b) {
    return a >= b;
}

void unit_test(int *elements, int size, int (*leq)(int, int)) {
    PQ_t pq = create(leq);
    print(pq);
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

int main()
{
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

    return 0;
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