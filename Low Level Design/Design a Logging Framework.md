
### Requirements

1. The logging framework should support different log levels, such as DEBUG, INFO, WARNING, ERROR, and FATAL.
2. It should allow logging messages with a timestamp, log level, and message content.
3. The framework should support multiple output destinations, such as console, file, and database.
4. It should provide a configuration mechanism to set the log level and output destination.
5. The logging framework should be thread-safe to handle concurrent logging from multiple threads.
6. It should be extensible to accommodate new log levels and output destinations in the future.

## Classes, Interfaces and Enumerations

1. The **LogLevel** enum defines the different log levels supported by the logging framework.

```
public enum LogLevel 
{
	DEBUG, INFO, WARNING, ERROR, FATAL
}
```

2. The **LogMessage** class represents a log message with a timestamp, log level, and message content.

```
public class LogMessage {

	private LogLevel logLevel;
	private long timestamp;
	private String messageContent;

	public LogMessage(LogLevel logLevel, String message) {
		this.logLevel = logLevel;
		messageContent = message;
		this.timestamp = System.currentTimeMillis();
	}

	// Setters , Getters and toString() method
}
```


3. The **LogAppender** interface defines the contract for appending log messages to different output destinations.

```
public interface LogAppender {
     void append(LogMessage logMessage);
}
```

4. The **ConsoleAppender**, **FileAppender**, and **DatabaseAppender** classes are concrete implementations of the LogAppender interface, supporting logging to the console, file, and database, respectively.

ConsoleAppender class

```
public class ConsoleAppender implements LogAppender {

	@Override
	public void append(LogMessage logMessage) {
		System.out.println(logMessage);
	}
}
```

FileAppender class

```
public class FileAppender implements LogAppender {
	private final String filePath;

	public FileAppender(String filePath) {
		this.filePath = filePath;
	}

	@Override
	public void append(LogMessage logMessage) {
		try(FileWriter writer = new FileWriter(filePath, true)) {
			writer.write(logMessage.toString() + "\n");
		} catch(IOException ex)  {
			ex.printStackTrace();
		}
	}
}
```

DatabaseAppender class

```
public class DatabaseAppender implements LogAppender {
	private final String jdbcUrl;
	private final String username;
	private final String password;
	
	public DatabaseAppender(String jdbcUrl, String username, String password) {
		this.jdbcUrl = jdbcUrl;
		this.username = username;
		this.password = password;
	}

	public void append(LogMessage logMessage) {
		try(Connection connection = DriverManager.getConnection(jdbcUrl, username, password);
			PreparedStatement statement = connection.prepareStatement("INSERT INTO logs (level, message, timestamp) Values(?, ?, ?)")) {
				statement.setString(1 , logMessage.getLevel().toString());
				statement.setString(2 , logMessage.getMessage());
				statement.setString(3 , logMessage.getTimestamp());
				statement.executeUpdate();
				
			} catch(SQLException e) {
				e.printStackTrace();
			}
		
		}
	}
}
```


5. The **LoggerConfig** class holds the configuration settings for the logger, including the log level and the selected log appender.

```
 public class LoggerConfig {
	 private LogLevel level;
	 private LogAppender logAppender;

	public LoggerConfig(LogLevel level, LogAppender logAppender) {
		this.level = level;
		this.logAppender = logAppender;
	}

	// Setters and Getters
 }
```

6. The **Logger** class is a singleton that provides the main logging functionality. It allows setting the configuration, logging messages at different levels, and provides convenience methods for each log level.

```
 public class Logger {
	private static final Logger logger = new Logger();
	private LoggerConfig config;

	private Logger() {
	    config = new LoggerConfig(LogLevel.INFO, new ConsoleAppender());
	}

	public static Logger getInstance() {
		return logger;
	}

	public void setConfig(LoggerConfig config) {
		this.config = config;
	}

	private void log(LogLevel level, String message) {
		if(level.ordinal() >= config.getLogLevel().oridinal()) {
			LogMessage logMessage = new LogMessage(level, message);
			config.getLogAppender().append(logMessage);
		}
	}

	public void debug(String message) {
		log(LogLevel.DEBUG, message);
	}

	public void info(String message) {
		log(LogLevel.INFO, message);
	}

	public void warning(String message) {
		log(LogLevel.WARNING, message);
	}

	public void error(String message) {
		log(LogLevel.ERROR, message);
	}

	public void fatal(String message) {
		log(LogLevel.FATAL, message);
	}
 }
```



