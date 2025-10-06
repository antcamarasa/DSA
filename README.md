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
En informatique, un dictionnaire est un type de donnée abstrait composé de paires : 
- Clé / Valeur

Il définit un ensemble d'opérations fondamentales : 
- Ajouter une paire clé-valeur

      d[key] = value
  
- Supprimer une paire clé-valeur

      d.pop(key)
  
- Mettre a jour une paire clé-valeur

      d.update({key: new_value})
  
- Trouver la valauer associé à une clé donnée

      d[key]

On peut le comparer à un dictionnaire bilingue, où les clés sont des mots étrangers et les valeurs leurs traductions.
Mais la relation entre clé et valeur n’est pas toujours une traduction : par exemple, un annuaire téléphonique est aussi un dictionnaire, associant des noms à des numéros de téléphone.

💡 À chaque fois que tu associes une chose à une autre (une valeur à une clé), tu utilises en réalité une forme de dictionnaire.
C’est pourquoi on appelle aussi ces structures maps ou tableaux associatifs (associative arrays).


#### Propriétés d’un dictionnaire
Un dictionnaire possède plusieurs caractéristiques intéressantes :
1. Paires clé–valeur uniquement → on ne peut pas avoir une clé sans valeur ni une valeur sans clé.
2. Clés et valeurs arbitraires → elles peuvent appartenir à des types différents (nombres, String, tableaux, etc.).
3. Paires non ordonnées → en général, les dictionnaires ne garantissent aucun ordre entre leurs éléments (cela dépend de l’implémentation).
4. Clés uniques → une même clé ne peut pas apparaître deux fois (cela violerait la définition d’une fonction mathématique, et le fondement meme de la clé de hachage).
5. Valeurs non uniques → une même valeur peut être associée à plusieurs clés.

#### Concepts associés
Certains concepts étendent le principe du dictionnaire :
- Un multimap permet d’associer plusieurs valeurs à une même clé.
- Un bidirectional map (ou map bidirectionnelle) associe les clés aux valeurs dans les deux sens.

      >>> glossary = {"BDFL": "Benevolent Dictator For Life"}
      >>> glossary["GIL"] = "Global Interpreter Lock"  # Add
      >>> glossary["BDFL"] = "Guido van Rossum"  # Update
      >>> del glossary["GIL"]  # Delete

      >>> glossary["BDFL"]  # Search
      'Guido van Rossum'

      >>> glossary
      {'BDFL': 'Guido van Rossum'}

Avec la syntaxe des crochets [ ], tu peux ajouter, modifier ou supprimer une paire clé–valeur dans un dictionnaire, et accéder à la valeur d’une clé donnée.

Mais une autre question se pose : comment fonctionne vraiment ce dictionnaire intégré ?, Comment peut-il gérer des clés de tout type et le faire si rapidement ?

Chercher une implémentation efficace de ce type abstrait est ce qu’on appelle le “problème du dictionnaire”. L’une des solutions les plus connues repose sur la table de hachage, que l'on va voir ici, mais d’autres existent, comme celle basée sur un arbre rouge-noir (red-black tree).

### Hash Table: Un Array avec une Hash Function
T’es-tu déjà demandé pourquoi l’accès aux éléments d’une séquence en Python est si rapide, quel que soit l’indice demandé ?

Par exemple, imagine que l'on travaille avec une très longue chaîne de caractères, comme celle-ci :
         
          >>>> import String
          >>>> text = string.ascii_uppercase * 100_000_000
          
          >>>> text[:50] # slice de l'indice 0 à 50, montre que les 50 premier charcatères
          'ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGHIJKLMNOPQRSTUVWX'

          >>>> len(text)
          26000000000

La variable text ci-dessus contient 2,6 milliards de caractères, formés par la répétition de lettres ASCII, ce que tu peux vérifier avec la fonction len() de Python.
Et pourtant, accéder au premier, au milieu, au dernier ou à n’importe quel caractère de cette chaîne est tout aussi rapide.

