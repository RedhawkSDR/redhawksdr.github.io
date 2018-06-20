---
title: "Managing and Defining Properties"
weight: 60
---

Properties are defined by their structure, kind, and type. The four different property structures include:

  - `simple` - single value such as 1.0, or “a string"
  - `simple sequence` - list/array of zero or more simples such as \[1, 2, 3\], or \[“first", “second"\]
  - `struct` - groups several simples and simple sequences together
  - `struct sequence` - list/array of zero or more instances of a struct

Three commonly used kinds of properties in REDHAWK include:

  - `property` - denotes properties that are used for configuration and status
  - `allocation` - expresses requirements that will be satisfied by a REDHAWK device
  - `message` - used only with structs and indicates that the struct will be used as an event message within REDHAWK

The property’s type corresponds with basic programming language primitive types such as floats, long integers, booleans, etc. Additionally, numeric types can be complex.

Through the use of generated code and the REDHAWK libraries, manipulation of properties uses fundamental types provided by C++, Python, or Java, as seen in [Properties]({{< relref "base-component-members.md#properties" >}}).  For example, a `simple sequence`, complex-float property is manipulated via a `std::vector< std::complex<float> >` variable in C++ and a list of Python complex objects in Python. Generated component code provides a class data field representing each property for that component.

The primitive data types supported for `simple` and `simple sequence` properties are: `boolean`, `octet`, `float`, `double`, `short`, `ushort`, `long`, `longlong`, `ulong`, `ulonglong`, `string`, `objref`, and `char`.

The following primitive data types can be marked as complex values: `boolean`, `octet`, `float`, `double`, `short`, `ushort`, `long`, `longlong`, `ulong`, and `ulonglong`.

Each component implements the `CF::PropertySet` interface, which provides remote access to the component’s properties through the `query()` and `configure()` methods. The `query()` method provides a means for reading a component’s current property settings and the `configure()` method provides a means for setting a component’s property values. Properties identified in these methods will use the property identifier value to resolve identifier access.

Properties can be `readonly`, `writeonly`, or `readwrite`. properties with read privileges can only be accessed using the `query()` method and properties with write privileges can only be set using the `configure()` method.

The REDHAWK library base classes provide a complete implementation of `configure()`, with the creation of specific properties handled per component by the generated base classes. Beyond the basic updating of local values, the standard `configure()` implementation provides:

  - Thread-safe updates via mutual exclusion
  - Automatic conversion of numeric types
  - Notification on changes to property values
  - External reporting of changes via events
  - Exception throwing for invalid input

Because of these enhancements, developers are strongly discouraged from overloading either the `query()` or `configure()` methods.

### Property ID

Properties are identified by ID and name. The ID must be unique to the scope of the component or device. This uniqueness applies to all properties, including the members of `struct` and `struct sequence` properties. Therefore, if two different `struct` properties in the same component each have a member with the name `abc`, both members cannot use the ID `abc`.

To eliminate ID conflicts, REDHAWK provides a naming convention that allows for multiple `struct` properties to use the same member names without creating ID conflicts. For members of a `struct`, the ID is created by combining the name of the member and the ID of the `struct`. For example, if `struct` property `foo` has a simple member `bar`, the member would have the name `bar` and ID `foo::bar`. The naming convention also applies to `struct sequence` properties as well.

### Property Name

The property name, if provided, is used for member variables in generated code and for display within the IDE. If not provided, the ID is used instead.

### Property Access Summary

The combination of property types and their related attributes, such as readwrite and command-line, can be confusing. Furthermore, different properties whose values are overloaded are initialized at different points in the component lifecycle. To clarify the behavior of these different properties, the table below is included.

