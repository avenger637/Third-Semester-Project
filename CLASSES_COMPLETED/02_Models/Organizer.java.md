public class Organizer extends User {

    private double salary;

    public Organizer(int userId, String name, String email,
                    String passwordHash, double salary);

    public double getSalary();
    public void setSalary(double salary);

    @Override
    public String getDashboardType();
}

```java
import java.time.LocalDateTime;
import java.util.Locale;
public class Organizer extends User {
    private double salary;
    public Organizer(int userId,String name,String email,String passwordHash,double salary){super(userId,name,email,passwordHash,UserRole.ORGANIZER);setSalary(salary);}
    public double getSalary(){return salary;} public void setSalary(double salary){if(!Double.isFinite(salary)||salary<0)throw new IllegalArgumentException("Salary must be nonnegative");this.salary=salary;}
    @Override public String getDashboardType(){return "Organizer";}
}
```
