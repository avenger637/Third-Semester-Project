public class EventSchedule {

    private int scheduleId;
    private int eventId;

    private LinkedList<ScheduleItem> items;

    public EventSchedule(int scheduleId, int eventId);

    public void addItem(ScheduleItem item);
    public void removeItem(int itemId);
    public void updateItem(ScheduleItem item);

    public LinkedList<ScheduleItem> getItems();

    public void sortByStartTime();
    public boolean hasConflict(ScheduleItem item);
}