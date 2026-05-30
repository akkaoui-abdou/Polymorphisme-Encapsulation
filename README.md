# Programmation Orientée Objet (POO) en Python

## Table des matières

- [Polymorphisme](#polymorphisme)
- [Encapsulation](#encapsulation)

---

# Polymorphisme

## Définition

Le polymorphisme permet d'utiliser la même interface (même méthode ou fonction) pour des objets de types différents.

Il rend le code :

- plus flexible
- plus réutilisable
- plus facile à maintenir

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

## Polymorphisme avec héritage

```python
class Vehicule:
    def deplacer(self):
        print("Le véhicule se déplace")

class Voiture(Vehicule):
    def deplacer(self):
        print("La voiture roule")

class Avion(Vehicule):
    def deplacer(self):
        print("L'avion vole")

vehicules = [Voiture(), Avion()]

for v in vehicules:
    v.deplacer()
```

### Résultat

```text
La voiture roule
L'avion vole
```

---

## Duck Typing

Python utilise fortement le concept de Duck Typing :

> "Si ça ressemble à un canard et agit comme un canard, alors c'est un canard."

```python
class PDF:
    def ouvrir(self):
        print("Ouverture PDF")

class Image:
    def ouvrir(self):
        print("Ouverture image")

def afficher(fichier):
    fichier.ouvrir()

afficher(PDF())
afficher(Image())
```

### Résultat

```text
Ouverture PDF
Ouverture image
```

---

## Avantages du polymorphisme

- Réduit les conditions `if/elif`
- Facilite l'ajout de nouvelles classes
- Rend le code plus propre
- Favorise la réutilisation

---

# Encapsulation

## Définition

L'encapsulation consiste à protéger les données d'un objet et à contrôler leur accès.

Objectifs :

- protéger l'état interne de l'objet
- empêcher les modifications incorrectes
- rendre le code plus robuste

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

Le solde peut être modifié librement, même avec une valeur incohérente.

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

## Niveaux d'accès en Python

### Public

```python
self.nom
```

Accessible partout.

---

### Protégé (Convention)

```python
self._nom
```

Accessible mais destiné à un usage interne.

---

### Privé

```python
self.__nom
```

Python applique un mécanisme appelé **Name Mangling**.

```python
class Test:
    def __init__(self):
        self.__x = 10

t = Test()

# print(t.__x)  # Erreur
```

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

Approche recommandée en Python.

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

## Avantages de l'encapsulation

- Protection des données
- Validation des modifications
- Réduction des erreurs
- Maintenance simplifiée
- Meilleure évolutivité

---

# Résumé

| Concept | Objectif |
|----------|-----------|
| Polymorphisme | Utiliser une même interface pour différents objets |
| Encapsulation | Protéger les données et contrôler leur accès |

### Exemple de polymorphisme

```python
objet.methode()
```

### Exemple d'encapsulation

```python
self.__attribut
```

L'utilisation combinée du polymorphisme et de l'encapsulation permet de créer des applications plus robustes, maintenables et évolutives.
