---
title: "Configuring Logging Capabilities"
weight: 10
---

A resource has the ability to provide a severity level (logging level) with each logging message. The following severity levels are listed in order of increasing verbosity. `FATAL` messages describe events that are unrecoverable to the resource, whereas `DEBUG` messages enable developers to understand the processing behavior.

  - `FATAL`
  - `ERROR`
  - `WARN`
  - `INFO`
  - `DEBUG`
  - `TRACE`

Each different logging implementation library uses a log4j configuration file format to define the configuration context for that library. This file defines how and where the logging messages are recorded. For example, if the configuration logging level is set to `INFO`, then messages published at the `INFO`, `WARN`, `ERROR`, and `FATAL` severity levels are displayed in the log. All other message levels are suppressed. The following two log-level configuration options are also available.

  - `OFF`: Suppress all logging messages from the log
  - `ALL`: Allow all logging messages

The following code displays the default log4j configuration settings used by all REDHAWK resources.

```bash
log4j.rootLogger=INFO,STDOUT
log4j.appender.STDOUT=org.apache.log4j.ConsoleAppender
log4j.appender.STDOUT.layout=org.apache.log4j.PatternLayout
log4j.appender.STDOUT.layout.ConversionPattern="%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n \n"
```

This configuration suppresses logging levels above `INFO` and writes those messages to the standard out console.

### Configuration Context Tokens

For REDHAWK release 1.10 and above, the log4j configuration file can contain special tokens that expand to provide the runtime execution context of the resource. These tokens can be located anywhere in the configuration file and are resolved during resource startup. The following table describes the tokens.

##### log4j Configuration File Special Tokens
| **Token**                       | **Definition**                                                                        |
| :------------------------------ | :------------------------------------------------------------------------------------ |
| `@@@HOST.NAME@@@`               | Host name returned from gethostname call (multi-homed systems may have issues).       |
| `@@@HOST.IP@@@`                 | Dot notation IP Address of the machine on which the processing is running.            |
| `@@@NAME@@@`                    | Name of the resource given on the command line.                                       |
| `@@@INSTANCE@@@`                | Instance name given to the resource on the command line.                              |
| `@@@PID@@@`                     | pid of the running resource.                                                          |
| `@@@DOMAIN.NAME@@@`             | Name of the domain under which your resource is running.                              |
| `@@@DOMAIN.PATH@@@`             | The contents of the DOM_PATH parameter on the command line.                          |
| `@@@DEVICE_MANAGER.NAME@@@`     | Name of the Device Manager that started the device or service.                         |
| `@@@DEVICE_MANAGER.INSTANCE@@@` | Instance name of the Device Manager from the command line.                             |
| `@@@SERVICE.NAME@@@`            | Name of the service as specified on the command line.                                 |
| `@@@SERVICE.INSTANCE@@@`        | Instance name of the service as specified on the command line.                        |
| `@@@SERVICE.PID@@@`             | pid of the running service.                                                           |
| `@@@DEVICE.NAME@@@`             | Name of the device as specified on the command line.                                  |
| `@@@DEVICE.INSTANCE@@@`         | Instance name of the device as specified on the command line.                         |
| `@@@DEVICE.PID@@@`              | pid of the device.                                                                    |
| `@@@WAVEFORM.NAME@@@`           | Name of the waveform from the DOM_PATH command line variable.             |
| `@@@WAVEFORM.INSTANCE@@@`       | Instance name of the waveform from the DOM_PATH command line variable.    |
| `@@@COMPONENT.NAME@@@`          | Name of the component binding parameter as specified on the command line.             |
| `@@@COMPONENT.INSTANCE@@@`      | Instance name of the component identifier parameter as specified on the command line. |
| `@@@COMPONENT.PID@@@`           | pid of the component. |


This table lists the availability of token definitions for each REDHAWK resource type.

