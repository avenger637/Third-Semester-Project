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