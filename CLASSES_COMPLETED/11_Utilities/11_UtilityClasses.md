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

```java
import java.math.*;
import java.time.LocalDateTime;
public class IDGenerator { private static final java.util.concurrent.atomic.AtomicLong COUNTER=new java.util.concurrent.atomic.AtomicLong(System.currentTimeMillis()); public static long generateId(){return COUNTER.incrementAndGet();} public static String generateReference(){return "EVT-"+java.util.UUID.randomUUID().toString().replace("-","").substring(0,16).toUpperCase(java.util.Locale.ROOT);} }
class DateTimeUtils { private static final java.time.format.DateTimeFormatter FORMATTER=java.time.format.DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm"); public static boolean isValidDate(LocalDateTime dateTime){return dateTime!=null&&!dateTime.isBefore(LocalDateTime.now().minusYears(100));} public static boolean isBefore(LocalDateTime first,LocalDateTime second){return first!=null&&second!=null&&first.isBefore(second);} public static boolean overlaps(LocalDateTime start1,LocalDateTime end1,LocalDateTime start2,LocalDateTime end2){if(start1==null||end1==null||start2==null||end2==null||!end1.isAfter(start1)||!end2.isAfter(start2))throw new IllegalArgumentException("Invalid interval");return start1.isBefore(end2)&&start2.isBefore(end1);} public static String formatDateTime(LocalDateTime dateTime){return dateTime==null?"":dateTime.format(FORMATTER);} }
class ValidationUtils { public static boolean isValidEmail(String email){if(email==null)return false;return email.matches("^[A-Za-z0-9.!#$%&'*+/=?^_`{|}~-]+@[A-Za-z0-9-]+(?:\\.[A-Za-z0-9-]+)*$");} public static boolean isValidPassword(String password){return password!=null&&password.length()>=8&&password.length()<=128;} public static boolean isValidName(String name){return name!=null&&name.trim().length()>=2&&name.trim().length()<=100;} public static boolean isValidPhone(String phone){return phone!=null&&phone.matches("\\+?[0-9][0-9 ()-]{6,19}");} public static boolean isValidAmount(BigDecimal amount){return amount!=null&&amount.signum()>=0&&amount.scale()<=2;} public static boolean isValidQuantity(int quantity){return quantity>0;} }
class MoneyUtils { public static BigDecimal add(BigDecimal a,BigDecimal b){require(a);require(b);return a.add(b).setScale(2,RoundingMode.HALF_UP);} public static BigDecimal subtract(BigDecimal a,BigDecimal b){require(a);require(b);BigDecimal r=a.subtract(b);if(r.signum()<0)throw new IllegalArgumentException("Result cannot be negative");return r.setScale(2,RoundingMode.HALF_UP);} public static BigDecimal calculatePercentage(BigDecimal amount,BigDecimal percentage){require(amount);require(percentage);if(percentage.signum()<0)throw new IllegalArgumentException("Percentage cannot be negative");return amount.multiply(percentage).divide(BigDecimal.valueOf(100),2,RoundingMode.HALF_UP);} public static String formatMoney(BigDecimal amount){require(amount);return java.text.NumberFormat.getCurrencyInstance(java.util.Locale.US).format(amount.setScale(2,RoundingMode.HALF_UP));} private static void require(BigDecimal v){if(v==null||v.signum()<0)throw new IllegalArgumentException("Invalid money value");} }
```
