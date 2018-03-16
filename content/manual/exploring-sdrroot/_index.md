---
title: "Exploring SDRROOT Using the REDHAWK IDE"
weight: 160
---

The REDHAWK IDE provides a visualization of the file system contained within the SDR Root in the **REDHAWK Explorer View** The `SDRROOT` is referred to within the IDE as the Target SDR. The Target SDR location may be changed from within the IDE preferences page.

To change the Target SDR:

1.  Select **Window > Preferences**
2.  Select the **REDHAWK > SDR** preference page
3.  Change the **Local SDR Location** value
4.  Click **OK**

## Browsing Installed SDR Objects

From the **REDHAWK Explorer** and the Target SDR, one can browse the installed components, devices, nodes, services and waveforms. Each of these REDHAWK Objects are placed in their own folder which may be expanded to show their content. Depending on the object type, different right-click operations and tree structure manipulation are permitted.

### Components

Mouse-hovering over a component within the **REDHAWK Explorer** shows the full path to the SPD file in the SDR root that defines it.

From the **REDHAWK Explorer View**, you can perform the following actions on components:

  - **Local Component Launch**: To launch an implementation of a component with default property values, right-click the component, select **Launch in Sandbox**, and select an implementation of the component to start within the Sandbox’s Chalkboard. Alternately, to launch an implementation of a Component with customized property values, right-click the component, select **Launch in Sandbox**, and select **Advanced**. The Launch wizard is displayed.  The Launch wizard is displayed. For more information, refer to [Launching Components in the IDE Sandbox]({{< relref "manual/sandbox/ide/_index.md#launching-components-in-the-ide-sandbox" >}}).

  - **View within REDHAWK Editor**: To open the component’s XML files in an editor, double-click the component. This opens the [REDHAWK Component Editor]({{< relref "manual/ide/editors-and-views/softpkg-editor.md" >}}) in read-only mode. You cannot edit the fields or perform any code generation from this version of the editor.

  - **Delete**: Delete a component from the SDR Root by right-clicking and selecting **Delete**. This removes all files pertaining to this component from the SDR Root including the SPD, PRF, SCD, and executable files. You can highlight multiple items and delete them at the same time.
      {{% notice warning %}}
Deleting an item from the SDR Root cannot be undone.
      {{% /notice %}}

### Devices

Mouse-hovering over a device within the **REDHAWK Explorer** shows the full path to the SPD file in the SDR root that defines it.

From the **REDHAWK Explorer View**, you can perform the following actions on device objects:

  - **Local Device Launch**: Right-click the device, select **Launch in Sandbox**, and select an implementation of the device to start within the Sandbox’s Device Manager. Expand **Sandbox** > **Device Manager** to view the running device. Alternately, to launch an implementation of a device with customized property values, right-click the device, select **Launch in Sandbox**, and select **Advanced**. The Launch wizard is displayed. For more information, refer to [Launching Devices in the IDE Sandbox]({{< relref "manual/sandbox/ide/_index.md#launching-devices-in-the-ide-sandbox">}}).

  - **View within REDHAWK Editor**: To open the device’s XML files in an editor, double-click the device. This opens the [REDHAWK Editor]({{< relref "manual/ide/editors-and-views/softpkg-editor.md" >}}) in read-only mode. You cannot edit the fields or perform any code generation from this version of the editor.

  - **Delete**: Delete a device from the SDR Root by right-clicking and selecting **Delete**. This removes all files pertaining to this device from the SDR Root including the SPD, PRF, SCD, and executable files. You can highlight multiple items and delete them at the same time.
    {{% notice warning %}}
Deleting an item from the SDR Root cannot be undone.
    {{% /notice %}}

### Nodes

Mouse-hovering over a node within the **REDHAWK Explorer** shows the full path to the DCD file in the SDR root that defines it.

From the **REDHAWK Explorer View**, you can perform the following actions on node objects:

  - **Launch Device Manager**: You may launch a Device Manager onto a running domain for this node. Right-click the node and select **Launch Device Manager** to bring up a **Launch Device Manager Dialog**. From this dialog, you may select from a list of running domains. Pressing **OK** in this dialog creates a new Console view and runs an instance of `nodeBooter` using this node’s DCD file.

  - **View within REDHAWK Editor**: To open the node’s XML files in an editor, double-click the node. This opens the [REDHAWK Node Editor]({{< relref "manual/ide/editors-and-views/node-editor.md" >}}) in read-only mode. You cannot edit the fields or perform any code generation from this version of the editor.

  - **Expand node**: Using the tree structure to expand the node, the IDE decomposes the sections of the DCD file into the referenced devices, Device Manager, Domain Manager, and Partitioning sections. By selecting these objects with the Properties View open, all the information from the XML is displayed in tabular format.

  - **Delete**: Delete a node from the SDR Root by right-clicking and selecting **Delete**. This removes only the DCD file and the folder containing it from the SDR and does not delete the referenced devices. You can highlight multiple items and delete them at the same time.

    {{% notice warning %}}
