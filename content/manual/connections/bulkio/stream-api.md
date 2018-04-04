---
title: "Stream API"
weight: 30
---

The BulkIO stream API provides a high-level interface to sending and receiving data via BulkIO ports. Each stream is tied to a port, and encapsulates both the SRI and the data associated with it.

Streams are automatically managed by the port that creates them. User code does not own the stream itself; instead, user instances are opaque stream handles. This allows them to be passed around by value or safely stored in other data structures.

All BulkIO port types, except for SDDS and VITA49, support the stream API.

Most stream methods are not thread-safe; it is assumed that each stream will be written to or read from by a single thread. However, it is safe to use multiple streams simultaneously.

{{% notice note %}}
The BulkIO stream API is not supported in Java as of REDHAWK {{% rhver %}}
{{% /notice %}}

### Data Types

The data type of a typical read or write operation is as follows:

| **Stream Type**  | **C++**                          | **Python**          |
| :--------------- | :------------------------------- | :------------------ |
| char             | `redhawk::shared_buffer<T>`      | `list(T)`           |
| octet            | `redhawk::shared_buffer<T>`      | `list(T)`           |
| short            | `redhawk::shared_buffer<T>`      | `list(T)`           |
| ushort           | `redhawk::shared_buffer<T>`      | `list(T)`           |
| long             | `redhawk::shared_buffer<T>`      | `list(T)`           |
| ulong            | `redhawk::shared_buffer<T>`      | `list(T)`           |
| longlong         | `redhawk::shared_buffer<T>`      | `list(T)`           |
| ulonglong        | `redhawk::shared_buffer<T>`      | `list(T)`           |
| float            | `redhawk::shared_buffer<T>`      | `list(T)`           |
| double           | `redhawk::shared_buffer<T>`      | `list(T)`           |
| bit              | `redhawk::shared_bitbuffer`      | `bitbuffer`         |
| XML              | `std::string`                    | `str`               |
| file             | `std::string`                    | `str`               |

Where "T" is one of the element types below:

| **Stream Type**  | **Complex** | **C++**                          | **Python**   |
| :--------------- | :---------- | :------------------------------- | :----------- |
| char             | no          | `int8_t`                         | `int`        |
| char             | yes         | `std::complex<int8_t>`           | `complex`    |
| octet            | no          | `CORBA::Octet`                   | `int`        |
| octet            | yes         | `std::complex<CORBA::Octet>`     | `complex`    |
| short            | no          | `CORBA::Short`                   | `int`        |
| short            | yes         | `std::complex<CORBA::Short>`     | `complex`    |
| ushort           | no          | `CORBA::UShort`                  | `int`        |
| ushort           | yes         | `std::complex<CORBA::UShort>`    | `complex`    |
| long             | no          | `CORBA::Long`                    | `int`/`long` |
| long             | yes         | `std::complex<CORBA::Long>`      | `complex`    |
| ulong            | no          | `CORBA::ULong`                   | `int`/`long` |
| ulong            | yes         | `std::complex<CORBA::ULong>`     | `complex`    |
| longlong         | no          | `CORBA::LongLong`                | `long`       |
| longlong         | yes         | `std::complex<CORBA::LongLong>`  | `complex`    |
| ulonglong        | no          | `CORBA::ULongLong`               | `long`       |
| ulonglong        | yes         | `std::complex<CORBA::ULongLong>` | `complex`    |
| float            | no          | `float`                          | `float`      |
| float            | yes         | `std::complex<float>`            | `complex`    |
| double           | no          | `double`                         | `float`      |
| double           | yes         | `std::complex<double>`           | `complex`    |

Python automatically promotes an `int` to a `long` if its value exceeds the maximum `int` value for the platform.

### Output Streams

Output streams ensure that data is always associated with an active SRI and simplify management of stream lifetime.

#### Output Stream Types (C++)

Each numeric output port type has a corresponding stream type (e.g., `bulkio::OutFloatStream` for `bulkio::OutFloatPort`) that provides the interface for sending stream data.

#### Creating

An output stream is created via the port's `createStream()` method.
The following examples create a new stream with ID "my_stream_id" and default SRI.

C++:

```c++
bulkio::OutFloatStream stream = dataFloat_out->createStream("my_stream_id");
```

Python:

```py
stream = self.port_dataFloat_out.createStream('my_stream_id')
```

The `createStream()` method also accepts an SRI.

