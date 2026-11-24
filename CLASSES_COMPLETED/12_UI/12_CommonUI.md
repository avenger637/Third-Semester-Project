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
