# Fichier d'en-tête

## Définition des fonctions de la liste chainée

```c
#ifndef LIST_H
#define LIST_H

struct List;
struct Node;
typedef struct List* List_t;
typedef struct Node* Node_t;

List_t new_list();
Node_t create_node(int value);

void insert_value_at_start(List_t list, int value);
void insert_value_at_end(List_t list, int value);
void insert_after(List_t list, Node_t node, int value);
void delete_node(Node_t node, int value);
void destory_list(List_t list);
void display_list(List_t list);

Node_t find_node(List_t list, int value);

#endif

```