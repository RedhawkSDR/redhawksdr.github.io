---
title: "Bit Data"
weight: 35
---

In REDHAWK 2.2.0 and above, BulkIO includes a packed bit data format, `BULKIO::dataBit`.
The transfer of packed bit data between processing stages is standardized, including the ability to transfer a non-byte aligned number of bits.

### General

REDHAWK manages bits as arrays of bytes, with each byte containing up to 8 consecutive bits.
Bit indices start with the most significant bit: bit index 0 is the most significant bit of the first byte, bit index 1 is the second most significant bit, and so on.

### C++

In C++, there are two classes, `redhawk::shared_bitbuffer` and `redhawk::bitbuffer` that provide high-level access to bit data.
These are analagous to `redhawk::shared_buffer<T>` and `redhawk::buffer<T>`, respectively, but for bit data.

BulkIO packed bit streams are designed to work with `redhawk::shared_bitbuffer`.

#### Read-Only

The `redhawk::shared_bitbuffer` class provides read-only access to packed bit data stored in a backing byte array.

Individual bits may be accessed via indexing:

```c++
int bit = buf[0];
```

A bit is returned as an integer value that is always `0` or `1`.

Integers up to 64 bits in size may be extracted from a given bit offset with the `getint()` method.
The following example extracts a 24-bit integer value at bit 36:

```c++
int value = buf.getint(36, 24);
```

The returned value is an unsigned 64-bit integer with the extracted value in the least significant bits.

#### Read/Write

The `redhawk::bitbuffer` class adds methods that provide write access to the `redhawk::shared_bitbuffer` class.

Individual bits may be set via indexing:

```c++
buf[0] = 1;
```

Any non-zero value sets the bit, while a zero clears the bit.

{{% notice note %}}
In C++, there is no primitive type that represents a single bit; index assignment is implemented with a private proxy class.
Taking the address of an indexed value is a compiler error.
{{% /notice %}}

Integer values up to 64 bits may be set at a given bit offset with the `setint()` method.
The following example sets a 24-bit integer value at bit 36:

```c++
buf.setint(36, 0xABCDEF, 24);
```

The least significant N bits of the value are stored.

#### Creating

To allocate a new `bitbuffer` with enough space to hold 256 bits:

```c++
redhawk::bitbuffer data(256);
```

The bit values in the new `bitbuffer` are not initialized.

To create a writable copy of an existing `shared_bitbuffer`:

```c++
redhawk::bitbuffer data = shared.copy();
```

To parse string literal, use the static class method `from_string()`:

```c++
redhawk::bitbuffer data = redhawk::bitbuffer::from_string("0101101010101");
```

The `from_string()` method parses the input string and returns a new `bitbuffer` that owns the memory.

To create a `bitbuffer` from a integer literal of up to 64 bits, use the static class method `from_int()`.
The following example creates a 36-bit `bitbuffer` from a hexadecimal literal:

```c++
redhawk::bitbuffer data = redhawk::bitbuffer::from_int(0x123456789, 36);
```

Only the least significant N bits are taken from the literal; any bits above the least significant N bits are discarded.

#### Sharing

Algorithms that create new bit data or transform existing bit data should allocate a new `bitbuffer` for each iteration.
Once a `bitbuffer` has been written to an output stream, it must not be modified.
Modifications to the contents of a `bitbuffer` are visible to all instances that share the same data.
However, algorithms that require history can preserve an inexpensive read-only reference to the outgoing data:

```c++
redhawk::shared_bitbuffer history = data;
```

#### Bit Operations

The `shared_bitbuffer` class includes several features useful for bit-level processing:

 - `distance(other)` returns the Hamming distance between two bitbuffers.
 - `find(pattern, maxDistance)` searches for a bit pattern within a maximum Hamming distance.
 - `popcount()` returns the population count (number of 1 bits).
 - `takeskip(M,N)` performs a take/skip, iteratively copying M bits and skipping N bits until the end of the data.

### Python

In Python, the `bitbuffer` class provides high-level access to bit data.
BulkIO packed bit streams are designed to work with `bitbuffer`.

To use `bitbuffer` in your Python code, import it from the `redhawk.bitbuffer` package:

```py
from redhawk.bitbuffer import bitbuffer
```

`bitbuffer` behaves as a standard Python container.
It supports indexing, slicing and concatenation.

```py
header[0] = 1
header[15] = header[14]
result = header[:16] + data[16:256]
```

To construct an uninitialized `bitbuffer` of a desired size:

```py
data = bitbuffer(bits=255)
```

The contents of the new `bitbuffer` are undefined.

To parse a string literal as a `bitbuffer`:

```py
data = bitbuffer('1011010110100101001')
```

Any other non-string sequence or iterable may be used to create a `bitbuffer`, as long as each item can be converted to an `int`:

```py
# Packs a list of integer values into a bitbuffer
data = bitbuffer([1, 0, 1, 1, 0, 1, 1, 1])
# Creates a 96-bit bitbuffer with alterating 0s and 1s
data2 = bitbuffer(x & 1 for x in xrange(96))
```

#### Bit Operations

The `bitbuffer` class includes several features useful for bit-level processing:

 - `distance(other)` returns the Hamming distance between two bitbuffers.
 - `find(pattern, maxDistance)` searches for a bit pattern within a maximum Hamming distance.
 - `popcount()` returns the population count (number of 1 bits).
 - `takeskip(M,N)` performs a take/skip, iteratively copying M bits and skipping N bits until the end of the data.

### Java

Java does not include a high level bit API.
Input and output ports use the raw CORBA `BULKIO.BitSequence` class for bit data.

### Sandbox

The REDHAWK sandbox has built-in support for working with packed bit data.
General sandbox features are discussed in more depth in [Sandbox]({{< relref "manual/sandbox/_index.md" >}}).

#### Sending Bit Data

In the Python sandbox, StreamSource supports writing packed bit data.
Data may be written using a `bitbuffer` instance, or by using string literals:

```py
source = sb.StreamSource()
comp = sb.launch('my_bit_component')
sb.start()

source.write('10110101110')
```

{{% notice note %}}
The legacy DataSource helper does not support packed bit data.
{{% /notice %}}

#### Receiving Bit Data

In the Python sandbox, StreamSink supports reading packed bit data:

```py
comp = sb.launch('my_bit_generator')
sink = sb.StreamSink()
comp.connect(sink)

data = sink.read()
```

If the read is successful, `data.data` will be a `bitbuffer` instance containing all of the data available for a particular stream.

{{% notice note %}}
The legacy DataSink helper does not support packed bit data.
{{% /notice %}}

#### Plotting Bit Data

Within REDHAWK IDE, packed bit ports can be monitored and plotted just like any other BulkIO type.
By default, plotting a packed bit port automatically brings up a raster plot.

![Packed Bit Raster](../images/PackedBitPlot.png)

General IDE plot use is described in [REDHAWK Plot View]({{< relref "manual/ide/editors-and-views/redhawk-plot-view.md" >}}).

The Python sandbox plots also support displaying packed bit data.
In particular, RasterPlot has several attributes that can be modified in real time to make it easy to visualize bit data:

 - `frameSize` changes the number of bits displayed per line
 - `frameOffset` adjusts the start of the line
