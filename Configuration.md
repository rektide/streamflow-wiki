## Overview

StreamFlow uses sensible configuration defaults to allow you to run the server with minimal configuration.  In fact, the default configuration requires no initial modification when starting the server for the first time.  

> **Warning:** If you plan to make any changes to the configuration, it is recommended that you read through this section in its entirety at least once.

Configuration settings for StreamFlow are made through a
[YAML](http://www.yaml.org/spec/1.2/spec.html) configuration file located at `${STREAMFLOW_HOME}/conf/streamflow.yml`.

The sample `streamflow.yml` configuration below outlines the default configuration for StreamFlow.  For detailed information on each setting please consult the associated section.

    # Server Configuration
    server:
        port: 8080

    # HTTP Proxy Configuration
    #proxy:
    #    host: 127.0.0.1
    #    port: 80

    # Authentication Configuration
    auth:
        enabled: false
        realmClass: streamflow.server.security.DatastoreRealm
        moduleClass: streamflow.server.security.DatastoreRealmModule
		
    # Datastore configuration
    datastore:
        moduleClass: streamflow.datastore.jdbc.config.JDBCDatastoreModule

        # JDBC Datastore specific properties
        url: jdbc:h2:${STREAMFLOW_HOME}/data/h2/streamflow
        driver: org.h2.Driver
        user: streamflow
        password: streamflow

    # Logging Configuration
    logger:
        level: INFO
        baseDir: /var/log/storm
        formatPattern: "%d{ISO8601,GMT} %p %X{topology} %X{component} %c - %m%n"

    # Cluster Configuration
    clusters:
      - id: develop
        displayName: Develop Cluster
        nimbusHost: localhost 
        nimbusPort: 6627 
        logServerHost: localhost
        logServerPort: 9200
   

## Server Configuration

The `server` configuration section is used to configure HTTP server settings.

##### `server.port`
- **Description:** HTTP port to bind the StreamFlow server to.  This setting will determine where you need to point your browser to access the StreamFlow UI and REST services. 
- **Default:** 8080


## Proxy Configuration

The `proxy` configuration section is used to configure HTTP proxy settings if required in your environment.  The HTTP proxy settings are made available to Spouts and Bolts via injection if needed.

##### `proxy.host`
- **Description:** (Optional) HTTP proxy host for your environment 
- **Default:** *None*

##### `proxy.port`
- **Description:** (Optional) HTTP proxy port for your environment 
- **Default:** *None*


## Auth Configuration

The `auth` configuration is used to configure HTTP authentication/authorization via [Apache Shiro](http://shiro.apache.org/).  Enabling this setting will protect all web resources  with a username and password.  If you would like to customize the Shiro authentication mechanism please consult the ["Building a Custom Realm"](Authentication#building-a-custom-realm) section.

> **Warning:** Before enabling authentication, start up StreamFlow and browse to the StreamFlow UI accounts page `http://{STREAMFLOW_SERVER}:{STREAMFLOW_PORT}/#/users` to create some users while authentication is disabled.

##### `auth.enabled`
- **Description:** "true" if authentication is enabled, "false" otherwise. 
- **Default:** false

##### `auth.realmClass`
- **Description:** (Optional) Specifies the fully qualified class name of the Shiro authorizing realm.  This property should be changed in conjunction with the `auth.moduleClass` property to fit your configuration.  This should only be changed if you plan to change the default Shiro authentication to use another method such as LDAP.
- **Default:** streamflow.server.security.DatastoreRealm

##### `auth.moduleClass`
- **Description:** (Optional) Specifies the fully qualified class name of the Guice module that supports injection of your `auth.realmClass`.  This property should be changed in conjunction with the `auth.realmClass` property to fit your configuration.  This should only be changed if you plan to change the default Shiro authentication to use another method such as LDAP.
- **Default:** streamflow.server.security.DatastoreRealmModule


## Datastore Configuration

The `datastore` configuration is used to configure the active [Datastore](Datastores) implementation.  Datastores control how entities such as Topologies, Frameworks, and Users are persisted for StreamFlow.  StreamFlow comes built in with a [JDBC Datastore](Datastores#jdbc-datastore) and [MongoDB Datastore](Datastores#mongodb-datastore) implementation.  Although the JDBC datastore is recommended for simple installations, please choose whichever Datastore implementation best fits your needs.  For detailed information on Datastores and the implementations please see [Datastores](Datastores).

> **Note:** Each Datastore implementation allows for custom properties which affect its behavior.  Consult the documentation for each Datastore to see which additional properties are available.

##### `datastore.moduleClass`
- **Description:** The fully qualified class name of the Guice module supporting the Datastore implementation. Each datastore implementation must implement an AbstractModule capable of loading any datastore dependencies.
- **Default:** streamflow.datastore.jdbc.config.JDBCDatastoreModule

> The following settings are specific to the default JDBC Datastore which utilizes an embedded H2 database.  If you would like to use a standalone database, override these settings to match your database.

##### `datastore.url`
- **Description:** JDBC URL of the target database 
- **Default:** jdbc:h2:${STREAMFLOW_HOME}/data/h2/streamflow

##### `datastore.driver`
- **Description:** JDBC driver for the target database 
- **Default:** org.h2.Driver

##### `datastore.user`
- **Description:** JDBC username for the target database 
- **Default:** streamflow

##### `datastore.password`
- **Description:** JDBC password for the target database 
- **Default:** streamflow


## Logger Configuration

The logging configuration is used to configure the location and configuration of topology log files.  This configuration data is used by topologies to publish log data and should be configured to fit the cluster environment.

Note: Changes made in this section should also be reflected in the Logstash agent configurations if Logstash is being used.

##### `logger.level`
- **Description:** Logging level to use when outputting data
- **Default:** INFO

##### `logger.baseDir`
- **Description:** Base directory to publish log data to.  This directory must already exist on the server and have write permission by the StreamFlow server 
- **Default:** /var/log/storm

##### `logger.formatPattern`
- **Description:** SLF4J log pattern to use when outputting topology log data 
- **Default:** "%d{ISO8601,GMT} %p %X{topology} %X{component} %c - %m%n"


## Clusters Configuration

The `clusters` configuration allows users to register Storm clusters with StreamFlow.  StreamFlow is designed to work with your existing Storm clusters which can selected when submitting topologies.  StreamFlow supports registration of multiple clusters allowing you to select from available Storm clusters at runtime.  In production, we typically see developers utilize a development cluster for testing and a production cluster for production.

> Note: Regardless of the configuration, the embedded "Local" cluster is always available for testing.  This cluster allows users to submit and test topologies without requiring an available Storm cluster.  This takes advantage of Storm's LocalCluster capability which does not require a Storm installation and is only recommended for testing purposes.

##### `clusters.id`
- **Description:** Unique id to assign to this cluster
- **Default:** *None*

##### `clusters.displayName`
- **Description:** User friendly name that will appear when selecting a cluster
- **Default:** *None*

##### `clusters.nimbusHost`
- **Description:** Hostname of the nimbus machine for the Storm cluster
- **Default:** localhost

##### `clusters.nimbusPort`
- **Description:** Thrift port of the nimbus machine for the Storm cluster
- **Default:** 6627

##### `clusters.logServerHost`
- **Description:** (Optional) Hostname of the Logstash Elasticsearch server
- **Default:** localhost

##### `clusters.logServerPort`
- **Description:** (Optional) HTTP port of the Logstash Elasticsearch server
- **Default:** 9200
