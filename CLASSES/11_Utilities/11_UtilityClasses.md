# 11. Utility classes

## IDGenerator.java
- `static generateId()`
- `static generateReference()`

Use database-generated primary keys for persisted records; utility IDs are for temporary/client references if needed.

## DateTimeUtils.java
- `static isValidDate(LocalDateTime dateTime)`
- `static isBefore(LocalDateTime first, LocalDateTime second)`
- `static overlaps(LocalDateTime start1, LocalDateTime end1, LocalDateTime start2, LocalDateTime end2)`
- `static formatDateTime(LocalDateTime dateTime)`

## ValidationUtils.java
- `static isValidEmail(String email)`
- `static isValidPassword(String password)`
- `static isValidName(String name)`
- `static isValidPhone(String phone)`
- `static isValidAmount(BigDecimal amount)`
- `static isValidQuantity(int quantity)`

## MoneyUtils.java
- `static add(BigDecimal a, BigDecimal b)`
- `static subtract(BigDecimal a, BigDecimal b)`
- `static calculatePercentage(BigDecimal amount, BigDecimal percentage)`
- `static formatMoney(BigDecimal amount)`

Use `BigDecimal` for currency.
