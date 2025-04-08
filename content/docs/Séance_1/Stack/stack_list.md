```c
//
// Created by Ahmed Soleiman on 03/03/2025.
//

#include <stdio.h>
#include <stdlib.h>

#include "../list/list.h"
#include "../stack/stack.h"


typedef struct Node {
    int value;
    struct Node *next;
} Node;

typedef struct Stack {
    Node *head;
    int size;
} Stack;

Node_t create_node(int data) {
    Node_t new_node = malloc(sizeof(Node));
    new_node->value = data;
    new_node->next = NULL;
    return new_node;
}

Stack_t new_stack() {
    Stack_t stack = malloc(sizeof(Stack));
    stack->head = NULL;
    stack->size = 0;

    return  stack;
}

void push(Stack_t stack, int value) {
    Node_t new_node = create_node(value);
    if (stack->head == NULL) {
        stack->head = new_node;

    }else {
        new_node->next = stack->head;
        stack->head = new_node;
    }
    stack->size++;
}


int pop(Stack_t stack) {
    if(stack->size==0) {
        return -1;
    }
    int value = stack->head->value;
    Node_t old_head = stack->head;

    if (stack->size==1) {
        stack->head = NULL;
    }else {

        stack->head = stack->head->next;
    }
    free(old_head);
    stack->size--;
    return value;
}

int size(Stack_t stack) {
    return stack->size;
}

int main(int argc, char **argv) {
    printf("Hello World!\n");

    Stack_t stack = new_stack();

    push(stack,3);
    printf("Push 3 \n");
    printf("3 -> NULL\n");
    printf("Taille:%i\n",size(stack));
    printf("-------------\n");


    push(stack,5);
    printf("Push 5 \n");
    printf("5 -> 3 -> NULL\n");
    printf("Taille:%i\n",size(stack));
    printf("-------------\n");

    push(stack,1);
    printf("Push 1 \n");
    printf("1 -> 5 -> 3 -> NULL\n");
    printf("Taille:%i\n",size(stack));
    printf("-------------\n");



    int a = pop(stack);
    printf("Pop \n");
    printf("Élement enlevé:%i\n",a);
    printf("5 -> 3 -> NULL\n");
    printf("-------------\n");



    int b = pop(stack);
    printf("Pop \n");
    printf("Élement enlevé:%i\n",b);
    printf("3 -> NULL\n");
    printf("-------------\n");



    int c = pop(stack);
    printf("Pop \n");
    printf("Élement enlevé:%i\n",c);

}

```

### Voici un exemple d'exécution d'une stack basée sur une liste chainée


```txt
Push élément dans une stack NULL: 
Stack invalide (NULL)
-------------
Init Stack... 
-------------
Push 3 
size 1 
3 -> 
-------------
Push 5 
size 2 
5 -> 3 -> 
-------------
Push 1 
size 3 
1 -> 5 -> 3 -> 
-------------
Pop 
Élement enlevé:1
size 2 
5 -> 3 -> 
-------------
Pop 
Élement enlevé:5
size 1 
3 -> 
-------------
Pop 
Élement enlevé:3
size 0 
-------------
Pop depuis une stack vide...
Stack invalide (vide/null)
-------------
Pop depuis une référence NULL...
Stack invalide (vide/null)
-------------

Process finished with exit code 0
```

