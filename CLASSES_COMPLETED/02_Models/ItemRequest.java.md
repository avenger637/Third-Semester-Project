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

```java
import java.time.LocalDateTime;
import java.util.Locale;
public class ItemRequest {
    private int requestId,eventId,organizerId,quantity; private String itemName,description; private RequestStatus status=RequestStatus.PENDING;
    public ItemRequest(int requestId,int eventId,int organizerId,String itemName,int quantity){
        if(itemName==null||itemName.trim().isEmpty()||quantity<=0)throw new IllegalArgumentException("Invalid item request");
        this.requestId=requestId;this.eventId=eventId;this.organizerId=organizerId;this.itemName=itemName.trim();this.quantity=quantity;
    }
    public int getRequestId(){return requestId;} public int getEventId(){return eventId;} public int getOrganizerId(){return organizerId;}
    public String getItemName(){return itemName;} public int getQuantity(){return quantity;} public String getDescription(){return description;} public RequestStatus getStatus(){return status;}
    public void setDescription(String description){this.description=description==null?"":description.trim();}
    public void setQuantity(int quantity){if(quantity<=0)throw new IllegalArgumentException("Quantity must be positive");this.quantity=quantity;}
    public void setStatus(RequestStatus status){if(status==null)throw new IllegalArgumentException("Status required");this.status=status;}
}
```
