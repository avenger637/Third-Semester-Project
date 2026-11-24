# 13. Application bootstrap

## Main.java
- `static main(String[] args)`

## AppLauncher.java
- `initializeDatabase()`
- `initializeRepositories()`
- `initializeServices()`
- `initializeSecurity()`
- `launchApplication()`

## AppContext.java
Fields: service instances and shared application dependencies.
Methods:
- `static getInstance()`
- `initialize()`
- `getAuthenticationService()`
- `getAdminService()`
- `getOrganizerService()`
- `getParticipantService()`

## SessionManager.java
Field: `SecurityContext securityContext`.
Methods:
- `SessionManager()`
- `startSession(User user)`
- `getCurrentUser()`
- `getCurrentUserId()`
- `getCurrentRole()`
- `isLoggedIn()`
- `endSession()`

```java
public final class SessionManager { private final SecurityContext securityContext=new SecurityContext(); public void startSession(User u){securityContext.setCurrentUser(java.util.Objects.requireNonNull(u));} public User getCurrentUser(){return securityContext.getCurrentUser();} public Integer getCurrentUserId(){return securityContext.getCurrentUserId();} public UserRole getCurrentRole(){return securityContext.getCurrentRole();} public boolean isLoggedIn(){return securityContext.isAuthenticated();} public void endSession(){securityContext.clear();} } public final class Main { private Main(){} public static void main(String[] args){javax.swing.SwingUtilities.invokeLater(()->new LoginFrame().setVisible(true));} }
```
