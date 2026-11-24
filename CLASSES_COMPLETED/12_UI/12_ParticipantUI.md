# 12.4 Participant UI classes

## ParticipantDashboard extends DashboardPanel
- `ParticipantDashboard(Participant participant)`
- `refreshDashboard()`
- `loadUpcomingEvents()`
- `loadMyRegistrations()`
- `loadNotifications()`

## BrowseEventsPanel
- `BrowseEventsPanel()`
- `loadEvents()`
- `searchEvents(String keyword)`
- `sortEventsByDate()`
- `filterUpcomingEvents()`
- `openEventDetails(int eventId)`

## EventRegistrationPanel
- `EventRegistrationPanel(int eventId)`
- `loadEventDetails()`
- `handleRegistration()`
- `handlePayment()`
- `validateRegistration()`

## MyRegistrationsPanel
- `MyRegistrationsPanel(int participantId)`
- `loadRegistrations()`
- `loadActiveRegistrations()`
- `loadCancelledRegistrations()`
- `handleCancellation()`
- `refreshRegistrationsTable()`
