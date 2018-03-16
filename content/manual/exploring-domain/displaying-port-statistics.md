---
title: "Displaying Port Statistics"
weight: 60
---

In addition to port plotting, a user may want to monitor the amount of data flowing out or into a particular port. The [**Port Monitor** view](({{< relref "manual/ide/editors-and-views/port-monitor-view.md" >}})) displays these link statistics, which are helpful when debugging and can help identify which component is slowing down or dropping information during data processing. The diagram also visually reflects the port statistics.

To display the **Port Monitor** view:

1.  Make sure that the component to be monitored is currently in the Started state.

2.  Right-click the port to monitor.

3.  Select **Monitor Port**.

    The **Port Monitor** view is displayed and contains the following information:

      - **Name**: The name of the port or port connection.
      - **Elements/sec**: The rate of CORBA elements transferred in the pushPacket data call.
      - **mbps**: Mega Bytes transferred per second.
      - **calls/sec**: Number of push calls per second to the port.
      - **Stream IDs**: List of all active stream ids.
      - **Avg. Queue Depth**: For components that queue data before processing/sending, the average queue depth measured as a percentage. If a port does not queue data this value is set to zero.
      - **Time**: The elapsed time, in seconds, since the last packet was transferred via a push packet call.

In the diagram, for uses (out) ports, the color of the connection (the arrow) to the provides port reflects the statistics. Green indicates that data is flowing. Yellow indicates it has been more than 1 second since the port pushed data, which may indicate a data flow issue. For provides (in) ports, the color of the box on the side of the component, which represents the port, reflects the statistics. A green port indicates the queue has plenty of space left. After the queue depth reaches 60 percent, the port color changes to yellow, and the port color slowly changes to red as the queue depth approaches 100 percent. Additionally, if there is a queue flush, the port remains red for 30 seconds after that queue flush.

To configure the colors displayed for the various port statistics:

1.  Select **Window > Preferences**.

2.  Select the **REDHAWK Port Statistics** preference page.

3.  Change the values.

4.  Click **OK**.
