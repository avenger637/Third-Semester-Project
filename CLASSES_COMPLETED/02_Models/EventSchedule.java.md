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

```java
import java.time.LocalDateTime;
import java.util.Locale;
public class EventSchedule {
    private int scheduleId; private int eventId; private LinkedList<ScheduleItem> items=new LinkedList<>();
    public EventSchedule(int scheduleId,int eventId){this.scheduleId=scheduleId;this.eventId=eventId;}
    public int getScheduleId(){return scheduleId;} public int getEventId(){return eventId;}
    public void addItem(ScheduleItem item){if(item==null||hasConflict(item))throw new IllegalArgumentException("Item is null or conflicts");items.addLast(item);}
    public void removeItem(int itemId){for(int i=0;i<items.size();i++){if(items.get(i).getItemId()==itemId){items.removeAt(i);return;}}}
    public void updateItem(ScheduleItem item){if(item==null)throw new IllegalArgumentException("Item required");for(int i=0;i<items.size();i++){if(items.get(i).getItemId()==item.getItemId()){items.removeAt(i);break;}}addItem(item);}
    public LinkedList<ScheduleItem> getItems(){return items;} public void sortByStartTime(){SchedulingAlgorithms.sortSchedule(items);}
    public boolean hasConflict(ScheduleItem item){for(int i=0;i<items.size();i++)if(SchedulingAlgorithms.detectConflict(items.get(i),item))return true;return false;}
}
```
