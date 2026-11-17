# 8. Database and JDBC classes

## DatabaseConfig.java
Fields: `url`, `username`, `password`.
Methods: `getUrl()`, `getUsername()`, `getPassword()`, `static load()`.

Keep credentials outside source control (environment variables or a protected configuration file).

## DatabaseConnection.java
Methods:
- `Connection getConnection()`
- `boolean testConnection()`
- `void closeConnection(Connection connection)`

Use try-with-resources for JDBC resources.

## DatabaseInitializer.java
Methods:
- `initializeDatabase()`
- `createTables()`
- `createIndexes()`

Prefer versioned migrations for a real project rather than destructive initialization.

## TransactionManager.java
Methods:
- `begin(Connection connection)`
- `commit(Connection connection)`
- `rollback(Connection connection)`
- `executeTransaction(TransactionCallback callback)`

Nested interface:
```java
public interface TransactionCallback {
    void execute(Connection connection) throws SQLException;
}
```

Use transactions for operations that update multiple related records.
