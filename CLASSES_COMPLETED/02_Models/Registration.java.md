public class Registration {

    private int registrationId;
    private int participantId;
    private int eventId;

    private LocalDateTime registrationDate;
    private RegistrationStatus status;

    private double registrationFee;
    private PaymentStatus paymentStatus;

    public Registration(int registrationId,
                       int participantId, int eventId);

    public int getRegistrationId();
    public int getParticipantId();
    public int getEventId();
    public RegistrationStatus getStatus();
    public double getRegistrationFee();
    public PaymentStatus getPaymentStatus();

    public void cancel();
    public void reactivate();
    public void setPaymentStatus(PaymentStatus status);
}

```java
import java.time.LocalDateTime;
import java.util.Locale;
public class Registration {
    private int registrationId,participantId,eventId; private LocalDateTime registrationDate=LocalDateTime.now(); private RegistrationStatus status=RegistrationStatus.REGISTERED;
    private double registrationFee; private PaymentStatus paymentStatus=PaymentStatus.PENDING;
    public Registration(int registrationId,int participantId,int eventId){this.registrationId=registrationId;this.participantId=participantId;this.eventId=eventId;}
    public int getRegistrationId(){return registrationId;} public int getParticipantId(){return participantId;} public int getEventId(){return eventId;} public LocalDateTime getRegistrationDate(){return registrationDate;} public RegistrationStatus getStatus(){return status;}
    public double getRegistrationFee(){return registrationFee;} public PaymentStatus getPaymentStatus(){return paymentStatus;} public void cancel(){status=RegistrationStatus.CANCELLED;} public void reactivate(){status=RegistrationStatus.REGISTERED;} public void setPaymentStatus(PaymentStatus status){if(status==null)throw new IllegalArgumentException("Status required");paymentStatus=status;} public void setRegistrationFee(double fee){if(!Double.isFinite(fee)||fee<0)throw new IllegalArgumentException("Fee must be nonnegative");registrationFee=fee;}
}
```
