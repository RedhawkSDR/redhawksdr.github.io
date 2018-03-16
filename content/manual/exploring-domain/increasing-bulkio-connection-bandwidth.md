---
title: "Increasing the Bandwidth of BulkIO Connections"
weight: 50
---

In the presence of high data rates, plots of BulkIO ports may not be able to keep up with the data stream. To increase the bandwidth of BulkIO CORBA connections, it is possible to connect using native omniORB libraries. This ability is currently disabled by default. The following procedure explains how to enable this ability from within the IDE:

1.  Select **Window  >  Preferences**.

    The **Preferences** dialog is displayed:

    ##### Preferences Dialog
    ![Preferences Dialog](../images/bulkioprefs.png)

2.  Expand **REDHAWK**.

3.  Select **BulkIO**.

4.  Set **Port Factory** to `omnijni`.

5.  Click **OK**.

{{% notice note %}}
This option should only be enabled if the domain matches the version of the IDE, and your application requires the increased performance of `omnijni`.
{{% /notice %}}
