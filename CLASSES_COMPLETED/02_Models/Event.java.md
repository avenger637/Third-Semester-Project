public class Event {

    private int eventId;
    private String title;
    private String description;

    private int organizerId;
    private int createdByAdminId;

    private LocalDateTime startDateTime;
    private LocalDateTime endDateTime;

    private Venue venue;
    private EventStatus status;

    public Event(int eventId, String title,
                 String description, int organizerId);

    public int getEventId();
    public String getTitle();
    public String getDescription();
    public int getOrganizerId();
    public LocalDateTime getStartDateTime();
    public LocalDateTime getEndDateTime();
    public Venue getVenue();
    public EventStatus getStatus();

    public void setTitle(String title);
    public void setDescription(String description);
    public void setOrganizerId(int organizerId);
    public void setStartDateTime(LocalDateTime dateTime);
    public void setEndDateTime(LocalDateTime dateTime);
    public void setVenue(Venue venue);
    public void setStatus(EventStatus status);
}

```java
import java.time.LocalDateTime;
import java.util.Locale;
public class Event {
    private int eventId; private String title; private String description; private int organizerId; private int createdByAdminId;
    private LocalDateTime startDateTime; private LocalDateTime endDateTime; private Venue venue; private EventStatus status=EventStatus.PLANNED;
    public Event(int eventId, String title, String description, int organizerId){
        if(eventId<0||title==null||title.trim().isEmpty())throw new IllegalArgumentException("Invalid event");
        this.eventId=eventId;this.title=title.trim();this.description=description==null?"":description.trim();this.organizerId=organizerId;
    }
    public int getEventId(){return eventId;} public String getTitle(){return title;} public String getDescription(){return description;}
    public int getOrganizerId(){return organizerId;} public LocalDateTime getStartDateTime(){return startDateTime;} public LocalDateTime getEndDateTime(){return endDateTime;}
    public Venue getVenue(){return venue;} public EventStatus getStatus(){return status;} public int getCreatedByAdminId(){return createdByAdminId;}
    public void setTitle(String title){if(title==null||title.trim().isEmpty())throw new IllegalArgumentException("Title required");this.title=title.trim();}
    public void setDescription(String description){this.description=description==null?"":description.trim();}
    public void setOrganizerId(int organizerId){this.organizerId=organizerId;} public void setCreatedByAdminId(int id){this.createdByAdminId=id;}
    public void setStartDateTime(LocalDateTime dateTime){this.startDateTime=dateTime;validateDates();}
    public void setEndDateTime(LocalDateTime dateTime){this.endDateTime=dateTime;validateDates();}
    public void setVenue(Venue venue){this.venue=venue;} public void setStatus(EventStatus status){if(status==null)throw new IllegalArgumentException("Status required");this.status=status;}
    private void validateDates(){if(startDateTime!=null&&endDateTime!=null&&(!endDateTime.isAfter(startDateTime)))throw new IllegalArgumentException("End time must be after start time");}
}
```
