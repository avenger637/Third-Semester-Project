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