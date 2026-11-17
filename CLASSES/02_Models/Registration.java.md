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