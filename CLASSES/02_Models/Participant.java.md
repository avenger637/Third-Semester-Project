public class Participant extends User {

    private String phone;

    public Participant(int userId, String name, String email,
                       String passwordHash, String phone);

    public String getPhone();
    public void setPhone(String phone);

    @Override
    public String getDashboardType();
}