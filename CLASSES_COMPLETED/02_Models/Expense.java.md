public class Expense {

    private int expenseId;
    private int eventId;
    private int organizerId;

    private String description;
    private double amount;
    private LocalDateTime expenseDate;

    public Expense(int expenseId, int eventId,
                   int organizerId, String description,
                   double amount);

    public int getExpenseId();
    public int getEventId();
    public double getAmount();
    public String getDescription();
    public LocalDateTime getExpenseDate();
}

```java
import java.time.LocalDateTime;
import java.util.Locale;
public class Expense {
    private int expenseId,eventId,organizerId; private String description; private double amount; private LocalDateTime expenseDate;
    public Expense(int expenseId,int eventId,int organizerId,String description,double amount){
        if(!Double.isFinite(amount)||amount<0)throw new IllegalArgumentException("Amount must be nonnegative");
        this.expenseId=expenseId;this.eventId=eventId;this.organizerId=organizerId;this.description=description==null?"":description.trim();this.amount=amount;this.expenseDate=LocalDateTime.now();
    }
    public int getExpenseId(){return expenseId;} public int getEventId(){return eventId;} public int getOrganizerId(){return organizerId;}
    public double getAmount(){return amount;} public String getDescription(){return description;} public LocalDateTime getExpenseDate(){return expenseDate;}
}
```
