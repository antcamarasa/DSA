# Data Structures and Algorithms

## Sommaire
- [Arrays & String](#arrays--string)
- [Hashing](#hashing)
- [Simple Sorting](#simple-sorting)
  - [Insertion Sort](#insertion-sort)
- [Recursion](#recursion)
- [Advanced Sorting](#advanced-sorting)
- [Stacks and Queues](#stacks-and-queues)
- [Linked List](#linked-list)
- [Binary Trees](#binary-trees)
- [2-3-4 Trees and external storage](#2-3-4-trees-and-external-storage)
- [AVL and Red-Black Trees](#avl-and-red-black-trees)
- [Heaps](#heaps)
- [Graphs](#graphs)

---

## Simple Sorting
### Insertion Sort
**Sous-menu**
- [Présentation](#insertion-sort--presentation)
- [Mauvaise interprétation (à éviter)](#insertion-sort--mauvaise-interpretation)
- [Version correcte (décalages, pas de swaps)](#insertion-sort--version-correcte)
- [Idée clé](#insertion-sort--idee-cle)

---

#### Présentation
<a id="insertion-sort--presentation"></a>

Le tri par insertion construit le tableau final trié **élément par élément** en maintenant un **préfixe trié**.  
Il est **moins efficace** que quicksort/heapsort/merge sort sur de grands jeux de données, mais reste appréciable pour sa **simplicité**, son efficacité sur **petits tableaux** ou **données presque triées**, sa **stabilité** et son caractère **en place** (in-place).

---

#### Mauvaise interprétation (à éviter)
<a id="insertion-sort--mauvaise-interpretation"></a>

On voit parfois une version qui **swappe** les éléments à chaque comparaison. Même si le tableau peut finir trié, ce **n’est pas** le tri par insertion : l’algorithme correct **décale** les éléments plus grands vers la droite puis **insère** la clé **une seule fois**.

---

#### Version correcte (décalages, pas de swaps)
<a id="insertion-sort--version-correcte"></a>

Exemple avec `arr = [5, 3, 8, 4, 2]` (tri croissant) :


- **État initial** : `[5, 3, 8, 4, 2]`
On part du principe que le premier élément 5 est trié. Donc la partie a gauche est trié, il reste a trié la partie de droit.
- Il faut imaginé la sous liste de gauche trié et la sous liste de droite non trié, comme cela
- `[5, | 3, 8, 4, 2]`

**Clé = 3 (i = 1)**
- `5 > 3` → décale `5` à droite → `[5, 5, 8, 4, 2]`
- Bord gauche atteint → insère `3` → `[3, 5, 8, 4, 2]`

**Clé = 8 (i = 2)**
- `5 ≤ 8` → aucun décalage → `[3, 5, 8, 4, 2]`

**Clé = 4 (i = 3)**
- `8 > 4` → décale `8` → `[3, 5, 8, 8, 2]`
- `5 > 4` → décale `5` → `[3, 5, 5, 8, 2]`
- `3 ≤ 4` → insère `4` → `[3, 4, 5, 8, 2]`

**Clé = 2 (i = 4)**
- `8 > 2` → décale `8` → `[3, 4, 5, 8, 8]`
- `5 > 2` → décale `5` → `[3, 4, 5, 5, 8]`
- `4 > 2` → décale `4` → `[3, 4, 4, 5, 8]`
- `3 > 2` → décale `3` → `[3, 3, 4, 5, 8]`
- Bord gauche atteint → insère `2` → `[2, 3, 4, 5, 8]`

- **Résultat final** : `[2, 3, 4, 5, 8]`

