public class ItemRequest {

    private int requestId;
    private int eventId;
    private int organizerId;

    private String itemName;
    private int quantity;
    private String description;

    private RequestStatus status;

    public ItemRequest(int requestId, int eventId,
                       int organizerId, String itemName,
                       int quantity);

    public int getRequestId();
    public int getEventId();
    public int getOrganizerId();
    public String getItemName();
    public int getQuantity();
    public RequestStatus getStatus();

    public void setQuantity(int quantity);
    public void setStatus(RequestStatus status);
}