---
title: "Using an FEI Device in a Domain"
weight: 30
---

After you have created the FEI device, it must be deployed before you can begin to use it. The following procedure explains how to deploy the FEI device.

1.  [Create a new node]({{< relref "manual/nodes/running-a-node#creating-a-new-node" >}}) or use an existing node and add your FEI device to the node.

2.  Launch the node (also known as a Device Manager) onto a running domain.

    The node is displayed in the REDHAWK Explorer View.

3.  From the REDHAWK Explorer View, expand the node and navigate to your device.

    The available tuners are displayed with a Tuning fork icon within a folder under your device:

    ##### Available FEI Tuners
    ![Available FEI Tuners](../../images/FEItuningfork.png)

#### Allocating a FrontEnd Tuner

The usage status of a FrontEnd Tuner device is IDLE until a successful allocation has been made. Allocation is the process where a specific tuner is requested for use, and initial setup of the tuner is performed. When at least one channel has been allocated, but there is one channel not allocated, the device is ACTIVE. If all channels have been allocated, the device is BUSY.

The following procedure explains how to allocate a FrontEnd tuner using the **Tuner Allocation** dialog.

1.  Right-click the **FrontEnd Tuners** folder and select **Allocate**:

    ##### Allocating an FEI Tuner
    ![Allocating an FEI Tuner](../../images/FEITunerAllocate.png)

    The **Allocate Tuner** wizard is displayed. In this dialog, you specify the desired tuner properties such as frequency, bandwidth, and so forth.

    ##### Allocate Tuner Wizard
    ![Allocate Tuner Wizard](../../images/TunerAllocationPage.png)

2.  In the **Allocation** drop-down box, verify **Control New Tuner** is selected.

3.  Optionally, in the **New Allocation ID** field, modify the text if needed.

4.  In **Tuner Type**, select the appropriate tuner type. You have the following options:

      - RX_DIGITIZER
      - CHANNELIZER
      - DDC
      - RX
      - RX_DIGITIZER_CHANNELIZER
      - TX

5.  In **Center Frequency (MHz)**, specify the center frequency.
    {{% notice note %}}
Bandwidth and sample rate must be specified during allocation. For an allocation to be successful, the tuner must be able to provide a value that is greater than or equal to the requested value without exceeding the appropriate tolerance value specified. Requesting a bandwidth or sample rate of zero (0.0) indicates to the tuner that any value is acceptable and that the tolerance values can be ignored. Requesting 0 typically results in the lowest value the tuner is capable of providing while still satisfying the remainder of the allocation request. If the **Any Value** checkbox is selected, a value of 0 is requested.
    {{% /notice %}}

6.  In **Bandwidth (MHz)**, specify the bandwidth, or if a specific bandwidth is not required, select the **Any Value** checkbox.

7.  In **Sample Rate (Msps)**, specify the sample rate, or if a specific sample rate is not required, select the **Any Value** checkbox.

8.  Optionally, in **Bandwidth Tolerance (%)**, enter the [bandwidth tolerance]({{< relref "manual/appendices/fei.md#optional-status-elements">}}).

9.  Optionally, in **Sample Rate Tolerance (%)**, enter the [sample rate tolerance]({{< relref "manual/appendices/fei.md#optional-status-elements">}}).

10. Optionally, in [**RF Flow ID**]({{< relref "manual/appendices/fei.md#required-status-elements">}}), enter the tag for the analog feed for which you are looking.

11. Click Finish.

    The tuner is allocated and is displayed under the **FrontEnd Tuners** folder with the truncated Allocation ID and an active tuning fork icon. If you left-click or hover over the allocated tuner, the full Allocation ID is displayed. A successful allocation tunes the hardware to the requested frequency and establishes a BulkIO data stream containing the content. In the case of a multi-out BulkIO port, the data stream will only be pushed over a connection with a Connection ID that is identical to the Allocation ID associated with the data stream. In cases where the BulkIO port is not a multi-out port, all data streams are pushed over all connections, regardless of Connection id.

    ##### Allocated Tuner
    ![Allocated Tuner](../../images/FEITunerAllocated.png)

#### Deallocating a Tuner

If a tuner was previously allocated, it can be deallocated. To deallocate a tuner, right-click the allocated FrontEnd tuner and select **Deallocate**. The tuner is deallocated and displays an inactive tuning fork icon along with a status of Unallocated.

#### Attaching a Listener to a Tuned Receiver

Sometimes, it is necessary for multiple different applications to share a single FEI source. To support this concept, FEI includes both allocation owners and listeners. The allocations that have been discussed so far are owners; they maintain complete control over the allocated hardware. Listeners, on the other hand, allow for an arbitrary number of applications to receive the same data that the allocation owner receives. However, listeners have no control over the source. Listener allocations can be made against a particular allocation (through the allocation id) or by parameters (i.e.: center frequency).

To allocate a listener, right-click the allocated FrontEnd tuner and select **Add Listener**. The **Listener Allocation** dialog is displayed:

##### Listener Allocation Dialog
![The Listener Allocation dialog](../../images/ListenerAllocationDiag.png)

It is possible to specify an allocation ID for the listener, but that is strictly optional (note that the listener allocation id is needed as the connection id when consuming the listener data). Click Finish to exit the wizard and allocate the listener. When the process is complete, the listener is associate with the receiver, and is displayed under the appropriate FrontEnd tuner.

