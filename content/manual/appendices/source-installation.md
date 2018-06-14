---
title: "Building and Installing REDHAWK from Source"
weight: 40
---

## Building the Framework

### Installing Build Dependencies

Building REDHAWK from source requires a few additional dependencies beyond those required to run REDHAWK. The following procedure explains how to build REDHAWK from source.

1.  First, ensure your system has the necessary [dependency software]({{< relref "dependencies.md" >}}) provided by RHEL / CentOS and Fedora EPEL.
2.  Ensure the REDHAWK Yum repository is set up using the process described in [Setting Up the REDHAWK Repostiory]({{< relref "manual/installation/_index.md#setting-up-the-redhawk-repository" >}}).  Install the [dependencies]({{< relref "manual/appendices/redhawk-yum.md#dependencies-packaged-with-redhawk" >}}) distributed with the REDHAWK tarball.

### Installing the Framework from Source

To install the Core Framework from source, the `redhawk-src-<version>.tar.gz` must be downloaded.

```bash
wget https://github.com/RedhawkSDR/redhawk/releases/download/<version>/redhawk-src-<version>.tar.gz
```

You must set the `OSSIEHOME` and `SDRROOT` environment variables (recommended defaults shown below) before running the installation script. You must have write permission for the locations of `OSSIEHOME` and `SDRROOT` or the installation will not work.

{{% notice note %}}
The installation script is interactive and will prompt you to continue before performing certain tasks.
{{% /notice %}}

To compile the source, execute the following commands:

```bash
export OSSIEHOME=/usr/local/redhawk/core
export SDRROOT=/var/redhawk/sdr
tar zxvf redhawk-src-<version>.tar.gz
cd redhawk-src-<version>/
./redhawk-install.sh
. $OSSIEHOME/environment-setup
```

{{% notice note %}}
If you wish to preserve the environment used to compile the source, add the following lines to `.bashrc`:

```bash
export OSSIEHOME=/usr/local/redhawk/core
export SDRROOT=/var/redhawk/sdr
. $OSSIEHOME/environment-setup
```
{{% /notice %}}

To build the the source code with or without optional features, provide the appropriate build option to the `configure` setup. The following table describes some common options.

| **Option**   | **Description** |
| :-------------- | :-------- |
| `--enable-affinity` | Enable numa affinity processing.  |
| `--enable-persistence=<type>` | Enable persistence support. Supported types: `bdb`, `gdbm`, `sqlite`.  |
| `--disable-log4cxx`  | Disable log4cxx support.   |


To view a complete list of configurations, enter the following commands:

```bash
cd <redhawk src directory>
cd redhawk/src
./reconf
./configure --help
```
To provide any of the build options, edit the `redhawk-install.sh` script and change the following line to include the appropriate option:

```bash
./configure
```

### Setting Environment Variables

REDHAWK expects several environment variables to be set to run. REDHAWK installs a set of scripts that appropriately set these variables in the `etc/profile.d` directory in your installation. Source the appropriate files for your shell before running. For example, if you installed to the default `OSSIEHOME` location (`/usr/local/redhawk/core`) and are using bash/dash:

```bash
. /usr/local/redhawk/core/etc/profile.d/redhawk.sh
. /usr/local/redhawk/core/etc/profile.d/redhawk-sdrroot.sh
```

or copy them to your system’s `/etc/profile.d` directory to make them global for all users:

```bash
sudo cp /usr/local/redhawk/core/etc/profile.d/* /etc/profile.d
```

{{% notice note %}}
Remember to restart your terminal if you modify the system’s `/etc/profile.d` directory for changes to take effect.
{{% /notice %}}

### Configuring omniORB

Refer to [Configuring omniORB]({{< relref "manual/installation/_index.md#configuring-omniorb" >}}) for information on how to edit the omniORB configuration file (`/etc/omniORB.cfg`) to provide information about how to reach the CORBA event service.
