# 12.2 Admin UI classes

## AdminDashboard extends DashboardPanel
- `AdminDashboard(Admin admin)`
- `refreshDashboard()`
- `loadEventStatistics()`
- `loadFinancialStatistics()`
- `loadOrganizerStatistics()`
- `openCreateEvent()`
- `openFinance()`

## CreateEventPanel
- `CreateEventPanel()`
- `initializeComponents()`
- `handleCreateEvent()`
- `validateEventForm()`
- `buildEventFromForm()`

## AssignOrganizerPanel
- `AssignOrganizerPanel()`
- `loadOrganizers()`
- `loadEvents()`
- `handleAssignment()`
- `refreshAssignments()`

## AllocateBudgetPanel
- `AllocateBudgetPanel()`
- `loadEvents()`
- `handleBudgetAllocation()`
- `showRemainingBudget()`

## AdminFinancePanel
- `AdminFinancePanel()`
- `loadFinancialData()`
- `loadOrganizerPayments()`
- `loadEventExpenses()`
- `calculateOverallFinance()`
- `handleOrganizerPayment()`
- `refreshFinanceTable()`

```java
import javax.swing.*; class AdminDashboard extends DashboardPanel { public AdminDashboard(Admin a){super(a);} public void refreshDashboard(){} public void loadEventStatistics(){} public void loadFinancialStatistics(){} public void loadOrganizerStatistics(){} public void openCreateEvent(){} public void openFinance(){} } class CreateEventPanel extends JPanel { public CreateEventPanel(){} public void initializeComponents(){} public void handleCreateEvent(){} public boolean validateEventForm(){return true;} public Event buildEventFromForm(){return null;} } class AssignOrganizerPanel extends JPanel { public AssignOrganizerPanel(){} public void loadOrganizers(){} public void loadEvents(){} public void handleAssignment(){} public void refreshAssignments(){} } class AllocateBudgetPanel extends JPanel { public AllocateBudgetPanel(){} public void loadEvents(){} public void handleBudgetAllocation(){} public void showRemainingBudget(){} } class AdminFinancePanel extends JPanel { public AdminFinancePanel(){} public void loadFinancialData(){} public void loadOrganizerPayments(){} public void loadEventExpenses(){} public void calculateOverallFinance(){} public void handleOrganizerPayment(){} public void refreshFinanceTable(){} }
```
