
public abstract class User {
    private int userId;
    private String name;
    private String email;
    private String passwordHash;
    private UserRole role;
    private boolean active;

    public User(int userId, String name, String email,
                String passwordHash, UserRole role);

    public int getUserId();
    public String getName();
    public String getEmail();
    public UserRole getRole();
    public boolean isActive();

    public void setName(String name);
    public void setEmail(String email);
    public void setActive(boolean active);

    public abstract String getDashboardType();
}
