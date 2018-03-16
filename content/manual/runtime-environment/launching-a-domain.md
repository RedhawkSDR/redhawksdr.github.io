---
title: "Launching a Domain"
weight: 10
---

If REDHAWK was installed from RPMs, a Domain Manager and Device Manager are ready for immediate use on the `localhost`. To launch a default domain in the IDE, follow these steps:

1.  In the REDHAWK Explorer View (by default, on the right-side of the window) right-click the **Target SDR** element and select **Launch Domain…**:
##### Launching a Domain
![Launching a Domain](../images/REDHAWK_Launch_domain.png)

2.  In the **Device Manager** section of the **Launch Domain Manager** window, select **DevMgr_*hostname***:
##### Device Manager Selection
![Device Manager Selection](../images/SelectDevMgr.png)

3.  Click **OK**.

This launches both a Domain Manager and a Device Manager that manages a single GPP device. The output from both the Domain Manager and Device Manager is displayed in the **Console** view. If this view is not visible, select **Window > Show View > Console**. To stop these processes, click the **Terminate** icon (red square). To toggle between consoles, click the **Display Selected Console** icon (computer monitor):

##### Terminate Icon
![Terminate Icon](../images/stop_doms.png)

##### Display Selected Console Icon
![Display Selected Console Icon](../images/switch_console.png)

The **REDHAWK_DEV** domain connection is displayed in the REDHAWK Explorer View. Its state is **CONNECTED** and there are no errors. A Domain Manager process and a Device Manager process now exist on the host.

##### Domain Connections Show in IDE
![Domain connections shown in IDE](../images/running_domain.png)

### Shutting Down the Domain

Normally, the Domain Manager and Device Manager remain running indefinitely; these programs are designed to remain running for extended periods of time as different parts of the overall domain (e.g., Device Managers, applications, and files on `$SDRROOT`) come and go. However, for the purpose of the following procedure, the process for shutting down a running domain is explained. To cleanly shutdown, it is best to disconnect the domain and stop the processes that have been started.

1.  In the REDHAWK Explorer View, right-click the **REDHAWK_DEV** domain and select **Disconnect**
##### Disconnect From Domain
![Disconnect From Domain](../images/domain_disconnect.png)
2.  In the **Console** view, select the Device Manager Console from the **Display Selected Console** icon.
3.  To stop the Device Manager, click the **Terminate** icon.
4.  In the **Console** view, select the Domain Manager Console from the **Display Selected Console** icon.
5.  To stop the Domain Manager, click the **Terminate** icon.
6.  Select **File > Exit**.

The Domain Manager and Device Manager processes no longer exist on the host. The domain entry remains in the REDHAWK Explorer View with a **DISCONNECTED** state indicating that the domain is no longer visible. This decoupling of the running domain from the environment enables the REDHAWK Explorer to interact with an arbitrary number of domains on a network where each domain’s life cycle is outside the control of the IDE.
