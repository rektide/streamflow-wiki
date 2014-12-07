## Overview

Datastores are used to persist all user created entities and are required when running StreamFlow.  A Datastore is simply an implementation of the StreamFlow Datastore interface which provides the API calls to properly integrate with a storage endpoint.  

All configuration for a Datastore is set in the `datastore` section of the `streamflow.yml` configuration file.  The only required property in the `datastore` section is the `moduleClass` which specifies the fully qualified class name of the datastore module class.  Each Datastore must implement a Guice module which initializes any required dependencies and provides an implementation for each of the StreamFlow Core DAOs.

Each Datastore can expose a set of custom properties in the `datastore` section of the configuration file which are passed to the datastore implementation at runtime.  These properties are used to configure things in the datastore such as the JDBC URL or MongoDB URL.  In each case, if the datastore specific property is not explicitly declared, the defaults listed below will be used.

StreamFlow comes prebuilt with support for JDBC databases and MongoDB databases.  If either of these databases does not suit your needs, you can always implement your own Datastore to integrate with another storage solution.  This section will cover the JDBC datastore, MongoDB datastore, and the procedure to implement a custom datastore to supplement the existing capabilities.  In each section, the properties that are unique to each Datastore will be covered in detail and a example configuration will be provided.


## JDBC Datastore (Default)

By default, StreamFlow is configured to use the JDBC datastore which is backed by an embedded [H2 database](http://www.h2database.com/html/main.html).  This configuration allows the StreamFlow server to run by default without needing to install a database on the server.  If the embedded database does not suit your needs, you can configure the JDBC datastore to point to a traditional database using simple JDBC properties.  

> **Note:** The JDBC Datastore uses JPA for communication with the database server and does not require scripts to initialize the database before use.  

The following sample datastore section of the `streamflow.yml` configuration file outlines each of the JDBC datastore properties:

```
# Datastore configuration
datastore:
    # Datastore module class
    moduleClass: streamflow.datastore.jdbc.config.JDBCDatastoreModule

    # JDBC Datastore specific properties
    url: jdbc:mysql://localhost/test
    driver: com.mysql.jdbc.Driver
    user: streamflow
    password: streamflow
```

##### `moduleClass`
- **Description:** The moduleClass for the JDBC datastore.  To use the JDBC datastore, this property must be `streamflow.datastore.jdbc.config.JDBCDatastoreModule`
- **Default:** streamflow.datastore.jdbc.config.JDBCDatastoreModule

##### `url`
- **Description:** The JDBC URL for the database
- **Default:** jdbc:h2:${STREAMFLOW_HOME}/data/h2/streamflow

##### `driver`
- **Description:** The JDBC driver for the database
- **Default:** org.h2.Driver

##### `user`
- **Description:** The JDBC user for the database
- **Default:** streamflow

##### `password`
- **Description:** The JDBC password for the database
- **Default:** streamflow


## MongoDB Datastore

TODO

The following sample datastore section of the `streamflow.yml` configuration file outlines each of the MongoDB datastore properties:

```
# Datastore configuration
datastore:
    # Datastore module class
    moduleClass: streamflow.datastore.mongodb.config.MongoDatastoreModule

    # MongoDB Datastore specific properties
    host: localhost
    port: 27017
    acceptableLatencyDifference: 
    connectTimeout:
    connectionsPerHost:
    cursorFinalizerEnabled:
    heartbeatConnectRetryFrequency:
    heartbeatConnectTimeout:
    heartbeatFrequency:
    heartbeatSocketTimeout:
    heartbeatThreadCount:
    maxConnectionIdleTime:
    maxConnectionLifeTime:
    maxWaitTime:
    minConnectionsPerHost:
    socketKeepAlive:
    socketTimeout:
    threadsAllowedToBlockForConnectionMultiplier:
```

##### `moduleClass`
- **Description:** The moduleClass for the MongoDB datastore.  To use the MongoDB datastore, this property must be `streamflow.datastore.mongodb.config.MongoDatastoreModule`
- **Default:** streamflow.datastore.mongodb.config.MongoDatastoreModule

##### `host`
- **Description:** The hostname of the MongoDB server
- **Default:** localhost

##### `port`
- **Description:** The port of the MongoDB server
- **Default:** 27017


## Building a Custom Datastore

*Coming soon...*