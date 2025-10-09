# Data Structures and Algorithms

- [Arrays & String](#arrays--string)
- [Hashing](#hashing)
- [Simple Sorting](#simple-sorting)
  - [Bubble Sort](#bubble-sort)
  - [Selection Sort](#selection-sort)   
  - [Insertion Sort](#insertion-sort)
- [Recursion](#recursion) 
- [Complex Sorting](#complex-sorting)
  - [Merge Sort](#merge-sort)
  - [Quick Sort](#quick-sort)
  - [Tim Sort](#tim-sort)
- [Advanced Sorting](#advanced-sorting)
  - [Shell Sort](#shell-sort) 
  - [Radix Sort](#shell-sort)
- [Stacks and Queues](#stacks-and-queues)
- [Linked List](#linked-list)
- [Binary Trees](#binary-trees)
- [2-3-4 Trees and external storage](#2-3-4-trees-and-external-storage)
- [AVL and Red-Black Trees](#avl-and-red-black-trees)
- [Heaps](#heaps)
- [Graphs](#graphs)






----

## Simple Sorting

### Bubble Sort

Le Bubble Sort compare les éléments adjacents et les échange s’ils sont dans le mauvais ordre, ce qui fait remonter progressivement les plus grands éléments vers la fin du tableau.

À chaque passage, la plus grande valeur “bulle” vers le haut, réduisant la zone à trier.

              def bubble_sort(arr):
                n = len(arr)
                for i in range(n - 1):
                  # On parcourt le tableau jusqu'à la zone non triée
                  for j in range(0, n - i - 1):
                  # Si les éléments sont dans le mauvais ordre, on les échange
                    if arr[j] > arr[j + 1]:
                      arr[j], arr[j + 1] = arr[j + 1], arr[j]
                return arr

          


--- 

### Selection Sort

Le Selection Sort parcourt le tableau pour trouver le plus petit élément et le place en première position, puis répète l’opération pour le reste du tableau.
Ainsi, à chaque itération, la partie gauche du tableau est triée, tandis que la partie droite reste à trier.

     def selection_sort(arr):
      for i in range(len(arr) - 1):
        for j in range(i + 1, len(arr)):
            if arr[j] < arr[i]:
                arr[i], arr[j] = arr[j], arr[i]

---

### Insertion Sort

Le tri par insertion construit le tableau final trié **élément par élément** en maintenant un **préfixe trié**.  
Il est **moins efficace** que quicksort/heapsort/merge sort sur de grands jeux de données, mais reste appréciable pour sa **simplicité**, son efficacité sur **petits tableaux** ou **données presque triées**, sa **stabilité** et son caractère **en place** (in-place).

#### Mauvaise interprétation (à éviter)

On voit parfois une version qui **swappe** les éléments à chaque comparaison. Même si le tableau peut finir trié, ce **n’est pas** le tri par insertion : l’algorithme correct **décale** les éléments plus grands vers la droite puis **insère** la clé **une seule fois**.


#### Version correcte (décalages, pas de swaps)

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

#### Insertion sort — version basique (in-place, stable)
      def insertion_sort(arr):
        n = len(arr)
        for current in range(1, n):
          current_card = arr[current]
          correct_position = current - 1  # ira de i-1 à 0

          # Décale les éléments plus grands vers la droite
          while correct_position >= 0 and arr[correct_position] > current_card:
              arr[correct_position + 1] = arr[correct_position]
              correct_position -= 1

        # Insère la clé à la position libérée
        arr[correct_position + 1] = current_card

    return arr

--- 

### Merge Sort
Le merge sort (ou tri par fusion) est un algorithme de tri diviser pour régner (divide and conquer).
Il est à la fois efficace (complexité en O(n log n)) et stable (il conserve l’ordre des éléments égaux).
Voyons comment il fonctionne, étape par étape, sans aucun code.

#### Le principe général
Le merge sort repose sur trois grandes étapes :
1. Diviser le tableau en deux moitiés jusqu’à obtenir des sous-tableaux de taille 1.
   → Un tableau de taille 1 est déjà trié.
2. Trier récursivement chaque moitié (en appliquant le même processus).
3. Fusionner ces deux moitiés triées pour obtenir un tableau trié.

#### Étape par étape sur un exemple
Imaginons qu’on veuille trier ce tableau :

      [8, 3, 5, 4, 7, 6, 1, 2]

Etape 1 : Division

On divise en deux moitiés : 
- Gauche -> [8, 3, 5, 4]
- Droite -> [7, 6, 1, 2]
On continue à diviser récursivement chaque partie jusqu’à arriver à des sous-tableaux de taille 1 :
- [8, 3, 5, 4] devient [8, 3] et [5, 4], puis [8], [3], [5], [4]
- [7, 6, 1, 2] devient [7, 6] et [1, 2], puis [7], [6], [1], [2]

À ce stade, on a :

          [8] [3] [5] [4] [7] [6] [1] [2]

Chaque bloc est trié individuellement (car un seul élément).

Étape 2 : Fusion successive

On commence maintenant à fusionner les petits tableaux deux à deux, en les triant lors de la fusion : 
- Fusion de [8] et [3] → [3, 8]
- Fusion de [5] et [4] → [4, 5]
- Fusion de [7] et [6] → [6, 7]
- Fusion de [1] et [2] → [1, 2]

Puis on fusionne à nouveau :
- [3, 8] et [4, 5] → [3, 4, 5, 8]
- [6, 7] et [1, 2] → [1, 2, 6, 7]

Enfin :
- Fusion finale de [3, 4, 5, 8] et [1, 2, 6, 7] → [1, 2, 3, 4, 5, 6, 7, 8]

⚙️ 3. Comment se fait la fusion exactement ?

Lorsqu’on fusionne deux tableaux triés, on les compare élément par élément :
- On prend le plus petit des deux premiers éléments et on le place dans un nouveau tableau.
- On avance dans le tableau d’où provient l’élément choisi.
- On répète jusqu’à ce qu’un des deux tableaux soit vide.
- Puis on ajoute le reste du tableau non vide (puisqu’il est déjà trié).

#### Vue d'ensemble 

Exemple d'entrée :
[38, 27, 43, 3, 9, 82, 10]

1. Décomposition (divide)
----------------------

                              [38, 27, 43, 3, 9, 82, 10]
                             /                         \
                   [38, 27, 43, 3]                   [9, 82, 10]
                  /               \                 /          \
            [38, 27]            [43, 3]         [9, 82]       [10]
            /     \            /     \          /     \         |
         [38]    [27]       [43]    [3]      [9]    [82]      [10]


2. Fusions (conquer)
-----------------

Étape 1 – fusion de paires élémentaires:
[38] + [27] -> [27, 38]
[43] + [3]  -> [3, 43]
[9]  + [82] -> [9, 82]
[10]        -> [10] (déjà trié)

Résultat partiel:
[27, 38]         [3, 43]           [9, 82]         [10]

Étape 2 – fusion des sous-tableaux triés:
[27, 38] + [3, 43]  -> [3, 27, 38, 43]
[9, 82]  + [10]     -> [9, 10, 82]

Résultat partiel:
[3, 27, 38, 43]                     [9, 10, 82]

Étape 3 – fusion finale:
[3, 27, 38, 43] + [9, 10, 82] 
=>
[3, 9, 10, 27, 38, 43, 82]


Arbre compltet
--------------

                         [38,27,43,3,9,82,10]
                       /                       \
           [38,27,43,3]                         [9,82,10]
          /             \                      /         \
      [38,27]          [43,3]               [9,82]      [10]
      /     \          /   \               /    \
   [38]   [27]      [43]  [3]           [9]    [82]

      \     /          \   /               \    /
     [27,38]          [3,43]              [9,82]        [10]

          \             /                      \         /
           [3,27,38,43]                          [9,10,82]
                       \                         /
                        [3,9,10,27,38,43,82]


Rappel complexité
-----------------
- Temps: O(n log n)
- Espace: O(n) (fusion externe)

---

### Quick Sort

Le Quick Sort est un algorithme de tri récursif fondé sur le principe du "divide and conquer". Il choisit un pivot, puis partitionne le tableau en éléments plus petits et plus grands que ce pivot.

Ensuite, il trie récursivement ces sous-tableaux avant de les concaténer pour obtenir le résultat final.

Exemple : [8, 3, 7, 4, 2, 6]

1️⃣ Choix du pivot = 4  
Partition :
→ [3, 2]   [4]   [8, 7, 6]

2️⃣ Tri récursif :
[3, 2]  -> pivot = 2  -> [ ] [2] [3]
[8, 7, 6] -> pivot = 6 -> [ ] [6] [8, 7]
                      -> [ ] [7] [8]

3️⃣ Fusion des résultats :
[2, 3] + [4] + [6, 7, 8] = [2, 3, 4, 6, 7, 8]


Vue arborescente rapide
-----------------------

                [8,3,7,4,2,6]
                        |
                     pivot=4
                    /    |    \
              [3,2]     [4]   [8,7,6]
               |               |
             p=2             p=6
             /|\             /|\
           []2[3]          []6[8,7]
                               |
                             p=7
                             /|\
                           []7[8]

Résultat final → [2,3,4,6,7,8]

#### Version Lomuto(pivot = fin)
Cette version de quick sort à pour but de trié en place un tableau en choisisant un pivot, dans ce cas le dernier éléments de la liste, et de comparer cet élément avec tous les autres éléments réstant afin de trouver sa position finale en déplacant à gauche tous les éléments plus petit que pivot et a droite tous les éléments plus grand que pivot. 

A la fin de la première itération on a pivot a sa position finale, a sa gauche tous les éléments plus petits que pivot et a sa droite tous les éléments plus grand que lui. De cette facon nous pouvons rappeler de facon récursive la fonction afin de la trié dans son ensemble.

Comment cela fonctionne ?

Le plus gros challenge de cet algorithme de trie est de comparer chaque element a pivot et de swap les éléments plus petits a sa gauche sans modifier pivot avant d'avoir terminé notre itération. 

                      idx  0, 1, 2, 3, 4, 5, 6
                ma_list = [3, 6, 1, 2, 7, 5, 4]
                                             ^
                                             |
                                           pivot
              
              pivot_final_index = 0
              pivot_index = high 
              pivot_value = arr[high]

              for j in range(low = 0, high = len(arr-1):
                  #on compare si la valeur < à pivot a gauche
                  
                  if arr[j] < pivot_value:
                    #Alors on swap et on incrémente (double opération pour : 
                    # - identifier la position final de pivot)
                    # - Positionner a gauche les éléments plus petits, a droite les éléments plus grands.
                    arr[j], arr[pivot_final_position] = arr[pivot_final_position], arr[j]
                    pivvot_final_position += 1

          #Ici, on obtient la position final de pivot pour l'élement 4 donc il nous reste à l'insérer, swapper:
          arr[pivot_final_index], arr[pivot_index] = arr[pivot_index], arr[pivot_final_index]

          # Appel récursig sur la partie gauche et droite
          quick_sort_pivot_ending(arr, low, pivot_start_position - 1)
          quick_sort_pivot_ending(arr, pivot_start_position + 1, high)
                
Explication : 

L'objectif est de trouver la position finale de pivot
1. On crée un pointeur i qui démarre à la borne low de notre fonction. Au début low = 0 et high = len(arr -1)
2. On boucle sur toutes la liste avec ub incrément j et on compare => j < valeur_pivot ? Si oui on swap la valeur du pointeur i avec la valeur du pointeur j et on incrémente i. 
3. A la fin de cette itération i est donc l'index ou doit se trouver pivot, donc il nous reste qu'a swapper pivot a cet endroit de la liste.
4. On rapelle cette fonction sur la partie de gauche donc => low, pivot_position - 1
5. On rapelle cette fonction sur la partie de droit donc => pivot_position + 1, high
6. On obtient une liste trié via l'algorithme quicksort !



  - On avance un pointeur i au fur et à mesure qu’on rencontre des éléments plus petits que le pivot.
  - On ne fait qu’une seule passe.
  - À la fin, on échange pivot ↔ arr[i].
  - Tout ce qui est avant i est plus petit, tout ce qui est après est plus grand.
✅ Le pivot est à sa vraie place finale apres chacune des étapes.

La récursion peut se faire sans problème : [low, p-1] et [p+1, high].  

      Code : A implémenter
--- 

### Version Partition Hoare(pivot = milieu ou autre)

L'objectif est le meme que pour l'implémentation précédente, on determine un pivot, ici au mileu de notre liste. Ensuite on swap les éléments pour que les plus petits soit à sa gauche et les plus grand a sa droite.

Mais attention, a la fin d'une itération l'element que l'on a determiné comme pivot n'est pas encore a sa position final, donc il faut l'inclure dans les appels récursif suivant, mais pas dans les deux au risque d'avoir une appel recursif infini.

1. Choix du pivot

        pivot_value = arr[(low+high) // 2]

2. Deux pointeurs
   - i part de low et avance vers la droite
   - j part de high et recule vers la gauche

3. Boucle de partition(tant que i < j)
   - Avance i tant que arr[i] < pivot_value -> i s'arrête sur le premier élément supérieur a pivot car il doit être mis a droite.
   - Recule j tant que arr[j] > pivot_value -> j s'arrête sur le premier element inférieur a pivot car il doit être mis a gauche.
   - A ce moment précis :
     - arr[i] est un candidat du mauvais côté (>= pivot mais a gauche et doit être mis a droite)
     - arr[j] est un candidat du mauvais côté (<= pivot mais a droite et doit être mis a gauche)

  - Swap arr[i] ↔ arr[j] pour remettre chacun de son bon côté.
  - Progrès garanti : i += 1, j -= 1.
  - On répète tant que i < j.

A ce moment : 
- arr[low ... j] < pivot
- arr[i ... high] > pivot

4. Appel récursif
- quick_sort_pivot_middle(arr, low, j)
- quick_sort_pivot_middle(arr, i,   high)

La condition d’arrêt if low >= high: return empêche de redescendre sur des segments vides/mono-élément.

    def quick_sort_pivot_middle(arr, low, high):
      if low >= high:
        return

      pivot_start_position = (low + high) // 2
      pivot_value = arr[pivot_start_position]
      i = low
      j = high

      while i < j:
        while arr[i] < pivot_value:
            i += 1
        while arr[j] > pivot_value:
            j -= 1
        arr[i], arr[j] = arr[j], arr[i]
        i +=1
        j -=1

      quick_sort_pivot_middle(arr, low, j)
      quick_sort_pivot_middle(arr, i, high)

### Version pure 

    def quick_sort_wrong(arr):
    if len(arr) == 0:
        return arr


    pivot = arr[-1]
    idx_to_delete = []

    for i in range(len(arr) - 1):
        if arr[i] > pivot:
            idx_to_delete.append(i)
            arr.append(arr[i])

    for idx in reversed(idx_to_delete):
        del arr[idx]

    pivot_index = arr.index(pivot)

    less = arr[:pivot_index]
    greater = arr[pivot_index + 1:]

    return quick_sort_wrong(less) + [pivot] + quick_sort_wrong(greater)

---

### Tim Sort

L'algorithme de tri mélange le meilleur des deux mondes entre un INSERTION SORT (trés éfficaces dans certains cas) et MERGE SORT (trés efficaces dans d'autre cas).

1. Découper le tableau en runs (petits morceaux) et trier chaque run avec insertion sort.
2. Fusionner par passes via merger sort, les runs triés (taille run, puis 2*run, 4*run, …) jusqu’à couvrir tout le tableau.

#### Phase 1 : Trier les runs

Premièrement, on va créer des sous liste de arr de la longeur de run.
Deuxièmement, on les tris avec un insertion sort et on remplace nos éléments pas trié par nos éléments triés

    # intput [6, 4, 3, 5, 1, 2], run = 2
    # On va créer des run c'est a dre sous-liste d'input de longeur 2
    # run_1 = [6, 4] -> insertion_sort([4, 6]) -> On remplace sur notre input par le result => [4, 6, 3, 5, 1, 2]
    # run_2 = [5, 3] -> insertion_sort([5, 3) -> On remplace sur notre input par le result => [4, 6, 5, 3, 1, 2]
    # run_2 = [1, 2] -> insertion_sort([1, 2) -> deja trié, mais en remplac quand meme ce qui ne change rien => [4, 6, 5, 3, 1, 2]

### Phase 2 : Fusion (merge) par passe

Maintenant que nos runs sont trié on va merge les runs adjacents jusqu'a obtenir une liste totalement trié:

      [4,6] [3,5] [1,2] [7,9]
         |     |     |     |
          merge       merge 
      => [3,4,5,6]  [1,2,7,9]

On continue pour obtenir version totalement trié
      
      => [1,2,3,4,5,6,7,9]

Bien sur on crée des copies temporaire du tableau via un SLICE notre arr original aux bornes correspondant aux run et aux run*2 ainsi de suite jusqu'a la fin du tableau. 

Ces copies vont être passé a notre fonction merge et le retour de cette fonction  va venir remplacer cette partie du tableau par la nouvelle partie trié.

### Implémentation : 

#--------- TIM SORT Correction--------------

    def merge_sort_2(arr_1, arr_2):
      new_arr = []

      idx_left = 0
      idx_right = 0

      while idx_left < len(arr_1) and idx_right < len(arr_2):
        if arr_1[idx_left] <= arr_2[idx_right]:
            new_arr.append(arr_1[idx_left])
            idx_left += 1
        else:
            new_arr.append(arr_2[idx_right])
            idx_right += 1

      if len(arr_1) > len(arr_2):
        new_arr += arr_1[idx_left:]
      else:
        new_arr += arr_2[idx_right:]

      return new_arr

      def time_sort_correction(arr, run):
        #1. Créer des sous liste de arr de la longeur de run.
        #2. On les tris avec un insertion sort et on remplace nos éléments pas trié par nos éléments triés

    # intput [6, 4, 3, 5, 1, 2], run = 2
    # On va créer des run c'est a dre sous-liste d'input de longeur 2
    # run_1 = [6, 4] -> insertion_sort([4, 6]) -> On remplace sur notre input par le result => [4, 6, 3, 5, 1, 2]
    # run_2 = [5, 3] -> insertion_sort([5, 3) -> On remplace sur notre input par le result => [4, 6, 5, 3, 1, 2]
    # run_2 = [1, 2] -> insertion_sort([1, 2) -> deja trié, mais en remplac quand meme ce qui ne change rien => [4, 6, 5, 3, 1, 2]
    for i in range(0, len(arr), run):
        arr[i: i+run] = insertion_sort(arr[i:i + run])
        tot = arr[i: i+run]


    #3. Maintenant qu'on a nos sous liste trié, on va merger les sous listes
    #   OR = merge(arr[i, i+run], arr[i + run, i+run*2])
    #   On remplace maintenant, arr[i à i + run*2] = OR
    #   OR_2 on fait la meme chose pour OR2
    
    #4. Tant que copy_of_run < longeur du tableau on continue mais en incrémentant copy_of_run pour trier la suite :
    # copy_of_run = copy_of_run * 2
    
    #5. Enfin notre liste est trié par TimSort() !

    copy_of_run = run

    while copy_of_run < len(arr):
        for i in range(0, len(arr), copy_of_run*2):
            arr[i:i + copy_of_run * 2] = merge_sort_2(arr[i: i + copy_of_run], arr[i + copy_of_run : i + copy_of_run*2])
        copy_of_run = copy_of_run * 2

    return arr
  
---
### 🌀 Shell Sort
#### 🔹 Présentation

Le Shell sort est une amélioration du tri par insertion.Il commence par trier des groupes d’éléments espacés (appelés gaps), puis réduit progressivement cet écart jusqu’à 1.

Quand le gap vaut 1, le Shell sort devient un simple insertion sort, mais sur un tableau déjà “presque trié” — donc bien plus rapide.

---

#### 🔹 Principe général
- Choisir un gap initial, souvent len(arr) // 2.
- Former des sous-suites conceptuelles d’éléments espacés de gap.
- Appliquer un tri par insertion sur chacune de ces sous-suites (en place).
- Réduire le gap (par exemple, gap //= 2) et recommencer.
- Quand gap == 1, on effectue un dernier tri par insertion classique.

Ainsi, les petits éléments peuvent “avancer” rapidement vers le début du tableau, ce qui accélère fortement la convergence vers un tableau trié.

#### 🔹 Exemple illustré

Entrée :

      arr = [3, 6, 2, 8, 1]

Étape 1 — Initialisation

    gap = len(arr) // 2 = 5 // 2 = 2

Étape 2 — Sous-suites conceptuelles (espacement de 2)

    Index :   0   1   2   3   4
    Valeur : [3,  6,  2,  8,  1]

    Sous-suites selon gap=2 :
    A : (0 → 2 → 4)  → [3, 2, 1]
    B : (1 → 3)      → [6, 8]

Visualisation ASCII (les flèches représentent le lien "espacé de gap") :

    A : 3 → 2 → 1
    B : 6 → 8


Étape 3 — Tri par insertion sur chaque sous-suite

Sous-suite A [3, 2, 1] → triée en [1, 2, 3]
→ Modifie directement arr aux indices 0, 2, 4 → arr = [1, 6, 2, 8, 3]

Sous-suite B [6, 8] → déjà triée
→ arr reste inchangé.

État du tableau :
        
        arr = [1, 6, 2, 8, 3]

Étape 4 — Réduction du gap

    gap = gap // 2 = 1

Étape 5 — Dernier passage (gap = 1)

Un insertion sort classique sur tout le tableau 

      arr = [1, 2, 3, 6, 8]
      
--- 
### Implementation 
#### Basic Implementation
    def insertion_sort_correction(arr):
        n = len(arr)
        for current in range(1, n):
            current_card = arr[current]
            correct_position = current - 1  # it will go form i-1 to 0

            while correct_position >= 0:
                if arr[correct_position] < current_card:
                    break
                else:
                    arr[correct_position + 1] = arr[correct_position]
                    correct_position -= 1
                arr[correct_position + 1] = current_card
        return arr


    def shell_sort(arr, gap):
      if gap <= 1:
        return insertion_sort_correction(arr)
      length = len(arr)

      for x in range(0, gap):
        current_idx = []
        for y in range(x, length, gap):
            current_idx.append(y)

        #Implementation d'un insertion sort local
        for i in range(1, len(current_idx)):
            current_value = arr[current_idx[i]]
            idx = i
            for j in range(i, 0, -1):
                tmp = arr[current_idx[j - 1]]
                if current_value < tmp:
                    arr[current_idx[j]] = arr[current_idx[j - 1]]
                    idx -=1
                else:
                    break
            arr[current_idx[idx]] = current_value

    return shell_sort(arr, gap // 2)


#### Better Implementation

        def shell_sort(arr):
        gap = len(arr) // 2

        while gap > 0:
          for start_index in range(gap):
            gap_insertion_sort(arr, start_index, gap)
        gap = gap //2


        def gap_insertion_sort_2(arr, start_index, gap):
          for i in range(start_index+gap, len(arr), gap):
          current_value = arr[i]
          position = i

          while position >= gap and arr[position-gap] < current_value:
            arr[position] = arr[position-gap]
            position = position-gap
          arr[position] = current_value




## Complex sorting
### Heap Sort
Fait, A réécrire correctement.

### Merge Sort
Fait, a Réécrire correctement.

### HeapSort
A faire après avoir vu les arbres. informations a vérifier.

