public class Organizer extends User {

    private double salary;

    public Organizer(int userId, String name, String email,
                    String passwordHash, double salary);

    public double getSalary();
    public void setSalary(double salary);

    @Override
    public String getDashboardType();
}