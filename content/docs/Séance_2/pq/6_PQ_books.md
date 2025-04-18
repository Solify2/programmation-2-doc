# Exercice : Gestion des livres avec une file de priorité basée sur un tas binaire

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

#define MAX_SIZE 10

typedef struct {
    char title[100];
    int year;
    int pages;
} Book, *Book_t;

typedef struct {
    Book_t *elements;
    int size;
    int capacity;
    int (*leq)(Book_t, Book_t);
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

Book_t left_child(Book_t *data, int index){
    return data[get_left_child_index(index)];
}

Book_t right_child(Book_t *data, int index){
    return data[get_right_child_index(index)];
}

Book_t parent(Book_t *data,int index){
    return data[get_parent_index(index)];
}

void swap(Book_t *arr, int index_1, int index_2){
    Book_t temp = arr[index_1];
    arr[index_1] = arr[index_2];
    arr[index_2] = temp;
}



PQ_t create(int (*leq)(Book_t, Book_t)) {
    PQ_t pq = malloc(sizeof(PQ));
    if (pq==NULL){
      return NULL;
    }
    pq->elements = malloc(MAX_SIZE * sizeof(Book));

    if (!pq->elements) {
        free(pq);
        return NULL;
    }

    pq->size = 0;
    pq->capacity = MAX_SIZE;
    pq->leq = leq;

    return pq;
}

void heapify_up(PQ_t pq){
    int index = pq->size - 1;
    while(has_parent(index) &&  !(pq->leq(parent(pq->elements,index),pq->elements[index]))){
        swap(pq->elements,get_parent_index(index), index);
        index = get_parent_index(index);
    }
}


void heapify_down(PQ_t pq){
    int index = 0;
    while(has_left_child(index, pq->size)){
        int smaller_child_index = get_left_child_index(index);
        if(has_right_child(index, pq->size) && pq->leq(right_child(pq->elements,index), left_child(pq->elements,index))){
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

Book_t serve(PQ_t pq){
    if (pq==NULL || pq->size==0) {
        printf("PQ invalide (empty/null)\n");
        return NULL;
    }
    Book_t item = pq->elements[0];
    pq->elements[0] = pq->elements[pq->size - 1];
    pq->size--;
    heapify_down(pq);
    return item;
}

Book_t peek(PQ_t pq){
    if (pq==NULL || pq->size==0) {
        printf("PQ invalide (empty/null)\n");
        return NULL;
    }
    return pq->elements[0];
}

void enqueue(PQ_t pq, Book_t item){
    pq->elements[pq->size] = item;
    pq->size++;
    heapify_up(pq);
}


void add_book(PQ_t pq) {
    if (pq==NULL) {
        printf("Book invalide\n");
    }

    Book_t new_book = malloc(sizeof(Book));
    printf("Book title: ");
    scanf(" %s", new_book->title);
    printf("Book Year: ");
    scanf("%d", &(new_book->year));
    printf("NB pages: ");
    scanf("%d", &(new_book->pages));
    enqueue(pq, new_book);
    printf("Enqueu OK\n");
}


void suggest_book(PQ_t pq) {
    if (pq->size == 0) {
        printf("no books available, the PQ is emtpy.\n");
        return;
    }

    Book_t best_book = serve(pq);

    printf("Title: %s\n", best_book->title);
    printf("Year: %d\n", best_book->year);
    printf("Pages: %d\n\n", best_book->pages);
    free(best_book);
}

int book_priority(Book_t a, Book_t b) {
    if (a->year != b->year) {
        return a->year >= b->year;
    }
    return a->pages <= b->pages;
}

void print_book(PQ_t pq) {
    if (!pq || pq->size == 0) {
        printf("There is no books in the PQ.\n");
        return;
    }

    printf("Books in PQ:\n");
    for (int i = 0; i < pq->size; i++) {
        printf("Title: %s , Year: %d , Pages: %d\n", pq->elements[i]->title, pq->elements[i]->year, pq->elements[i]->pages);
    }
    printf("\n");
}

int main() {
    PQ_t pq = create(book_priority);

    int choice;
    do {
        printf("Type (1) to Add a new Book\n");
        printf("Type (2) to Get a Book Suggestion\n");
        printf("Type (3) to display all books\n");
        printf("Type (4) to close the program\n");
        printf("Enter Your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                add_book(pq);
                break;
            case 2:
                suggest_book(pq);
                break;
            case 3:
                print_book(pq);
                break;
            case 4:
                printf("Close\n");
                break;
            default:
                printf("Try again\n");
        }
    } while (choice != 4);

    free(pq);
    return 0;
}
```