- 🧱 Array (en général, comme en C ou Java) →
✅ l’accès à un élément par index est toujours en O(1), donc le premier, le dernier ou le millième prennent le même temps. (Attention ce n'est pas le cas pour le déplacement ou suppression d’éléments, qui eux ne sont pas en O(1).)

- 🐍 Python list →
même chose : c’est un tableau dynamique, donc l’accès par index est O(1).

- 🧩 dict →
l’accès à une clé est aussi O(1) en moyenne grâce à la table de hachage (mais pas garanti en cas de collisions extrêmes).

- ⚙️ set ->
identique à dict, il utilise une hash table, donc test d’appartenance et insertion en O(1).


      >>> text[0]  # The first element
      'A'

      >>> text[len(text) // 2]  # The middle element
      'A'

      >>> text[-1]  # The last element, same as text[len(text) - 1]
      'Z'

Mais comment est-ce possible ?

Le secret de cette grande rapidité réside dans le fait que les séquences(lst, tuple, str, array, range, bytes) Python reposent sur un tableau (array), une structure de données à accès aléatoire (random access).

Random acces, ou accès aléatoires, ne siginifie pas "Accès au hasard" mais "Accès direct à n'importe quel élément sans parcourir les autres".

Une structure à accès aléatoire (random access data structure) permet d’aller directement à un élément en connaissant son indice (position).

Elle obéit à deux principes :
- Le tableau occupe un bloc de mémoire contigu.
- Chaque élément du tableau a une taille fixe connue à l’avance.

Quand on connaît l’adresse mémoire du tableau (appelée offset), on peut accéder instantanément à n’importe quel élément en appliquant une formule simple :
            
            Adresse d’un élément = offset + (taille_élément × index)

Autrement dit, on part de l’adresse du premier élément (index 0), puis on avance du nombre d’octets correspondant à la taille de l’élément multipliée par son index.

Cette opération prend toujours le même temps, car elle se résume à une addition et une multiplication.

🔎 Remarque : contrairement aux tableaux classiques, les listes Python peuvent contenir des éléments hétérogènes (de tailles différentes), ce qui casserait cette formule.
Pour pallier cela, Python ajoute un niveau d’indirection : la liste contient un tableau de pointeurs vers les zones mémoire réelles où sont stockées les valeurs.

#### Représentation d'une list en python : 

          Array
      ----------------------          0x7f -> "Hello
        0x7f | 0xb5 | 0x7f
      ----------------------
Mon tableau stock des adresse mémoires et non des éléments, une des carractéristique d'une tableau dynamique.

Les pointeurs ne sont en réalité que de simples nombres entiers, qui occupent toujours la même quantité d’espace mémoire. Par convention, les adresses mémoire sont représentées en notation hexadécimale. En Python (et dans d’autres langages), ces nombres sont préfixés par 0x.

En résumé, trouver un élément dans un tableau est rapide; quelle que soit sa position en mémoire. Peut on réutiliser cette idée dans un dictionnaire ? -> Oui!

Les tables de hachage tirent leur nom d’une astuce appelée hachage (hashing), qui permet de traduire une clé quelconque en un nombre entier, utilisable comme indice dans un tableau classique.

Ainsi, au lieu de chercher une valeur via un index numérique, tu peux la retrouver à partir d’une clé arbitraire, sans perte notable de performance — malin, non ?

En pratique, le hachage ne fonctionne pas avec toutes les clés, mais la plupart des types intégrés de Python sont hachables.

Et si tu respectes certaines règles, tu pourras aussi créer tes propres types hachables.

### Understand the Hash Function
...




----

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

---

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
    
      def merge_sort(arr):
        if len(arr) <= 1:
           return arr

       middle = len(arr) // 2
       left = merge_sort(arr[:middle])
       right = merge_sort(arr[middle:])

       new_arr = []
       left_index = right_index = 0

       while left_index < len(left) and right_index < len(right):
        if left[left_index] <= right[right_index]:
            new_arr.append(left[left_index])
            left_index += 1
        else:
            new_arr.append(right[right_index])
            right_index += 1

      new_arr.extend(left[left_index:])
      new_arr.extend(right[right_index:])

      return new_arr



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

