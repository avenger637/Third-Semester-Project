public class Venue {

    private int venueId;
    private String name;
    private String location;
    private int capacity;
    private double rentalCost;

    public Venue(int venueId, String name,
                 String location, int capacity,
                 double rentalCost);

    public int getVenueId();
    public String getName();
    public String getLocation();
    public int getCapacity();
    public double getRentalCost();

    public boolean isAvailable(LocalDateTime start,
                              LocalDateTime end);
}

```java
import java.time.LocalDateTime;
import java.util.Locale;
public class Venue {
    private int venueId,capacity; private String name,location; private double rentalCost;
    public Venue(int venueId,String name,String location,int capacity,double rentalCost){if(name==null||name.trim().isEmpty()||capacity<0||!Double.isFinite(rentalCost)||rentalCost<0)throw new IllegalArgumentException("Invalid venue");this.venueId=venueId;this.name=name.trim();this.location=location==null?"":location.trim();this.capacity=capacity;this.rentalCost=rentalCost;}
    public int getVenueId(){return venueId;} public String getName(){return name;} public String getLocation(){return location;} public int getCapacity(){return capacity;} public double getRentalCost(){return rentalCost;}
    public boolean isAvailable(LocalDateTime start,LocalDateTime end){return start!=null&&end!=null&&end.isAfter(start);} 
}
```
