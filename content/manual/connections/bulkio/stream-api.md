---
title: "Stream API"
weight: 30
---

The BulkIO stream API provides a high-level interface to sending and receiving data via BulkIO ports. Each stream is tied to a port, and encapsulates both the SRI and the data associated with it.

Streams are automatically managed by the port that creates them. User code does not own the stream itself; instead, user instances are opaque stream handles. This allows them to be passed around by value or safely stored in other data structures.

All numeric BulkIO port types support the stream API. UDP multicast (SDDS and VITA49) and string-based (string, file and XML) interfaces do not.

Most stream methods are not thread-safe; it is assumed that each stream will be written to or read from by a single thread. However, it is safe to use multiple streams simultaneously.

{{% notice note %}}
The BulkIO stream API is C++-only in REDHAWK {{% rhver %}}
{{% /notice %}}

### Output Streams

Each numeric output port type has a corresponding stream type (e.g., `bulkio::OutFloatStream` for `bulkio::OutFloatPort`) that provides the interface for sending stream data. Using streams ensures that data is always associated with an active SRI and simplifies management of stream lifetime.

Create an output stream via the port:

```c++
// Create a new stream with ID "my_stream_id" and default SRI
bulkio::OutFloatStream stream = dataFloat_out->createStream("my_stream_id");
```

Output streams provide convenience methods for modifying common SRI fields:

```c++
// Stream is complex data at a sample rate of 250Ksps, centered at 91.1MHz
stream.complex(true);
stream.xdelta(1.0 / 250000.0);
stream.setKeyword("CHAN_RF", 91.1e6);
```

The SRI can be updated in its entirety with the `sri()` method. Updates to the SRI are stored and pushed before the next packet goes out.

{{% notice note %}}
It is not necessary to manually call `pushSRI()` when using streams.
{{% /notice %}}

Data is sent with the `write()` method:

```c++
// Assume data is of type std::vector<float>
stream.write(data, bulkio::time::utils::now());

// Assume buffer is a pointer to std::complex<float>
stream.write(buffer, size, bulkio::time::utils::now());
```

Complex data should be written with a vector of or pointer to `std::complex` values to ensure that an integral number of samples is sent. When writing scalar data to a complex stream, make sure that the size is a multiple of 2.

Closing an output stream sends and end-of-stream packet and dissociates the stream from the output port.

```c++
stream.close();
```

### Input Streams

An input stream encapsulates SRI and all received packets associated with that stream ID. Buffering and overlap are built in, removing the need for client code to implement these features. Each numeric input port type has a corresponding stream type (e.g., `bulkio::InFloatStream` for `bulkio::InFloatPort`).

Input streams are created automatically by the input port when an SRI is received with a new stream ID. Only one stream per port can exist with a given stream ID; in the event that an input stream has an unacknowledged end-of-stream waiting, a new SRI with the same stream ID will be queued until the end-of-stream has been reached.

Methods that accept or return a number of samples take the input stream’s complex mode into account. For example, requesting 1024 samples from a complex stream returns 1024 complex pairs, which is equivalent to 2048 scalar values.

