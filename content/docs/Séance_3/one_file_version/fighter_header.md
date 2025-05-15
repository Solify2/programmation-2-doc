# Séance 3 - Fichier d’en-tête 

```c
#ifndef FIGHTER_H
#define FIGHTER_H

struct Position;
struct Fighter;
struct Node;
struct Fighter_list;
struct Tir;
struct PQ;


typedef struct Position* Position_t;
typedef struct Fighter* Fighter_t;
typedef struct Node* Node_t;
typedef struct Fighter_list* Fighter_list_t;
typedef struct Tir* Tir_t;
typedef struct PQ* PQ_t;

Fighter_t create_fighter(char* name);
Fighter_list_t create_fighter_list();

void add_fighter(Fighter_list_t fighters, Fighter_t fighter);

PQ_t create(int (*leq)(Tir_t,Tir_t));
void enqueue(PQ_t pq, Tir_t item);
Tir_t serve(PQ_t pq);
Tir_t peek(PQ_t pq);
Tir_t empty(PQ_t pq);

#endif //FIGHTER_H

```