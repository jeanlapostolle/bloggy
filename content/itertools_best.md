Title: Présentation de certaines fonctions built-in et itertools
Date: 2024-01-23 19:28
Category: Review
Summary: Je vous présente les éléments que j'utilise les plus régulièrement quand je développe en python.
Tags:python, Zeste de Savoir

Dans ce billet, nous verrons :

**Built-in**
- any()
- all()
- enumerate()
- isinstance()
- map()
- next() (generateur)
- sorted()
- staticmethod()
- zip()

**Itertools**
- count()
- cycle()
- repeat()
- pairwise()
- filterfalse()
- product()
- permutations()
- combinations()


## Built-in


Les fonctions built-in sont des fonctions intégrées de base à python, on y retrouve les fonctions de python classiques comme `int`, `float`, `list`, `dict`, `tuple`, `len`,`max`, `min`.
Ces fonctions sont directement intégrées à python et n'ont donc besoin d'aucun import pour les utiliser.


### any()

`any` prend en argument un iterable et retourne un booléen, elle retourne vrai si au moins une des valeurs de l'itérable est vrai.




```python
print(f"{any((False, False, True, False)) = }")
print(f"{any((False, False, False, False)) = }")
print(f"{any((True, True, True, True)) = }")
```

    any((False, False, True, False))=True
    any((False, False, False, False)) = False
    any((True, True, True, True)) = True


### all()

La fonction `all` prend en argument, elle aussi, un iterable et retourne un booléen, mais elle retourne vraie si toutes les valeurs de l'itérable sont vraies.


```python
print(f"{all((False, False, True, False)) = }")
print(f"{all((False, False, False, False)) = }")
print(f"{all((True, True, True, True)) = }")

```

    all((False, False, True, False)) = False
    all((False, False, False, False)) = False
    all((True, True, True, True)) = True


Les fonctions `any` et `all` sont très pratiques dans les tests.


> Mais attention à leurs négations, `not any` retourne vraie si toutes les valeurs de l'itérable en entrés sont fausses alors qu'on aurait pu faussement penser que ce rôle revenait à `not all`.

En python, un itérable est un objet qui sur lequel on peut itérer. C'ést à dire qu'u  élément à un élément qui le suit, excepté le dernier. Par exemple, une liste est un iterable. Mais aussi, un tuple, une string, un dictionnaire sont des itérables.
On peut aussi générer des itérables à la volée (à la demande), on appelle ça un générateur.

Un générateur utilise le mot clé yield afin de permettre de "mettre en pause" celui-ci et retourne une valeur. Ce mécanisme garde l'état de la fonction au moment du yield et ceci jusqu'à la prochaine demande d'élement au générateur où elle reprendra à la suite du yield.

Exemple d'un générateur avec le suite de fibonacci, tiré du [tutoriel sur les notions avancées de python](https://zestedesavoir.com/tutoriels/954/notions-de-python-avancees/3-further/1-generators/#1-1-generator) par @entwanne


```python
def fibo(n, a=0, b=1):
    for _ in range(n):
        a, b = b, a+b
        yield a

fib = fibo(8)
fib
```




    <generator object fibo at 0x7f7a1ece56c0>




> Ce qui nous est renvoyé est un objet de type `generator`.

Pour demandés de générer les éléments du génrateur, le mieux est encore d'essayer de la transformer en liste.


```python
list(fib)
```




    [1, 1, 2, 3, 5, 8, 13, 21]



### next()

La fonction `next` permet de générer l'élément suivant du générateur.


```python
fib = fibo(8)
print(next(fib))
print(next(fib))
print(next(fib))
print(next(fib))
print(next(fib))
```

    1
    1
    2
    3
    5


### enumerate()

`enumerate` est probablement la fonction que je vous présente ici que j'utilise le plus souvent. Elle permet, comme son nom l'indique, d'énumérer un itérable. C'est très pratique quand on a besoin de connaître la valeur et l'index d'un élément dans une boucle.



```python
x = [7,5,6,9,13,14,5,7]
for i, value in enumerate(x, start=5):
    if i % 2 == 0:
        value = value **2

list(enumerate(x, start=5))
```




    [(5, 7), (6, 5), (7, 6), (8, 9), (9, 13), (10, 14), (11, 5), (12, 7)]



``enumerate`` prend aussi un argument start pour faire commencer l'énumération à partir de n'importe quel entier, de base il est à 0, ce qui permet de suivre l'index.

### isinstance()

`isinstance` Permet de vérifier qu'un objet est une instance d'une classe. Contrairement à ``type`` qui fait la même chose, ``isinstance`` permet aussi de regarder si le type est une instance indirect.



```python
print( isinstance(3, int) )
print( type(3) == int )
```

    True
    True



```python
class my_int(int):
    pass

x = my_int(3)
print( isinstance(x, int) )
print( type(x) == int )
```

    True
    False


### map()

``map`` permet d'appliquer une fonction à un itérable. Attention cependant cette fonction retourne un itérateur et non une liste. Il faut donc la transformer en liste avant de pouvoir l'afficher.


```python
print( list( map(len, ["banane", "abricot", "orange"]) ) )
```

    [6, 7, 6]


### sorted()

``sorted`` permet de trier un itérable. Il permet d'utiliser l'ordre décroissant et de donner un fonction clé. On peut ainsi ne pas trier les éléments dans l'ordre des entiers croissants mais dans l'ordre inverse par valeurs absolus de leur deuxième élément par exemple.


