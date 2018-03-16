---
title: "SRI View"
weight: 110
---

The SRI View enables the user to view the [SRI data]({{< relref "manual/connections/bulkio/sri.md" >}}) from a particular port.  The following procedure explains how to open the SRI View.

1.  To open the SRI View, right-click the port of a started component and select **Display SRI** from the context menu:
    ##### Port Context Menu with Display SRI Selected
    ![The Port Context Menu with Display SRI Selected](../../images/SRIMenu.png)

    The SRI View is opened:
    ##### SRI View
    ![The SRI View](../../images/SRIView.png)

2.  The following options are available in the SRI View.

      - To clear the display of specific SRI data once an EOS has been received, click **Clear Selected SRI**.
      - To clear the display of all SRI data once the respective EOSs have been received, click **Clear All SRIs**.
      - To pause the SRI data, click **Pause Incoming SRI Data**.
      - To receive notification when new SRI data is displayed, click **Notify on receiving new Push SRI**.
      - To change the active SRI data stream between different active components, click **Change Active Stream**.
      - To generate both an `.xml` and a `.SRI` file for each stream being monitored, click **Save SRI Data to file**.

{{% notice tip %}}
If a component is deleted from the Chalkboard, the SRI View closes automatically.
{{% /notice %}}
