# 3. Enumerations

## UserRole.java
```java
public enum UserRole {
    ADMIN, ORGANIZER, PARTICIPANT
}
```

## EventStatus.java
```java
public enum EventStatus {
    PLANNED, APPROVED, ONGOING, COMPLETED, CANCELLED
}
```

## RegistrationStatus.java
```java
public enum RegistrationStatus {
    REGISTERED, CANCELLED
}
```

## RequestStatus.java
```java
public enum RequestStatus {
    PENDING, APPROVED, REJECTED, FULFILLED
}
```

## TransactionType.java
```java
public enum TransactionType {
    REGISTRATION_FEE, EVENT_EXPENSE, ORGANIZER_PAYMENT
}
```

## PaymentStatus.java
```java
public enum PaymentStatus {
    PENDING, PAID, FAILED, REFUNDED
}
```
