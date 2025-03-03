# Éléments de réponse des exercices

## Exercice 1

### User.java

```java
import java.util.UUID;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.nio.charset.StandardCharsets;

public abstract class User {
    private String username;
    private String passwordHash;
    private UUID userId;

    public User(String username, String password) {
        this.username = username;
        this.passwordHash = hashPassword(password);
        this.userId = UUID.randomUUID();
    }

    public String getUsername() {
        return username;
    }

    public UUID getUserId() {
        return userId;
    }

    private String hashPassword(String password) {
        try {
            MessageDigest digest = MessageDigest.getInstance("SHA-256");
            byte[] encodedhash = digest.digest(
                    password.getBytes(StandardCharsets.UTF_8));
            return bytesToHex(encodedhash);
        } catch (NoSuchAlgorithmException e) {
            throw new RuntimeException("Failed to hash password", e);
        }
    }

    private static String bytesToHex(byte[] hash) {
        StringBuilder hexString = new StringBuilder();
        for (byte b : hash) {
            String hex = Integer.toHexString(0xff & b);
            if (hex.length() == 1) hexString.append('0');
            hexString.append(hex);
        }
        return hexString.toString();
    }

    public boolean authenticate(String password) {
        return passwordHash.equals(hashPassword(password));
    }

    public abstract boolean hasPermission(Permission permission);
}
```

### Permission.java

```java
public enum Permission {
    READ,
    WRITE,
    EXECUTE,
    ADMIN
}
```

### Admin.java

```java
public class Admin extends User {
    public Admin(String username, String password) {
        super(username, password);
    }

    @Override
    public boolean hasPermission(Permission permission) {
        // Admin has all permissions
        return true;
    }
}
```

### RegularUser.java

```java
public class RegularUser extends User {
    public RegularUser(String username, String password) {
        super(username, password);
    }

    @Override
    public boolean hasPermission(Permission permission) {
        // Regular users have READ and EXECUTE permissions only
        return permission == Permission.READ || permission == Permission.EXECUTE;
    }
}
```

### SecurityException.java

```java
public class SecurityException extends Exception {
    public SecurityException(String message) {
        super(message);
    }
}
```

### AuthenticationService.java

```java
import java.util.ArrayList;
import java.util.List;

public class AuthenticationService {
    private User[] users;

    public AuthenticationService() {
        this.users = new User[10];
    }

    public User authenticate(String username, String password) throws SecurityException {
        for (User user : users) {
            if (user.getUsername().equals(username)) {
                if (user.authenticate(password)) {
                    return user;
                } else {
                    throw new SecurityException("Invalid password for user: " + username);
                }
            }
        }
        throw new SecurityException("User not found: " + username);
    }

    public void registerUser(User user) {
        // Check if username already exists
        for (User existingUser : users) {
            if (existingUser.getUsername().equals(user.getUsername())) {
                throw new IllegalArgumentException("Username already exists: " + user.getUsername());
            }
        }
        users.add(user);
    }
}
```
