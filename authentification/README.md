# Système d'authentification

## Objectif

Créer un système d'authentification simple qui implémente le contrôle d'accès des utilisateurs.
Le système doit gérer deux types d'utilisateurs : administrateur et utilisateur régulier avec
différents niveaux de permissions. Il faudra également implémenter une gestion appropriée des
exceptions pour les problèmes liés à la sécurité.

## Diagramme de classes

```
@startuml
abstract class User {
  -String username
  -String passwordHash
  -UUID userId
  +boolean authenticate(String password)
  +{abstract} boolean hasPermission(Permission permission)
}

class Admin extends User {
  +boolean hasPermission(Permission permission)
}

class RegularUser extends User {
  +boolean hasPermission(Permission permission)
}

class AuthenticationService {
  -List<User> users
  +User authenticate(String username, String password) throws SecurityException
  +void registerUser(User user)
}

enum Permission {
  READ
  WRITE
  EXECUTE
  ADMIN
}

class SecurityException extends Exception {
  +SecurityException(String message)
}

AuthenticationService o-- User
@enduml
```

## Travail demandé

### 1. Classe abstraite `User`

Créez une classe abstraite `User` qui :

- stocke le nom de l'utilisateur (`username`), la valeur de hachage du mot de passe (`passwordHash`), et un identifiant unique (`userId`) ;
- fournit des fonctionnalités d'authentification au travers de la méthode `authenticate()` ;
- possède une méthode abstraite pour la vérification des permissions `hasPermission(Permission permission)` ;
- utilise l'algorithme SHA-256 pour hacher les mots de passe ;
- implémente un constructeur qui prend en paramètre un nom d'utilisateur et un mot de passe.

**Méthodes importantes:**

```java
public boolean authenticate(String password) // Vérifie si le mot de passe fourni correspond au hash stocké
public abstract boolean hasPermission(Permission permission) // Vérifie si l'utilisateur a la permission spécifiée
```

### 2. Enum `Permission`

Créez une énumération `Permission` qui définit les niveaux d'accès suivants :

- `READ` : permission de lecture ; 
- `WRITE` : permission d'écriture ;
- `EXECUTE` : permission d'exécution ;
- `ADMIN` : permission d'administration (niveau le plus élevé).

### 3. Classes concrètes d'utilisateurs

Implémentez deux classes concrètes qui héritent de `User` :

#### `Admin`

- Un administrateur qui a toutes les permissions possibles.
- Sa méthode `hasPermission()` doit toujours retourner `true`, quelle que soit la permission demandée.

#### `RegularUser`

- Un utilisateur régulier qui n'a que des permissions limitées (`READ` et `EXECUTE`).
- Sa méthode `hasPermission()` doit retourner `true` uniquement pour les permissions `READ` et `EXECUTE`.

### 4. Classe `AuthenticationService`

Implémentez une classe de service qui :

- gère une liste d'utilisateurs ;
- permet d'enregistrer de nouveaux utilisateurs ;
- Permet d'authentifier les utilisateurs existants ;
- lance des exceptions appropriées pour les problèmes de sécurité.

**Méthodes importantes :**

```java
public User authenticate(String username, String password) throws SecurityException
// Authentifie un utilisateur en vérifiant son nom d'utilisateur et son mot de passe
// Lance une SecurityException si l'authentification échoue

public void registerUser(User user)
// Enregistre un nouvel utilisateur
// Vérifie si le nom d'utilisateur existe déjà
```

### 5. Classe `SecurityException`

Créez une classe d'exception personnalisée qui :

- hérite de la classe `Exception` ;
- permet de signaler des erreurs liées à la sécurité comme :
  - échec d'authentification ;
  - utilisateur non trouvé ;
  - tentative d'accès non autorisée.

## Conseils d'implémentation

1. Utilisez la classe `java.util.UUID` pour générer des identifiants uniques.
2. Utilisez `MessageDigest` de Java pour implémenter le hachage SHA-256.
3. Pensez à l'encapsulation: rendez privés tous les attributs qui ne doivent pas être modifiés directement.
4. Assurez-vous que votre système empêche les doublons de noms d'utilisateurs.
5. Vérifiez le bon fonctionnement de votre système à l'aide des tests fournis.
