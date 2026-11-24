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

```java
import javax.swing.*; class ParticipantDashboard extends DashboardPanel { public ParticipantDashboard(Participant p){super(p);} public void refreshDashboard(){} public void loadUpcomingEvents(){} public void loadMyRegistrations(){} public void loadNotifications(){} } class BrowseEventsPanel extends JPanel { public BrowseEventsPanel(){} public void loadEvents(){} public void searchEvents(String k){} public void sortEventsByDate(){} public void filterUpcomingEvents(){} public void openEventDetails(int id){} } class EventRegistrationPanel extends JPanel { public EventRegistrationPanel(int id){} public void loadEventDetails(){} public void handleRegistration(){} public void handlePayment(){} public boolean validateRegistration(){return true;} } class MyRegistrationsPanel extends JPanel { public MyRegistrationsPanel(int id){} public void loadRegistrations(){} public void loadActiveRegistrations(){} public void loadCancelledRegistrations(){} public void handleCancellation(){} public void refreshRegistrationsTable(){} }
```
