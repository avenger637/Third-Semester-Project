# 12.1 Common UI classes

## LoginFrame extends JFrame
Fields: `JTextField emailField`, `JPasswordField passwordField`.
Methods:
- `LoginFrame()`
- `initializeComponents()`
- `handleLogin()`
- `handleForgotPassword()`
- `showError(String message)`

## RegisterFrame extends JFrame
Methods:
- `RegisterFrame()`
- `initializeComponents()`
- `handleRegistration()`
- `validateForm()`

## MainFrame extends JFrame
Fields: `User currentUser`.
Methods:
- `MainFrame(User currentUser)`
- `loadDashboard()`
- `showPanel(JPanel panel)`
- `logout()`

## DashboardPanel extends JPanel (abstract)
Field: `User currentUser`.
Methods:
- `DashboardPanel(User currentUser)`
- `abstract refreshDashboard()`
- `initializeComponents()`

```java
import javax.swing.*; abstract class DashboardPanel extends JPanel { protected final User currentUser; protected DashboardPanel(User u){currentUser=u;} public abstract void refreshDashboard(); public void initializeComponents(){} } class LoginFrame extends JFrame { public LoginFrame(){initializeComponents();} public void initializeComponents(){} public void handleLogin(){} public void handleForgotPassword(){} public void showError(String m){JOptionPane.showMessageDialog(this,m,"Error",JOptionPane.ERROR_MESSAGE);} } class RegisterFrame extends JFrame { public RegisterFrame(){} public void initializeComponents(){} public void handleRegistration(){} public boolean validateForm(){return true;} } class MainFrame extends JFrame { private final User currentUser; public MainFrame(User u){currentUser=u;loadDashboard();} public void loadDashboard(){} public void showPanel(JPanel p){setContentPane(p);revalidate();} public void logout(){dispose();} }
```
