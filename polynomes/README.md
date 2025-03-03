# Exercice 2 : Polynômes

## Objectif

Implémenter un système permettant de représenter et d'effectuer des opérations sur des polynômes. 
Le système devra gérer les opérations algébriques de base telles que l'addition, la soustraction
la multiplication, et la dérivation de polynômes tout en respectant les règles mathématiques appropriées.

## Diagramme de classes

Les classes à définir pour ce système sont les suivantes :

![Diagramme de classes des polynomes](polynomes/polynome.svg)

## Description

### 1. Classe `Term`

Créez une classe `Term` qui :

- représente un terme individuel dans un polynôme (coefficient et exposant) ;
- stocke le coefficient sous forme de `double` et l'exposant sous forme d'`int` ;
- fournit des méthodes pour les opérations sur les termes (ex : multiplication) ;
- implémente correctement les méthodes `equals()` et `toString()`.

**Méthodes importantes** :

```java
public Term multiplyBy(Term other) /* Multiplie le terme courant par un autre et retourne un nouveau terme */
public String toString() /* Retourne une représentation textuelle du terme (ex : "3.0x^2") */
```

**Règles de représentation textuelle** :

- Si le coefficient est 0, retournez simplement "0".
- Si l'exposant est 0, retournez seulement le coefficient (ex: "5.0").
- Si le coefficient est 1 et l'exposant > 0, ne pas afficher le coefficient (ex: "x^2" au lieu de "1.0x^2").
- Si l'exposant est 1, ne pas afficher l'exposant (ex: "3.0x" au lieu de "3.0x^1").

### 2. Classe `Polynomial`

Implémentez une classe `Polynomial` qui :

- représente un polynôme comme une collection de termes ;
- gère la combinaison des termes semblables (même exposant) ;
- offre des méthodes pour ajouter des termes au polynôme ;
- implémente correctement les méthodes `equals()` et `toString()`.

**Méthodes importantes** :

```java
public void addTerm(Term term) /* Ajoute un terme au polynôme, en combinant si nécessaire */
public List<Term> getTerms() /* Retourne une copie de la liste des termes */
public String toString() /* Retourne une représentation textuelle du polynôme (ex: "3.0x^2 + 2.0x + 1.0") */
```

**Règles pour la méthode addTerm()** :

- Si un terme avec le même exposant existe déjà, combinez les coefficients.
- Si la combinaison résulte en un coefficient de 0, supprimez le terme.
- N'ajoutez pas de termes avec un coefficient de 0.

### 3. Classe abstraite `MathOperation<T>`

Créez une classe abstraite `MathOperation<T>` qui :

- définit une interface générique pour les opérations mathématiques ;
- utilise les génériques Java pour permettre la réutilisation du code ;
- déclare des méthodes abstraites pour les opérations fondamentales.

**Méthodes abstraites** :

```java
public abstract T add(T a, T b)      /* Addition de deux objets de type T */
public abstract T subtract(T a, T b) /* Soustraction de deux objets de type T */
public abstract T multiply(T a, T b) /* Multiplication de deux objets de type T */
```

### 4. Classe `PolynomialOperation`

Implémentez une classe `PolynomialOperation` qui :

- étend la classe `MathOperation<Polynomial>` ;
- implémente les opérations pour les polynômes (addition, soustraction, multiplication).

**Implémentation des méthodes** :

```java
@Override
public Polynomial add(Polynomial a, Polynomial b) /* Addition de deux polynômes */
@Override
public Polynomial subtract(Polynomial a, Polynomial b) /* Soustraction de deux polynômes */
@Override
public Polynomial multiply(Polynomial a, Polynomial b) /* Multiplication de deux polynômes */
```

**Règles** :

- Addition : ajoutez tous les termes des deux polynômes, en combinant ceux qui ont le même exposant ;
- Soustraction : ajoutez tous les termes du premier polynôme, puis ajoutez les termes du second polynôme avec leurs coefficients inversés ;
- Multiplication : multipliez chaque terme du premier polynôme par chaque terme du second polynôme, puis combinez les termes avec le même exposant.

### 5. Classe `MathException`

Créez une classe d'exception personnalisée `MathException` qui :

- hérite de la classe `Exception` ;
- permet de signaler des erreurs mathématiques spécifiques.

## Implémentation

1. Utilisez `ArrayList` pour stocker les termes dans la classe `Polynomial`.
2. Soyez attentif à la manipulation des nombres à virgule flottante pour l'égalité. Utilisez une marge d'erreur (epsilon) pour comparer les coefficients.
3. Prenez soin d'implémenter correctement la méthode `equals()` pour les deux classes `Term` et `Polynomial`.
4. Testez chaque fonctionnalité avec des cas simples et plus complexes.
5. Pour la représentation en chaîne, assurez-vous de traiter correctement les signes (+ et -) entre les termes.
