---
title: "Using a REDHAWK Shared Library Project"
weight: 20
---

To add a shared library dependency to a component or device:

{{% notice note %}}
The shared library must be installed to `SDRROOT` before you can add it to a component or device.
{{% /notice %}}

1.  Open the SPD file for the component or device.
    #####  Component SPD File
    ![Component SPD File](../images/componentSPD.png)

2.  Select the **Implementations** tab.
    ##### Component Implementations Tab
    ![Component Implementations Tab](../images/componentimplementations.png)

3.  On the left side of the editor, select the appropriate implementation.

4.  On the right side of the editor, under **Dependencies**, next to the **Dependency** box, click **Add...**

    The Edit Dependency dialog is displayed.
    ##### Edit Dependency Dialog
    ![Edit Dependency Dialog](../images/editdependency.png)

5.  In the dialog, change the **Kind** to **Shared Library (SoftPkg) Reference**.

6.  In the **Type** box, select **other**.

7.  Select a shared library from the list of shared libraries installed in the `SDRROOT`.
    ##### Shared Library Dependency Dialog
    ![Shared Library Dependency Dialog](../images/sharedlibdependimplementationstab.png)

8.  Click **Finish**.

    The shared library dependency is displayed under the All Implementations section of the Implementations tab.
    ##### Shared Library Project Dependency
    ![The Shared Library Project Dependency](../images/sharedlibdependallimpl.png)

9.  To update your code to use the dependency, click the **Generate All Implementations** icon.
