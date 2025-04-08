# Fichier d'en-tête
## Définition des fonctions de la file d'attente

```c
#ifndef QUEUE_H
#define QUEUE_H

#include <stdlib.h>
#include <stdio.h>

//Invariant de structure: pour toute Queue initialisée, size(q)>=0

struct Queue; //déclaration du nom de ma structure
typedef struct Queue* Queue_t; //déclaration d'un alias qui désigne le type "pointeur vers struct Queue"

//PRE: /
//POST: initialise une Queue vide et renvoie un pointeur vers celle-ci
Queue_t new_queue();

//PRE: q est initialisé
//POST: renvoie le nombre d'éléments présents dans q. q n'est pas modifiée.
int size(Queue_t queue);

//PRE: q est initialisé
//POST: size(q') = size(q)+1 et le dernier élément de q est i. Le reste n'a pas été modifié
void enqueue(Queue_t queue, int i);

//PRE: q est initialisé, size(q)>0
//POST: renvoie le premier élément de q (élément le plus anciennement mis dans q) et l'enlève de q.
//      size(q')=size(q)-1
//      Le reste n'est pas modifié.
int dequeue(Queue_t queue);

//PRE: q est initialisée
//POST: toute la mémoire allouée pour q est libérée
void destroy_queue(Queue_t queue);

#endif
```