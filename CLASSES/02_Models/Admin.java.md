public class Admin extends User {

    public Admin(int userId, String name, String email,
                 String passwordHash);

    @Override
    public String getDashboardType();
}