---
title: "Message Producer"
weight: 10
---
To create a message producer, create a new component or edit an existing component.

1.  Add a Struct property
      - Choose an ID that uniquely identifies the message produced.
      - Select the kind for the property as only **message**
      - Add any arbitrary number of members to the struct
2.  Add **uses** port of **IDL Interface** `ExtendedEvent> MessageEvent`
3.  Regenerate the component

For the purposes of the following examples, assume that the structure is as follows:

  - id: `foo`
  - Contains two members:
      - name: `some_string`, type: `string`
      - name: `some_float`, type: `float`
  - The component’s uses port is called `message_out`
  - The component’s name is `message_producer`
  - If a connection exists between this component and either a message consumer or an EventChannel, the following code examples send a message.

#### C++

Whenever a message needs to be generated (e.g., in the `serviceFunction()` method of the implementation file):

```c++
foo_struct my_msg;
my_msg.some_string = "hello";
my_msg.some_float = 1.0;
this->message_out->sendMessage(my_msg);
```

#### Java

Whenever a message needs to be generated (e.g., in the `serviceFunction()` method):

```Java
foo_struct my_msg = new foo_struct();
my_msg.some_string.setValue("hello");
my_msg.some_float.setValue((float)1.0);
this.port_message_out.sendMessage(my_msg);
```

#### Python

Whenever a message needs to be generated (e.g., in the `process()` method of the implementation file):

```py
my_msg = message_producer_base.Foo()
my_msg.some_string = "hello"
my_msg.some_float = 1.0
self.port_message_out.sendMessage(my_msg)
```
