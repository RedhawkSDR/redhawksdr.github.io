---
title: "Message Consumer"
weight: 20
---
To create a message consumer, create a new component or edit an existing component.

1.  Add a Struct property
      - Enter the ID of the message you want the component to consume
      - Select the kind for the property as only **message**
      - Add any arbitrary number of members to the struct

2.  Add **uses/provides** (bidirectional) port of **IDL Interface**  
    `ExtendedEvent> MessageEvent`
3.  Regenerate the component

The bidirectional port is needed because in point-to-point connections, the port behaves like a Provides port, while in connections with an EventChannel, the consumer behaves like a Uses port. This is an artifact of mapping a uses/provides pattern over a producer/consumer pattern. A single object is created for the port, and a call to `getPort()` returns a pointer to the same object irrespective of whether or not the port is to be employed as a Uses or Provides; the object implements both sets of interfaces. The scd contains two entries for this port, both with the same name, but one is a uses and the other is a provides.

For the purposes of the following examples, assume that the structure is as follows:

  - id: `foo`
  - Contains two members:
      - name: `some_string`, type: `string`
      - name: `some_float`, type: `float`
  - The component’s uses/provides port is called `message_in`
  - The component’s callback function for this message is `messageReceived()`
  - The component’s name is `message_consumer`

If a connection exists between this component and either a message producer or an EventChannel, the following code examples process an incoming message.

{{% notice note %}}
Any message that comes in with the property ID `foo` will trigger the callback function `messageReceived()`.
{{% /notice %}}

#### C++

Given the asynchronous nature of events, a callback pattern was selected for the consumer.

In the component header file, declare the following callback function:

```c++
void messageReceived(const std::string &id, const foo_struct &msg);
```

In the component source file, implement the callback function:

```c++
void message_consumer_i::messageReceived(const std::string &id, const foo_struct &msg) {
  LOG_INFO(message_consume_i, id<<" "<<msg.some_float<<" "<<msg.some_string);
}
```

In the `constructor()` method, register the callback function:

```c++
message_in->registerMessage("foo", this, &message_consumer_i::messageReceived);
```

#### Java

Java callbacks use the `org.ossie.events.MessageListener` interface, which has a single `messageReceived()` method. The recommended style for Java messaging is to define the callback as a private method on the component class, and use an anonymous subclass of `MessageListener` to dispatch the message to your callback.

Add to the list of imports:

```Java
import org.ossie.events.MessageListener;
```

Implement the callback as a method on the component class:

```Java
private void messageReceived(String id, foo_struct msg) {
  logger.info(id + " " + msg.some_float.getValue() + " " + msg.some_string.getValue());
}
```

In the `constructor()` method, register a `MessageListener` for the message to dispatch the message to your callback:

```Java
this.port_message_in.registerMessage("foo", foo_struct.class, new MessageListener<foo_struct>() {
  public void messageReceived(String messageId, foo_struct messageData) {
    message_consumer.this.messageReceived(messageId, messageData);
  }
});
```

#### Python

In the `constructor()` method, register the expected message with a callback method:

```py
self.port_message_in.registerMessage("foo", message_consumer_base.Foo, self.messageReceived)
```

In the class, define the callback method. In this example, the method is called `messageReceived()`:

```py
def messageReceived(self, msgId, msgData):
  self._log.info("messageReceived *************************")
  self._log.info("messageReceived msgId " + str(msgId))
  self._log.info("messageReceived msgData " + str(msgData))
```
