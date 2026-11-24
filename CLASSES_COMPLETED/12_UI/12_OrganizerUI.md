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
