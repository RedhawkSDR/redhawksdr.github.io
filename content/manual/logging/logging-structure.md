---
title: "Logging Structure"
weight: 10
---

Java logging is based on log4j, and C++ logging is based on log4cxx, a C++ mapping of the log4j. Python logging is based on log4py, a Python mapping between the Python loggers and log4j configuration files developed for REDHAWK. These three logging technologies were selected because they all follow the same structure and they can all be configured using the same configuration files.

Log4j loggers provide the ability to associate a particular logger, identifier by its string name, with one or more appenders (e.g.: standard out, a file, a socket connection) and a log level (e.g.: TRACE, DEBUG, INFO, WARN, ERROR, FATAL).

Log4j loggers are identified by their string names, and the name selected for the logger determines the lineage for the logger. The root logger is the empty string, and all loggers in the hierarchy are derived from this root logger; this means that whatever settings the root logger has, all child loggers inherit the logger settings (unless the child logger was given its own configuration, in which case the settings inheritance tree is broken).

Logger names follow the Java naming pattern, where the logger "abc.def.ghi" inherits from the logger "abc.def", which inherits from the logger "abc", which in turn inherits from the root logger.

All REDHAWK-generated resources (such as components and devices) contain a logger called "_baseLog" that has the same name as the resource (e.g.: my_component_1) and is removed by one level from the root logger. Under this parent logger, three categories or namespaces are available by default: "system", "ports", and "user". While these three logger namespaces are created automatically, additional ones can be created programmatically (more on this later). The "system" namespace is the parent for the REDHAWK system services (like property management) that log framework-level activity. The "ports" namespace is a logger that is parent to all component ports, where the specific port logger has the same name as the port (e.g.: my_component_1.ports.my_float_input). Finally, the "user" namespace was created as a root for all component- or device-specific logging; note that additional namespaces can be created under the "user" namespace. This hierarchy is shown in the following image:

##### Logger Hierarchy
![Logger Hierarchy](../images/LoggerHierarchy.png)

Loggers inherit their parent's settings. In other words, whatever settings are given to the root logger are passed to all child loggers. Any change to the parent logger's settings (like the log level) are automatically passed down to all its child loggers. This relationship is broken if a logger's settings are specifically changed. Once a named logger's settings are changed, if a parent changes its settings, those changes are not propagated to the child with the altered settings.

In log4j, log messages are processed through appenders. An appender is a mechanism that provides a mapping between a message and some deliver mechanism for the message. For example, there can be file appenders and standard out appenders that write messages to a file and to a console, respectively. Appenders are managed through the log configuration file. REDHAWK systems can use a wide variety of general-purpose appenders (like file and standard out) as well as some custom appenders (like event channel appenders).

There are four main aspects to managing loggers in REDHAWK: configuring a logger's start settings, creating log messages, changing a logger during runtime, and viewing logging events. The following four sections describe these aspects. The logging section ends with a description of how to view logging events from the IDE.