#### Metadata from the Tuner Device

An FEI device provides metadata along with its data stream. This metadata is attached to the SRI’s keywords passed along BulkIO. A total of 5 SRI elements have been defined, and they are described below:


##### FEI Data Passed as SRI
| **Identifier**         | **Value**                                                                        | **Type** |
| :--------------------- | :------------------------------------------------------------------------------- | :------- |
| `COL_RF`               | The center frequency (in Hz) for the receiver that was used to receive the data. | double   |
| `CHAN_RF`              | The center frequency (in Hz) for the tuned receiver.                             | double   |
| `FRONTEND::RF_FLOW_ID` | The identifier for the stream (usually the antenna), if available.               | string   |
| `FRONTEND::BANDWIDTH`  | The bandwidth (in Hz) for the tuned signal.                                      | double   |
| `FRONTEND::DEVICE_ID`  | The identifier for the device acquiring the data.                                | string   |


#### Deallocating a Listener

If a listener was previously allocated, you can deallocate it. To deallocate a listener, right-click the allocated listener and select **Deallocate**.

#### Controlling a Tuned Receiver

FEI devices can be controlled using a FrontEnd Tuner port. The port provides an interface to modify parameters such as center frequency, bandwidth, gain, and sample rate.

This tuner interface provides the ability to set parameters like center frequency and bandwith for the receiver. Any FEI compliant device may not output any data until the transients from any receiver setting change have settled to steady state. In short, all data output from an FEI compliant device must correspond to unambiguous receiver settings.

There is a custom view under each tuned receiver that sends commands over the control port. This custom view is mapped to the properties view. Note that this is not a device property, but instead it is the use of the property view as the artifact for interacting with the port.

The following procedure explains how to control an allocated tuner.

1.  Select the allocated tuner.

2.  Select the properties tab if available. Otherwise, from the top IDE Menu, select **Window > Show View > Properties**.
    ##### Properties View of an Allocated Tuner
    ![Properties View of an allocated tuner](../../images/TunerProps.png)

3.  Select the entry, or entries, to change from this view.

4.  Press **Enter** to have the change take affect or **Esc** to cancel.

Even though it is the property view, the Tuner port is exercised to effect the requested changes. If successful, the view updates and displays the new value.

#### Plotting a Tuned Receiver

The process described in only applies to a single-channel system. In multi-channel devices, a single port is used to send out all the data, so additional structures are used to identify which channel to plot.

1.  To plot an FEI device, right-click the allocated FrontEnd tuner and select **Plot Port Data**, or any other **Plot Port** option:

    ##### Plotting an Allocated Tuner
    ![Plotting an Allocated Tuner](../../images/TunerPlotting.png)

2.  If the FrontEnd tuner has multiple BulkIO ports, the **Ambiguous Data Port** dialog is displayed. Select the port to plot.

    For more information about interacting with the plots, refer to [REDHAWK Plot View]({{< relref "manual/ide/editors-and-views/redhawk-plot-view.md" >}}).

#### Plotting a Tuned Receiver with Multiple Channels

In multi-channel devices, all data is pushed out of a single port. To disambiguate the traffic, additional structures are used. When a device or component contains both a BulkIO output port and the well-defined property `connectionTable`, different data streams can be directed out of different connections on the port. When the multi-out selection is checked on the output selection of the FEI wizard, the `connectionTable` property is automatically added to the FEI device.

Declare the association between a connection ID, a stream ID, and a port name:

```c++
// create an association between an allocation, a stream ID,  and a port
//  the request data structure contains the allocation id
//  "my_data" is some arbitrary stream id
//  my_port is whatever output port the data is pushed out of
std::string stream_id = "my_data";
this->matchAllocationIdToStreamId(request.allocation_id, stream_id, this->my_port->getName());

// at this point, also push SRI that contains information relevant to this connection (i.e.: bandwidth)
BULKIO::StreamSRI SRI = bulkio::SRI::create(stream_id);
redhawk::PropertyMap& keywords = redhawk::PropertyMap::cast(SRI.keywords);
keywords["FRONTEND::BANDWIDTH"] = request.bandwidth;
keywords["FRONTEND::DEVICE_ID"] = this->_identifier;
this->my_port->pushSRI(SRI);
```

Send data with the stream ID associated with a particular allocation:

```c++
// push the data out using the arbitrary stream ID defined in the allocation id/stream id/port association
this->my_port->pushPacket(data, bulkio::time::utils::now(), false, "my_data");
```

While this example is in C++, this functionality is also supported in Python and Java. Unfortunately, the `allocation_csv element` of the `frontend_tuner_status` structure pre-dates the availability of sequences as elements in structures (added in REDHAWK 2.0), so it is a comma-separated value whose first value is the owner allocation.

{{% notice note %}}
`connectionTable` is a readonly property, so software running outside the scope of the device can inspect but not change these associations.
{{% /notice %}}

The state of the `connectionTable` is managed by the FEI base classes. The device developer must set up the state of `connectionTable` when a new tuner is added. However, when a listener is added to a tuner that is already on the `connectionTable`, the listener entry is managed automatically.

When plotting an allocated tuner on a multi-channel device, the IDE automatically creates a listener allocation so that the correct stream is routed to the plot.
