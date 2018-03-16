---
title: "Logging Within A Resource"
weight: 30
---

Resource code generated using the REDHAWK code generators provides all the necessary prerequisites for establishing and configuring the logging capability.

### C++

For C++ implementations, the generated REDHAWK code contains macro definitions that define a logger object with the class name for your resource. For example, for component `SigGen`, the logger is `SigGen_i`.

#### Setup

The following example describes how to define and declare the `SigGen_i` logger.

```c++
//SigGen.h
// this macro defines a logging object
ENABLE_LOGGING;
//SigGen.cpp
// this macro declares the logging object
PREPARE_LOGGING( SigGen_i );
```

#### Use

To add logging messages within your resource’s code, the following macros are available. These macros use the predefined class logger as the input parameter.

  - `LOG_FATAL(<component_class_name>, message text )`
  - `LOG_ERROR(<component_class_name>, message text )`
  - `LOG_WARN(<component_class_name>, message text )`
  - `LOG_INFO(<component_class_name>, message text )`
  - `LOG_DEBUG(<component_class_name>, message text )`
  - `LOG_TRACE(<component_class_name>, message text )`

The following example adds `DEBUG`-level logging messages to the predefined logger `SigGen_i`.

```c++
...
LOG_DEBUG(SigGen_i, "serviceFunction() example log message");
```

In addition to the resource’s predefined logger, you can request a new logger object using the `rh_logger` interface and named logger macros.

  - `RH_FATAL(logger_object, message text)`
  - `RH_ERROR(logger_object, message text)`
  - `RH_WARN(logger_object, message text)`
  - `RH_INFO(logger_object, message text)`
  - `RH_DEBUG(logger_object, message text)`
  - `RH_TRACE(logger_object, message text)`

The following example creates a new logger object (`myLogger`) and adds `DEBUG`-level logging messages to `myLogger` using the `rh_logger` interface.

```c++
// local named logger
LOGGER myLogger = rh_logger::getLogger("AnotherLogger");
...
RH_DEBUG(myLogger, "serviceFunction() example log message");
```

### Java

For Java implementations, the log4j logger module must be imported for use before the logging capability can be established and configured.

#### Setup

The following example describes how to import the log4j logger module.

```java
//component.java
import org.apache.log4j.Logger;
class MyComponent {
  // gets logger object based on the class name...
  public final static Logger logger = Logger.getLogger(component.class.getName());
  ...
}
```

#### Use

The following example describes how to enable the logger with `DEBUG`-level logging messages.

```java
void someMethod() {
  logger.debug("serviceFunction() example log message")
};
```

Log4j supports the following severity levels for logging and enables you to programmatically change the severity level of the logger object.

```java
logger.fatal(...)
logger.warn(...)
logger.error(...)
logger.info(...)
logger.debug(...)
logger.trace(...)

logger.setLevel(Level.WARN)
```

### Python

#### Setup

All REDHAWK resources inherit from the Resource class, which defines the `_log` data member.

#### Use

An example logging method call for Python is:

```py
self._log.debug("process() example log message")
```

REDHAWK has extended the Python logging support to include the trace method functionality. As with the other logging capabilities, you can programatically change the logging level.

```py
self._log.fatal(...)
self._log.warn(...)
self._log.error(...)
self._log.info(...)
self._log.debug(...)
self._log.trace(...)

self._log.setLevel(logging.WARN)
```
