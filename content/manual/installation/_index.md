---
title: "Installation"
weight: 10
---

This chapter explains how to install the Core Framework, the IDE, the basic assets, and the REDHAWK Enterprise Integration software. The Core Framework is the software back-end of REDHAWK. The IDE is a GUI for development and interaction with REDHAWK systems. The basic assets are a collection of components, devices, and waveforms that developers can use to create simple software-defined radio applications. The REDHAWK Enterprise Integration software is a suite of software for interacting with a REDHAWK Domain in the Java Runtime Environment.

To configure and install REDHAWK and associated dependencies, you must have root permissions. The REDHAWK installation is compatible with RHEL or CentOS 6 (32-and 64-bit) and RHEL or CentOS 7 (64-bit). The current REDHAWK release was tested against CentOS 6.9 (32-and 64-bit) and CentOS 7.4 (64-bit).

## Installing REDHAWK from RPMs

This section provides step-by-step instructions for installing a REDHAWK release using the YUM command-line package management tool. The installation process includes:

  - Configuring the host system to install REDHAWK dependencies from Fedora EPEL
  - Downloading and configuring the REDHAWK YUM repository on the host system
  - Installing the REDHAWK software
  - Setting up the user environment for immediate REDHAWK runtime or development use

{{% notice note %}}
Before beginning the installation process, if you are upgrading from a 1.8.x version of REDHAWK or for more information about external dependencies, refer to [External Dependencies]({{<relref "manual/appendices/dependencies.md" >}}).
{{% /notice %}}

### Configuring the Host System to Install REDHAWK EPEL Dependencies

REDHAWK has several open-source software dependencies from the EPEL repository. If your system is not configured to receive software packages from EPEL, you can configure it as follows:

For RHEL/CentOS 7:

```bash
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```

For RHEL/CentOS 6:

```bash
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
```

### Downloading and Configuring the REDHAWK Yum Repository on the Host System

The following conventions are used in the instructions that follow.

| **Variable** | **Description**                                 | **Example**          |
| :----------- | :---------------------------------------------- | :------------------- |
| `<version>`  | REDHAWK version                                 | *2.0.3*              |
| `<dist>`     | Linux distribution as represented by rpm macros | *el6* (for CentOS 6) |
| `<arch>`     | host architecture                               | *x86\_64*            |

Adjust the variables to match the desired REDHAWK version, host Linux distribution, and host machine architecture. For example, for REDHAWK version 2.0.3, 64-bit CentOS 6, `redhawk-yum-2.0.3-el6-x86_64.tar.gz`.

#### Downloading the YUM Archive of REDHAWK

Download the archive of the desired version of REDHAWK for your host OS and architecture.

```bash
wget https://github.com/RedhawkSDR/redhawk/releases/download/<version>/redhawk-yum-<version>-<dist>-<arch>.tar.gz
```

#### Setting Up the REDHAWK Repository

1.  In the directory that you want to use for the REDHAWK yum repository, extract the contents of the tar file.

    ```bash
    tar xzvf redhawk-yum-<version>-<dist>-<arch>.tar.gz
    cd redhawk-<version>-<dist>-<arch>
    ```

2.  Install the redhawk-release package (containing the REDHAWK GPG signing key):

    ```bash
    sudo yum install -y redhawk-release*.rpm
    ```

3.  Enter the following commands to add the following file, `/etc/yum.repos.d/redhawk.repo`:

    ```bash
    cat<<EOF|sed 's@LDIR@'`pwd`'@g'|sudo tee /etc/yum.repos.d/redhawk.repo
    [redhawk]
    name=REDHAWK Repository
    baseurl=file://LDIR/
    enabled=1
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhawk
    EOF
    ```

### Installing REDHAWK

Use one of the following options to install the IDE, Core Framework, REDHAWK Enterprise Integration, and accompanying dependencies from RPMs.

  - To install only the runtime REDHAWK software, enter the following command:

    ```bash
    sudo yum groupinstall "REDHAWK Runtime"

    ```

  - To install the REDHAWK development software, enter the following command:

    ```bash
    sudo yum groupinstall "REDHAWK Development"

    ```

  - To install the REDHAWK Enterprise Integration software, enter the following command:

    ```bash
    sudo yum groupinstall "REDHAWK Enterprise Integration"

    ```

{{% notice note %}}
If you want to be more selective about the packages you install, refer to [REDHAWK Yum Repository and Packages
]({{<relref "manual/appendices/redhawk-yum.md" >}}) for a list of packages that can be installed. You can also [install a stand-alone IDE]({{<relref "manual/appendices/standalone-ide.md" >}}).
{{% /notice %}}

### Setting Up the User Environment

