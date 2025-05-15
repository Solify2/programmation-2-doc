
# Séance 4: tableaux associatifs et arbres binaires de recherche

```c
#include <stdio.h>
#include "./annuaire.h"
#include <stdlib.h>


typedef struct Node {
    char* key;
    char* value;
    struct Node* left;
    struct Node* right;
} Node;


struct Dict {
    Node_t root;
    int size;
} Dict;


Dict_t new_dict() {
    Dict_t dict = malloc(sizeof(Dict));
    if (dict == NULL) {
        printf("Erreur lors de l'allocation mémoire.\n");
        return NULL;
    }

    dict->root = NULL;
    dict->size = 0;

    printf("Dictionnaire a créé avec succès.\n");
    return dict;

}

bool dict_add(Dict_t dict, char *key, char *value) {
    if (dict==NULL) {
        printf("Invalid Dict...");
        return false;
    }

    Node_t new_node = malloc(sizeof(Node));
    if (new_node == NULL) {
        printf("Erreur lors de l'allocation mémoire .\n");
        return NULL;
    }

    new_node->key = key;
    new_node->value = value;
    new_node->left = NULL;
    new_node->right = NULL;


    // Si l'arbre est vide le premier élement à inserer serait la racine

    if (dict->root==NULL) {
        dict->root = new_node;
        dict->size++;
        printf("Élément ajouté comme racine: clé=%s, valeur=%s ...\n", key, value);
        return true;
    }

    Node_t current_node = dict->root;
    Node_t parent_node = NULL;

    while (current_node!=NULL) {
        if (current_node->key==key) {
            printf("Erreur: La clé %s existe déjà dans le dictionnaire ...\n", key);
            return false;
        }

        parent_node = current_node;

        if (key < current_node->key) {
            current_node = current_node->left;
        } else {
            current_node = current_node->right;
        }
    }

    if (key < parent_node->key) {
        parent_node->left = new_node;
    } else {
        parent_node->right = new_node;
    }

    dict->size++;
    printf("Élément ajouté: clé=%s, valeur=%s\n", key, value);
    return true;
}

char* dict_get(Dict_t dict, char *key) {

    if (dict==NULL) {
        printf("Invalid Dict...");
        return NULL;
    }

    Node_t current_node = dict->root;

    while (current_node!=NULL) {
        if (current_node->key==key) {
            printf("Clé %s trouvée, valeur: %s\n", key, current_node->value);
            return current_node->value;
        }
        else if (key > current_node->key) {
            current_node = current_node->right;
        }else {
            current_node = current_node->left;
        }
    }

    printf("Clé %s non trouvée dans le dictionnaire.\n", key);
    return NULL;
}

bool dict_modify(Dict_t dict, char *key, char *value) {

    if (dict==NULL) {
        printf("Invalid Dict...");
        return NULL;
    }

    Node_t current_node = dict->root;

    while (current_node!=NULL) {
        if (current_node->key==key) {
            current_node->value = value;
            printf("Valeur de la clé %s modifiée avec succès: %s\n", key, value);
            return true;
        }
        else if (key > current_node->key) {
            current_node = current_node->right;
        }else {
            current_node = current_node->left;
        }
    }

    printf("Clé %s non trouvée, impossible de modifier la valeur.\n", key);
    return NULL;
}


void unit_test_dict(Dict_t dict) {

    printf("\n#### Test d'ajout d'éléments ####\n");

    dict_add(dict, "Jean", "0422 777666");
    dict_add(dict, "Ana", "0455 222 333");
    dict_add(dict, "Molly", "0411 555 666");

    printf("\n--- Test d'ajout d'une clé existante ---\n");
    dict_add(dict, "Jean", "0422 777333");

    printf("\n--- Test de recherche d'éléments existants ---\n");
    dict_get(dict, "Ana");
    dict_get(dict, "Molly");

    printf("\n--- Test de recherche d'éléments existants ---\n");
    dict_get(dict, "Toto");

   printf("\n#### Test de la fonction modify ####\n");

    printf("\n--- Test de modification d'éléments existants ---\n");

    dict_get(dict, "Jean");
    dict_modify(dict, "Jean", "022 222111");
    dict_get(dict, "Jean");
    printf("\n------------------\n");
    dict_get(dict, "Molly");
    dict_modify(dict, "Molly", "033 333 444");
    dict_get(dict, "Molly");

    printf("\n--- Test de modification d'éléments inexistants ---\n");

    dict_get(dict, "Angela");
    dict_get(dict, "Anna");
}

int main() {

    printf("#### Test de création du dictionnaire et la fonction add ####\n");
    Dict_t dict = new_dict();

    if (dict == NULL) {
        printf("Impossible de créer le dictionnaire\n");
    }

    unit_test_dict(dict);
    free(dict);
    return 0;
}
```