## Overview

StreamFlow uses sensible configuration defaults to allow you to run the server with minimal configuration.  In fact, the default configuration requires no initial modification when starting the server for the first time.  Configuration settings for StreamFlow are made through a
[YAML](http://www.yaml.org/spec/1.2/spec.html) configuration file located at `${STREAMFLOW_HOME}/conf/streamflow.yml`.

The sample `streamflow.yml` configuration below outlines the default configuration for StreamFlow.  For detailed information on each setting please consult the following sections.

    # Server Configuration
    server:
        port: 

    # Authentication Configuration
    auth:
        enabled: false

    # Logging Configuration
    logger:
        level: INFO
        baseDir: /var/log/storm
        formatPattern: "%d{ISO8601,GMT} %p %X{topology} %X{component} %c - %m%n"
		
    # Datastore configuration
    datastore:
        moduleClass: jetstream.datastore.mongodb.config.MongoDatastoreModule
        # Datastore specific properties included here ...

    # HTTP Proxy Configuration
    proxy:
        host: 127.0.0.1
        port: 80

    # Cluster Configuration
    clusters:
      - id: develop
        displayName: Develop Cluster
        nimbusHost: localhost 
        nimbusPort: 6627 
        logServerHost: localhost
        logServerPort: 9200
   

## Server Configuration

## Datastore Configuration

## Auth Configuration

## Logger Configuration

## Proxy Configuration