1.  Enter the following commands to set up the environment variables:

    ```bash
    . /etc/profile.d/redhawk.sh
    . /etc/profile.d/redhawk-sdrroot.sh

    ```

2.  Use the following command to add each REDHAWK user to the `redhawk` group:

    ```bash
    sudo /usr/sbin/usermod -a -G redhawk <username>

    ```

    Where `<username>` is the name of a user to add to the group. If you are logged into an account that you modify with `usermod`, you must log out and back in for the changes to take effect.

## Configuring omniORB

The omniORB configuration file (`/etc/omniORB.cfg`) must be edited to provide information about how to reach the CORBA name service. By default, the config file contains the following entries:

```bash
InitRef = NameService=corbaname::127.0.0.1:2809
supportBootstrapAgent = 1
```

{{% notice note %}}
The `NameService` line provides information about how to reach the CORBA naming service. The number is an IP address followed by a colon and a port number. The port number is used as a default if no other number is specified. `SupportBootstrapAgent` is a server side option. This enables omniORB servers and Sun’s JavaIDL clients to work together. When set to 1, an omniORB server responds to a bootstrap agent request.
{{% /notice %}}

1.  Add the following line to the config file to configure the CORBA event service (this requires root permissions):

    ```bash
    InitRef = EventService=corbaloc::127.0.0.1:11169/omniEvents
    ```

    The first number is the IP address followed by a colon and a port number. `omniEvents` is the object key.

2.  Enter the following command to start the `omniNames` and `omniEvents` services:

    ```bash
    sudo $OSSIEHOME/bin/cleanomni
    ```

3.  For CentOS 6 systems, to have `omniNames` and `omniEvents` start automatically at system boot (recommended), enter the following commands:

    ```bash
    sudo /sbin/chkconfig --level 345 omniNames on
    sudo /sbin/chkconfig --level 345 omniEvents on
    ```

4.  For CentOS 7 systems, to have `omniNames` and `omniEvents` start automatically at system boot (recommended), enter the following commands:

    ```bash
    sudo systemctl enable omniNames.service
    sudo systemctl enable omniEvents.service
    ```

For more information about omniORB configuration file settings (`/etc/omniORB.cfg`), refer to Chapter 4 of the omniORB User’s Guide (<http://omniorb.sourceforge.net/omni41/omniORB/omniORB004.html>.

### Configuring omniORB for Distributed Systems

If you want to run a Domain Manager and Device Manager from two different computers, the following procedure explains how to configure omniORB for distributed systems.

1.  On the computer from which you want to run the Domain Manager, start `omniNames` and `omniEvents` and then launch a Domain Manager.

    {{% notice note %}}
The firewall may need to be disabled to allow the Device Manager to connect.
    {{% /notice %}}

2.  On the computer from which you want to run the Device Manager, modify the `omniORB.cfg` file so that the IP address for the `NameService` and `EventService` is the address of the computer running the Domain Manager.

    The following example is a modified Domain Manager `omniORB.cfg` file:

    ```bash
    InitRef = NameService=corbaname::127.0.0.1:
    InitRef = EventService=corbaloc::127.0.0.1:11169/omniEvents
    ```

    The following example is a modified Device Manager `omniORB.cfg` file:

    ```bash
    InitRef = NameService=corbaname::<IP address of Domain Manager>:
    InitRef = EventService=corbaloc::<IP address of Domain Manager>:11169/omniEvents
    ```

    {{% notice note %}}
Neither `omniEvents` nor `omniNames` needs to be running on this computer.
    {{% /notice %}}

3.  On the computer running the Device Manager, test that you can see the Domain Manager by running `nameclt list`.

    The name of the Domain Manager is displayed.

4.  Start the Device Manager.

    Any devices in the node are registered with the Domain Manager.

5.  To verify that you can view both the Device Manager and Domain Manager, from either computer, run `nameclt list <Domain Manager Name>`.

    The Device Manager and Domain Manager are displayed.

{{% notice note %}}
omniORB may have trouble automatically resolving its location. In this case, it may be necessary to set the endpoints in the `omniORB.cfg` files by adding the following to each `omniORB.cfg` file: `endpoint = giop:tcp:<IP address of achine>`. You must restart `omniEvents` and `omniNames` for these changes to take effect.
{{% /notice %}}

{{% notice note %}}
Run `rh_net_diag` to help diagnose any problems. Refer to for more information on how to use `rh_net_diag`.
{{% /notice %}}

## Configuring JacORB to Support the IDE

The IDE uses JacORB version 3.3 for CORBA communication. The IDE includes a configuration file for JacORB in the IDE’s directory (in `configuration/jacorb.properties`). The file includes explanations and examples for many of JacORB’s configuration options. For more information, refer to chapter 3 of the JacORB 3.3 Programming Guide. Typically, there is no need to adjust any JacORB settings.
