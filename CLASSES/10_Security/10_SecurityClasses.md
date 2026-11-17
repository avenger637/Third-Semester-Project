# 10. Security classes

## PasswordHasher.java
- `hashPassword(String password)`
- `verifyPassword(String password, String storedHash)`

Use a vetted BCrypt or Argon2 library.

## SecurityContext.java
Fields: `User currentUser`.
Methods:
- `setCurrentUser(User user)`
- `getCurrentUser()`
- `getCurrentUserId()`
- `getCurrentRole()`
- `isAuthenticated()`
- `clear()`

## AccessControl.java
Methods:
- `checkRole(User user, UserRole role)`
- `canCreateEvent(User user)`
- `canAssignOrganizer(User user)`
- `canAllocateBudget(User user)`
- `canViewFinance(User user)`
- `canManageParticipants(User user)`
- `canRegisterForEvent(User user)`
- `canCancelRegistration(User user, Registration registration)`
- `canModifyEvent(User user, Event event)`

## EventOwnershipValidator.java
Methods:
- `isOrganizerAssigned(int organizerId, int eventId)`
- `validateOwnership(int organizerId, Event event)`
- `validateParticipantRegistration(int participantId, int eventId)`
- `requireOwnership(int organizerId, int eventId)`

Never trust role/user IDs supplied by UI fields. Derive identity from the authenticated session and verify authorization on every service operation.