##### Property Summary
<table style="width:100%">
  <tr>
    <th colspan="3" style="border:1px solid gray;">Setting</th>
    <th colspan="5" style="border:1px solid gray;">Behavior (Yes/No/Undefined)</th>
  </tr>
  <tr>
    <td align="center"><b>kind</b></td>
    <td align="center"><b>mode</b></td>
    <td align="center"><b>command line</b></td>
    <td align="center"><b>overload</b></td>
    <td align="center"><b>config</b></td>
    <td align="center"><b>query</b></td>
    <td colspan="2" align="center"><b>initialization</b></td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td></td>
    <td><b>language constructor</b></td>
    <td><b>generated constructor</b></td>
  </tr>
  <tr>
    <td><code>property</code></td>
    <td><code>readwrite</code></td>
    <td>No</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>No</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td><code>property</code></td>
    <td><code>readonly</code></td>
    <td>No</td>
    <td>Yes</td>
    <td>No</td>
    <td>Yes</td>
    <td>No</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td><code>property</code></td>
    <td><code>writeonly</code></td>
    <td>No</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>No</td>
    <td>No</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td><code>property</code></td>
    <td><code>readwrite</code></td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>No</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td><code>property</code></td>
    <td><code>readonly</code></td>
    <td>Yes</td>
    <td>Yes</td>
    <td>No</td>
    <td>Yes</td>
    <td>No</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td><code>property</code></td>
    <td><code>writeonly</code></td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>No</td>
    <td>No</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td><code>allocation</code></td>
    <td><code>readwrite</code></td>
    <td>N/A</td>
    <td>No</td>
    <td>No</td>
    <td>U</td>
    <td>U</td>
    <td>U</td>
  </tr>
  <tr>
    <td><code>allocation</code></td>
    <td><code>readonly</code></td>
    <td>N/A</td>
    <td>No</td>
    <td>No</td>
    <td>U</td>
    <td>U</td>
    <td>U</td>
  </tr>
  <tr>
    <td><code>allocation</code></td>
    <td><code>writeonly</code></td>
    <td>N/A</td>
    <td>No</td>
    <td>No</td>
    <td>No</td>
    <td>U</td>
    <td>U</td>
  </tr>
  <tr>
    <td><code>message</code></td>
    <td>any</td>
    <td>N/A</td>
    <td>No</td>
    <td>No</td>
    <td>No</td>
    <td>U</td>
    <td>U</td>
  </tr>
</table>

There are several aspects of the table that are of note. The language-specific constructor (for example, `__init__` in Python) never has the correct overloaded default value for a property; that value is only available when the generated `constructor` function is invoked. The use of the command-line marker for a property does not make it available at the language-specific constructor. The command-line marker is used to pass a property value in the command-line to services.

A service normally does not support interfaces necessary to initialize properties over CORBA. In order to support properties in services, a property can be defined for a service and set as command-line, allowing the property to be overloaded for the specific service instance. The resulting command line argument will resolve from the service’s PRF file, and optionally be overridden from the DCD file. Standard property processing will apply to all types with the exception of the boolean type; in which case the exact contents supplied in either the PRF or DCD file will be passed on the command line.

Allocation properties are not to be used as value containers; they are to be used to describe allocations to the device. While it is technically possible to query allocation properties, and while they may hold a value in the device implementation, that value is undefined, and could be anything. Allocation properties only have meaning in the context of `allocateCapacity` calls, and by extension have meaning only when servicing an allocation callback function.

Message properties have no storage meaning at all. They are prototypes describing structure message formats to be sent over `MessageEvent` ports. Furthermore, message properties can only be structures; they cannot be `simple`, `simplesequence`, or `structsequence`.

### Property Change Listeners

Often, it is useful to trigger additional actions when the value of a property changes. components support a type of notification called property change listeners that enable the developer to register callback methods that are executed whenever `configure()` is called with new values for the particular property.

Property change listeners are executed while holding the lock that protects access to all properties for the component. This ensures that no outside changes can occur while responding to property changes. The callback may alter the value of the property or call additional functions; however, avoid computationally expensive or blocking operations.

#### C++

C++ components support notification of property value changes using member function callbacks.

The following example explains how to add a property change listener for the `freqMHz` simple property of type `float`, of a component named `MyComponent`.

In `[component].h`, add a private method declaration for your callback. The callback receives two arguments, the old and new values:

```c++
void freqMHz_changed(float oldValue, float newValue);
```

Implement the function in `[component].cpp`.

Then, in the component `constructor()`, register the change listener:

```c++
this->addPropertyListener(freqMHz, this, &MyComponent_i::freqMHz_changed);
```

`addPropertyListener` takes three arguments: the property’s member variable, the target object (typically `this`) and a pointer to a member function.

