
# Séance 3 : utilisation de listes chainées, files d’attente, piles, files de priorité





## Organisation du code

> [!TIP]
>Je dois reconnaître que l'organisation actuelle de mon code n'est pas optimale du point de vue de la maintenabilité et de la clarté. Pour améliorer la structure du projet, il serait judicieux de le découper en plusieurs modules fonctionnels, chacun avec ses propres fichiers d'en-tête (.h) et d'implémentation (.c).
>
>Une meilleure organisation pourrait suivre l'arborescence suivante :
>
> 1. Dossier `fighter/` contenant :
>   - `fighter.h` : déclarations des structures et des fonctions
>   - `fighter.c` : implémentation des fonctions liées aux combattants
>
> 2. Dossier `position/` contenant :
>   - `position.h` : définition de la structure position et fonctions associées
>   - `position.c` : implémentation des opérations sur les positions
>
> 3. Dossier `tir/` contenant :
>   - `tir.h` : déclarations des structures de tirs et file de priorité
>   - `tir.c` : implémentation des fonctions de gestion des tirs



Dans mon implémentation, j'ai conçu plusieurs structures qui forment l'architecture de mon jeu de combat. Voici une description détaillée de ces composants :

### struct Fighter
Cette structure représente un combattant dans le jeu et contient les propriétés essentielles suivantes :
- `name` : nom du combattant
- `pv` : points de vie du combattant
- `speed` : vitesse du combattant 
- `position` : coordonnées (x,y) actuelles du combattant

### struct FighterList
J'ai implémenté cette structure sous forme d'une liste chaînée qui permet de gérer efficacement l'ensemble des combattants présents dans la partie. Cette approche me permet d'ajouter ou de retirer des combattants dynamiquement sans avoir à redimensionner un tableau.

### struct Position
Définit les coordonnées spatiales d'une entité dans le jeu. Elle contient deux valeurs :
- `x` : position horizontale
- `y` : position verticale
Cette structure est utilisée à la fois par les combattants et les tirs.

### struct Node
Représente un noeud dans la liste chainée des combattants. Chaque noeud contient :
- Les données d'un combattant (Fighter pointeur)
- Un pointeur vers le nœud suivant
Cette structure est essentielle pour l'implémentation de la liste chaînée.

### struct Tir
Modélise un projectile dans le jeu avec les caractéristiques suivantes :
- `speed` : vitesse d'un tir
- `power` : puissance d'un tir
- `position` : coordonnées du tir

### struct PQ
Une file de priorité (Priority Queue) spécifiquement pour gérer les tirs. Cette structure permet de traiter les tirs selon leur priorité, par exemple en fonction de leur vitesse.

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#include "fighter_header.h"
#include <string.h>
#include <time.h>
#include <unistd.h>

#define MAX_PV 30
#define MAX_X 10
#define MAX_Y 10
#define MAX_SPEED 10
#define MAX_SIZE 50

typedef struct Postition{
    int x;
    int y;
} Position;

typedef struct Fighter{
    char name[50];
    int pv;
    int speed;
    Position position;
} Fighter;

typedef struct Node {
    Fighter* value;
    struct Node* next;
} Node;

typedef struct Fighter_list {
    Node* head;
} Fighter_list;

typedef struct Tir {
  int speed;
  int power;
  Position target;
} Tir;

typedef struct PQ {
  Tir_t *elements;
  int size;
  int capacity;
  int (*leq)(Tir_t, Tir_t);
} PQ;


// PQ

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

Tir_t left_child(Tir_t *data, int index){
  return data[get_left_child_index(index)];
}

Tir_t right_child(Tir_t *data, int index){
  return data[get_right_child_index(index)];
}

Tir_t parent(Tir_t *data,int index){
  return data[get_parent_index(index)];
}

void swap(Tir_t *arr, int index_1, int index_2){
  Tir_t temp = arr[index_1];
  arr[index_1] = arr[index_2];
  arr[index_2] = temp;
}

