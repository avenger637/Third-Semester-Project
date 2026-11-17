# 9. JDBC repository implementations

## JdbcUserRepository implements UserRepository
- `findById(Integer id)`
- `findAll()`
- `findByEmail(String email)`
- `authenticate(String email, String password)`
- `save(User user)`
- `update(User user)`
- `delete(Integer id)`
- `getAllOrganizers()`
- `getAllParticipants()`

## JdbcEventRepository implements EventRepository
- `findById(Integer id)`
- `findAll()`
- `findByOrganizer(int organizerId)`
- `findByParticipant(int participantId)`
- `findByStatus(EventStatus status)`
- `save(Event event)`
- `update(Event event)`
- `delete(Integer id)`

## JdbcRegistrationRepository implements RegistrationRepository
- `findById(Integer id)`
- `findAll()`
- `findByParticipant(int participantId)`
- `findByEvent(int eventId)`
- `save(Registration registration)`
- `update(Registration registration)`
- `delete(Integer id)`

## JdbcFinanceRepository implements FinanceRepository
- `saveExpense(Expense expense)`
- `saveOrganizerPayment(OrganizerPayment payment)`
- `getExpensesByEvent(int eventId)`
- `getPaymentsByOrganizer(int organizerId)`
- `getTransactionsByEvent(int eventId)`

## JdbcRequestRepository implements RequestRepository
- `findById(Integer id)`
- `findAll()`
- `findByEvent(int eventId)`
- `findByOrganizer(int organizerId)`
- `findPendingRequests()`
- `save(ItemRequest request)`
- `update(ItemRequest request)`
- `delete(Integer id)`

All SQL must use PreparedStatement parameters. Repositories should perform persistence—not application sorting, filtering, or algorithm demonstrations.