There are two ways of retrieving an input stream: [Stream Polling](#stream-polling) or [Stream Callback](#stream-callback).

#### Stream Polling

For the basic case, the `getCurrentStream()` method returns the next input stream that is ready for reading. Similar to `getPacket()`, the next packet in the queue is consulted; however, if any stream has buffered data from a prior read (such as when using fixed-sized reads), it is given priority. Developers accustomed to using `getPacket()` will find that `getCurrentStream()` provides a familiar flow, while extending the available functionality.

The optional timeout argument is identical to the timeout argument for `getPacket`. If the timeout is omitted, `getCurrentStream()` defaults to blocking mode:

```c++
// Wait indefinitely for a stream to become ready
bulkio::InFloatStream stream = dataFloat_in->getCurrentStream();
if (!stream) {
  return NOOP;
}
```

If there are no streams ready, such as when the timeout expires or the component receives a `stop()` call, the returned stream will be invalid. The boolean not (\!) operator returns true if the stream is invalid; no other operations are safe to call on an invalid stream.

#### Advanced Polling

For more advanced use, the input port’s `pollStreams()` family of methods allow you to wait for one or more streams to be ready to read. Like `getCurrentStream()`, `pollStreams` takes a timeout argument to set the maximum wait time.

The ready streams are returned as a list:

```c++
// Wait up to 1/8th second for a stream to be ready
bulkio::InFloatPort::StreamList streams = dataFloat_in->pollStreams(0.125);
if (streams.empty()) {
  return NOOP;
}
for (bulkio::InFloatPort::StreamList::iterator stream = streams.begin();
     stream != streams.end();
     ++stream) {
  // Handle each stream; note that stream is an iterator
  LOG_TRACE(Component_i, "Reading stream " << stream->streamID());
}
```

If no streams are ready, the returned list is empty. `pollStreams()` returns as soon as one stream is ready.

If a minimum number of samples is required, it may be provided in the `pollStreams()` call:

```c++
bulkio::InFloatPort::StreamList streams = dataFloat_in->pollStreams(1024, bulkio::Const::BLOCKING);
```

#### Stream Callback

As opposed to polling, callback functions may be registered with the input port to be notified when a new stream has been created. Using a callback supports more sophisticated patterns, such as handling each stream in a separate thread or disabling unwanted streams.

The callback has no return value and takes a single argument, the input stream type:

```c++
void MyComponent_i::newStreamCreated(bulkio::InFloatStream newStream) {
  // Store the stream in the component, set up supporting data structures, etc.
}
```

Register the callback with the port in the REDHAWK constructor:

```c++
void MyComponent_i::constructor()
{
  ...
  dataFloat_in->addStreamListener(this, &MyComponent_i::newStreamCreated);
  ...
}
```

### Data Blocks

In BulkIO input stream-based code, data is retrieved from data streams as blocks. Each input stream data type has a corresponding data block type, such as `bulkio::FloatDataBlock`. Data blocks can be retrieved on a per-packet basis, or they can be retrieved as a definite-sized buffer, with or without overlap.

#### Reading Data Blocks
The `read()` family of methods synchronously fetch data from a stream. The basic `read()` returns the next packet worth of data for the stream, blocking if necessary:

```c++
bulkio::FloatDataBlock block = stream.read();
```

You may request a set amount of data by supplying the number of samples:

```c++
bulkio::FloatDataBlock block = stream.read(1024);
```

The `read()` call blocks until at least the requested number of samples is available. Packets are combined or split as necessary to return the correct amount of data. The returned block may contain less than the requested number of samples if the stream has ended or the component is stopped.

For algorithms that require data to overlap between iterations, you may also pass the number of samples to consume:

```c++
// Read with 1K samples with 50% overlap
bulkio::FloatDataBlock block = dataFloat_in->read(1024, 512);
```

The input stream’s read pointer is advanced up to the consume length. The next call to `read()` will return data starting at that point.

If the an end-of-stream flag is received, or the component is interrupted, `read()` may return early. In the overlap case, if end-of-stream is reached before receiving the requested number of samples, all remaining data is consumed and no further reads are possible.

```c++
if (!block) {
  if (stream.eos()) {
    // Stream has ended, no more data will be received
  }
}
```

Data can be dropped with `skip()`:

```c++
// Drop the next 256 samples
size_t skipped = stream.skip(256);
```

The returned value is the number of samples that were dropped. If the streams ends or the component is stopped, this may be less than the requested value.

#### Non-Blocking Read

The `read()` family of methods is always blocking. For non-blocking reads, use `tryread()`:

```c++
bulkio::FloatDataBlock block = stream.tryread(2048);
```

`tryread()` will only return a valid block of data if the entire request can be satisfied, or if no more data will be received. In the case that the stream has ended or that component has been stopped, all remaining queued data in the stream will be returned.

### Interacting with Data Blocks

Data blocks contain the data as an array of sample data, as well as the SRI that describes the data. The memory is managed automatically inside the object to minimize copies, so there is no need to explicitly delete data blocks. A variety of functions are contained in data blocks that help the developer manage and interact with the data block’s contents.

#### Verifying Correctness

The boolean not (\!) operator returns true if a block is invalid. This occurs when the stream is unable to read the requested data, such as when the component receives a `stop()` call.

```c++
bulkio::FloatDataBlock block = stream.read();
// Check if a valid block was returned
if (!block) {
  return;
}
// Operate on the block
```

#### Metadata

- `sri()` returns the SRI at the time the data was received
- `xdelta()` returns the SRI xdelta

Occasionally, the input stream’s state may change between data blocks. To handle this situation, the data block provides methods to check these conditions:

  - `inputQueueFlushed()`
  - `sriChanged()`
  - `sriChangeFlags()` returns the changed SRI fields as a bit field

```c++
if (block.inputQueueFlushed()) {
  // Handle data discontinuity...
}
if (block.sriChangeFlags() & bulkio::XDELTA) {
  // Update processing...
}
```

#### Scalar Data

- `data()` returns a pointer to the first element
- `size()` returns the number of values

```c++
std::float* data = block.data();
for (size_t index = 0; index < block.size(); ++index) {
  data[index] = std::abs(data[index]);
}
```

#### Complex Data

If the input stream is complex, the returned data buffer should be treated as complex data. Data block objects provide convenience methods to make it easy to work with complex data:

  - `complex()` returns true if the data is complex (i.e., SRI mode is 1)
  - `cxdata()` returns a pointer to the first element as a complex value
  - `cxsize()` returns the number of complex values

```c++
if (block.complex()) {
  // Take the complex conjugate of each sample
  std::complex<float>* data = block.cxdata();
  for (size_t index = 0; index < block.cxsize(); ++index) {
    data[index] = std::conj(data[index]);
  }
}
```

#### Time Stamps

Because a single data block may span multiple input packets, it can contain more than one time stamp. Data blocks returned from an input stream are guaranteed to have at least one time stamp.

Time stamps are accessed with the `getTimestamps()` method:

```c++
std::list<bulkio::SampleTimestamp> timestamps = block.getTimestamps();
BULKIO::PrecisionUTCTime time = timestamps.front().time;
```

The `bulkio::SampleTimestamp` class contains three fields:

  - `time` - a `BULKIO::PrecisionUTCTime` time stamp
  - `offset` - the sample number at which this time stamp applies
  - `synthetic` - is true if the time stamp was calculated based on a prior data block

When the start of a data block does not match up exactly with a packet, the input stream will use the last known time stamp, the SRI `xdelta` and the number of samples to calculate a time stamp. Only the first time stamp in a data block can be synthetic.

### Ignoring Streams

Some components may prefer to only handle one stream at a time. Unwanted input streams can be disabled, typically in a new stream callback:

```c++
stream.disable();
```

All data for the stream will be discarded until end-of-stream is reached, preventing queue backups due to unhandled data.