Deleting an item from the SDR Root cannot be undone.
    {{% /notice %}}

### Services

Mouse-hovering over a service within the **REDHAWK Explorer** shows the full path to the SPD file in the SDR root that defines it.

From the **REDHAWK Explorer View**, you can perform the following actions on service objects:

  - **Local service Launch**: Right-click the service, select **Launch in Sandbox**, and select an implementation of the service to start within the Sandbox’s Chalkboard. Alternately, to launch an implementation of a service with customized property values, right-click the service, select **Launch in Sandbox**, and select **Advanced**. The Launch wizard is displayed. For more information, refer to [Launching Services in the IDE Sandbox]({{< relref "manual/sandbox/ide/_index.md#launching-services-in-the-ide-sandbox">}}).

  - **View within REDHAWK Editor**: To open the SPD file within an editor, double-click the SPD file. This opens the [**REDHAWK Editor**]({{< relref "manual/ide/editors-and-views/softpkg-editor.md" >}}) in read-only mode. You cannot edit the fields or perform any code generation from this version of the editor.

  - **Delete**: Delete a service from the SDR Root by right-clicking and selecting **Delete**. This removes all files pertaining to this service from the SDR Root including any SPD, PRF, SCD, and executable files. You can highlight multiple items and delete them at the same time.

    {{% notice warning %}}
Deleting an item from the SDR Root cannot be undone.
    {{% /notice %}}

### Waveforms

Mouse-hovering over a waveform within the **REDHAWK Explorer** shows the full path to the SAD file in the SDR root that defines it.

You can perform the following actions from the **REDHAWK Explorer View** on waveform objects:

  - **Local waveform Launch**: Right-click the waveform, select **Launch in Sandbox**, and select an implementation of the waveform to start within the Sandbox’s Chalkboard. Alternately, to launch an implementation of a waveform with customized property values, right-click the waveform, select **Launch in Sandbox**, and select **Advanced**. The Launch wizard is displayed. For more information, refer to [Launching Waveforms in the IDE Sandbox]({{< relref "manual/sandbox/ide/_index.md#launching-waveforms-in-the-ide-sandbox">}}).

  - **View within REDHAWK Editor**: To open the SAD file within an editor, double-click the SAD file. This opens the [Waveform Editor]({{< relref "manual/ide/editors-and-views/waveform-editor.md" >}}) in read-only mode. You cannot edit the fields or add any additional components.

  - **Expand waveform**: Using the tree structure to expand the waveform, the IDE decomposes the sections of the SAD file into the referenced Assembly Controller, Component Files, and Partitioning sections. By selecting these objects with the Properties View open, all the information from the XML is displayed in a tabular format.

  - **Delete**: Delete a waveform from the SDR Root by right-clicking and selecting **Delete**. This removes only the SAD file and the folder containing it from the SDR and does not delete the referenced components. You can highlight multiple items and delete them at the same time.

    {{% notice warning %}}
Deleting an item from the SDR Root cannot be undone.
    {{% /notice %}}

### Browsing Installed IDL Libraries

Even though the IDL Repository is displayed within the Target SDR tree structure, the deployed IDLs and the core IDL library are not stored within the SDR Root directory on the file system. All IDLs exist within the `OSSIEHOME` directory. However, the IDL repository is shown here so that one may browse the content and view the installed IDLs.

Expanding the IDL Repository tree structure exposes the individual IDLs and a list of module folders which contain IDLs. To view the content of an IDL in a text editor, one may either double-click the IDL or right-click and select **Text Editor** or **Open With… > Text Editor**.

One may also expand an IDL. When an IDL is expanded, the IDE parses its content to decompose it into referenced methods and objects. By selecting these methods and objects with the **properties View** open one can view all the information from the IDL in a tabular format.

### Getting Details About Error Conditions

If an error condition occurs within the SDR Root that the IDE can detect, it marks the object in error with a decorator in the lower left corner. Mouse hovering over the item’s icon provides a short description of the issue; however, if more than one problem has occurred the hover text reads “Multiple Problems exist with this item”.

More detail about an error can be found within the **Properties View** of the item.  
To view the details about an error condition:

1.  With the item selected, select or open the **Properties View**.
2.  From the **Properties View**, select the **Advanced** tab
3.  Select the status row. This causes the **Details** key to appear.
4.  Press the **Details** button to bring up a detailed dialog of the current error conditions

##### Error Event Details
![Error event details dialog](../images/REDHAWK_Property_View_Error_Dialog.png)
