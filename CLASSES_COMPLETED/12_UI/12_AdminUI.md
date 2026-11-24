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