##### Availability of Tokens
| **Token**                            | **Domain Manager**        | **Device Manager**       | **Device** | **Service** | **Component** |
| :----------------------------------- | :------------------------ | :----------------------- | :--------- | :---------- | :------------ |
| `@@@HOST.IP@@@`                      | Y                         | Y                        | Y          | Y           | Y             |
| `@@@HOST.NAME@@@`                    | Y                         | Y                        | Y          | Y           | Y             |
| `@@@NAME@@@`                         | Domain Manager            | Y                        | Y          | Y           | Y             |
| `@@@INSTANCE@@@`                     | DOMAIN_MANAGER_1          | Y                        | Y          | Y           | Y             |
| `@@@PID@@@`                          | Y                         | Y                        | Y          | Y           | Y             |
| `@@@DOMAIN.NAME@@@`                  | Y                         | Y                        | Y          | Y           | Y             |
| `@@@DOMAIN.PATH@@@`                  | Y                         | Y                        | Y          | Y           | Y             |
| `@@@DEVICE_MANAGER.NAME@@@`          | N                         | DEVICE\_MANAGER          | Y          | Y           | N             |
| `@@@DEVICE_MANAGER.INSTANCE@@@`      | N                         | Y                        | Y          | Y           | N             |
| `@@@SERVICE.NAME@@@`                 | N                         | Y                        | N          | Y           | N             |
| `@@@SERVICE.INSTANCE@@@`             | N                         | Y                        | N          | Y           | N             |
| `@@@SERVICE.PID@@@`                  | N                         | Y                        | N          | Y           | N             |
| `@@@DEVICE.NAME@@@`                  | N                         | Y                        | Y          | N           | N             |
| `@@@DEVICE.INSTANCE@@@`              | N                         | Y                        | Y          | N           | N             |
| `@@@DEVICE.PID@@@`                   | N                         | Y                        | Y          | N           | N             |
| `@@@WAVEFORM.NAME@@@`                | N                         | N                        | N          | N           | Y             |
| `@@@WAVEFORM.INSTANCE@@@`            | N                         | N                        | N          | N           | Y             |
| `@@@COMPONENT.NAME@@@`               | N                         | N                        | N          | N           | Y             |
| `@@@COMPONENT.INSTANCE@@@`           | N                         | N                        | N          | N           | Y             |
| `@@@COMPONENT.PID@@@`                | N                         | N                        | N          | N           | Y             |


### Log Configuration Example - Simple Appender with a Named Logger

In the following example, the root most logger passes logging messages with a severity level `INFO` or less. Those messages are sent to the appenders called: `CONSOLE` and `FILE`. The `CONSOLE` appender messages are displayed in the console of the running application. The `FILE` appender writes log messages to a file called `allmsgs.out`.

If the resource uses a named logger, `EDET_cpp_impl1_i`, then log messages with a severity of `DEBUG` or less are diverted to a file called `edet_log.out`.

```bash
# Set root logger default levels and appender
log4j.rootLogger=INFO, CONSOLE, FILE

# Console Appender
log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
log4j.appender.STDERR=org.apache.log4j.ConsoleAppender
log4j.appender.STDERR.Threshold=ERROR
log4j.appender.STDERR.Target=System.err

# Default Log Appender
log4j.appender.FILE=org.apache.log4j.FileAppender
log4j.appender.FILE.Append=true
log4j.appender.FILE.File=allmsgs.out

# Edet Appender
log4j.appender.edetLog=org.apache.log4j.FileAppender
log4j.appender.edetLog.Append=true
log4j.appender.edetLog.File=edet_log.out

# Appender layout
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.CONSOLE.layout.ConversionPattern=%d{ISO8601}:
    %p:%c - %m [%F:%L]%n
log4j.appender.STDERR.layout=org.apache.log4j.PatternLayout
log4j.appender.STDERR.layout.ConversionPattern=%d{ISO8601}:
    %p:%c - %m [%F:%L]%n
log4j.appender.FILE.layout=org.apache.log4j.PatternLayout
log4j.appender.FILE.layout.ConversionPattern=%d{ISO8601}:
    %p:%c - %m [%F:%L]%n
log4j.appender.edetLog.layout=org.apache.log4j.PatternLayout
log4j.appender.edetLog.layout.ConversionPattern=%d{ISO8601}:
    %p:%c - %m [%F:%L]%n
log4j.appender.NULL.layout=org.apache.log4j.PatternLayout
log4j.appender.NULL.layout.ConversionPattern=%n

log4j.category.EDET_cpp_impl1_i=DEBUG, edetLog
log4j.additivity.EDET_cpp_impl1_i=false
```

### Log Configuration Example - Configuring a Component with Token Macros

The logging configuration information for component `MEGA_WORKER` is configured from `$SDRROOT/dom/logcfg/component.log4j.cfg`. Prior to configuring the underlying logging library, the configuration information is processed for the context macros (in this example, `@@@WAVEFORM.NAME@@@`, `@@@COMPONENT.NAME@@@` and `@@@COMPONENT.PID@@@`). The root most logger passes logging messages with a severity level `INFO` or less, to the appenders called: `CONSOLE` and `FILE`. The `CONSOLE` appender messages are displayed in the console of the running application. For the `FILE` appender, the destination file is: `/data/logdir/MY_EXAMPLE_1/MEGA_WORKER_1.212.log`.