When defining a property listener for `struct` or `sequence` property, the new and old values are passed by `const` reference:

```c++
void taps_changed(const std::vector<float>& oldValue, const std::vector<float>& newValue);
```

#### Python

Like C++, Python components allow registering listeners by property. The callback is typically a member function.

The following example explains how to add a property change listener for the `freqMHz` property.

Define the callback as a member function on your component. Excluding the implicit `self` argument, the callback receives three arguments: the property ID and the old and new values.

```py
def freqMHz_changed(self, propid, oldval, newval):
    # Perform action based on change
```

In your component `constructor()` method, register the change listener:

```py
self.addPropertyChangeListener("freqMHz", self.freqMHz_changed)
```

#### Java

Java properties support an idiomatic listener interface for responding to changes. As opposed to C++ and Python, listener registration is performed directly on the property object.

The following example explains how to add a property change listener for the `freqMHz` property of a component named `MyComponent`.

Define your callback that will respond to changes for the property as a member function on your component class. For simple numeric properties, the old and new value arguments can be the primitive type (for example, `float`):

```java
private void freqMHz_changed(float oldValue, float newValue) {
    // Perform action based on change
}
```

In your component’s `constructor()` method, define an anonymous subclass of `org.ossie.properties.PropertyListener` that connects the property’s change notification to your callback. For simple numeric properties, the type parameter of the `PropertyListener` class must be the boxed type (for example, `Float`).

```java
this.freqMHz.addChangeListener(new PropertyListener<Float>() {
    public void valueChanged(Float oldValue, Float newValue) {
        MyComponent.this.freqMHz_changed(oldValue, newValue);
    }
});
```

### Customizing Query and Configure

{{% notice note %}}
This feature is C++-only in the REDHAWK 2.0 series baseline.
{{% /notice %}}

The REDHAWK libraries and generated component code automatically handle `query()` and `configure()` for all defined properties. However, in some cases, it may be preferable to retrieve the current value of a property in response to a `query()`, such as when fetching status from an external library. A developer may also want more control over how the property value is set. components support per-property callbacks to customize query and configure behavior.

The query callback is called when the component receives a `query()` for that property, in lieu of consulting the local state. Likewise, the configure callback is called when the component receives a `configure()` for that property, instead of updating the component local state.

{{% notice note %}}
Unlike property listeners, the configure callback is always called regardless of whether the new value is equal to the old value.
{{% /notice %}}

Query and configure callbacks are executed while holding the lock that protects access to all properties for the component. This ensures that the callback has exclusive access to the component properties. If possible, avoid computationally expensive or blocking operations to ensure that the component remains responsive.

#### C++

In C++, query and configure callbacks are registered on the components. Registering a new callback replaces the old one.

##### Query Callbacks

To create a query callback, in `[component].h`, add a private member function declaration. It takes no arguments and returns the value:

```c++
float get_freqMHz();
```

Implement the function in `[component].cpp`.

Then, in the body of `constructor()`, register the query function:

```c++
this->setPropertyQueryImpl(freqMHz, this, &MyComponent_i::get_freqMHz);
```

`setPropertyQueryImpl` takes three arguments: the property’s member variable, the target object (typically `this`) and a pointer to a member function.

##### Configure Callbacks

To create a configure callback, in `[component].h`, add a private member function declaration. It takes one argument, the new value, and returns `void`:

```c++
void set_freqMHz(float value);
```

Implement the function in `[component].cpp`.

Then, in the body of `constructor()`, register the configure function:

```c++
this->setPropertyConfigureImpl(freqMHz, this, &MyComponent_i::set_freqMHz);
```

`setPropertyConfigureImpl` takes three arguments: the property’s member variable, the target object (typically `this`) and a pointer to a member function.

When a configure callback is set, the member variable is not updated automatically. It is up to the component developer to update the member variable, if desired.

#### Overriding the `configure()` Method

For the vast majority of cases, the standard `configure()` implementation is sufficient. Developers are strongly discouraged from overriding `configure()`. However, in the event that additional functionality beyond what is provided is required, the overridden method should call the base class `configure()` method to ensure that the behavior expected by the library and framework is preserved. Whether the base class method is pre- or post-extended is left to the discretion of the component developer.