PQ_t create_pq(int (*leq)(Tir_t, Tir_t)) {
  PQ_t pq = malloc(sizeof(PQ));
  if (pq==NULL){
    return NULL;
  }
  pq->elements = malloc(MAX_SIZE * sizeof(Tir));

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


Tir_t serve(PQ_t pq){
  if (pq==NULL || pq->size==0) {
    printf("PQ invalide (empty/null)\n");
    return NULL;
  }
  Tir_t item = pq->elements[0];
  pq->elements[0] = pq->elements[pq->size - 1];
  pq->size--;
  heapify_down(pq);
  return item;
}

Tir_t peek(PQ_t pq){
  if (pq==NULL || pq->size==0) {
    printf("PQ invalide (empty/null)\n");
    return NULL;
  }
  return pq->elements[0];
}

void enqueue(PQ_t pq, Tir_t item){
  pq->elements[pq->size] = item;
  pq->size++;
  heapify_up(pq);
}


Fighter_t create_fighter(char* name) {
  Fighter_t fighter = malloc(sizeof(Fighter));
  strcpy(fighter->name, name);

  fighter->pv = rand() % (MAX_PV - 1) + 1;
  fighter->speed = rand() % (MAX_SPEED - 1) + 1;
  fighter->position.x = rand() % MAX_X;
  fighter->position.y = rand() % MAX_Y;
  return fighter;
}

Fighter_list_t create_fighter_list(){
  Fighter_list_t list = malloc(sizeof(Fighter_list));
  if(list == NULL){
    printf("");
    return NULL;
  }

  list->head = NULL;
  return list;
}

void add_fighter(Fighter_list_t list, Fighter_t fighter){
  Node_t new_node = malloc(sizeof(Node));
  if(new_node == NULL){
    printf("error.\n");
    return;
  }

  new_node->value = fighter;
  new_node->next = list->head;
  list->head = new_node;
}

void remove_dead_fighter(Fighter_list_t list) {
  if(list == NULL || list->head == NULL) {
    printf("error.\n");
    return;
  }

  Node_t current_fighter = list->head;
  Node_t previous_fighter = NULL;
  while(current_fighter != NULL) {
    Fighter_t fighter = current_fighter->value;
    if (fighter->pv < 0) {
      printf("The fighter %s is dead.\n", fighter->name);

      Node_t fighter_over = current_fighter;
      if (previous_fighter == NULL) {
        list->head = current_fighter->next;
        current_fighter = list->head;
      }else {
        previous_fighter->next = current_fighter->next;
        current_fighter = current_fighter->next;
      }
      free(fighter_over->value);
      free(fighter_over);

    }else {
      previous_fighter = current_fighter;
      current_fighter = current_fighter->next;
    }
  }

}

void print_fighters(Fighter_list_t list){
  Node_t current = list->head;
  while(current != NULL){
    Fighter_t fighter = current->value;
    printf("Nom-> %s , PV-> %d , Position-> (X:%d,Y:%d) , Vitesse: %d\n",
               fighter->name, fighter->pv, fighter->position.x, fighter->position.y, fighter->speed);
    current = current->next;
  }
}


void add_new_fighter(Fighter_list_t list) {
  if (list == NULL) {
    return;
  }
  if (rand() % 3 != 0) {
    return;
  }

  int sum_pv = 0;
  int nb_fighters = 0;
  int moyenne_pv = 0;

  Node_t current_fighter = list->head;
  while(current_fighter != NULL) {
    sum_pv += current_fighter->value->pv;
    nb_fighters++;
    current_fighter = current_fighter->next;
  }

  if (nb_fighters == 0) {
    return;
  }

  moyenne_pv = sum_pv / nb_fighters;

  Fighter_t new_fighter = malloc(sizeof(Fighter));
  char *names[] = {"new_fighter_1", "new_fighter_2", "new_fighter_3", "new_fighter_4", "new_fighter_5"};
  int len = sizeof(names) / sizeof(names[0]);
  char *name = names[rand()%len];
  strcpy(new_fighter->name, name);
  new_fighter->pv = moyenne_pv;
  new_fighter->speed = rand() % (MAX_SPEED - 1) + 1;
  new_fighter->position.x = rand() % MAX_X;
  new_fighter->position.y = rand() % MAX_Y;
  add_fighter(list, new_fighter);
}

void generate_tirs(Fighter_list_t list, PQ_t tir_queue) {
  if (list == NULL || list->head == NULL) {
    return;
  }

  Node_t current_fighter = list->head;
  while(current_fighter != NULL) {
    Fighter_t fighter = current_fighter->value;
    printf("%s\n",fighter->name);
    if (rand() % 2 == 0) {
      Tir_t new_tir = malloc(sizeof(Tir));
      new_tir->speed = rand() % MAX_SPEED + 1;
      new_tir->power =  rand() % 16 + 5;
      new_tir->target.x = rand() % MAX_X;
      new_tir->target.y = rand() % MAX_Y;
      enqueue(tir_queue, new_tir);

      printf("%s tire vers (%d,%d) [vitesse: %d | puissance: %d]\n",
       fighter->name, new_tir->target.x, new_tir->target.y,
       new_tir->speed, new_tir->power);

    }
    current_fighter = current_fighter->next;
  }

}

int tir_priority(Tir_t a, Tir_t b) {
  return a->speed >= b->speed;
}

void print_tirt(PQ_t pq) {
  for (int i = 0; i < pq->size; i++) {
    printf("Speed: %d , Power: %d \n", pq->elements[i]->speed, pq->elements[i]->power);
  }
  printf("\n");
}


void handle_tirs(Fighter_list_t list, PQ_t tirs) {

  if (tirs == NULL || tirs->size==0) {
    return;
  }

  Tir_t highest_spped = peek(tirs);

  if (highest_spped ==NULL) {
    return;
  }

  while (tirs->size > 0) {
    Tir_t current_tir = peek(tirs);
    if (current_tir->speed != highest_spped->speed) {
      printf("");
      return;
    }
    current_tir = serve(tirs);

    Node_t fighter_node = list->head;
    while (fighter_node != NULL) {
      Fighter_t fighter = fighter_node->value;
      if (fighter->position.x == current_tir->target.x && fighter->position.y == current_tir->target.y) {
        fighter->pv = fighter->pv - current_tir->power;
        printf("💥 %s est touché à (%d,%d) et perd %d PV (reste %d PV)\n",
        fighter->name, fighter->position.x, fighter->position.y, current_tir->power, fighter->pv);
        break;

      }
      fighter_node = fighter_node->next;
    }
    free(current_tir);
  }
}

void main_loop(Fighter_list_t list, PQ_t tirs) {
  int iteration = 0;
  while (list != NULL && list->head != NULL) {
    printf("Iteration: %d\n", iteration);


    generate_tirs(list, tirs);
    handle_tirs(list, tirs);
    remove_dead_fighter(list);
    //add_new_fighter(list);
    print_fighters(list);
    iteration++;
  }
  printf("Game over there are no longer any fighters...\n");
}

int main(){
  srand(time(NULL));

  Fighter_list_t list = create_fighter_list();
  char *names[] = {"fighter_1", "fighter_2", "fighter_3", "fighter_4", "fighter_5"};
  int len = sizeof(names) / sizeof(names[0]);

  for(int i = 0; i < len; i++){
    Fighter_t fighter = create_fighter(names[i]);
    add_fighter(list, fighter);
  }
  PQ_t pq = create_pq(tir_priority);
  main_loop(list, pq);

  return 0;
}
```