```bash
Waveform: MY_EXAMPLE
Component: MEGA_WORKER
 property: LOGGING_CONFIG_URI = "sca://logcfg/component.log4j.cfg"

 \$SDRROOT/
          dev/
             ...
          deps/
             ..
          dom/
              ...
             logcfg/
                    component.log4j.cfg

  /data/logdir/
```

```bash
# Set root logger default levels and appender
log4j.rootLogger=INFO, CONSOLE, FILE

# Console Appender
log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender

# File Appender
log4j.appender.FILE=org.apache.log4j.FileAppender
log4j.appender.FILE.File=/data/logdir/@@@WAVEFORM.NAME@@@/@@@COMPONENT.NAME@@@.@@@COMPONENT.PID@@@.log
log4j.appender.FILE.layout=org.apache.log4j.PatternLayout
log4j.appender.FILE.layout.ConversionPattern=%d{ISO8601}: %p:%c - %m [%F:%L]%n
```

When the waveform `MY_EXAMPLE` is deployed on the domain, the component is launched with the following logging configuration:

`MEGA_WORKER "....." LOGGING_CONFIG_URI sca://logcfg/component.log4j.cfg?fs= <br>IOR:010…`

### Logging with Event Channels for Components, Devices and Services

For REDHAWK resources, the underlying logging functionality has been extended to include support for publishing log messages to a specified Event Channel. To include this capability, add the `org.ossie.logging.RH_LogEventAppender` in your log4j configuration file. This appender responds to the following configuration options (all options are string values unless otherwise noted):

##### RH_LogEventAppender Configuration Options
| **Appender Option** | **Description**                                                                            |
| :------------------ | :----------------------------------------------------------------------------------------- |
| `EVENT_CHANNEL`     | Event Channel name where logging messages are published.                                    |
| `NAME_CONTEXT`      | Directory in `omniNames` to lookup Event Channel (domain name - `REDHAWK_DEV`).             |
| `PRODUCER_ID`       | Identifier of the resource producing the log message (resource name).                      |
| `PRODUCER_NAME`     | Name of the resource producing the log message.                                            |
| `PRODUCER_FQN`      | Fully qualified name of the resource (domain-name/waveform-name/resource-name). |
| `RETRIES`           | Number of times to retry connecting to the Event Channel. (Integer)                         |
| `THRESHOLD`         | log4cxx log level; `FATAL`, `WARN`, `ERROR`, `INFO`, `DEBUG`, `TRACE`.                     |


{{% notice note %}}
The `NAME_CONTEXT` option, is required for C++ resources to maintain backwards compatibility. For REDHAWK 2.0 and greater resources developed in Python or Java, the `EVENT_CHANNEL` is acquired using the `EventChannelManager` interface that is available to resources with domain awareness and ignores the `NAME_CONTEXT` option.
{{% /notice %}}

{{% notice note %}}
Eventable logging is currently not supported for the Domain Manager and Device Manager. The following messages may occur during start up of these services with configurations that use eventable logging.
```bash
log4cxx: Could not instantiate class [org.ossie.logging.RH_LogEventAppender].
log4cxx: Class not found: org.ossie.logging.RH_LogEventAppender
log4cxx: Could not instantiate appender named "pse".
```
{{% /notice %}}

In the following example, a component configured with this log4j properties file publishes log messages with a severity of `ERROR` or less to the Event Channel `ERROR_LOG_CHANNEL` in the domain, `REDHAWK_DEV`. The threshold level for the appender supersedes the rootLogger’s logging level.

```bash
log4j.rootLogger=INFO,stdout,pse

# Direct log messages to stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n

# Direct error messages to an event channel
log4j.appender.pse=org.ossie.logging.RH_LogEventAppender
log4j.appender.pse.Threshold=ERROR
log4j.appender.pse.event_channel=ERROR_LOG_CHANNEL
log4j.appender.pse.name_context=@@@DOMAIN.NAME@@@
log4j.appender.pse.producer_id=@@@COMPONENT.INSTANCE@@@
log4j.appender.pse.producer_name=@@@COMPONENT.NAME@@
log4j.appender.pse.producer_FQN=@@@DOMAIN.NAME@@@.@@@WAVEFORM.NAME@@@.@@@COMPONENT.NAME@@@
log4j.appender.pse.layout=org.apache.log4j.PatternLayout
log4j.appender.pse.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c:%L - %m%n
```
