```
Q. Logging
Q. Swagger
```
---

**Q. Logging**

https://betterstack.com/community/guides/logging/how-to-install-setup-and-use-winston-and-morgan-to-log-node-js-applications/

---

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
  level : 'info'
  transports: [
    new winston.transports.Console()
  ]
});
```

When you want to log a message, you can reference the desired level directly on the custom logger, as shown below:

```
logger.info("System launch"); // {"message":"System launch","level":"info"}
logger.fatal("A critical failure!"); // {"message":"A critical failure!","level":"fatal"}
```

The level property on the logger determines which log messages will be emitted to the configured transports.
For example, level property was set to info in the previous section, only log entries with a minimum severity
 of info (or maximum integer priority of 3) will be written while all others are suppressed.
This means that only the info, warn,fatal and error messages will produce output with the current configuration.

To cause the other levels to produce output, you'll need to change the value of level property to the desired minimum.
 The reason for this configuration is so that you'll be able to run your application in production
 at one level (say info) and your development/testing/staging environments can be set to a less severe level like debug or silly, causing more information to be emitted.

---

**Formatting your log messages**
	Winston outputs its logs in JSON format by default, but it also supports other formats which are accessible on the winston.format object

 **adds a timestamp field to the each log entry:**
 
```
const winston = require('winston');
const { combine, timestamp, json } = winston.format;

const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  format: combine(timestamp(), json()),
  transports: [new winston.transports.Console()],
});

When you use a level method on the logger, you'll see a datetime value formatted as new Date().toISOString().

logger.info('Info message')

o/p: {"level":"info","message":"Info message","timestamp":"2022-01-25T15:50:09.641Z"}

```

combine() :  method merges multiple formats into one,

colorize() : assigns colors to the different log levels so that each level is easily identifiable.

timestamp() : method outputs a datatime value that corresponds to the time that the message was emitted.

align() :  method aligns the log messages

While you can format your log entries in any way you wish, the recommended practice for server applications is to stick with a structured logging format (like JSON)

---

**Configuring transports in Winston**

Transports in Winston refer to the storage location for your log entries. 
Winston provides great flexibility in choosing where you want your log entries to be outputted to. 

The following transport options are available in Winston by default:

Console : output logs to the Node.js console.

File : store log messages to one or more files.

HTTP : stream logs to an HTTP endpoint.

Stream : output logs to any Node.js stream.

Thus far, we've demonstrated the default Console transport  to output log messages to the Node.js console. 

Let's look at how we can store logs in a file next.

```
const winston = require('winston');
const { combine, timestamp, json } = winston.format;

const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  format: combine(timestamp(), json()),
  transports: [
    new winston.transports.File({
      filename: 'combined.log',
    }),
  ],
});

logger.info('Info message');
logger.error('Error message');
logger.warn('Warning message');
```

The snippet above configures the logger to output all emitted log messages to a file named combined.log.
When you run the snippet above, this file will be created with the following contents:

```
combined.log file:

{"level":"info","message":"Info message","timestamp":"2022-01-26T09:38:17.747Z"}
{"level":"error","message":"Error message","timestamp":"2022-01-26T09:38:17.748Z"}
{"level":"warn","message":"Warning message","timestamp":"2022-01-26T09:38:17.749Z"}

```

In a production application, it may not be ideal to log every single message into a single file as that will make filtering critical
issues harder since it will be mixed together with inconsequential messages.
A potential solution would be to use two File transports, one that logs all messages to a combined.log file,
 and another that logs messages with a minimum severity of error to a separate file.

```
const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  format: combine(timestamp(), json()),
  transports: [
    new winston.transports.File({
      filename: 'combined.log',
    }),
    new winston.transports.File({
      filename: 'app-error.log',
      level: 'error',
    }),
  ],
});

With this change in place, all your log entries will still be outputted to the combined.log file,
but a separate app-error.log will also be created and it will contain only error messages.

Note that the level property on the File() transport signifies the minimum severity that should be logged to the file.
If you change it from error to warn, it means that any message with a minimum severity of warn will be logged to the app-error.log file (fatal, warn and error levels in this case).

```

**3. Add the Right Amount of Context to Your Logs**
	Add basic information to the log, such as the timestamp of the event and the method where it occurred (or a stack trace, in the case of errors). 

**4. Avoid Logging Sensitive Information**
	Sensitive information includes social security numbers, addresses, passwords, credit card details, access tokens etc...

