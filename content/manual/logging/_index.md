---
title: "Logging"
weight: 140
---

REDHAWK provides a logging capability for use by all resources (components, devices, services, and core services like the Domain Manager). By default, the REDHAWK Framework and its support libraries have been developed to take advantage of an underlying language-specific logging implementation: log4cxx for C++, log4j for Java, and log4py for Python. All three libraries follow the basic premise of the log4j capabilities. A root logger definition is the parent of all loggers and configuration of the loggers is controlled through a log4j-style properties file. Each REDHAWK resource creates a named logger using the resource name as a child of the root logger with additional loggers created as child loggers for different parts of the resource. This chapter explains how to configure and use the logging capability. For a detailed description of log4j and its capabilities, refer to the log4j website (<http://logging.apache.org/log4j/1.2/>).

{{% children depth="999" %}}
