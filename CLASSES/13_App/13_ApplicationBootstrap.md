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
