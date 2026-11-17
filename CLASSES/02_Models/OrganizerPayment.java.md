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