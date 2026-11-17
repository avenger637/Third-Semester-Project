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