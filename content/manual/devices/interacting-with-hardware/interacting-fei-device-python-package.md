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

where `comp` is a reference to the component being connected to an allocated tuner.

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
