# 14. Exception classes

Each custom exception extends `RuntimeException` (or `Exception` if checked handling is preferred) and provides a constructor accepting a message.

- `AuthenticationException(String message)`
- `AuthorizationException(String message)`
- `EventNotFoundException(String message)`
- `RegistrationException(String message)`
- `InsufficientBudgetException(String message)`
- `ValidationException(String message)`

Use exceptions for exceptional failures; use return values/results for expected outcomes such as a validation error or duplicate registration where appropriate.

```java
class AuthenticationException extends RuntimeException { public AuthenticationException(String m){super(m);} } class AuthorizationException extends RuntimeException { public AuthorizationException(String m){super(m);} } class EventNotFoundException extends RuntimeException { public EventNotFoundException(String m){super(m);} } class RegistrationException extends RuntimeException { public RegistrationException(String m){super(m);} } class InsufficientBudgetException extends RuntimeException { public InsufficientBudgetException(String m){super(m);} } class ValidationException extends RuntimeException { public ValidationException(String m){super(m);} }
```
