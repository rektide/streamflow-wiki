## Overview

StreamFlow uses sensible configuration defaults to allow you to run the server with minimal configuration.  In fact, the default configuration requires no initial modification when starting the server for the first time.  

> **Warning:** If you plan to make any changes to the configuration, it is recommended that you read through this section at least once.

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

    # Logging Configuration
    logger:
        level: INFO
        baseDir: /var/log/storm
        formatPattern: "%d{ISO8601,GMT} %p %X{topology} %X{component} %c - %m%n"
		
    # Datastore configuration
    datastore:
        moduleClass: jetstream.datastore.mongodb.config.MongoDatastoreModule
        # Datastore specific properties included here ...

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

The `proxy` configuration section is used to configure HTTP proxy settings if required in your environment

##### `proxy.host`
- **Description:** (Optional) HTTP proxy host for your environment 
- **Default:** *None*

##### `proxy.port`
- **Description:** (Optional) HTTP proxy port for your environment 
- **Default:** *None*


## Auth Configuration

The `auth` configuration is used to configure HTTP authentication/authorization via [Apache Shiro](http://shiro.apache.org/).  Enabling this setting will protect all web resources  with a username and password.  If you would like to customize the Shiro authentication mechanism please consult the ["Building a Custom Realm"](Authentication#building-a-custom-realm) section.

> **Warning:** Before enabling authentication, browse to the StreamFlow UI accounts page `http://{STREAMFLOW_SERVER}:{STREAMFLOW_PORT}/#/users` and create some users while authentication is disabled.

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

The `datastore` configuration is used to configure the active [Datastore](Datastores) implementation.  Datastores control how entities such as Topologies, Frameworks, and Users are persisted for StreamFlow.  StreamFlow comes built in with a [JDBC Datastore](Datastores#jdbc-datastore) and [MongoDB Datastore](Datastores#mongodb-datastore) implementation.  Although the JDBC datastore is recommended for simple installations, please select whichever Datastore implementation best fits your needs.  For detailed information on Datastores and the implementations please see [Datastores](Datastores).

> **Note:** Each Datastore implementation allows for custom properties which affect its behavior.  Consult the documentation for each Datastore to see which configuration properties can be specified.



## Logger Configuration


## Clusters Configuration