---
title: "The Connection Manager"
weight: 80
---

The Connection Manager provides a single point for creation and inspection of connections between domain objects. Connections between domain objects can be defined on the Connection Manager irrespective of whether or not these objects are currently deployed. If one or more of the endpoints defined in a connection are not available, the connection is pending. Connections are resolved anytime domain objects enter or leave the domain

Endpoints can be applications, devices, services, Event Channels, or CORBA object references.
