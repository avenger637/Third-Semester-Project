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


Use transactions for operations that update multiple related records.

```java
import java.sql.*;
import java.util.Objects;
public class DatabaseConfig {
 private final String url,username,password;
 private DatabaseConfig(String url,String username,String password){if(url==null||username==null||password==null)throw new IllegalArgumentException("Database configuration is incomplete");this.url=url;this.username=username;this.password=password;}
 public String getUrl(){return url;} public String getUsername(){return username;} public String getPassword(){return password;}
 public static DatabaseConfig load(){String url=System.getenv("DB_URL");String user=System.getenv("DB_USERNAME");String password=System.getenv("DB_PASSWORD");if(url==null||url.isBlank())throw new IllegalStateException("DB_URL is required");if(user==null||password==null)throw new IllegalStateException("Database credentials are required");return new DatabaseConfig(url,user,password);}
}
class DatabaseConnection {
 private final DatabaseConfig config;
 public DatabaseConnection(DatabaseConfig config){this.config=Objects.requireNonNull(config);} public Connection getConnection() throws SQLException{return DriverManager.getConnection(config.getUrl(),config.getUsername(),config.getPassword());}
 public boolean testConnection(){try(Connection c=getConnection()){return c.isValid(2);}catch(SQLException e){return false;}} public void closeConnection(Connection connection){if(connection!=null)try{connection.close();}catch(SQLException ignored){}}
}
class DatabaseInitializer { private final DatabaseConnection connection; public DatabaseInitializer(DatabaseConnection connection){this.connection=Objects.requireNonNull(connection);} public void initializeDatabase(){try(Connection c=connection.getConnection()){createTables(c);createIndexes(c);}catch(SQLException e){throw new IllegalStateException("Database initialization failed",e);}} public void createTables(){try(Connection c=connection.getConnection()){createTables(c);}catch(SQLException e){throw new IllegalStateException(e);}} public void createIndexes(){try(Connection c=connection.getConnection()){createIndexes(c);}catch(SQLException e){throw new IllegalStateException(e);}} private void createTables(Connection c)throws SQLException{try(Statement s=c.createStatement()){s.execute("CREATE TABLE IF NOT EXISTS users (user_id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY, auth_user_id UUID UNIQUE, full_name TEXT NOT NULL, email TEXT NOT NULL UNIQUE, phone TEXT, created_at TIMESTAMPTZ NOT NULL DEFAULT now())");}} private void createIndexes(Connection c)throws SQLException{try(Statement s=c.createStatement()){s.execute("CREATE INDEX IF NOT EXISTS idx_users_email ON users(email)");}}
}
class TransactionManager { public interface TransactionCallback{void execute(Connection connection)throws SQLException;} public void begin(Connection c)throws SQLException{c.setAutoCommit(false);} public void commit(Connection c)throws SQLException{c.commit();c.setAutoCommit(true);} public void rollback(Connection c)throws SQLException{try{c.rollback();}finally{c.setAutoCommit(true);}} public void executeTransaction(Connection c,TransactionCallback callback){begin(c);try{callback.execute(c);commit(c);}catch(SQLException|RuntimeException e){rollback(c);throw e instanceof RuntimeException? (RuntimeException)e:new IllegalStateException(e);}}
}
```
