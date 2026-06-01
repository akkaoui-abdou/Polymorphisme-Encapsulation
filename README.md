# Programmation Orientée Objet (POO) en Python

## Table des matières

* [Polymorphisme](#polymorphisme)

  * [Types de polymorphisme](#types-de-polymorphisme)
* [Encapsulation](#encapsulation)

---

# Polymorphisme

## Définition

Le polymorphisme permet d'utiliser une même interface (méthode ou fonction) pour manipuler différents types d'objets.

### Avantages

* Réduction des conditions `if/elif`
* Code plus flexible
* Meilleure extensibilité
* Réutilisation du code

---

## Exemple simple

```python
class Chien:
    def parler(self):
        return "Wouf"

class Chat:
    def parler(self):
        return "Miaou"

animaux = [Chien(), Chat()]

for animal in animaux:
    print(animal.parler())
```

### Résultat

```text
Wouf
Miaou
```

---

# Types de polymorphisme

## 1. Polymorphisme d'inclusion (Héritage)

Une sous-classe redéfinit une méthode héritée de la classe parent.

```python
class Animal:
    def parler(self):
        print("Un animal fait un bruit")

class Chien(Animal):
    def parler(self):
        print("Wouf")

class Chat(Animal):
    def parler(self):
        print("Miaou")

animaux = [Chien(), Chat()]

for animal in animaux:
    animal.parler()
```

### Résultat

```text
Wouf
Miaou
```

---

## 2. Polymorphisme ad hoc (Surcharge)

Python ne supporte pas directement la surcharge de méthodes comme Java ou C++, mais elle peut être simulée.

```python
class Calcul:
    def addition(self, a, b, c=0):
        return a + b + c

calc = Calcul()

print(calc.addition(2, 3))
print(calc.addition(2, 3, 4))
```

### Résultat

```text
5
9
```

---

### Comprendre le polymorphisme ad hoc (Surcharge)

Le polymorphisme **ad hoc** consiste à utiliser le même nom pour une opération, mais avec des comportements différents selon les arguments ou les types manipulés.

Dans des langages comme Java ou C++, il est possible de définir plusieurs méthodes portant le même nom mais ayant des signatures différentes.

#### Exemple en Java

```java
class Calcul {

    int addition(int a, int b) {
        return a + b;
    }

    double addition(double a, double b) {
        return a + b;
    }

    int addition(int a, int b, int c) {
        return a + b + c;
    }
}
```

Le compilateur choisit automatiquement la bonne méthode à appeler.

---

### Pourquoi Python est différent ?

Python ne permet pas de définir plusieurs méthodes portant le même nom dans une même classe.

```python
class Calcul:

    def addition(self, a, b):
        return a + b

    def addition(self, a, b, c):
        return a + b + c
```

La deuxième définition remplace complètement la première.

```python
calc = Calcul()

calc.addition(1, 2)
```

Résultat :

```text
TypeError: addition() missing 1 required positional argument: 'c'
```

---

### Simuler la surcharge avec des arguments par défaut

```python
class Calcul:

    def addition(self, a, b, c=0):
        return a + b + c

calc = Calcul()

print(calc.addition(2, 3))
print(calc.addition(2, 3, 4))
```

Résultat :

```text
5
9
```

---

### Simuler la surcharge avec *args

```python
class Calcul:

    def addition(self, *args):
        return sum(args)

calc = Calcul()

print(calc.addition(1, 2))
print(calc.addition(1, 2, 3))
print(calc.addition(1, 2, 3, 4))
```

Résultat :

```text
3
6
10
```

---

### Adapter le comportement selon le type

```python
class Afficheur:

    def afficher(self, valeur):

        if isinstance(valeur, int):
            print("Entier :", valeur)

        elif isinstance(valeur, str):
            print("Texte :", valeur)

        elif isinstance(valeur, list):
            print("Liste :", valeur)

a = Afficheur()

a.afficher(10)
a.afficher("Bonjour")
a.afficher([1, 2, 3])
```

Résultat :

```text
Entier : 10
Texte : Bonjour
Liste : [1, 2, 3]
```

---

### Utiliser functools.singledispatch

Python fournit un mécanisme officiel permettant de surcharger une fonction selon le type du premier argument.

```python
from functools import singledispatch

@singledispatch
def afficher(obj):
    print("Type inconnu")

@afficher.register
def _(obj: int):
    print("Entier :", obj)

@afficher.register
def _(obj: str):
    print("Texte :", obj)

afficher(10)
afficher("Bonjour")
```

Résultat :

```text
Entier : 10
Texte : Bonjour
```

---

### La surcharge d'opérateurs : une forme de polymorphisme ad hoc

Lorsque Python rencontre l'opérateur `+`, il exécute une méthode différente selon le type des objets.

```python
print(2 + 3)
print("Bon" + "jour")
print([1, 2] + [3, 4])
```

Résultat :

```text
5
Bonjour
[1, 2, 3, 4]
```

Le même opérateur possède donc plusieurs comportements.

---

### Différence avec le polymorphisme d'inclusion

#### Polymorphisme ad hoc

Le comportement dépend des arguments ou de leurs types.

```python
addition(2, 3)
addition(2, 3, 4)
```

#### Polymorphisme d'inclusion

Le comportement dépend de l'objet qui exécute la méthode.

```python
animal.parler()
```

Si `animal` est un `Chien` :

```text
Wouf
```

Si `animal` est un `Chat` :

```text
Miaou
```

---

### À retenir

Le polymorphisme ad hoc est généralement implémenté en Python grâce à :

| Technique            | Utilisation                            |
| -------------------- | -------------------------------------- |
| Arguments par défaut | Simuler plusieurs signatures           |
| *args                | Nombre variable d'arguments            |
| **kwargs             | Paramètres nommés variables            |
| isinstance()         | Comportement selon le type             |
| singledispatch       | Surcharge officielle basée sur le type |
| **add**, **eq**, ... | Surcharge d'opérateurs                 |

Dans la pratique, les développeurs Python utilisent surtout les arguments par défaut, `*args` et la surcharge d'opérateurs.



## 3. Polymorphisme paramétrique

Une même fonction peut manipuler plusieurs types de données.

```python
def afficher(element):
    print(element)

afficher("Bonjour")
afficher(100)
afficher([1, 2, 3])
```

### Résultat

```text
Bonjour
100
[1, 2, 3]
```

---

## 4. Polymorphisme par Duck Typing

Python vérifie le comportement de l'objet plutôt que son type.

```python
class PDF:
    def ouvrir(self):
        print("Ouverture PDF")

class Image:
    def ouvrir(self):
        print("Ouverture Image")

def afficher_document(doc):
    doc.ouvrir()

afficher_document(PDF())
afficher_document(Image())
```

### Résultat

```text
Ouverture PDF
Ouverture Image
```

---

## 5. Polymorphisme par surcharge d'opérateurs

Les opérateurs peuvent être redéfinis grâce aux méthodes spéciales.

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, autre):
        return Point(
            self.x + autre.x,
            self.y + autre.y
        )

    def __str__(self):
        return f"({self.x}, {self.y})"

p1 = Point(2, 3)
p2 = Point(4, 5)

print(p1 + p2)
```

### Résultat

```text
(6, 8)
```

---

## Résumé des types de polymorphisme

| Type                   | Description                                |
| ---------------------- | ------------------------------------------ |
| Inclusion              | Redéfinition de méthodes via l'héritage    |
| Ad hoc                 | Même opération avec différentes signatures |
| Paramétrique           | Même code pour différents types            |
| Duck Typing            | Basé sur les comportements de l'objet      |
| Surcharge d'opérateurs | Personnalisation des opérateurs Python     |

---

# Encapsulation

## Définition

L'encapsulation consiste à protéger les données d'un objet et à contrôler leur accès.

Objectifs :

* Masquer les détails internes
* Préserver la cohérence des données
* Contrôler les modifications
* Faciliter la maintenance

---

## Exemple sans encapsulation

```python
class Compte:
    def __init__(self, solde):
        self.solde = solde

c = Compte(1000)

c.solde = -5000

print(c.solde)
```

### Résultat

```text
-5000
```

---

## Exemple avec encapsulation

```python
class Compte:
    def __init__(self, solde):
        self.__solde = solde

    def deposer(self, montant):
        if montant > 0:
            self.__solde += montant

    def retirer(self, montant):
        if 0 < montant <= self.__solde:
            self.__solde -= montant

    def afficher_solde(self):
        return self.__solde

c = Compte(1000)

c.retirer(200)

print(c.afficher_solde())
```

### Résultat

```text
800
```

---

## Niveaux d'accès

### Public

```python
self.nom
```

Accessible partout.

### Protégé

```python
self._nom
```

Convention indiquant un usage interne.

### Privé

```python
self.__nom
```

Protégé via le mécanisme de Name Mangling.

---

## Getters et Setters

### Approche classique

```python
class Personne:
    def __init__(self):
        self.__age = 0

    def get_age(self):
        return self.__age

    def set_age(self, age):
        if age >= 0:
            self.__age = age
```

---

## Utilisation de @property

```python
class Personne:
    def __init__(self):
        self.__age = 0

    @property
    def age(self):
        return self.__age

    @age.setter
    def age(self, valeur):
        if valeur >= 0:
            self.__age = valeur

p = Personne()

p.age = 25

print(p.age)
```

### Résultat

```text
25
```

---

## Résumé final

| Concept       | Objectif                                           |
| ------------- | -------------------------------------------------- |
| Polymorphisme | Utiliser une même interface pour différents objets |
| Encapsulation | Protéger les données et contrôler leur accès       |

### Exemple de polymorphisme

```python
objet.methode()
```

### Exemple d'encapsulation

```python
self.__attribut
```

Ces deux concepts sont essentiels pour construire des applications Python robustes, maintenables et évolutives.
