This section covers the process to build a new StreamFlow distribution from source and install it on your local desktop or server.  If you already have a precompiled StreamFlow *.tar.gz or *.zip distribution, you can skip ahead to the [Installation](#installation) section.


## System Requirements

StreamFlow was designed to use minimal server resources and should run easily on any server purchased within the last few years.  As StreamFlow it built entirely in Java, it should run on any server capable of running Java.

Although not hard requirements, the recommended minimum settings for a StreamFlow installation are as follows:

* Red Hat Enterprise Linux 6 / Apple OSX / Windows 7
* 1 GHz CPU
* 512 MB RAM
* 1 GB Available Disk Space (Used for storing user data and logs)

> **Note:** StreamFlow typically runs fine on other operating systems not listed above, although the above distributions are the primary operating systems we use for testing and verification.

StreamFlow is built entirely in Java and is the only required dependency for a server installation.  StreamFlow supports both Java 6 and Java 7, however a current Java 7 release is recommended.

A specific version of Java can be specified by setting the `JAVA_HOME` environment variable on your server.


## Building the Project

If you wish to manually build StreamFlow from source, download or clone a copy of our git repo.

    git clone https://github.com/lmco/streamflow.git

StreamFlow is built using [Maven 3](http://maven.apache.org/) and is required when building the distribution from source.  Please ensure that Maven is properly installed and configured before attempting to build StreamFlow.  To build StreamFlow please execute one of the following commands:

**Build distribution with unit AND integration tests:**

    mvn clean install

**Build distribution with unit tests only:**

    mvn clean package

When the source has finished compiling, the StreamFlow *.tar.gz and *.zip distributions can be found at `streamflow-dist/target/streamflow-{VERSION}.tar.gz` and `streamflow-dist/target/streamflow-{VERSION}.zip` respectively.  These compiled archives can be used to install or upgrade StreamFlow.


## Installation

This section covers the process to install a new distribution of StreamFlow on your system.  If you already have a running StreamFlow installation, please skip to the [Upgrades](#upgrades) section to update your existing installation.

If you have not done so already, please download a pre-built StreamFlow distribution or build your own using the above steps.  Once you have your distribution ready, copy it to a directory of your choosing.  The examples below assume you will be installing StreamFlow to `/opt/streamflow-{VERSION}`

> **Note:** In the following examples replace `{VERSION}` with the StreamFlow distribution version (e.g. 0.8.0)

#### For *nix Systems

    cp streamflow-{VERSION}.tar.gz /opt
    cd /opt
    tar -xvzf streamflow-{VERSION}.tar.gz

> (Optional) If your system supports chkconfig also execute the following steps to start StreamFlow automatically on system reboot:

    ln -s /opt/streamflow-{VERSION}/bin/init.d/streamflow /etc/init.d/streamflow
    chkconfig --add streamflow

#### For Windows Systems

    copy streamflow-{VERSION}.zip /opt
    cd /opt
    // Unzip using Windows explorer or using your installed compression library

Thats it!  You are now ready to startup StreamFlow and get started building your first topology.


## Startup

StreamFlow comes packaged with Bash scripts (`streamflow.sh`) for *nix systems and Batch scripts (`streamflow.bat`) for Windows to make starting StreamFlow easy on any environment.  If you are running on a `chkconfig` supported system such as Red Hat Enterprise Linux, CentOS, or Fedora, StreamFlow also provides an init.d script for automatic startup of the server on reboot.  Execute the following commands to startup StreamFlow:

> **Note:** If you installed StreamFlow to a location other than `/opt/streamflow-0.8.0` replace the path with the path to your StreamFlow installation.

### For *nix Systems (using bash script)

**To start the server:**

    cd /opt/streamflow-0.8.0
    ./bin/streamflow.sh 

**To stop the server:**

    Ctrl-C

### For Linux Systems (using chkconfig)

**To start the server:**

    service streamflow start

**To stop the server:**

    service streamflow stop

### For Windows Systems 

**To start the server:**

    cd /opt/streamflow-0.8.0
    ./bin/streamflow.bat

**To stop the server:**

    Ctrl-C

When started, the StreamFlow server bootstraps its configuration using the `streamflow.yml` YAML file located at `${STREAMFLOW_HOME}/conf/streamflow.yml` to configure the application.  This file contains information such as `server.port` which defines which port the StreamFlow HTTP server is bound to.  For detailed information on configuration StreamFlow, please check the [Configuration](Configuration) section.

> **Warning:** If you experience any binding errors on startup, change the `server.port` property in the `streamflow.yml` file to use an alternative free port.

## Upgrades

The StreamFlow distribution when unpacked contains some directories which store user data and configuration.  When upgrading to a new StreamFlow distribution, it is important to make backups of these directories/files.  The following directories contain user data and should be copied from the original distribution and pasted into the corresponding directory of the new distribution.

> **Note:** `${STREAMFLOW_HOME}` represents the path to your StreamFlow installation

* `${STREAMFLOW_HOME}/data` - Contains all user content such as topologies and frameworks
* `${STREAMFLOW_HOME}/conf` - Contains `streamflow.yml` configuration settings for the server

When upgrading your `streamflow.yml` always remember to check the upgrade notes to see if any configuration properties have been added or deprecated.  Please adjust your streamflow.yml as necessary to remain valid with new distributions.


## Logstash Configuration (optional)

Coming soon...