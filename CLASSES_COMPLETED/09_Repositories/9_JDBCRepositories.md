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

```java
import java.sql.*;
import java.util.Objects;
class JdbcUserRepository implements UserRepository {
 private final DatabaseConnection db; public JdbcUserRepository(DatabaseConnection db){this.db=Objects.requireNonNull(db);} public User findById(Integer id){try(Connection c=db.getConnection();PreparedStatement p=c.prepareStatement("SELECT user_id,full_name,email,phone FROM users WHERE user_id=?")){p.setInt(1,id);try(ResultSet r=p.executeQuery()){if(r.next())return new Participant(r.getInt(1),r.getString(2),r.getString(3),"unavailable",r.getString(4));}}catch(SQLException e){throw new IllegalStateException(e);}return null;} public LinkedList<User> findAll(){return new LinkedList<>();} public User findByEmail(String email){try(Connection c=db.getConnection();PreparedStatement p=c.prepareStatement("SELECT user_id,full_name,email,phone FROM users WHERE email=?")){p.setString(1,email);try(ResultSet r=p.executeQuery()){if(r.next())return new Participant(r.getInt(1),r.getString(2),r.getString(3),"unavailable",r.getString(4));}}catch(SQLException e){throw new IllegalStateException(e);}return null;} public User authenticate(String email,String password){Objects.requireNonNull(password);User u=findByEmail(email);return u;} public boolean save(User u){try(Connection c=db.getConnection();PreparedStatement p=c.prepareStatement("INSERT INTO users(full_name,email,phone) VALUES(?,?,?)")){p.setString(1,u.getName());p.setString(2,u.getEmail());p.setObject(3,u instanceof Participant?((Participant)u).getPhone():null);return p.executeUpdate()==1;}catch(SQLException e){throw new IllegalStateException(e);}} public boolean update(User u){try(Connection c=db.getConnection();PreparedStatement p=c.prepareStatement("UPDATE users SET full_name=?,email=?,phone=? WHERE user_id=?")){p.setString(1,u.getName());p.setString(2,u.getEmail());p.setObject(3,u instanceof Participant?((Participant)u).getPhone():null);p.setInt(4,u.getUserId());return p.executeUpdate()==1;}catch(SQLException e){throw new IllegalStateException(e);}} public boolean delete(Integer id){try(Connection c=db.getConnection();PreparedStatement p=c.prepareStatement("DELETE FROM users WHERE user_id=?")){p.setInt(1,id);return p.executeUpdate()==1;}catch(SQLException e){throw new IllegalStateException(e);}} public LinkedList<Organizer> getAllOrganizers(){return new LinkedList<>();} public LinkedList<Participant> getAllParticipants(){return new LinkedList<>();}
}
class JdbcEventRepository implements EventRepository {private final DatabaseConnection db;public JdbcEventRepository(DatabaseConnection db){this.db=db;} public Event findById(Integer id){try(Connection c=db.getConnection();PreparedStatement p=c.prepareStatement("SELECT event_id,name,description,start_time,end_time,status,created_by FROM events WHERE event_id=?")){p.setInt(1,id);try(ResultSet r=p.executeQuery()){if(r.next())return readEvent(r);}}catch(SQLException e){throw new IllegalStateException(e);}return null;} private Event readEvent(ResultSet r)throws SQLException{Event e=new Event(r.getInt(1),r.getString(2),r.getString(3),r.getInt(7));Timestamp s=r.getTimestamp(4),f=r.getTimestamp(5);if(s!=null)e.setStartDateTime(s.toLocalDateTime());if(f!=null)e.setEndDateTime(f.toLocalDateTime());String status=r.getString(6);if(status!=null)try{e.setStatus(EventStatus.valueOf(status.toUpperCase(java.util.Locale.ROOT));}catch(IllegalArgumentException ignored){}return e;} public LinkedList<Event> findAll(){return query("SELECT event_id,name,description,start_time,end_time,status,created_by FROM events");} private LinkedList<Event> query(String sql,Object...args){LinkedList<Event> r=new LinkedList<>();try(Connection c=db.getConnection();PreparedStatement p=c.prepareStatement(sql)){for(int i=0;i<args.length;i++)p.setObject(i+1,args[i]);try(ResultSet rs=p.executeQuery()){while(rs.next())r.addLast(readEvent(rs));}}catch(SQLException e){throw new IllegalStateException(e);}return r;} public LinkedList<Event> findByOrganizer(int organizerId){return query("SELECT event_id,name,description,start_time,end_time,status,created_by FROM events WHERE created_by=?",organizerId);} public LinkedList<Event> findByParticipant(int participantId){return query("SELECT e.event_id,e.name,e.description,e.start_time,e.end_time,e.status,e.created_by FROM events e JOIN registrations r ON r.event_id=e.event_id WHERE r.participant_id=?",participantId);} public LinkedList<Event> findByStatus(EventStatus status){return query("SELECT event_id,name,description,start_time,end_time,status,created_by FROM events WHERE status=?",status.name().toLowerCase(java.util.Locale.ROOT));} public boolean save(Event e){try(Connection c=db.getConnection();PreparedStatement p=c.prepareStatement("INSERT INTO events(event_type_id,name,description,start_time,end_time,location_text,expected_attendees,budget_limit,status,created_by) VALUES(1,?,?,?, ?,NULL,NULL,NULL,?,?)")){p.setString(1,e.getTitle());p.setString(2,e.getDescription());p.setTimestamp(3,e.getStartDateTime()==null?null:Timestamp.valueOf(e.getStartDateTime()));p.setTimestamp(4,e.getEndDateTime()==null?null:Timestamp.valueOf(e.getEndDateTime()));p.setString(5,e.getStatus().name().toLowerCase(java.util.Locale.ROOT));p.setInt(6,e.getOrganizerId());return p.executeUpdate()==1;}catch(SQLException ex){throw new IllegalStateException(ex);}} public boolean update(Event e){try(Connection c=db.getConnection();PreparedStatement p=c.prepareStatement("UPDATE events SET name=?,description=?,start_time=?,end_time=?,status=?,updated_at=now() WHERE event_id=?")){p.setString(1,e.getTitle());p.setString(2,e.getDescription());p.setTimestamp(3,e.getStartDateTime()==null?null:Timestamp.valueOf(e.getStartDateTime()));p.setTimestamp(4,e.getEndDateTime()==null?null:Timestamp.valueOf(e.getEndDateTime()));p.setString(5,e.getStatus().name().toLowerCase(java.util.Locale.ROOT));p.setInt(6,e.getEventId());return p.executeUpdate()==1;}catch(SQLException ex){throw new IllegalStateException(ex);}} public boolean delete(Integer id){try(Connection c=db.getConnection();PreparedStatement p=c.prepareStatement("DELETE FROM events WHERE event_id=?")){p.setInt(1,id);return p.executeUpdate()==1;}catch(SQLException ex){throw new IllegalStateException(ex);}}
}
```
