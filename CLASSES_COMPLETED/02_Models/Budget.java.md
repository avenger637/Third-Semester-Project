public class Budget {

    private int budgetId;
    private int eventId;

    private double allocatedAmount;
    private double spentAmount;

    public Budget(int budgetId, int eventId,
                 double allocatedAmount);

    public double getAllocatedAmount();
    public double getSpentAmount();
    public double getRemainingAmount();

    public void allocate(double amount);
    public boolean canSpend(double amount);
    public void recordExpense(double amount);
}

```java
import java.time.LocalDateTime;
import java.util.Locale;
public class Budget {
    private int budgetId; private int eventId; private double allocatedAmount; private double spentAmount;
    public Budget(int budgetId, int eventId, double allocatedAmount) {
        if (budgetId < 0 || eventId < 0 || !Double.isFinite(allocatedAmount) || allocatedAmount < 0) throw new IllegalArgumentException("Invalid budget");
        this.budgetId=budgetId; this.eventId=eventId; this.allocatedAmount=allocatedAmount;
    }
    public int getBudgetId(){return budgetId;} public int getEventId(){return eventId;}
    public double getAllocatedAmount(){return allocatedAmount;} public double getSpentAmount(){return spentAmount;}
    public double getRemainingAmount(){return allocatedAmount-spentAmount;}
    public void allocate(double amount){if(!Double.isFinite(amount)||amount<0)throw new IllegalArgumentException("Amount must be nonnegative"); allocatedAmount+=amount;}
    public boolean canSpend(double amount){return Double.isFinite(amount)&&amount>=0&&amount<=getRemainingAmount();}
    public void recordExpense(double amount){if(!canSpend(amount))throw new IllegalArgumentException("Expense exceeds remaining budget"); spentAmount+=amount;}
}
```
