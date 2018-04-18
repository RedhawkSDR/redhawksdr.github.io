---
title: "Logging Structure"
weight: 10
---

Java logging is based on log4j, and C++ logging is based on log4cxx, a C++ mapping of the log4j. Python logging is based on log4py, a Python mapping between the Python loggers and log4j configuration files developed for REDHAWK. These three logging technologies were selected because they all follow the same structure, and they can all be configured using the same configuration files.

Log4j loggers provide the ability to associate a particular logger identifier by its string name with one or more appenders (for example: standard out, a file, a socket connection) and a log level (for example: TRACE, DEBUG, INFO, WARN, ERROR, FATAL).

Log4j loggers are identified by their string names, and the name selected for the logger determines the lineage for the logger. The root logger is the empty string, and all loggers in the hierarchy are derived from this root logger; thus, all child loggers inherit the root logger settings (unless the child logger is given its own configuration, in which case the settings inheritance tree is broken).

Logger names follow the Java naming pattern, where the logger `abc.def.ghi` inherits from the logger `abc.def`, which inherits from the logger `abc`, which in turn inherits from the root logger.

All REDHAWK-generated resources (such as components and devices) contain a logger, `_baseLog`, that has the same name as the resource (for example, `my_component_1`) and is removed by one level from the root logger. Under this parent logger, three categories or namespaces are available by default: "system", "ports", and "user". These three logger namespaces are created automatically, and additional namespaces may be created programmatically under the "user" namespace). The "system" namespace is the parent for the REDHAWK system services (such as property management) that log framework-level activity. The "ports" namespace is a logger that is parent to all component ports, where the specific port logger has the same name as the port (for example, `my_component_1.ports.my_float_input`). Finally, the "user" namespace is a root for all component- or device-specific logging. This hierarchy is shown in the following image:

##### Logger Hierarchy
![Logger Hierarchy](../images/LoggerHierarchy.png)

Loggers inherit their parent's settings. In other words, root logger settings are passed to all child loggers. Any change to the parent logger's settings (such as the log level) are automatically passed down to all its child loggers. This relationship is broken if a logger's settings are specifically changed. After a named logger's settings are changed, if the parent logger settings change, the changes are not propagated to the child with the altered settings.

In log4j, log messages are processed through appenders. An appender is a mechanism that provides a mapping between a message and some delivery mechanism for the message. For example, there may be file appenders and standard out appenders that write messages to a file and to a console, respectively. Appenders are managed through the log configuration file. REDHAWK systems can use a wide variety of general-purpose appenders (like file and standard out) as well as some custom appenders (like event channel appenders).

There are four main aspects to managing loggers in REDHAWK:

 - [configuring a logger's start settings]({{< relref "manual/logging/configuring-logging-capabilities.md" >}})
 - [creating log messages]({{< relref "manual/logging/logging-within-a-resource.md" >}})
 - [adjusting a logger during runtime]({{< relref "manual/logging/adjusting-logging-at-runtime.md" >}}), and
 - [viewing logging events]({{< relref "manual/logging/viewing-logging-events.md" >}}).
