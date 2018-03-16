---
title: "The Connection Manager"
weight: 80
---

The Connection Manager provides a single point for creation and inspection of connections between domain objects. Connections between domain objects can be defined on the Connection Manager irrespective of whether or not these objects are currently deployed. If one or more of the endpoints defined in a connection are not available, the connection is pending. Connections are resolved anytime domain objects enter or leave the domain.

Endpoints can be applications, devices, services, Event Channels, or CORBA object references.
### Inspecting Connections using the IDE

To inspect the current connections using the IDE:

1.  In the REDHAWK Explorer view, right-click a Domain Manager and select **Connection Manager**.

    The Connection Manager view is displayed. If a green check mark is displayed in the Connected column, this indicates that the connection has been made; otherwise, it is pending.
    ##### Connection Manager
    ![Connection Manager](../images/ConnectionManager.png)

2.  To view the connection properties, select the connection in the Connection Manager view.

    The Properties view displays the connection properties.
    ##### Connection Manager Properties
    ![Connection Manager Properties](../images/ConnMgrPropertiesView.png)
3.  To view the source (provides/out port) of a connection, from the Connection Manager view, right-click the connection and select **Find Source**.

    The source is highlighted in the REDHAWK Explorer View.
4.  To view the target (uses/in port) of a connection, from the Connection Manager view, right-click the connection and select **Find Target**.

    The target is highlighted in the REDHAWK Explorer View.

### Disconnecting Connections using the IDE

To disconnect a current connection using the IDE:

1.  From the Connection Manager view, right-click the connection and select **Disconnect**.

    The connection is disconnected.