The output port keeps track of the streams that have been created, up until they are closed.
The `getStream()` method provides a way to look up a stream by ID, eliminating the need to keep local references to output streams.

#### Modifying Stream Metadata

Output streams provide convenience methods (C++) or attributes (Python) for modifying common SRI fields.
The following examples configure an output stream for complex data at a sample rate of 250Ksps, centered at 91.1MHz.

C++:

```c++
stream.complex(true);
stream.xdelta(1.0 / 250000.0);
stream.setKeyword("CHAN_RF", 91.1e6);
```

Python:

```py
stream.complex = True
stream.xdelta = 1.0 / 250000.0
stream.setKeyword('CHAN_RF', 91.1e6)
```

The SRI can be updated in its entirety with the `sri()` method in C++:

```c++
stream.sri(newSri);
```

In Python, assign a new SRI to the `sri` attribute:

```py
stream.sri = newSri
```

All SRI fields are updated from the new SRI, with the exception of the stream ID.
The stream ID is immutable and cannot be changed after creation.

Updates to the SRI are stored and pushed before the next packet goes out.

{{% notice note %}}
It is not necessary to manually call `pushSRI()` when using streams.
{{% /notice %}}

#### Writing

Data is sent with the `write()` method.
With the exception of XML streams, which do not support time stamps, `write()` must be given a `PrecisionUTCTime` representing the date of birth of the first element in the data being written.

C++:
```c++
redhawk::buffer<float> data(1024);
// ...fill data...
stream.write(data, bulkio::time::utils::now());
```

Python:
```py
data = range(1024)
stream.write(data, bulkio.timestamp.now())
```

#### Writing Complex Data

In C++, if the stream is configured for complex data, give `write()` a complex data type:

```c++
redhawk::buffer< std::complex<float> > data(1024);
// ...fill data...
stream.write(buffer, size, bulkio::time::utils::now());
```

When writing scalar data to a complex stream, make sure that the size is a multiple of 2.

In Python, data written to a complex stream is assumed to be a list of complex values.
If an element in the list is not a Python `complex`, its imaginary component is treated as 0.

#### Write Buffering

Most BulkIO output stream types, with the exception of XML and File, support a buffered write mode.
When buffering is enabled, the stream can queue up small writes into a single push.

By default, write buffering is disabled.
To enable buffering, set the desired size with the `setBufferSize()` method.
On writes, the stream will copy data into its internal buffer until the desired size is reached, then push out the buffered data as a single push.

Buffered data will be pushed to the port immediately if the SRI changes or on close.
A push may also be triggered explicitly by calling the `flush()` method.

To disable buffering once it has been enabled, set the buffer size to 0.

{{% notice info %}}
Write buffering does not preserve every time stamp.
If precise time information is required, disable write buffering.
{{% /notice %}}

#### Closing

When an output stream is complete, close the stream.
The `close()` method sends an end-of-stream packet and dissociates the stream from the output port.

### Input Streams

An input stream encapsulates SRI and all received packets associated with that stream ID. Buffering and overlap are built in, removing the need for client code to implement these features.

Input streams are created automatically by the input port when an SRI is received with a new stream ID. Only one stream per port can exist with a given stream ID; in the event that an input stream has an unacknowledged end-of-stream waiting, a new SRI with the same stream ID will be queued until the end-of-stream has been reached.

Methods that accept or return a number of samples take the input stream’s complex mode into account. For example, requesting 1024 samples from a complex stream returns 1024 complex pairs, which is equivalent to 2048 scalar values.

