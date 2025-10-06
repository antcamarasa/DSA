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
  - [Heap Sort](#heap-sort)
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

---

## Hashing  

### Table des matières
### 1. Get to Know the Hash Table Data Structure
- [Hash Table vs Dictionary](#hash-table-vs-dictionary)
- [Hash Table: An Array With a Hash Function](#hash-table-an-array-with-a-hash-function)

### 2. Understand the Hash Function
- [Examine Python’s Built-in hash()](#examine-pythons-built-in-hash)
- [Dive Deeper Into Python’s hash()](#dive-deeper-into-pythons-hash)
- [Identify Hash Function Properties](#identify-hash-function-properties)
- [Compare an Object’s Identity With Its Hash](#compare-an-objects-identity-with-its-hash)
- [Make Your Own Hash Function](#make-your-own-hash-function)

### 3. Build a Hash Table Prototype in Python With TDD
- [Take a Crash Course in Test-Driven Development](#take-a-crash-course-in-test-driven-development)
- [Define a Custom HashTable Class](#define-a-custom-hashtable-class)
- [Insert a Key-Value Pair](#insert-a-key-value-pair)
- [Find a Value by Key](#find-a-value-by-key)
- [Delete a Key-Value Pair](#delete-a-key-value-pair)
- [Update the Value of an Existing Pair](#update-the-value-of-an-existing-pair)
- [Get the Key-Value Pairs](#get-the-key-value-pairs)
- [Use Defensive Copying](#use-defensive-copying)
- [Get the Keys and Values](#get-the-keys-and-values)
- [Report the Hash Table’s Length](#report-the-hash-tables-length)
- [Make the Hash Table Iterable](#make-the-hash-table-iterable)
- [Represent the Hash Table in Text](#represent-the-hash-table-in-text)
- [Test the Equality of Hash Tables](#test-the-equality-of-hash-tables)

### 4. Resolve Hash Code Collisions
- [Find Collided Keys Through Linear Probing](#find-collided-keys-through-linear-probing)
- [Use Linear Probing in the HashTable Class](#use-linear-probing-in-the-hashtable-class)
- [Let the Hash Table Resize Automatically](#let-the-hash-table-resize-automatically)
- [Calculate the Load Factor](#calculate-the-load-factor)
- [Isolate Collided Keys With Separate Chaining](#isolate-collided-keys-with-separate-chaining)

### 5. Retain Insertion Order in a Hash Table
- [Preserve Order While Maintaining Performance](#preserve-order-while-maintaining-performance)

### 6. Use Hashable Keys
- [Hashability vs Immutability](#hashability-vs-immutability)
- [The Hash-Equal Contract](#the-hash-equal-contract)

### 7. Conclusion
- [Wrap-Up and Key Takeaways](#wrap-up-and-key-takeaways)

--- 

### Introduction
Inventée il y a plus d'un demi siècle, la table de hachage(hash table) est une structure de données classique, fondamentale en programmation. 

Encore aujourd'hui, elle permet de résoudre de nombreux problème concrets, comme :
- l’indexation des tables d’une base de données,
- la mise en cache de valeurs calculées,
- ou encore l’implémentation d’ensembles (sets).

Certains languages possède leur propre table de hachage intégrée, il est indispensable de comprendre comment fonctionne une table de hachage en interne.

On va voir :
- Quelle est la différence entre une table de hachage et un dictionnaire
- Comment implémenter une table de hachage en python depuis zéro.
- Comment gérer les collision de hachage et autres difficultés
- Quelles sont les caractéristiques souhaitables d'une fonction de hachage
- Comment la fonction hash() fonctionne en coulissses

### Découvrir la structure de données "Table de hachage"
Avant d'aller plus loin, il est utile de se familiariser avec le terminologie, car elle peut préter a confusion.
Dans le languge courant, les termes table de hachage (hash table) ou Hashmap sont souvent utilisés comme synonyme du mot dictionnaire (dictionary).
Cependant, il existe une différence subtile :
- La table de hachage est un cas particulier du concept plus général de dicitonnaire(python), Hashmap(java) et objet en JS.

Pour la simpliciter de ce cours on utilisera python pour l'implémentation ainsi que le terme dictionnaire.

### Table de hachage vs Dictionnaire
En informatique 


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

# Insertion sort — version basique (in-place, stable)
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

## Advanced Sorting
### Shell Sort
- [Présentation](#shell-sort--presentation)
Shell sort est un algorithme de tri en place qui généralise l’insertion sort :
- il trie d’abord des sous-suites d’éléments espacés d’un gap, puis réduit gap progressivement jusqu’à 1.

Ce procédé permet aux petits éléments d’avancer rapidement vers leur zone cible, rendant le dernier passage (insertion classique à gap=1) beaucoup plus efficace.

---

Entrée : arr = [3, 6, 2, 8, 1]

#### 1. Initialiser le gap
  gap = len(arr) // 2 = 5 // 2 = 2.

#### 2. Former les sous-suites (conceptuelles)
On considère les éléments espacés de gap
- Sous-suite A (indices 0,2,4) → valeurs 3, 2, 1
- Sous-suite B (indices 1,3) → valeurs 6, 8

Important : on ne crée pas de nouveaux tableaux. On travaille en place dans arr, aux positions i(Une liste d’index peut aider à comprendre, mais n’est pas nécessaire pour l’implémentation.)

#### 3. Appliquer l’insertion gappée à chaque sous-suite
- Sous-suite A 3, 2, 1 → tri par insertion (déplacements vers la droite, puis insertion de la “clé”) → 1, 2, 3
  - arr devient : [1, 6, 2, 8, 3] (car on a modifié directement aux indices 0,2,4).
- Sous-suite B 6, 8 → déjà triée
  - arr reste : [1, 6, 2, 8, 3]

#### 4. Réduire le gap
  gap = gap // 2 = 1

#### 5. Dernier passage (gap = 1)
- Appliquer un insertion sort classique sur tout arr.
- Résultat : [1, 2, 3, 6, 8].

#### 6) Fin
- Tableau trié.

---

### Basic Implementation
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

## Complex sorting
### Heap Sort
Fait, A réécrire correctement.

### Merge Sort
Fait, a Réécrire correctement.

### HeapSort
A faire après avoir vu les arbres. informations a vérifier.

