Complete the following steps to build a new StreamFlow distribution from source and install it on your local desktop or server.  If you already have a precompiled StreamFlow *.tar.gz or *.zip distribution, you can skip ahead to the [Installation](#installation) section.

## System Requirements

## Building the Project

## Installation

This section covers the process to install a new distribution of StreamFlow on your system.  If you already have a running StreamFlow installation, please skip to the [Upgrades](#upgrades) section to modify your existing installation.

If you have not done so already, please download a pre-built StreamFlow distribution or build your own using the above steps.  Once you have your distribution ready, copy it to a directory of your choosing.  The examples below assume you will be installing StreamFlow to `/opt/streamflow-{VERSION}`

Note: In the following examples replace `{VERSION}` with the StreamFlow distribution version (e.g. 0.8.0)

#### For *nix Systems

    cp streamflow-{VERSION}.tar.gz /opt
    cd /opt
    tar -xvzf streamflow-{VERSION}.tar.gz

If your system supports chkconfig also execute the following steps to start StreamFlow automatically on system reboot:

    ln -s /opt/streamflow-{VERSION}/bin/init.d/streamflow /etc/init.d/streamflow
    chkconfig --add streamflow

#### For Windows Systems

    copy streamflow-{VERSION}.zip /opt
    cd /opt
    // Unzip using Windows explorer or using your installed compression library

Thats it!  You are now ready to startup StreamFlow and get started building your first topology.

## Startup

StreamFlow comes packaged with Bash scripts (`streamflow.sh`) for *nix systems and Batch scripts (`streamflow.bat`) for Windows to make starting StreamFlow easy on any environment.  If you are running on a `chkconfig` supported system such as Red Hat Enterprise Linux, CentOS, or Fedora, StreamFlow also provides an init.d script for automatic startup of the server on reboot.  Execute the following commands to startup StreamFlow

Note: If you installed StreamFlow to a location other than `/opt/streamflow-0.8.0` replace the path with the path to your StreamFlow installation.

### For *nix Systems (using bash script)

To start the server:

    cd /opt/streamflow-0.8.0
    ./bin/streamflow.sh 

To stop the server:

    Ctrl-C

### For Linux Systems (using chkconfig)

To start the server:

    service streamflow start

To stop the server:

    service streamflow stop

### For Windows Systems 

To start the server:

    cd {STREAMFLOW_HOME}
    ./bin/streamflow.bat

To stop the server:

    Ctrl-C

## Upgrades

## Logstash Configuration (optional)