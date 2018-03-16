---
title: "Custom IDL Interfaces"
weight: 100
---

REDHAWK provides FEI and standard CF interfaces (like CF::Resource) to control entities and promote interoperability. There are some use cases where you may find the need to use custom IDL to control entities. For these use cases, you can create custom IDL projects in the [IDE]({{< relref "manual/ide/_index.md" >}}).

Adding ports from either the FEI interface or a custom IDL interface to a component or device allows that entity to control other entities through CORBA. Because of the generic nature of these ports, it is not possible to create a language mapping like BulkIO, so interaction is through the standard CORBA API, a full description of which is outside the scope of this manual. However, the REDHAWK code generators will generate ports that simplify the interaction with the port. The following sections explain uses (output) ports because they are the most likely to be generated, for example, to control FEI devices.

### Connectivity Feedback

In all three supported languages, an FEI, standard CF, or custom IDL port will have all methods and attributes mapped to the port, and the port will then delegate the call to the remote connection. In REDHAWK, it is possible for a port to have no connections, one connection, or many connections. Each of these conditions can create issues for someone using a port for communications; for example, if a control request is sent out and there are no connections, then the user should be informed that the request did not go anywhere.

At the same time, not all methods are the same. Some methods push data in only one direction, some methods have a return value, and some methods have arguments that are pointers to be filled with information (out or inout arguments). When a port method is called and it is not possible for the port to make a call or for the call to be unambiguous (for example, if two connections exist and the function contains a return value), then a PortCallError is raised in the user code. The table below describes the method signature criteria met and its corresponding behavior.

##### Control method and error conditions based on connectivity
| **method signature**     | **no connection** | **one connection** | **many connections** |
| :----------------------- | :---------------- | :----------------- | :------------------- |
| No return value, only in | Error             | ok                 | ok                   |
| Has return value         | Error             | ok                 | Error                |
| has inout                | Error             | ok                 | Error                |
| has out                  | Error             | ok                 | Error                |


If a method has any kind of return value as part of its non-exception API (manifested as a non-void return value, or an out or inout argument), then an exception is raised if there is more than one connection out of the port. Furthermore, if a call is attempted with no connection in effect, an error is raised.

### Connection Selection

While the generated port class triggers an error when the desired connection is ambiguous, it also contains an API to allow the developer to select which connection should be exercised. Each method has an optional argument, `connection_id`, that allows the caller to disambiguate which connection should be exercised. The default value behavior will use the last connection made. If the `connection_id` specified does not exist, a `PortCallError` will be raised.

{{% notice note %}}
In the following sections, the same pattern used to disambiguate the connections is provided in all three supported languages.
{{% /notice %}}

The following code example uses the default behavior when calling the read method of the CF::File interface.

```c++
 CF::OctetSequence_var _data = new CF::OctetSequence();
 CF::OctetSequence_out data(_data);
 this->file_out->read(data, 10); // read 10 characters from the last connection made to the port
```

The following code example disambiguates the read call to a specific connection, `connection1`.

```c++
 CF::OctetSequence_var _data = new CF::OctetSequence();
 CF::OctetSequence_out data(_data);
 this->file_out->read(data, 10, "connection1"); // read 10 characters from the connection called "connection1"
```

To view the connections that are available, use the following code:

```c++
 std::vector<std::string> _connection_ids = this->file_out->getConnectionIds();
```

### Method Mapping

Method name mapping follows the pattern described in [Connection Selection](#connection-selection); namely that methods have the same name as described in the IDL with an additional argument (to be optionally exercised) that can specify which connection should be used. Attributes are mapped as functions to the CORBA objects. REDHAWK provides additional APIs to disambiguate these calls for multiple connections.

#### Reading Attributes

Reading attributes is performed by invoking the name of the attribute as a function. For example, if the port, `my_port`, contains the string attribute `greeting`, the value of `greeting` can be retrieved as follows:

```c++
 std::string _greeting = this->my_port->greeting();
```

To retrieve the value from a specific connection, the `_get_` prefix is needed:

```c++
 std::string _greeting = this->my_port->_get_greeting("some_connection_name");
```

#### Writing Attributes

Writing attributes in C++ and Java involves invoking the function with the appropriate argument:

```c++
 this->my_port->greeting("hello"); // write "hello" to the attribute "greeting"
 this->my_port->greeting("hello", "some_connection_name"); // write "hello" to the attribute "greeting" over connection "some_connection_name"
```

Python requires the prefix `_set_` because it cannot be overloaded:

```py
 self.my_port._set_greeting("hello") # write "hello" to the attribute "greeting"
 self.my_port._set_greeting("hello", "some_connection_name") # write "hello" to the attribute "greeting" over connection "some_connection_name"
```
