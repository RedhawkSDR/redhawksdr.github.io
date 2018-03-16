---
title: "External Dependencies"
weight: 20
---

The following sections explain how to install the dependencies from the Fedora Extra Packages for Enterpise Linux (EPEL) and Red Hat/CentOS repositories. Dependencies not available from either of those sources are [included with REDHAWK]({{< relref "manual/appendices/redhawk-yum.md#dependencies-packaged-with-redhawk" >}}).

{{% notice note %}}
If you are upgrading from a previous 1.8.x version of REDHAWK, some software from the 1.8 series dependencies must be uninstalled before installing the REDHAWK 2.0 series. Enter the following command to uninstall the software:
{{% /notice %}}

```bash
sudo yum erase \
     libomniORB4.1 omniORB-debuginfo omniORB-doc \
     libomniORBpy3 omniORBpy-debuginfo omniORBpy-doc \
     omniEvents-doc omniEvents-debuginfo \
     apache-log4cxx apache-log4cxx-debuginfo
```

## Installing the EPEL Repository

{{% notice note %}}
For more information on the Fedora EPEL project, refer to <http://fedoraproject.org/wiki/EPEL>.
{{% /notice %}}

### From the Fedora Downloads Site

Install the EPEL repository on your system from the Fedora downloads site.

For RHEL/CentOS 7:

```bash
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```

For RHEL/CentOS 6:

```bash
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
```

### Standalone EPEL from REDHAWK

REDHAWK provides a condensed version of the EPEL yum repository that can be used to satisfy the required external dependencies from the EPEL repository. The repository is available from the REDHAWK release page on github (<https://github.com/RedhawkSDR/redhawk/releases/<version>>). (Where `<version>` corresponds to the version of the REDHAWK IDE. For example, for REDHAWK version 2.0.3, <https://github.com/RedhawkSDR/redhawk/releases/2.0.3)>).

To install the Standalone EPEL yum repository from REDHAWK, use the following commands:

```bash
wget https://github.com/RedhawkSDR/redhawk/releases/download/<version>/redhawk-yum-<version>-<dist>-<arch>.tar.gz
tar xf redhawk-yum-<version>-<dist>-<arch>.tar.gz
cd redhawk-yum-<version>-<dist>-<arch>
sudo yum install --nogpgcheck *.rpm
```

Where `<version>`, `<dist>`, and `<arch>` correspond to the associated REDHAWK version, Linux distribution, and architecture respectively. For example, for REDHAWK version 2.0.3, 64-bit CentOS 6, `redhawk-yum-2.0.3-el6-x86_64.tar.gz`.

## Runtime-only Dependencies

The following dependencies are required for the REDHAWK Framework for runtime, operational-only systems that do not need to support development. These dependencies will enable the installation of Domain Manager, Device Manager, GPP, Core Framework, omniNames, and omniEvents.

### Dependencies for RHEL/CentOS 6

  - `python-matplotlib`
  - `omniORB`
  - `omniORB-devel`
  - `omniORB-doc`
  - `omniORB-servers`
  - `omniORB-utils`
  - `python-jinja2-26`

To install the dependencies for RHEL/CentOS 6, enter the following commands:

```bash
sudo yum install python-matplotlib \
     omniORB \
     omniORB-devel \
     omniORB-doc \
     omniORB-servers \
     omniORB-utils \
     python-jinja2-26
```

### Dependencies for RHEL/CentOS 7

  - `gstreamer-python`
  - `python-matplotlib-qt4`
  - `log4cxx`
  - `omniORB`
  - `omniORB-devel`
  - `omniORB-doc`
  - `omniORB-servers`
  - `omniORB-utils`
  - `python-jinja2`

To install the dependencies for RHEL/CentOS 7, enter the following commands:

```bash
sudo yum install gstreamer-python \
     python-matplotlib-qt4 \
     log4cxx \
     omniORB \
     omniORB-devel \
     omniORB-doc \
     omniORB-servers \
     omniORB-utils \
     python-jinja2
```

## Dependencies for Development and Building from Source

The following dependencies are required for development with the REDHAWK Framework and building REDHAWK from source.

### Dependencies for RHEL/CentOS 6

  - `libuuid-devel`
  - `boost-devel`
  - `autoconf automake libtool`
  - `cppunit-devel`
  - `expat-devel`
  - `gcc-c++`
  - `java-1.8.0-openjdk-devel`
  - `junit4`
  - `python-devel`
  - `python-matplotlib`
  - `numpy`
  - `PyQt4`
  - `omniORB`
  - `omniORB-devel`
  - `omniORB-doc`
  - `omniORB-servers`
  - `omniORB-utils`
  - `python-jinja2-26`
  - `xsd`

To install the dependencies for RHEL/CentOS 6, enter the following commands:

```bash
sudo yum install libuuid-devel \
     boost-devel \
     autoconf automake libtool \
     cppunit-devel \
     expat-devel \
     gcc-c++ \
     java-1.8.0-openjdk-devel \
     junit4 \
     python-devel \
     python-matplotlib \
     numpy \
     PyQt4 \
     omniORB \
     omniORB-devel \
     omniORB-doc \
     omniORB-servers \
     omniORB-utils \
     python-jinja2-26 \
     xsd
```

### Dependencies for RHEL/CentOS 7

  - `gstreamer-python`
  - `libuuid-devel`
  - `boost-devel`
  - `cppunit-devel`
  - `autoconf automake libtool`
  - `expat-devel`
  - `gcc-c++`
  - `java-1.8.0-openjdk-devel`
  - `python-devel`
  - `python-matplotlib-qt4`
  - `numpy`
  - `PyQt4`
  - `log4cxx`
  - `log4cxx-devel`
  - `omniORB`
  - `omniORB-devel`
  - `omniORB-doc`
  - `omniORB-servers`
  - `omniORB-utils`
  - `python-jinja2`
  - `xsd`

To install the dependencies for RHEL/CentOS 7, enter the following commands:

```bash
sudo yum install gstreamer-python \
     libuuid-devel \
     boost-devel \
     cppunit-devel \
     autoconf automake libtool \
     expat-devel \
     gcc-c++ \
     java-1.8.0-openjdk-devel \
     python-devel \
     python-matplotlib-qt4 \
     numpy \
     PyQt4 \
     log4cxx \
     log4cxx-devel \
     omniORB \
     omniORB-devel \
     omniORB-doc \
     omniORB-servers \
     omniORB-utils \
     python-jinja2 \
     xsd
```

### Optional Dependencies for Development

The following dependencies are required for Octave component development.

  - `octave-devel`

To install the dependencies, enter the following command:

```bash
sudo yum install octave-devel
```
