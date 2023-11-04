```
Q. Logging
Q. Swagger
```

**Logging**

for local development, Console.log used, but cant depend on Console.log in prod bcz it doesnt support log levels like warn, error or debug

There are three major concerns for choosing a suitable logging library: recording, formatting, and storing messages.

Another critical consideration for selecting a logging library is performance.
Since the logger will be used a lot throughout the codebase, it can harm your application's runtime performance.
Therefore, you should also investigate the performance characteristics of a library

**1. Use a Node.js Logging Library like Winston, Pino, Bunyan**

**Winston**
	The most popular logging library, with support for multiple transports. This allows you to easily configure your preferred storage location for your logs.
	it's only being used here because it's the most popular logging framework for Node.js.

	npm install winston

**2. Use the Correct Log Levels**
	If you used correct log levels in your application, it will be easy to distinguish between critical events that need to be immediately addressed 

	logging systems give different names to severity levels

**FATAL**: Used to represent a catastrophic situation — your application cannot recover. Logging at this level usually signifies the end of the program.

**ERROR**: Represents an error condition in the system that happens to halt a specific operation, but not the overall system. You can log at this level when a third-party API is returning errors.

**WARN**: Indicates runtime conditions that are undesirable or unusual, but not necessarily errors. An example could be using a backup data source when the primary source is unavailable or if child is killed then replace with other child in cluster

**INFO**: Info messages are purely informative. Events such as the startup or shutdown of a service are logged

**DEBUG**: Used to represent diagnostic information that may be needed for troubleshooting.

**TRACE**: Captures every possible detail about an application's behavior during development.


The winston library uses the following log levels by default — with error being the most severe

**{
  "error": 0,
  "warn": 1,
  "info": 2,
  "http": 3,
  "verbose": 4,
  "debug": 5,
  "silly": 6
}**


If the defaults do not suit your needs, you can change them while initializing a custom logger. 
For example, you can instead use the log levels discussed above.

```
const { createLogger, format, transports } = require("winston");

const logLevels = {
  fatal: 0,
  error: 1,
  warn: 2,
  info: 3,
  debug: 4,
  trace: 5,
};

const logger = createLogger({
  levels: logLevels,
  transports: [new transports.Console()],
});


When you want to log a message, you can reference the desired level directly on the custom logger, as shown below:

logger.info("System launch"); // {"message":"System launch","level":"info"}
logger.fatal("A critical failure!"); // {"message":"A critical failure!","level":"fatal"}

```

**3. Add the Right Amount of Context to Your Logs**
	Add basic information to the log, such as the timestamp of the event and the method where it occurred (or a stack trace, in the case of errors). 

**4. Avoid Logging Sensitive Information**
	Sensitive information includes social security numbers, addresses, passwords, credit card details, access tokens etc...

