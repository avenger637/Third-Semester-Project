# 12.3 Organizer UI classes

## OrganizerDashboard extends DashboardPanel
- `OrganizerDashboard(Organizer organizer)`
- `refreshDashboard()`
- `loadAssignedEvents()`
- `loadUpcomingEvents()`
- `loadPendingRequests()`
- `loadFinancialSummary()`

## OrganizerEventsPanel
- `OrganizerEventsPanel()`
- `loadEvents()`
- `searchEvents(String keyword)`
- `sortEventsByDate()`
- `openEventDetails(int eventId)`

## EventDetailsPanel
- `EventDetailsPanel(Event event)`
- `loadEventDetails()`
- `updateEventDetails()`
- `openSchedule()`
- `openRequests()`
- `openParticipants()`
- `openFinance()`

## SchedulePanel
- `SchedulePanel(int eventId)`
- `loadSchedule()`
- `sortSchedule()`
- `handleAddActivity()`
- `handleUpdateActivity()`
- `handleDeleteActivity()`
- `checkScheduleConflicts()`
- `refreshScheduleTable()`

## RequestsPanel
- `RequestsPanel(int eventId)`
- `loadRequests()`
- `handleNewRequest()`
- `handleUpdateRequest()`
- `handleCancelRequest()`
- `refreshRequestsTable()`

## ParticipantsPanel
- `ParticipantsPanel(int eventId)`
- `loadRegisteredParticipants()`
- `loadCancelledParticipants()`
- `searchParticipant(String keyword)`
- `sortParticipants()`
- `refreshParticipantsTable()`

## OrganizerFinancePanel
- `OrganizerFinancePanel(int eventId)`
- `loadExpenses()`
- `loadBudget()`
- `loadRegistrationIncome()`
- `calculateRemainingBudget()`
- `sortExpenses()`
- `handleRecordExpense()`
- `refreshFinanceTable()

```java
import javax.swing.*; class OrganizerDashboard extends DashboardPanel { public OrganizerDashboard(Organizer o){super(o);} public void refreshDashboard(){} public void loadAssignedEvents(){} public void loadUpcomingEvents(){} public void loadPendingRequests(){} public void loadFinancialSummary(){} } class OrganizerEventsPanel extends JPanel { public OrganizerEventsPanel(){} public void loadEvents(){} public void searchEvents(String k){} public void sortEventsByDate(){} public void openEventDetails(int id){} } class EventDetailsPanel extends JPanel { public EventDetailsPanel(Event e){} public void loadEventDetails(){} public void updateEventDetails(){} public void openSchedule(){} public void openRequests(){} public void openParticipants(){} public void openFinance(){} } class SchedulePanel extends JPanel { public SchedulePanel(int id){} public void loadSchedule(){} public void sortSchedule(){} public void handleAddActivity(){} public void handleUpdateActivity(){} public void handleDeleteActivity(){} public void checkScheduleConflicts(){} public void refreshScheduleTable(){} } class RequestsPanel extends JPanel { public RequestsPanel(int id){} public void loadRequests(){} public void handleNewRequest(){} public void handleUpdateRequest(){} public void handleCancelRequest(){} public void refreshRequestsTable(){} } class ParticipantsPanel extends JPanel { public ParticipantsPanel(int id){} public void loadRegisteredParticipants(){} public void loadCancelledParticipants(){} public void searchParticipant(String k){} public void sortParticipants(){} public void refreshParticipantsTable(){} } class OrganizerFinancePanel extends JPanel { public OrganizerFinancePanel(int id){} public void loadExpenses(){} public void loadBudget(){} public void loadRegistrationIncome(){} public void calculateRemainingBudget(){} public void sortExpenses(){} public void handleRecordExpense(){} public void refreshFinanceTable(){} }
```