There are two ways of retrieving an input stream: [Stream Polling](#stream-polling) or [Stream Callback](#stream-callback).

#### Input Stream Types (C++)

Each input port type has a corresponding stream type (e.g., `bulkio::InFloatStream` for `bulkio::InFloatPort`).

#### Stream Polling

For the basic case, the `getCurrentStream()` method returns the next input stream that is ready for reading. Similar to `getPacket()`, the next packet in the queue is consulted; however, if any stream has buffered data from a prior read (such as when using fixed-sized reads), it is given priority. Developers accustomed to using `getPacket()` will find that `getCurrentStream()` provides a familiar flow, while extending the available functionality.

The optional timeout argument is identical to the timeout argument for `getPacket`.
If the timeout is omitted, `getCurrentStream()` defaults to blocking mode.
The following examples wait indefinitely for a stream to become ready.

C++:

```c++
bulkio::InFloatStream stream = dataFloat_in->getCurrentStream();
if (!stream) {
    return NOOP;
}
```

Python:

```py
stream = self.port_dataFloat_in.getCurrentStream()
if not stream:
    return NOOP
```

If there are no streams ready, such as when the timeout expires or the component receives a `stop()` call, the returned stream will be invalid.
Streams should be checked for validity before performing any operations.
In C++, the boolean not (`!`) operator returns true if the stream is invalid.
In Python, `getCurrentStream()` returns `None`.

#### Advanced Polling (C++ Only)

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

The callback has no return value and takes a single argument, the input stream.

C++:

```c++
void MyComponent_i::newStreamCreated(bulkio::InFloatStream newStream) {
    // Store the stream in the component, set up supporting data structures, etc.
}
```

Python:

```py
def newStreamCreated(self, newStream):
    # Store the stream in the component, set up supporting data structures, etc.
```

The callback should be registered with the port in the REDHAWK constructor.

C++:

```c++
void MyComponent_i::constructor()
{
    // Other setup code...
    dataFloat_in->addStreamListener(this, &MyComponent_i::newStreamCreated);
}
```

Python:

```py
def constructor(self):
    # Other setup code...
    self.port_dataFloat_in.addStreamListener(self.newStreamCreated)
```

### Data Blocks

In BulkIO input stream-based code, data is retrieved from data streams as blocks.
Data blocks can be retrieved on a per-packet basis, or they can be retrieved as a definite-sized buffer, with or without overlap.

#### Data Block Types (C++)

Each input stream data type has a corresponding data block type, such as `bulkio::FloatDataBlock`.

#### Reading Data Blocks
The `read()` family of methods synchronously fetch data from a stream.
The basic `read()` returns the next packet worth of data for the stream, blocking if necessary.

{{% notice note %}}
For common use cases, reading a packet at a time is the most efficent approach because it avoids the need to copy data.
{{% /notice %}}

C++:

```c++
bulkio::FloatDataBlock block = stream.read();
```

Python:

```py
block = stream.read()
```

#### Sized Reads

You may request a set amount of data by supplying the number of samples.
The following examples read 1K samples.

C++:

```c++
bulkio::FloatDataBlock block = stream.read(1024);
```

Python:

```py
block = stream.read(1024)
```

The `read()` call blocks until at least the requested number of samples is available.
Packets are combined or split as necessary to return the correct amount of data.
The returned block may contain less than the requested number of samples if the stream has ended or the component is stopped.

{{% notice note %}}
XML and File streams do not support sized reads.
{{% /notice %}}

#### Overlapping Reads

For algorithms that require data to overlap between iterations, you may also pass the number of samples to consume.
The following examples read 1K samples with 50% overlap.

C++:

```c++
bulkio::FloatDataBlock block = dataFloat_in->read(1024, 512);
```

Python:

```py
block = self.port_dataFloat_in.read(1024, 512)
```

The input stream’s read pointer is advanced up to the consume length. The next call to `read()` will return data starting at that point.

{{% notice note %}}
XML and File streams do not support overlapping reads.
{{% /notice %}}

#### Read Failures

If the an end-of-stream flag is received, or the component is interrupted, `read()` may return early.
In the overlap case, if end-of-stream is reached before receiving the requested number of samples, all remaining data is consumed and no further reads are possible.

When a `read()` returns an invalid block, it is important to check for an end-of-stream.

C++:

```c++
if (!block) {
    if (stream.eos()) {
        // Stream has ended, no more data will be received
    }
}
```

Python:
```py
if not block:
    if stream.eos():
        # Stream has ended, no more data will be received
```

#### Skipping

Data can be dropped with the `skip()` method.
In the following examples, 256 samples are dropped.

C++:

```c++
size_t skipped = stream.skip(256);
```

Python:

```py
skipped = stream.skip(256)
```

The returned value is the number of samples that were dropped. If the streams ends or the component is stopped, this may be less than the requested value.

#### Non-Blocking Read

The `read()` family of methods is always blocking.
For non-blocking reads, use `tryread()`.

C++:

```c++
bulkio::FloatDataBlock block = stream.tryread(2048);
```

Python:

```py
block = stream.tryread(2048)
```

`tryread()` will only return a valid block of data if the entire request can be satisfied or if no more data will be received.
In the case that the stream has ended or that component has been stopped, all remaining queued data in the stream will be returned.

### Interacting with Data Blocks

Data blocks contain the input data, as well as the SRI that describes the data.
A variety of functions are contained in data blocks that help the developer manage and interact with the data block’s contents.

#### Memory Management (C++)

The memory is managed automatically inside the object to minimize copies, so there is no need to explicitly delete data blocks.

#### Validity Checking

If a read fails, such as when the component receives a `stop()` call, it will return an invalid block.
The block should be checked for validity using a boolean test before attempting to access the block's data or metadata.

In C++, data block objects support boolean tests.
Commonly, a block is tested for validity with the boolean not operator (`!`):

```c++
bulkio::FloatDataBlock block = stream.read();
// Check if a valid block was returned
if (!block) {
    return NOOP;
}
// Operate on the block
```

In Python, failed read operations return `None`.
A valid data block will always evaluate to `True` in a boolean context:

```py
block = stream.read()
# Check if a valid block was returned
if not block:
    return NOOP
# Operate on the block
```

#### Metadata

Data blocks provide methods (C++) or attributes (Python) to access common metadata:

- `sri` returns the SRI at the time the data was received
- `xdelta` returns the SRI xdelta

Occasionally, the input stream’s state may change between data blocks.
To handle this situation, the data block provides methods (C++) or attributes (Python) to check these conditions:

  - `inputQueueFlushed`
  - `sriChanged`
  - `sriChangeFlags` returns the changed SRI fields as a bit field

C++ example:

```c++
if (block.inputQueueFlushed()) {
    // Handle data discontinuity...
}
if (block.sriChangeFlags() & bulkio::XDELTA) {
    // Update processing...
}
```

Python example:

```py
if block.inputQueueFlushed:
    # Handle data discontinuity...
if block.sriChangeFlags & bulkio.sri.XDELTA:
    # Update processing
```

#### Data

The `buffer` method (C++) or attribute (Python) provides access the data stored in a data block with minimal overhead.
For sample-based data block types (such as `float`), refer to [Real Data](#real-data) or [Complex Data](#complex-data).

#### Real Data

For sample-based data block types, `buffer` accesses the data as real samples.

In C++:

```c++
float blocksum = 0.0;
const redhawk::shared_buffer<float>& data = block.buffer();
for (size_t index = 0; index < block.size(); ++index) {
    blocksum += data[index];
}
```

In Python, `buffer` is a list of numeric values:

```py
blocksum = 0.0
for val in block.buffer:
    blocksum += val
```

#### Complex Data

If the input stream is complex, the returned data buffer should be treated as complex data.
Data block objects provide convenience methods (C++) or attributes (Python) to make it easy to work with complex data:

  - `complex` returns true if the data is complex (i.e., SRI mode is 1)
  - `cxbuffer` returns the sample data reinterpreted as complex numbers

C++:

```c++
if (block.complex()) {
    std::complex<float> blocksum = 0.0;
    redhawk::shared_buffer<std::complex<float> > data = block.cxbuffer();
    for (size_t index = 0; index < data.size(); ++index) {
        blocksum += data[index];
    }
}
```

Python:

```py
if block.complex:
    blocksum = 0j
    for val in block.cxbuffer:
        blocksum += val
```

#### Time Stamps

Because a single data block may span multiple input packets, it can contain more than one time stamp.
Data blocks returned from an input stream, with the exception of XML streams, are guaranteed to have at least one time stamp.

The first time stamp may be accessed with the `getStartTime()` method.
This returns the `PrecisionUTCTime` of the first sample.

If the data block contains more than one time stamp, the full list of time stamps may be accessed with the `getTimestamps()` method.

C++:

```c++
std::list<bulkio::SampleTimestamp> timestamps = block.getTimestamps();
```

Python:

```py
timestamps = block.getTimestamps()
```

The `SampleTimestamp` class contains three fields:

  - `time` - a `PrecisionUTCTime` time stamp
  - `offset` - the sample number at which this time stamp applies
  - `synthetic` - is true if the time stamp was calculated based on a prior data block

When the start of a data block does not match up exactly with a packet, the input stream will use the last known time stamp, the SRI `xdelta` and the number of samples to calculate a time stamp. Only the first time stamp in a data block can be synthetic.

### Ignoring Streams

Some components may prefer to only handle one stream at a time.
Unwanted input streams can be disabled by calling the `disable()` method.

All data for the stream will be discarded until end-of-stream is reached, preventing queue backups due to unhandled data.