```python
liste = {"Beta": 3, "Alpha": -2, "Gamma": -7}

print(sorted(liste, key=lambda x: abs(liste[x]), reverse=True))
```

    ['Gamma', 'Beta', 'Alpha']


### @staticmethod

@staticmetod est un décorateur permettant de rendre une méthode statique. C'est une méthode qui permet d'être utilisé sans instance de la classe.


```python
class Model:

    @staticmethod
    def modelnames():
        return ["Mistral", "chatGPT", 'Llama']

print(Model.modelnames())
```

    ['Mistral', 'chatGPT', 'Llama']


### zip()

``zip`` permet d'associer n à n les valeurs de n listes. C'est très utile quand on doit itérer sur deux ou plusieurs listes en même temps.


```python
viandes = ["Poulet", "Boeuf", "Porc"]
legumes = ["Haricot", "Chou fleur", "Tomates"]

for viande, legume in zip(viandes, legumes):
    print(legume, viande)

list(zip(viandes, legumes))
```

    Haricot Poulet
    Chou fleur Boeuf
    Tomates Porc





    [('Poulet', 'Haricot'), ('Boeuf', 'Chou fleur'), ('Porc', 'Tomates')]


 
> Il faut que les listes soit de la même taille, dans le cas contraire, on pourra utiliser `zip_longest` de module `itertools`


## itertools

Itertools est un module qui permet de faciliter la gestion d'itérable. Ce module appartenant à poython est installé en m^me tempsque python , donc pas besoin d'installation supplémentaire. Il faut toutefois l'importer pour pouvoir l'utiliser.

### count()

``count`` est un compteur qui ne s'arrète jamais. Il permet de créer un compteur avec un pas défini qui n'est pas forcément entier.


```python
from itertools import count

x = count(5.3, step=0.6)
[next(x) for _ in range(5)]

```




    [5.3,
     5.8999999999999995,
     6.499999999999999,
     7.099999999999999,
     7.699999999999998]



### cycle()

``cycle`` permet de boucler sur un itérable.


```python
from itertools import cycle

cy = cycle("RVB")
[next(cy) for _ in range(10)]
```




    ['R', 'V', 'B', 'R', 'V', 'B', 'R', 'V', 'B', 'R']



### pairwise()

``pairwise`` permet de lier deux par deux les éléments d'un itérable. 


```python
from itertools import pairwise

values = [7, 3, 8, 6, 14, 6, 9]

for x, y in pairwise(values):
    print(y - x)
```

    -4
    5
    -2
    8
    -8
    3


### filterfalse()

``filterfalse`` permet de ne renvoyer que les valeurs fausses d'une fonction.


```python
def est_bissextile(annee):
    return (annee % 4 == 0 and annee % 100 != 0) or (annee % 400 == 0)

from itertools import filterfalse

fi = filterfalse(est_bissextile, range(1900, 2030))

[next(fi) for i in range(10)]
```




    [1900, 1901, 1902, 1903, 1905, 1906, 1907, 1909, 1910, 1911]



### product()

``product`` fait le produit cartésien d'itérable. Ainsi il associe chaque élément à chaque combinaison des autres éléments.

Il peut prendre plusieurs itérables en entrées et peut permettre de les utiliser plusieurs fois avec l'argument ``repeat``


```python
from itertools import product

prod = product("ABC", "xyz")
print([i for i in prod])

pr = product("01", repeat=3)
[i for i in pr]
```

    [('A', 'x'), ('A', 'y'), ('A', 'z'), ('B', 'x'), ('B', 'y'), ('B', 'z'), ('C', 'x'), ('C', 'y'), ('C', 'z')]

    [('0', '0', '0'),
     ('0', '0', '1'),
     ('0', '1', '0'),
     ('0', '1', '1'),
     ('1', '0', '0'),
     ('1', '0', '1'),
     ('1', '1', '0'),
     ('1', '1', '1')]



### permutation()

``permutation`` permet de générer les permutations d'un itérable, on peut spécifier la longueur de la permutation avec l'argument `r`


```python
from itertools import permutations

lettres = "GARE"
pe = permutations(lettres)
[i for i in pe]
```

    [('G', 'A', 'R', 'E'),
     ('G', 'A', 'E', 'R'),
     ('G', 'R', 'A', 'E'),
     ('G', 'R', 'E', 'A'),
     ('G', 'E', 'A', 'R'),
     ('G', 'E', 'R', 'A'),
     ('A', 'G', 'R', 'E'),
     ('A', 'G', 'E', 'R'),
     ('A', 'R', 'G', 'E'),
     ('A', 'R', 'E', 'G'),
     ('A', 'E', 'G', 'R'),
     ('A', 'E', 'R', 'G'),
     ('R', 'G', 'A', 'E'),
     ('R', 'G', 'E', 'A'),
     ('R', 'A', 'G', 'E'),
     ('R', 'A', 'E', 'G'),
     ('R', 'E', 'G', 'A'),
     ('R', 'E', 'A', 'G'),
     ('E', 'G', 'A', 'R'),
     ('E', 'G', 'R', 'A'),
     ('E', 'A', 'G', 'R'),
     ('E', 'A', 'R', 'G'),
     ('E', 'R', 'G', 'A'),
     ('E', 'R', 'A', 'G')]



### combinations()

``combinations`` est comme permutation sans l'importance de l'ordre, c'est-à-dire que 'BA' est une solution identique à 'AB'



Et voilà ! J’espère que cette rétrospective des fonctionnalités que j’utilise le plus souvent pourra vous être utile pour améliorer votre python.
