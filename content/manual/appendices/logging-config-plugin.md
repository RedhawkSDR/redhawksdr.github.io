---
title: "Logging Configuration Plugin"
weight: 90
---

The DomainManager may be extended to use a loadable library that can assist in the resolution of the `LOGGING_CONFIG_URI` parameter during application deployments. The following code and build files provide a template to build the loadable library: `libossielogcfg.so`.

The library should be installed in `$OSSIEHOME``/lib64` or `$OSSIEHOME``/lib` depending on your hardware and operating system. If you choose to install the library in a different directory, you will need to add this path to `LD_LIBRARY_PATH` before starting the DomainManager. To enable this feature in the DomainManager, use the `—useloglib` option when launching the `nodeBooter` program.

```bash
nodeBooter -D --useloglib
```

### `LogConfigUriResolver` Class and Example

The `LogConfigUriResolver` class is the base class that your customized class will inherit. The class contains a single method `get_uri`, that will be used to resolve the logging configuration file location during deployment. The method accepts a single parameter `path` that describes the resource’s path in the `Domain`. The following list describes the different resource paths:

- **Component**
  - syntax
  ```bash
  rsc:<DomainName>/<Application ID>/<Component's Naming Service Name>
  ```

  - example
  ```bash
  rsc:REDHAWK_DEV/TestWave_2/MyComp_4
  ```

- **Device**
  - syntax
  ```bash
  dev:<DomainName>/<Node ID>/<Device's Naming Service Name>
  ```

  - example
  ```bash
  dev:REDHAWK_DEV/Node_1/MyDevice_4
  ```

- **Service**
  - syntax
  ```bash
  svc:<DomainName>/<Node ID>/<Service's Naming Service Name>
  ```

  - example
  ```bash
  svc:REDHAWK_DEV/Node_1/MyRedis_3
  ```

Using this path, the customized code should return the location of a logging configuration file or an empty string (use current default resolution method). Logging configuration file locations should be formatted as follows:

  - `sca://path/to/config/file`
  ```bash
   sca://logcfg/comp.log.cfg
  ```

  - `file:///absolute/path/to/config/file`
  ```bash
  file:///var/redhawk/sdr/dom/logcfg/comp.log.cfg
  ```

The following example code creates a custom resolver class that provides logging configuration files from the local `SDRROOT` directory (file:///var/redhawk/sdr/dom/logcfg/device.log.cfg). The macro `MAKE_FACTORY` allows the class to be dyamically loaded by the Domain Manager.

```c++
/*
 * This file is protected by Copyright. Please refer to the COPYRIGHT file
 * distributed with this source distribution.
 *
 * This file is part of REDHAWK core.
 *
 * REDHAWK core is free software: you can redistribute it and/or modify it
 * under the terms of the GNU Lesser General Public License as published by the
 * Free Software Foundation, either version 3 of the License, or (at your
 * option) any later version.
 *
 * REDHAWK core is distributed in the hope that it will be useful, but WITHOUT
 * ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
 FITNESS FOR A PARTICULAR PURPOSE.  See the GNU Lesser General Public License
 * for more details.
 *
 * You should have received a copy of the GNU Lesser General Public License
 * along with this program.  If not, see http://www.gnu.org/licenses/.
 */
#include <ossie/logging/LogConfigUriResolver.h>
#include <iostream>
#include <sstream>

class ossielogcfg : public ossie::logging::LogConfigUriResolver {

public:

  ossielogcfg();

  virtual ~ossielogcfg();

  std::string get_uri( const std::string &path );
};


class CustomLogConfigResolver : public ossie::logging::LogConfigUriResolver {

public:

  CustomLogConfigResolver() {};

  virtual ~CustomLogConfigResolver(){};

  /**
    get_uri

    Return a string object that will be passed to a resource on the command line as
    LOGGING_CONFIG_URI parameter. An empty string will be ignored by the DomainManager.

    @param path  Path of the resource in the domain.
                 for components, rsc:<domain name>/<application name>/<component naming service name>
                 e.g.            rsc:REDHAWK_DEV/TestWave_2/MyComp_4

                 for devices, dev:<domain name>/<node name>/<device's naming service name>
                 e.g.            dev:REDHAWK_DEV/Node_1/MyDevice_4

                 for service, svc:<domain name>/<node name>/<service's naming service name>
                 e.g.            svc:REDHAWK_DEV/Node_1/MyRedis_1
  */

  std::string get_uri( const std::string &path ) {

    std::string sdrroot("");
    if ( ::getenv("SDRROOT")){
      sdrroot = ::getenv("SDRROOT");
    }

    if ( path.find("dev:") != std::string::npos ) {
      std::ostringstream os;
      os << "file://" << sdrroot << "/dom/logcfg/device.log.cfg";
      return std::string(os.str());
    }
    if ( path.find("svc:") != std::string::npos ) {
      std::ostringstream os;
      os << "file://" << sdrroot << "/dom/logcfg/serviceq.log.cfg";
      return std::string(os.str());
    }

    if ( path.find("rsc:") != std::string::npos ) {
      std::ostringstream os;
      os << "file://" << sdrroot << "/dom/logcfg/comp.log.cfg";
      return std::string(os.str());
    }
    // an empty string return value will be ignored by the DomainManager
    return std::string("");
  };

};

MAKE_FACTORY(CustomLogConfigResolver);
```

## Build Files

Use the following build files to compile and build the above example code in a file called `ossielogcfg.cpp`. The compiled code produces a library called `libossielogcfg.so` that is installed in `$OSSIEHOME``/lib` or `$OSSIEHOME``/lib64`. The build process follows the same paradigm as standard REDHAWK generated software: `reconf; configure; make install`.

### `reconf`

```bash
#!/bin/sh

rm -f config.cache
[ -d m4 ] || mkdir m4
autoreconf -i
```

### `configure.ac`

```bash
AC_INIT(ossielogcfg, 1.0.0)
AM_INIT_AUTOMAKE([nostdinc foreign])
AC_CONFIG_MACRO_DIR([m4])

AC_PROG_CC
AC_PROG_CXX
AC_PROG_INSTALL
AC_PROG_LIBTOOL

AC_CORBA_ORB
OSSIE_CHECK_OSSIE
# TODO: Make this an installed macro
OSSIE_SDRROOT_AS_PREFIX
prefix="${OSSIEHOME}"
libdir="${OSSIEHOME}/lib"
AS_IF( [ test `uname -i` == "x86_64"  ], [ libdir="$prefix/lib64" ], [ libdir="$prefix/lib" ] )

m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

# Dependencies
PKG_CHECK_MODULES([REDHAWK], [ossie >= 2.0])
OSSIE_ENABLE_LOG4CXX
AX_BOOST_BASE([1.41])
AX_BOOST_SYSTEM
AX_BOOST_THREAD
AX_BOOST_REGEX

AC_CONFIG_FILES([Makefile ])
AC_OUTPUT
```

### `Makefile.am`

```bash
ACLOCAL_AMFLAGS = -I m4 -I${OSSIEHOME}/share/aclocal/ossie
AUTOMAKE_OPTIONS = subdir-objects

lib_LTLIBRARIES = libossielogcfg.la

xmldir = $(prefix)
dist_xml_DATA =

distclean-local:
        rm -rf m4

libossielogcfg_la_SOURCES = ossielogcfg.cpp
libossielogcfg_la_LIBADD = $(REDHAWK_LIBS)
libossielogcfg_la_CPPFLAGS = -I . -I $(srcdir)/include $(REDHAWK_CFLAGS) $(BOOST_CPPFLAGS)
bossielogcfg_la_CXXFLAGS = -Wall
```
