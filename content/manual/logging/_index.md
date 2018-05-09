---
title: "Logging"
weight: 140
---

REDHAWK provides a logging capability for use by all resources (components, devices, services, and core services like the Domain Manager). By default, the REDHAWK Framework and its support libraries have been developed to take advantage of an underlying language-specific logging implementation: log4cxx for C++, log4j for Java, and log4py for Python. All three libraries follow the basic premise of the log4j capabilities and configuration. This chapter explains how to configure and use the logging capability. For a description of log4cxx, refer to the log4cxx website (https://logging.apache.org/log4cxx/latest_stable/index.html), and for a description of log4j, refer to the log4j website (http://logging.apache.org/log4j/1.2/). The Python library log4py is a REDHAWK-developed library that maps the log4j log configuration to the Python logging package and is not distributed outside the context of REDHAWK.

{{% children depth="999" %}}
