```c
#ifndef STACK_H
#define STACK_H

struct Stack;

typedef struct Stack* Stack_t;

Stack_t new_stack();

void push(Stack_t stack, int value);

int pop(Stack_t stack);

int size(Stack_t stack);

void destroy_stack(Stack_t stack);

void display_stack(Stack_t stack);



#endif //STACK_H
```