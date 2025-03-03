# Exercice 3 : Personnages de jeu de rôles

## Objectif

Développez un système de personnages pour un jeu de rôles simple. Le système permettra la
création de différents types de personnages avec des capacités uniques et des équipements
variés. Vous devrez également implémenter une gestion appropriée des exceptions pour les
actions liées au jeu.

## Diagramme de Classes
```
@startuml
abstract class Character {
  -String name
  -int healthPoints
  -int level
  -List<Equipment> equipment
  -List<Ability> abilities
  +void attack(Character target)
  +void useAbility(Ability ability, Character target) throws GameException
  +void equip(Equipment item)
  +void takeDamage(int damage)
  +void addAbility(Ability ability)
  +{abstract} int calculateDamage()
}

class Warrior extends Character {
  -int strength
  +int calculateDamage()
}

class Mage extends Character {
  -int magicPower
  +int calculateDamage()
}

class Equipment {
  -String name
  -EquipmentType type
  -int bonusPoints
}

class Ability {
  -String name
  -int cooldown
  -int damage
  -int currentCooldown
  +void use(Character source, Character target) throws GameException
  +void reduceCooldown()
  +boolean isOnCooldown()
}

enum EquipmentType {
  WEAPON
  ARMOR
  ACCESSORY
}

class GameException extends Exception {
  +GameException(String message)
}

Character o-- Equipment
Character o-- Ability
@enduml
```

## Description

### 1. Classe abstraite `Character`

Créez une classe abstraite `Character` qui :

- stocke les attributs fondamentaux d'un personnage (nom, points de vie, niveau) ;
- maintient des listes d'équipements et de capacités ;
- fournit des méthodes pour les actions communes (attaquer, subir des dégâts) ;
- déclare une méthode abstraite pour le calcul des dégâts.

**Attributs** :

```java
private String name;
private int healthPoints;
private int level;
private List<Equipment> equipment;
private List<Ability> abilities;
```

**Méthodes importantes** :

```java
public void attack(Character target) // Attaque un autre personnage en infligeant des dégâts
public void useAbility(Ability ability, Character target) throws GameException // Utilise une capacité sur une cible
public void equip(Equipment item) // Équipe un objet, remplace l'équipement du même type si présent
public void takeDamage(int damage) // Subit des dégâts et réduit les points de vie
public void addAbility(Ability ability) // Ajoute une nouvelle capacité au personnage
public abstract int calculateDamage() // Calcule les dégâts que le personnage peut infliger
```

### 2. Classes de personnages

Implémentez deux classes concrètes qui héritent de `Character`:

#### `Warrior`

- Se concentre sur les attaques physiques et la force.
- Possède un attribut supplémentaire : `strength`.
- Calcule les dégâts principalement basés sur la force.
- Reçoit un bonus de dégâts (`WEAPON`).

**Calcul des dégâts** :

```
Dégâts de base = force * 2 + niveau * 3
Bonus d'arme = points de bonus de l'arme
Dégâts totaux = Dégâts de base + bonus d'arme
```

#### `Mage`

- Se concentre sur les attaques magiques et la puissance des sortilèges.
- Possède un attribut supplémentaire : `magicPower`.
- Calcule les dégâts principalement basés sur la puissance magique.
- Reçoit un bonus de dégâts (`ACCESSORY`).

**Calcul des dégâts** :

```
Dégâts de base = puissance magique * 3 + niveau * 2
Bonus d'accessoire = points de bonus de l'accessoire
Dégâts totaux = Dégâts de base + bonus d'accessoire
```

### 3. Classe `Equipment`

Créez une classe `Equipment` qui :

- représente un objet permettant d'équiper un personnage ;
- possède un nom, un type, et des points de bonus ;
- les points de bonus modifient les statistiques du personnage.

**Attributs** :

```java
private String name;
private EquipmentType type;
private int bonusPoints;
```

### 4. Énumération `EquipmentType`

Créez une énumération `EquipmentType` qui définit les types d'équipement suivants :

- `WEAPON`
- `ARMOR`
- `ACCESSORY`

### 5. Classe `Ability`

Implémentez une classe `Ability` qui :

- représente une action spéciale qu'un personnage peut effectuer ;
- possède un nom, un temps de recharge, et une valeur de dégâts ;
- gère son propre état de temps de recharge.

**Attributs** :

```java
private String name;
private int cooldown;
private int damage;
private int currentCooldown;
```

**Méthodes importantes** :

```java
public void use(Character source, Character target) throws GameException // Utilise la capacité sur une cible
public void reduceCooldown()  // Réduit le temps de recharge actuel
public boolean isOnCooldown() // Vérifie si la capacité est en cours de rechargement
```

**Règles d'utilisation** :

- Une capacité ne peut pas être utilisée si elle est en cours de recharge
- Après utilisation, le temps de recharge est réinitialisé à la valeur de cooldown
- Utiliser une capacité inflige les dégâts définis à la cible

### 6. Classe `GameException`

Créez une classe d'exception personnalisée qui :

- hérite de la classe `Exception` ;
- permet de signaler des erreurs liées au jeu telles que :
  - tentative d'utilisation d'une capacité lors de la recharge ;
  - tentative d'utilisation d'une capacité non possédée ;
  - d'autres erreurs spécifiques au jeu.

## Règles de jeu

### Équipement

- Un personnage ne peut s'équiper qu'avec un seul objet d'un type donné.
- Équiper un personnage avec un nouvel objet du même type remplace l'ancien.
- L'équipement affecte le calcul des dégâts selon le type de personnage.

### Combat

- L'attaque de base utilise la méthode `calculateDamage()` pour déterminer les dégâts.
- Les capacités infligent des dégâts fixes, indépendamment du personnage qui les utilise.
- Un personnage est vaincu lorsque ses points de vie atteignent 0.

### Capacités

- Les capacités ont une durée de recharge qui empêche leur utilisation répétée.
- Un personnage ne peut utiliser que les capacités qu'il possède.

## Conseils d'implémentation

1. Utilisez `ArrayList` pour stocker les équipements et les capacités.
2. Pensez à l'encapsulation des données et à la protection des attributs.
3. Créez des tests unitaires complets pour vérifier le comportement du système
4. Utilisez des méthodes de clonage ou de copie défensive pour protéger les collections internes
