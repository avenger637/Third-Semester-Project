public class Participant extends User {

    private String phone;

    public Participant(int userId, String name, String email,
                       String passwordHash, String phone);

    public String getPhone();
    public void setPhone(String phone);

    @Override
    public String getDashboardType();
}

```java
import java.time.LocalDateTime;
import java.util.Locale;
public class Participant extends User {
    private String phone;
    public Participant(int userId,String name,String email,String passwordHash,String phone){super(userId,name,email,passwordHash,UserRole.PARTICIPANT);setPhone(phone);}
    public String getPhone(){return phone;} public void setPhone(String phone){this.phone=phone==null?"":phone.trim();}
    @Override public String getDashboardType(){return "Participant";}
}
```
