---
title: "Viewing Logging Events"
weight: 60
---

A live view of events logged by components or devices can be displayed in the IDE. The component or device provides logging events to an event channel, and the IDE displays them as it receives them. To view the log:

1.  Right-click the running component or device and select **Logging > Tail Log**. The Specify logging details dialog is displayed:

    ##### Edit Log Config
    ![Edit Log Config Editor](../images/loggingdetails.png)

2.  Select the logging level.

3.  If desired, specify the logger to which the IDE should attach. Leave the field blank to attach to the root logger.

4.  Click OK.

    A new Console view displays logging events as they are received.

5.  To no longer view events, click the Stop icon on the Console toolbar.
