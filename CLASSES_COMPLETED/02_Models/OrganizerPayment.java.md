public class OrganizerPayment {

    private int paymentId;
    private int organizerId;

    private double amount;
    private LocalDateTime paymentDate;

    private PaymentStatus status;

    public OrganizerPayment(int paymentId,
                           int organizerId, double amount);

    public int getPaymentId();
    public int getOrganizerId();
    public double getAmount();
    public PaymentStatus getStatus();

    public void setStatus(PaymentStatus status);
}

```java
import java.time.LocalDateTime;
import java.util.Locale;
public class OrganizerPayment {
    private int paymentId,organizerId; private double amount; private LocalDateTime paymentDate=LocalDateTime.now(); private PaymentStatus status=PaymentStatus.PENDING;
    public OrganizerPayment(int paymentId,int organizerId,double amount){if(!Double.isFinite(amount)||amount<0)throw new IllegalArgumentException("Amount must be nonnegative");this.paymentId=paymentId;this.organizerId=organizerId;this.amount=amount;}
    public int getPaymentId(){return paymentId;} public int getOrganizerId(){return organizerId;} public double getAmount(){return amount;} public LocalDateTime getPaymentDate(){return paymentDate;} public PaymentStatus getStatus(){return status;}
    public void setStatus(PaymentStatus status){if(status==null)throw new IllegalArgumentException("Status required");this.status=status;}
}
```
