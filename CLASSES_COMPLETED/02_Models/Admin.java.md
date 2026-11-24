public class Admin extends User {

    public Admin(int userId, String name, String email,
                 String passwordHash);

    @Override
    public String getDashboardType();
}

```java
import java.time.LocalDateTime;
import java.util.Locale;
public class Admin extends User {
    public Admin(int userId, String name, String email, String passwordHash) {
        super(userId, name, email, passwordHash, UserRole.ADMIN);
    }
    @Override public String getDashboardType() { return "Admin"; }
}
```
