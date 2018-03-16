---
title: "Interacting with an FEI Device with the Python Package"
weight: 20
---

The Python package contains helpers that simplify the allocation/deallocation of FEI tuners. For example, to allocate a tuner to receive, centered at 1 MHz with a bandwidth of 1kHz, a 10% tolerance in the requested values, and a sample rate of 2 kHz, the following functionality can be used:

```py
>>> import frontend
>>> allocation = frontend.createTunerAllocation(tuner_type="RX", allocation_id="someid",center_frequency=1e6, bandwidth=1e3,bandwidth_tolerance=0.1, sample_rate=2e3,sample_rate_tolerance=0.1)
>>> retval = dev.allocateCapacity(allocation)
```

where `dev` is a reference to the device object and `retval` is `True` if the allocation succeeded.

To connect to an allocated tuner, use the allocation ID as the connection id:

```py
>>> dev.connect(comp, connectionId="someid")
```

where `comp` is a reference to the component being connected to an allocated tuner. If the connection id corresponding to an allocation is not provided and the possible sources of data are not ambiguous, the Python package will create a connection that delivers the allocated stream.

When a device contains a single allocated tuner and there is a single output bulkio port, it is possible to unambiguously determine what tuner to connect to and over which Port to connect. In such instances, the Python package creates a listener allocation against the allocated tuner and establishes a connection between the FEI device and the destination using the listener allocation id as the connection id. The created listener enables the owner of the allocation to establish a connection to the data source with the given allocation id. When the connection is removed, the Python package automatically deallocates the listener allocation. In such instances, the following call can be used to connect:

```py
>>> dev.connect(comp)
```

To deallocate the tuner, use the following call:

```py
>>> dev.deallocateCapacity(allocation)
```

To allocate a listener to a specific allocated tuner, use the following call:

```py
>>> listen_alloc = frontend.createTunerListenerAllocation(allocation_id, "some ID listener")
>>> retval = dev.allocateCapacity(listen_alloc)
```

To allocate a listener to any tuner with a particular set of values, use the following call:

```py
>>> allocation=frontend.createTunerGenericListenerAllocation(tuner_type="RX", allocation_id="someidanotherlistener",center_frequency=1e6, bandwidth=1e3,bandwidth_tolerance=0.1,sample_rate=2e3, sample_rate_tolerance=0.1)
>>> retval = dev.allocateCapacity(allocation)
```

Deallocation of listeners follows the same pattern as the deallocation of tuners.
