---
title: "Deploying Projects to the SDRROOT"
weight: 80
---

The following methods may be used to deploy a REDHAWK project into the target `SDRROOT`.

  - Drag-and-drop from the **Project Explorer**:

    1.  In the **Project Explorer View**, drag the top-level REDHAWK project onto the **Target SDR** in the REDHAWK Explorer View.
    2.  In the REDHAWK Explorer View, expand **Target SDR**, then expand the appropriate sub-area: **Components**, **Devices**, **Nodes**, **Services**, or **Waveforms**, to display the deployed project.

  - From the **Project** Menu:

    1.  In the **Project Explorer View**, select the top-level REDHAWK project.
    2.  From the **Project** menu, select **Export to SDR**.

  - From the Context Menu:

    1.  In the **Project Explorer View**, right-click the top-level REDHAWK project.
    2.  Select **Export to SDR**.

  - From the Overview page of the SPD Editor:

    1.  Open the project’s `spd.xml` file.
    2.  From the Overview tab of the SoftPkg Editor, in the **Exporting** section, click **Export Wizard**.
    3.  In the **Export Wizard**, from the **Available REDHAWK Projects** section, check the desired projects to deploy.
    4.  The **Destination** directory is prepopulated with the **Target SDR**.
    5.  Click **Finish** to deploy selected projects into the **Target SDR**.

  - From the **File** Menu:

    1.  From the **File** menu, select **Export**.
    2.  From the **Export Wizard**, expand **REDHAWK Development** and select **Deployable REDHAWK Project**.
    3.  Click **Next**.
    4.  From the **Available REDHAWK Projects** section, check the desired projects to deploy.
    5.  The **Destination** directory is prepopulated with the **Target SDR**.
    6.  Click **Finish** to deploy the selected projects into the **Target SDR**.
