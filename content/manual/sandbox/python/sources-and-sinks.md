---
title: "Built-in Sources and Sinks"
weight: 50
---

REDHAWK includes a variety of helpers that allow a developer to inject data into or extract data from components deployed through the Sandbox. The following section describes each of these helpers and how they are used.

#### Data Sources

The `DataSource` module provides a mechanism for producing BulkIO data to be sent to a provides (input) port. Once instantiated, a Python vector of data can be pushed to the `DataSource`.

An example instantiation and use of the Data Source module can be seen below:

```py
>>> input_source = sb.DataSource()
>>> input_source.connect(my_comp)
>>> my_data = range(10000)
>>> sb.start()
>>> input_source.push(my_data)
```

When the `DataSource` sends data, it attempts to match the data type to the type of the associated provides (input) port. Alternatively, the data type may be set explicitly in the `DataSource` constructor. Note that the default type for the `DataSource` is `short`, which implies that values over 32768 may induce an exception.

The default setting for number of bytes per `pushPacket()` is 512000 bytes. Data is broken up into chunks of this size before being sent via the port’s `pushPacket()` method. To change this default size, set the `bytesPerPush` argument in the `DataSource` constructor.

A specific module is provided for reading data from a file. This module, `FileSource`, is used and instantiated much like the `DataSource` module. The most significant difference between the two modules is the presence of a file name in the `FileSource` constructor.

```py
>>> input_file = sb.FileSource("~/short_file.tmp", dataFormat="short")
```

SRI keywords may be generated and sent with data from the `DataSource` module.

An example generate/send can be seen below:

```py
>>> kw = sb.SRIKeyword("SOME_RF",155000000.0,"double")
>>> kw2 = sb.SRIKeyword("EFFECTIVE_BITS_PER_SAMPLE",16,"long")
>>> keywords = [kw, kw2]
>>> input_data = sb.DataSource()
>>> data = range(1000)
>>> input_data.connect(my_comp)
>>> sb.start()
>>> input_data.push(data,SRIKeywords=keywords)
```

The `SRIKeyword()` constructor takes in the following arguments:

  - name - A string representing the name of the keyword being set.
  - value - The value to be set.
  - format - A string indicating the data type of the value. Valid data types are short, ushort, float, double, long, ulong, longlong, ulonglong, char, octet, string, boolean.

Messages can be sent to components using the `MessageSource` module. messages sent to the `sendMessage()` method can be one of four types: struct, dictionary, CORBA Any, and data types that can be mapped to a CORBA Any.

The default message ID is `sb_struct`:

```py
>>> mySource = sb.MessageSource()
>>> myComponet = sb.launch("test_message_rx_cpp")
>>> mySource.connect(myComponent)
>>> sb.start()
>>> msg = {"val1":"test string", "val2":123}
>>> mySource.sendMessage(msg)
```

#### Data Sinks

The Sandbox provides a `DataSink` module, which simply reads data from a uses (output) port. Below is an example instantiation and use of the `DataSink` module. In this example, data sent from `myComponent`’s uses (output) port is written to the `received_data` variable.

SRI associated with the packet can be viewed using the `SRI()` method:

```py
>>> output_data = sb.DataSink()
>>> myComponent.connect(output_data)
>>> received_data = output_data.getData()
>>> received_SRI  = output_data.SRI()
```

To block until a certain amount of data is received, specify the data length as an argument to the `getData()` method:

```py
>>> received_data = output_data.getData(100)
```

The `eos()` method indicates whether or not an eos was received:

```py
>>> output_data.eos()
False
```

The `eos_block` argument may be used to configure the `getData()` method to block until an eos is received:

```py
>>> received_data = output_data.getData(eos_block=True)
```

Similar to the `DataSource`’s `FileSource` counterpart, the `DataSink` has an associated `FileSink` module for writing data to a file:

```py
>>> output_file = sb.FileSink("~/some_file.tmp")
>>> another_comp.connect(output_file)
```

Messages may be displayed using the `MessageSink` module. Data sent to a running `MessageSink` is printed in the Python interpreter.

Below is an example of `MessageSink` usage:

```py
>>> myComponent = sb.launch("test_message_send_cpp")
>>> myMessageSink = sb.MessageSink()
>>> myComponent.connect(myMessageSink)
>>> sb.start()
```

#### Plotting Data Example

Note that for plotting data, the REDHAWK IDE must be installed and the path to the `eclipse` directory of the installed IDE must be specified to the Sandbox.

This can be done through the `IDELocation()` function:

```py
>>> sb.IDELocation("/path/to/ide/eclipse")
```

This can also be done by setting the `RH_IDE` environment variable prior to starting the Python session.

```py
>>> input_source = sb.DataSource()
>>> my_data = range(10000)
>>> my_plot = sb.Plot()
>>> input_source.connect(my_plot)
>>> sb.start()
>>> input_source.push(my_data)
```

#### Continuous Data Flow Example

```py
>>> input_source.push(my_data,loop=True)
```

  - To display a falling raster of the data being pushed, click **Show Raster** in the upper right of the plot window
  - To stop looping data, enter the following command:

    ```py
    >>> input_source.stop()
    ```

#### File Data Plotting Example

```py
>>> input_file = sb.FileSource("~/short_file.tmp", dataFormat="short")
>>> my_plot = sb.Plot()
>>> input_file.connect(my_plot)
>>> input_file.start()
```

#### Passing a Struct to sendMessage Example

```py
from ossie.utils import sb
from ossie.properties import simple_property

class MessageProp(object):
    val1 = simple_property(id_="val1",
                           type_="string",
                           defvalue="trm")
    val2 = simple_property(id_="val2",
                           type_="double",
                           defvalue=1211)
    def __init__(self):
          """Construct an initialized instance of this struct
             definition"""
          for attrname, classattr in type(self).__dict__.items():
              if type(classattr) == simple_property:
                  classattr.initialize(self)

      def __str__(self):
          """Return a string representation of this structure"""
          d = {}
          d["val1"] = self.val1
          d["val2"] = self.val2
          return str(d)

      def getId(self):
          return "message_prop"

      def isStruct(self):
          return True

      def getMembers(self):
          return [("val1",self.val1),("val2",self.val2)]

testmessage = MessageProp()
testmessage.val1 = "test string"
testmessage.val2 = 123
a = sb.MessageSource()
b = sb.launch("test_message_rx_cpp")
a.connect(b)
sb.start()
a.sendMessage(testmessage)
```
