---
title: "Including External Libraries"
weight: 90
---

This section details how to link a C++ component with a library. Two examples are given: using a `pkg-config` (`.pc`) file to find and link against a library, and directly linking against a library. Using the `pkg-config` file will allow your project to check for the presence of the library and issue an error while running configure if it is not found. It also will let you avoid hard-coded options. If a `pkg-config` file is not available, or if you wish to directly supply the compiler/linker flags, you can use the second method.

### Adding a Library by Referencing a `pkg-config` File

To add a library by referencing a `pkg-config` (`.pc`) file, edit the `configure.ac` file in your component’s implementation directory. Find the line referencing PROJECTDEPS in the code that looks like this:

```c++
PKG_CHECK_MODULES([PROJECTDEPS], [ossie >= 2.0 omniORB4 >= 4.1.0])
```

and add your library to the list of requirements. For example, if you need version 1.2.3 or greater of the foo library:

```c++
PKG_CHECK_MODULES([PROJECTDEPS], [
  ossie >= 2.0
  omniORB4 >= 4.1.0
  foo >= 1.2.3
])
```

If your `pkg-config` file is not on the `pkg-config` path, you can augment the `pkg-config` search path by adding a line just before the call to PKG_CHECK_MODULES:

```bash
export PKG_CONFIG_PATH=/custom/path:$PKG_CONFIG_PATH
```

### Adding a Library Directly

To add a library, edit the `Makefile.am` file in your component’s implementation directory. Append compiler and linker flags to the end of the `CXXFLAGS` and `LDADD` lines, respectively. For example:

```make
MyComponent_CXXFLAGS += -I/usr/local/include/foo
MyComponent_LDADD += -L/usr/local/lib64 -lfoo
```
