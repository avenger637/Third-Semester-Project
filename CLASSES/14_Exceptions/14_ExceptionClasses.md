# 14. Exception classes

Each custom exception extends `RuntimeException` (or `Exception` if checked handling is preferred) and provides a constructor accepting a message.

- `AuthenticationException(String message)`
- `AuthorizationException(String message)`
- `EventNotFoundException(String message)`
- `RegistrationException(String message)`
- `InsufficientBudgetException(String message)`
- `ValidationException(String message)`

Use exceptions for exceptional failures; use return values/results for expected outcomes such as a validation error or duplicate registration where appropriate.
