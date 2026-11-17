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