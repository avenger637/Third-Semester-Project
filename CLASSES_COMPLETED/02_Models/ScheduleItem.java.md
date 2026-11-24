public class ScheduleItem {

    private int itemId;
    private String title;
    private String description;

    private LocalDateTime startTime;
    private LocalDateTime endTime;

    public ScheduleItem(int itemId, String title,
                        LocalDateTime startTime,
                        LocalDateTime endTime);

    public int getItemId();
    public String getTitle();
    public LocalDateTime getStartTime();
    public LocalDateTime getEndTime();

    public void setTitle(String title);
    public void setStartTime(LocalDateTime startTime);
    public void setEndTime(LocalDateTime endTime);
}

```java
import java.time.LocalDateTime;
import java.util.Locale;
public class ScheduleItem {
    private int itemId; private String title,description=""; private LocalDateTime startTime,endTime;
    public ScheduleItem(int itemId,String title,LocalDateTime startTime,LocalDateTime endTime){setTitle(title);setStartTime(startTime);setEndTime(endTime);this.itemId=itemId;}
    public int getItemId(){return itemId;} public String getTitle(){return title;} public String getDescription(){return description;} public LocalDateTime getStartTime(){return startTime;} public LocalDateTime getEndTime(){return endTime;}
    public void setTitle(String title){if(title==null||title.trim().isEmpty())throw new IllegalArgumentException("Title required");this.title=title.trim();}
    public void setDescription(String description){this.description=description==null?"":description.trim();}
    public void setStartTime(LocalDateTime startTime){if(startTime==null)throw new IllegalArgumentException("Start required");this.startTime=startTime;validate();}
    public void setEndTime(LocalDateTime endTime){if(endTime==null)throw new IllegalArgumentException("End required");this.endTime=endTime;validate();}
    private void validate(){if(startTime!=null&&endTime!=null&&!endTime.isAfter(startTime))throw new IllegalArgumentException("End time must be after start time");}
}
```
