# Dictionnaires, tuples et sets

Jusqu'à maintenant nous avons vu et manipulé le type d'objet séquentiel le plus classique : les listes.  
On se rappelle qu'elles sont modifiables, ordonnées et itérables. Dans ce chapitre nous allons voir trois nouveaux types d'objet séquentiel avec des propriétés différentes : les dictionnaires, les tuples et les sets.


> Remarque
> 
> Les objets séquentiels peuvent être aussi appelés parfois containers.


## Dictionnaires
###  Définition


Les dictionnaires se révèlent très pratiques lorsque vous devez manipuler des structures complexes à décrire et que les listes présentent leurs limites. Les dictionnaires sont des collections non ordonnées d'objets, c'est-à-dire qu'il n'y a pas de notion d'ordre (i.e. pas d'indice). On accède aux valeurs d'un dictionnaire par des clés. Ceci semble un peu confus ? Regardez l'exemple suivant :


```python
>>> ani1 = {}
>>> ani1["nom"] = "girafe"
>>> ani1["taille"] = 5.0
>>> ani1["poids"] = 1100
>>> ani1
{'nom': 'girafe', 'taille': 5.0, 'poids': 1100}
```


En premier, on définit un dictionnaire vide avec les accolades {} (tout comme on peut le faire pour les listes avec []). Ensuite, on remplit le dictionnaire avec différentes clés ("nom", "taille", "poids") auxquelles on affecte des valeurs ("girafe", 5.0, 1100). Vous pouvez mettre autant de clés que vous voulez dans un dictionnaire (tout comme vous pouvez ajouter autant d'éléments que vous voulez dans une liste).


> Remarque
> 
> Un dictionnaire est affiché sans ordre particulier.


On peut aussi initialiser toutes les clés et les valeurs d'un dictionnaire en une seule opération :

```python
>>> ani2 = {"nom": "singe", "poids": 70, "taille": 1.75}
```


Mais rien ne nous empêche d'ajouter une clé et une valeur supplémentaire :

```python
>>> ani2["age"] = 15
```


Pour récupérer la valeur associée à une clé donnée, il suffit d'utiliser la syntaxe suivante dictionnaire["cle"]. Par exemple :

```python
>>> ani1["taille"]
5.0
```


> Remarque
> 
> Toutes les clés de dictionnaire utilisées jusqu'à présent étaient des chaînes de caractères. Rien n'empêche d'utiliser d'autres types d'objets comme des entiers (voire même des tuples, cf. rubrique suivante), cela peut parfois s'avérer très utile.
> 
> Néanmoins, nous vous conseillons, autant que possible, d'utiliser systématiquement des chaînes de caractères pour vos clés de dictionnaire.


Après ce premier tour d'horizon, on voit tout de suite l'avantage des dictionnaires. Pouvoir retrouver des éléments par des noms (clés) plutôt que par des indices. Les humains retiennent mieux les noms que les chiffres. Ainsi, l'usage des dictionnaires rend en général le code plus lisible. Par exemple, si nous souhaitions stocker les coordonnées (x,y,z)
d'un point dans l'espace : coors = [0, 1, 2] pour la version liste, coors = {"x": 0, "y": 1, "z": 2} pour la version dictionnaire. Un lecteur comprendra tout de suite que coors["z"] contient la coordonnée z
, ce sera moins intuitif avec coors[2].


### Itération sur les clés pour obtenir les valeurs


Il est possible d'obtenir toutes les valeurs d'un dictionnaire à partir de ses clés :

```python
>>> ani2 = {'nom': 'singe', 'poids': 70, 'taille': 1.75}
>>> for key in ani2:
...     print(key, ani2[key])
...
poids 70
nom singe
taille 1.75
```


### Méthodes .keys(), .values() et .items()


Les méthodes .keys() et .values() renvoient, comme vous pouvez vous en doutez, les clés et les valeurs d'un dictionnaire :

```python
>>> ani2.keys()
dict_keys(['poids', 'nom', 'taille'])
>>> ani2.values()
dict_values([70, 'singe', 1.75])
```


Les mentions dict_keys et dict_values indiquent que nous avons à faire à des objets un peu particuliers. Ils ne sont pas indexables (on ne peut pas retrouver un élément par indice, par exemple dico.keys()[0] renverra une erreur). Si besoin, nous pouvons les transformer en liste avec la fonction list() :

```python
>>> ani2.values()
dict_values(['singe', 70, 1.75])
>>> list(ani2.values())
['singe', 70, 1.75]
```


Toutefois, ce sont des objets « itérables », donc utilisables dans une boucle.


Enfin, il existe la méthode .items() qui renvoie un nouvel objet dict_items :

```python
>>> dico = {0: "t", 1: "o", 2: "t", 3: "o"}
>>> dico.items()
dict_items([(0, 't'), (1, 'o'), (2, 't'), (3, 'o')])
```


Celui-ci n'est pas indexable (on ne peut pas retrouver un élément par un indice) mais il est itérable :

```python
>>> dico.items()[2]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'dict_items' object is not subscriptable
>>> for key, val in dico.items():
...    print(key, val)
...
0 t
1 o
2 t
3 o
```


Notez la syntaxe particulière qui ressemble à la fonction enumerate() vue au chapitre 5 Boucles et comparaisons. On itère à la fois sur key et sur val. On verra plus bas que cela peut-être utile pour construire des dictionnaires de compréhension.


### Existence d'une clé


Pour vérifier si une clé existe dans un dictionnaire, on peut utiliser le test d’appartenance avec l'instruction in qui renvoie un booléen :

```python
>>> ani2 = {'nom': 'singe', 'poids': 70, 'taille': 1.75}
>>> if "poids" in ani2:
...     print("La clé 'poids' existe pour ani2")
...
La clé 'poids' existe pour ani2
>>> if "age" in ani2:
...     print("La clé 'age' existe pour ani2")
...
```


Dans le second test (lignes 5 à 7), le message n'est pas affiché car la clé age n'est pas présente dans le dictionnaire ani2.


### Méthode .get()

La méthode .get() extrait la valeur associée à une clé mais ne renvoie pas d'erreur si la clé n'existe pas :

```python
>>> ani2 = {'nom': 'singe', 'poids': 70, 'taille': 1.75}
>>> ani2.get("nom")
'singe'
>>> ani2.get("age")
>>> 
```


Ici la valeur associée à la clé nom est singe mais la clé age n'existe pas. On peut également indiquer à .get() une valeur par défaut si la clé n'existe pas :

```python
>>> ani2.get("age", 42)
42
```


### Tri par clés


On peut utiliser la fonction sorted() vue précédemment avec les listes pour trier un dictionnaire par ses clés :

```python
>>> ani2 = {'nom': 'singe', 'taille': 1.75, 'poids': 70}
>>> sorted(ani2)
['nom', 'poids', 'taille']
```


Les clés sont triées ici par ordre alphabétique.


### Tri par valeurs

Pour trier un dictionnaire par ses valeurs, il faut utiliser la fonction sorted avec l'argument key :

```python
>>> dico = {"a": 15, "b": 5, "c":20}
>>> sorted(dico, key=dico.get)
['b', 'a', 'c']
```


L'argument key=dico.get indique explicitement qu'il faut réaliser le tri par les valeurs du dictionnaire. On retrouve la méthode .get() vue plus haut, mais sans les parenthèses (key=dico.get mais pas key=dico.get()).


Attention, ce sont les clés du dictionnaires qui sont renvoyées, pas les valeurs. Ces clés sont cependant renvoyées dans un ordre qui permet d'obtenir les clés triées par ordre croissant :

```python
>>> dico = {"a": 15, "b": 5, "c":20}
>>> for key in sorted(dico, key=dico.get):
...     print(key, dico[key])
... 
b 5
a 15
c 20
```


Enfin, l'argument reverse=True fonctionne également :

```python
>>> dico = {"a": 15, "b": 5, "c":20}
>>> sorted(dico, key=dico.get, reverse=True)
['c', 'a', 'b']
```


> Remarque
> 
> Lorsqu'on trie un dictionnaire par ses valeurs, il faut être sûr que cela soit possible. Ce n'est, par exemple, pas le cas pour le dictionnaire ani2 car les valeurs sont des valeurs numériques et une chaîne de caractères :

```python
>>> ani2 = {'nom': 'singe', 'poids': 70, 'taille': 1.75}
>>> sorted(ani2, key=ani2.get)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: '<' not supported between instances of 'int' and 'str'
```


On obtient ici une erreur car Python ne sait pas comparer une chaîne de caractères (singe) avec des valeurs numériques (70 et 1.75).


### Clé associée au minimum ou au maximum des valeurs


Les fonctions min() et max(), que vous avez déjà manipulées dans les chapitres précédents, acceptent également l'argument key=. On peut ainsi obtenir la clé associée au minimum ou au maximum des valeurs d'un dictionnaire :

```python
>>> dico = {"a": 15, "b": 5, "c":20}
>>> max(dico, key=dico.get)
'c'
>>> min(dico, key=dico.get)
'b'
```


### Liste de dictionnaires


En créant une liste de dictionnaires qui possèdent les mêmes clés, on obtient une structure qui ressemble à une base de données :

```python
>>> animaux = [ani1, ani2]
>>> animaux
[{'nom': 'girafe', 'poids': 1100, 'taille': 5.0}, {'nom': 'singe',
'poids': 70, 'taille': 1.75}]
>>>
>>> for ani in animaux:
...     print(ani["nom"])
...
girafe
singe
```


Vous constatez ainsi que les dictionnaires permettent de gérer des structures complexes de manière plus explicite que les listes.


### Fonction dict()


> Conseil  
> Pour les débutants vous pouvez sauter cette rubrique.


La fonction dict() va convertir l'argument qui lui est passé en dictionnaire. Il s'agit donc d'une fonction de casting comme int(), str(), etc. Toutefois, l'argument qui lui est passé doit avoir une forme particulière : un objet séquentiel contenant d'autres objets séquentiels de 2 éléments. Par exemple, une liste de listes de 2 éléments :

```python
>>> liste_animaux = [["girafe", 2], ["singe", 3]]
>>> dict(liste_animaux)
{'girafe': 2, 'singe': 3}
```


Ou un tuple de tuples de 2 éléments (cf. rubrique suivante pour la définition d'un tuple), ou encore une combinaison liste / tuple :

```python
>>> tuple_animaux = (("girafe", 2), ("singe", 3))
>>> dict(tuple_animaux)
{'girafe': 2, 'singe': 3}
>>>
>>> dict([("girafe", 2), ("singe", 3)])
{'girafe': 2, 'singe': 3}
```


Si un des sous-éléments a plus de 2 éléments (ou moins), Python renvoie une erreur :

```python
>>> dict([("girafe", 2), ("singe", 3, 4)])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: dictionary update sequence element #1 has length 3; 2 is required
```


##  Tuples
### Définition


Les tuples (« n-uplets » en français) correspondent aux listes à la différence qu'ils sont non modifiables. On a vu dans le chapitre 11 Plus sur les listes que les listes pouvaient être modifiées par références, notamment lors de la copie de listes. Les tuples s'affranchissent de ce problème puisqu'ils sont non modifiables. Pratiquement, ils utilisent les parenthèses au lieu des crochets :

```python
>>> x = (1, 2, 3)
>>> x
(1, 2, 3)
>>> x[2]
3
>>> x[0:2]
(1, 2)
>>> x[2] = 15
Traceback (innermost last):
File "<stdin>", line 1, in ?
TypeError: object doesn't support item assignment
```


L'affectation et l'indiçage fonctionnent comme avec les listes. Mais si on essaie de modifier un des éléments du tuple, Python renvoie un message d'erreur. Si vous voulez ajouter un élément (ou le modifier), vous devez créer un autre tuple :

```python
>>> x = (1, 2, 3)
>>> x + (2,)
(1, 2, 3, 2)
```


> Remarque
> 
> Pour utiliser un tuple d'un seul élément, vous devez utiliser une syntaxe avec une virgule (element,), ceci pour éviter une ambiguïté avec une simple expression.
> 
> Autre particularité des tuples, il est possible d'en créer de nouveaux sans les parenthèses, dès lors que ceci ne pose pas d'ambiguïté avec une autre expression :


```python
>>> x = (1, 2, 3)
>>> x
(1, 2, 3)
>>> x = 1, 2, 3
>>> x
(1, 2, 3)
```


Toutefois, nous vous conseillons d'utiliser systématiquement les parenthèses afin d'éviter les confusions.

Enfin, on peut utiliser la fonction tuple(sequence) qui fonctionne exactement comme la fonction list(), c'est-à-dire qu'elle prend en argument un objet séquentiel et renvoie le tuple correspondant (opération de casting) :

```python
>>> tuple([1,2,3])
(1, 2, 3)
>>> tuple("ATGCCGCGAT")
('A', 'T', 'G', 'C', 'C', 'G', 'C', 'G', 'A', 'T')
```


> Remarque
> 
> Les listes, les dictionnaires et les tuples sont des objets qui peuvent contenir des collections d'autres objets. On peut donc construire des listes qui contiennent des dictionnaires, des tuples ou d'autres listes, mais aussi des dictionnaires contenant des tuples, des listes, etc.


### Itérations sur plusieurs valeurs à la fois


Pratiquement, nous avons déjà croisé les tuples avec la fonction enumerate() dans le chapitre 5 Boucles et comparaisons. Cette dernière permettait d'itérer en même temps sur les indices et les éléments d'une liste :

```python
>>> for i, elt in enumerate([75, -75, 0]):
...     print(i, elt)
...
0 75
1 -75
2 0
>>> for obj in enumerate([75, -75, 0]):
...     print(obj, type(obj))
...
(0, 75) <class 'tuple'>
(1, -75) <class 'tuple'>
(2, 0) <class 'tuple'>
```


En fin de compte, la fonction enumerate() itère sur une série de tuples. Pouvoir séparer i et elt dans la boucle est possible du fait que Python autorise l'affectation multiple du style i, elt = 0, 75 (cf. rubrique suivante).


Dans le même ordre d'idée, nous avons vu à la rubrique précédente la méthode .dict_items() qui permettait d'itérer sur des couples clé / valeur d'un dictionnaire :

```python
>>> dico = {"pinson": 2, "merle": 3}
>>> for key, val in dico.items():
...     print(key, val)
...
pinson 2
merle 3
>>> for obj in dico.items():
...     print(obj, type(obj))
...
('pinson', 2) <class 'tuple'>
('merle', 3) <class 'tuple'>
```


On voit que cette méthode .dict_items() itère comme enumerate() sur une série de tuples.


Sur la même base, on peut finalement itérer sur 3 valeurs en même temps à partir d'une liste de tuples de 3 éléments :

```python
>>> liste = [(i, i+1, i+2) for i in range(5, 8)]
>>> liste
[(5, 6, 7), (6, 7, 8), (7, 8, 9)]
>>> for x, y, z in liste:
...     print(x, y, z)
...
5 6 7
6 7 8
7 8 9
```


On pourrait concevoir la même chose sur 4 éléments, ou finalement autant que l'on veut. La seule restriction est d'avoir une correspondance systématique entre le nombre de variables d'itération (par exemple 3 ci-dessus avec x, y, z) et la longueur de chaque sous-tuple de la liste sur laquelle on itère (chaque sous-tuple a 3 éléments ci-dessus).


### Affectation multiple et le nom de variable _


L'affectation multiple est un mécanisme très puissant et important en Python. Pour rappel, il permet d'effectuer sur une même ligne plusieurs affectations en même temps, par exemple : x, y, z = 1, 2, 3. On voit que cette syntaxe correspond à un tuple de chaque côté de l'opérateur =. Notez qu'il serait possible de le faire également avec les listes : [x, y, z] = [1, 2, 3]. Toutefois, cette syntaxe est alourdie par la présence des crochets. On préfèrera donc la première syntaxe avec les tuples sans parenthèse.

> Remarque
> 
> Nous avons appelé l'opération x, y, z = 1, 2, 3 affectation multiple pour signifier que l'on affectait des valeurs à plusieurs variables en même temps. Toutefois, vous pourrez rencontrer aussi l'expression tuple unpacking que l'on pourrait traduire par « désempaquetage de tuple ». Cela signifie que l'on décompose le tuple initial 1, 2, 3 en 3 variables différentes (comme si on vidait son sac à dos, d'où le terme désempaquetage !).
 

Nous avions croisé l'importance de l'affectation multiple dans le chapitre 9 Fonctions lorsqu'une fonction renvoyait plusieurs valeurs.

```python
>>> def fct():
...     return 3, 14
...
>>> x, y = fct()
>>> print(x, y)
3 14
```


La syntaxe x, y = fct() permet de récupérer les 2 valeurs renvoyées par la fonction et de les affecter à la volée dans 2 variables différentes. Cela évite l'opération laborieuse de récupérer d'abord le tuple, puis de créer les variables en utilisant l'indiçage :

```python
>>> resultat = fct()
>>> resultat
(3, 14)
>>> x = resultat[0]
>>> y = resultat[1]
>>> print(x, y)
3 14
```


> Conseils
> 
> Lorsqu'une fonction renvoie plusieurs valeurs sous forme de tuple, ce sera bien sûr la forme x, y = fct() qui sera privilégiée.



Quand une fonction renvoie plusieurs valeurs mais que l'on ne souhaite pas les utiliser toutes dans la suite du code, on peut utiliser le nom de variable _ (underscore) pour indiquer que certaines valeurs ne nous intéressent pas :

```python
>>> def fct():
...     return 1, 2, 3, 4
...
>>> x, _, y, _ = fct()
>>> x
1
>>> y
3
```


Cela envoie le message à celui qui lit le code « je me fiche des valeurs récupérées dans ces variables _ ». Notez que l'on peut utiliser une ou plusieurs variables underscores(s). Dans l'exemple ci-dessus, la 2ème et la 4ème variable renvoyées par la fonction seront ignorées dans la suite du code. Cela a le mérite d'éviter la création de variables dont on ne se sert pas.


> Remarque
> 
> La variable _ a une autre signication spéciale dans l'interpréteur interactif, elle prend automatiquement la dernière valeur affichée :


```python
>>> 3
3
>>> _
3
>>> "mésange"
'mésange'
>>> _
'mésange'
```


Attention, cela n'est vrai que dans l'interpréteur.


> Remarque
> 
> Le underscore est couramment utilisé dans les noms de variable pour séparer les mots et être explicite, par exemple seq_ADN ou liste_listes_residus. On verra dans le chapitre 15 Bonnes pratiques en programmation Python que ce style de nommage est appelé snake_case. Toutefois, il faut éviter d'utiliser les underscores en début et/ou en fin de nom de variable (e.g. `_var`, `var_`, `__var`, `__var__`). On verra au chapitre 19 Avoir la classe avec les objets que ces underscores ont une signification particulière.


## Sets


Les containers de type set représentent un autre type d'objet séquentiel qui peut se révéler très pratique. Ils ont la particularité d'être non modifiables, non ordonnés et de ne contenir qu'une seule copie maximum de chaque élément. Pour créer un nouveau set on peut utiliser les accolades :

```python
>>> s = {1, 2, 3, 3}
>>> s
{1, 2, 3}
>>> type(s)
<class 'set'>
```

Notez que la répétition du 3 dans la définition du set en ligne 1 donne au final un seul 3 car chaque élément ne peut être présent qu'une seule fois. A quoi différencie-t-on un set d'un dictionnaire alors que les deux utilisent des accolades ? Le set sera défini seulement par des valeurs {val1, val2, ...} alors que le dictionnaire aura toujours des couples clé/valeur {clé1: val1, clé2: val2, ...}.

En général, on utilisera la fonction interne à Python set() pour générer un nouveau set. Celle-ci prend en argument n'importe quel objet itérable et le convertit en set (opération de casting) :

```python
>>> set([1, 2, 4, 1])
{1, 2, 4}
>>> set((2, 2, 2, 1))
{1, 2}
>>> set(range(5))
{0, 1, 2, 3, 4}
>>> set({"clé1": 1, "clé2": 2})
{'clé1', 'clé2'}
>>> set(["ti", "to", "to"])
{'ti', 'to'}
>>> set("Maître corbeau sur un arbre perché")
{'h', 'u', 'o', 'b', ' ', 'M', 'a', 'p', 'n', 'e', 'é', 'c', 'î', 's', 't', 'r'}
```


Nous avons dit plus haut que les sets ne sont pas ordonnés, il est donc impossible de récupérer un élément par sa position. Il est également impossible de modifier un de ses éléments. Par contre, les sets sont itérables :

```python
>>> s = set([1, 2, 4, 1])
>>> s[1]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'set' object is not subscriptable
>>> for elt in s:
...     print(elt)
...
1
2
4
```


Les containers de type set sont très utiles pour rechercher les éléments uniques d'une suite d'éléments. Cela revient à éliminer tous les doublons. Par exemple :

```python
>>> import random as rd
>>> l = [rd.randint(0, 9) for i in range(10)]
>>> l
[7, 9, 6, 6, 7, 3, 8, 5, 6, 7]
>>> set(l)
{3, 5, 6, 7, 8, 9}
```


On peut bien sûr transformer dans l'autre sens un set en liste. Cela permet par exemple d'éliminer les doublons de la liste initiale tout en récupérant une liste à la fin :

```python
>>> list(set([7, 9, 6, 6, 7, 3, 8, 5, 6, 7]))
[3, 5, 6, 7, 8, 9]
```


On peut faire des choses très puissantes. Par exemple, un compteur de lettres en combinaison avec une liste de compréhension, le tout en une ligne !

```python
>>> seq = "atctcgatcgatcgcgctagctagctcgccatacgtacgactacgt"
>>> set(seq)
{'c', 'g', 't', 'a'}
>>> [(base, seq.count(base)) for base in set(seq)]
[('c', 15), ('g', 10), ('t', 11), ('a', 10)]
```


Les sets permettent aussi l'évaluation d'union ou d'intersection mathématiques en conjonction avec les opérateurs respectivement | et & :

```python
>>> l = [3, 3, 5, 1, 3, 4, 1, 1, 4, 4]
>>> l2 = [3, 0, 5, 3, 3, 1, 1, 1, 2, 2]
>>> set(l) & set(l2)
{1, 3, 5}
>>> set(l) | set(l2)
{0, 1, 2, 3, 4, 5}
```


> Conseils
> 
> Pour aller plus loin, vous pouvez consulter deux articles sur les sites programiz et towardsdatascience.


### Dictionnaires et sets de compréhension


> Conseil : pour les débutants, vous pouvez passer cette rubrique.


Nous avons vu au chapitre 11 Plus sur les listes les listes de compréhension. Il est également possible de générer des dictionnaires de compréhension :

```python
>>> dico = {"a": 10, "g": 10, "t": 11, "c": 15}
>>> dico.items()
dict_items([('a', 10), ('g', 10), ('t', 11), ('c', 15)])
>>> {key:val*2 for key, val in dico.items()}
{'a': 20, 'g': 20, 't': 22, 'c': 30}
>>>
>>> {key:val for key, val in enumerate("toto")}
{0: 't', 1: 'o', 2: 't', 3: 'o'}
>>>
>>> seq = "atctcgatcgatcgcgctagctagctcgccatacgtacgactacgt"
>>> {base:seq.count(base) for base in set(seq)}
{'a': 10, 'g': 10, 't': 11, 'c': 15}
```


De manière générale, tout objet sur lequel on peut faire une double itération du type for var1, var2 in obj est utilisable pour créer un dictionnaire de compréhension. Si vous souhaitez aller plus loin, vous pouvez consulter cet article sur le site Datacamp.

Il est également possible de générer des sets de compréhension sur le même modèle que les listes de compréhension :

```python
>>> {i for i in range(10)}
{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
>>> {i**2 for i in range(10)}
{0, 1, 64, 4, 36, 9, 16, 49, 81, 25}
```


### Module collections


> Conseil : pour les débutants, vous pouvez passer cette rubrique.


Le module collections contient d'autres types de containers qui peuvent se révéler utiles, c'est une véritable mine d'or ! Nous n'aborderons pas tous ces objets ici, mais nous pouvons citer tout de même certains d'entre eux si vous souhaitez aller un peu plus loin :

*    les dictionnaires ordonnés qui se comportent comme les dictionnaires classiques mais qui sont ordonnés ;
*    les defaultdicts permettant de générer des valeurs par défaut quand on demande une clé qui n'existe pas (cela évite que Python génère une erreur) ;
*    les compteurs dont un exemple est montré ci-dessous ;
*    les namedtuples que nous évoquerons au chapitre 19 Avoir la classe avec les objets.


L'objet collection.Counter() est particulièrement intéressant et simple à utiliser. Il crée des compteurs à partir d'objets itérables, par exemple :

```python
>>> import collections
>>> compo_seq = collections.Counter("aatctccgatcgatcgatcgatgatc")
>>> compo_seq
Counter({'a': 7, 't': 7, 'c': 7, 'g': 5})
>>> type(compo_seq)
<class 'collections.Counter'>
>>> compo_seq["a"]
7
>>> compo_seq["n"]
0
```


On voit que Python a automatiquement compté chaque atgc de la chaîne de caractères passée en argument ! Cela crée un objet de type Counterqui se comporte ensuite comme un dictionnaire, à une exception près : si on appelle une clé qui n'existe pas dans l'itérable initiale (comme le n ci-dessus) cela renvoie 0.
