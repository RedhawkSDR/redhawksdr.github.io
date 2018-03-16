---
title: "Auto-Generated Component Files"
weight: 10
---

The [REDHAWK IDE]({{< relref "manual/ide/_index.md" >}}) provides a tool for auto-generating component code for C++, Python, or Java languages. This process takes care of REDHAWK compliance and allows the developer to insert their own custom processing algorithm for working with the data sent/received by the component. The following section provides a brief description of the generated files. Note that some files may be freely modified, while other files should not be modified.

Modifying particular files is discouraged for two reasons:

1.  If a user regenerates the component using the IDE (for example, to add a port), particular files are overwritten by the code generators.

    {{% notice note %}}
1. Files whose modification is not discouraged (e.g., `componentName.cpp`) are not affected by such an action.

2. The REDHAWK IDE provides the option of not rewriting particular files.
    {{% /notice %}}

2.  Modification of files implementing REDHAWK interfaces may impact compatibility with other REDHAWK modules.

The word `componentName` is replaced with the component name provided during component creation.

### Files Generated for All Components

#### Build-Related Files

The code generators create the following files for building and installing the component using Autotools:

  - `build.sh` - Two of these files are generated: one of them resides in the top-level component directory and the other resides in the source directory. The first of the two calls the `build.sh` script in the source directory.

    The `build.sh` script in the source directory runs `./reconf; ./configure; make`. The Autotools documentation can provide more information on these commands: in short, this process creates the component entry point.

    {{% notice note %}}
If you use this script, you must install the dependency rpm-build by entering the following command: `sudo yum install rpm-build`.
    {{% /notice %}}
  - `configure.ac` - A standard Autoconf configuration file. Refer to the Autoconf documentation for more information.

  - `Makefile.am` - A standard Automake configuration file. Refer to the Automake documentation for more information.

  - `Makefile.am.ide` - Provides build information for the REDHAWK IDE.

  - `reconf` - Script that is used to generate the configure script in the Autotools chain.

  - `componentName.spec` - Provides `rpmbuild` configuration information for facilitating building RPMs for the component. Refer to the RPM documentation for more information.

### Component XML Descriptors

  - `componentName.prf.xml` - Describes the component’s properties. More information on the component’s `prf.xml` file can be found in the REDHAWK specification.

  - `componentName.scd.xml` - Describes the component’s ports and interfaces. More information on the component’s `scd.xml` file can be found in the REDHAWK specification.

  - `componentName.spd.xml` - Provides a top-level description of the component, including the names and locations of the component entry point and XML files. More information on the component’s `spd.xml` file can be found in the REDHAWK specification.

### Unit Tests File

  - `test_componentName.py` - Contains a unit test built on the standard Python `unittest` Framework. The provided test exercises the generated code. Additional tests may be added for testing algorithms produced by the developer. For more information on how to add unit tests, consult the Python `unittest` Framework documentation.

    To run the unit test(s), simply execute the file from the command-line `./test_componentName.py`.

## Files Generated for C++ Components

### Files where your implementation-specific code should be

  - `componentName.h` - The appropriate place to put header-related code in support of `componentName.cpp`.

  - `componentName.cpp` - The appropriate place for developers to add their own processing behavior.

  - `port_impl.h` (Optional) - This file is only generated for ports that utilize interfaces other than BulkIO, BurstIO, FEI and Messaging. Contains the port-related code for the component. If the type of ports changes, then this file needs to be re-generated, overwriting your application-specific code.

  - `port_impl.cpp` (Optional) - This file is only generated for ports that utilize interfaces other than BulkIO, BurstIO, FEI and Messaging. Contains the port-related code for the component. If the type of ports changes, then this file needs to be re-generated, overwriting your application-specific code.

### Files that you should not modify

If these files are modified, then your ability to regenerate the component is affected.

  - `main.cpp` - Contains the function that is used to create an instance of the component. For shared library components, this is a dynamically-loadable function called `make_component()`. For executable components, this is the `main()` function for the process. Modification of this file is not recommended.

  - `struct_props.h` - Contains support classes for struct properties that are defined in the code-generation interface. Modification of this file is discouraged.

  - `componentName_base.h` - Provides interface implementations for the component based on the component’s ports and properties. Modification of `componentName_base.h` is discouraged.

  - `componentName_base.cpp` - Provides interface implementations for the component based on the component’s ports and properties. Modification of `componentName_base.cpp` is discouraged.

### Files Generated for Python Components

#### Files where your implementation-specific code should be

  - `componentName.py` - The appropriate place for developers to add their own processing behavior.

#### Files that you should not modify

If these files are modified, then your ability to regenerate the component is affected.

  - `componentName_base.py` - Provides interface implementations for the component based on the component’s ports and properties. Modification of this file is discouraged.

### Files Generated for Java Components

#### Files where your implementation-specific code should be

  - `componentName.java` - The appropriate place for developers to add their own processing behavior.

#### Files that you should not modify

If these files are modified, then your ability to regenerate the component is affected.

  - `startJava.sh` - Script containing the process that is started when the component is instantiated.

  - `componentName_base.java` - Provides interface implementations for the component based on the component’s ports and properties. Modification of this file is discouraged.

#### Transitioning Java Components from REDHAWK version 1.8 to later versions

If `componentName.java` is not regenerated when migrating from REDHAWK 1.8 to later versions, the component does not inherit from the base class, so it does not take advantage of the new code pattern. In addition, in order for the component to build, the method `configureOrb` must be added to `componentName.java` because the base class makes a call on that method.

If you want to inherit from the base class and take advantage of the new pattern, save off the custom main class Java file, regenerate the main Java class to get the new code pattern, and for example, move source from the `run()` method to the `serviceFunction()` method to integrate the custom code in the 1.8 main class into the new main class.

{{% notice warning %}}
If `componentName.java` is regenerated, any custom code added to it is removed.
{{% /notice %}}

Add this class to the `componentName.java` right before the last line which is "}"

```Java
/**
 * Set additional options for ORB startup. For example:
 *
 *   orbProps.put("com.sun.CORBA.giop.ORBFragmentSize",
 *                          Integer.toString(fragSize));
 *
 * @param orbProps
 */
public static void configureOrb(final Properties orbProps) {
}
```
