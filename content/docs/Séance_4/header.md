```c
#ifndef DICT_H
#define DICT_H

#include <stdbool.h>

struct Dict;
struct Node;

typedef struct Dict* Dict_t; // déclaration de la structure Dict
typedef struct Node* Node_t; // déclaration del la structure Node

// initialise une Dict vide et renvoie un pointeur vers celle-ci
Dict_t new_dict();

// renvoie True si l'ajout est réussi, false si la clé existe déjà
bool dict_add(Dict_t dict, int key, char* value);

// renvoie la valeur associée à la clé
char* dict_get(Dict_t dict, char *key);

// renvoie True si la suppression est réussie, false si la clé n'existe pas
bool dict_remove(Dict_t dict, char *key);

// renvoie True si la modif est réussie, false si la clé n'existe pas
bool dict_modify(Dict_t dict, int key, char* value);


#endif //DICT_H
```