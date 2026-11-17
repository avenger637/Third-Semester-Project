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