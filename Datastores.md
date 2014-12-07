## Overview

Datastores are used to persist all user created entities and are required when running StreamFlow.  A Datastore is simply an implementation of the StreamFlow Datastore interface which provides the API calls to properly integrate with a storage endpoint.  Each Datastore can expose a set of custom properties which can be passed to the datastore implementation at runtime via the `streamflow.yml` configuration file.  Swapping out a datastore is as simple as modifying the `streamflow.yml` configuration file and restarting the StreamFlow server.

StreamFlow comes prebuilt with support for JDBC databases and MongoDB databases.  If either of these databases does not suit your needs, you can always implement your own Datastore to integrate with another storage solution.  

This section will cover the JDBC datastore, MongoDB datastore, and the procedure to implement a custom datastore to supplement the existing capabilities.  In each section, the properties that are unique to each Datastore will be covered in detail and a example configuration will be provided.

## JDBC Datastore (Default)
By default, StreamFlow is configured to use the JDBC datastore which is backed by an embedded [H2 database](http://www.h2database.com/html/main.html).  This configuration allows the StreamFlow server to run by default without needing to install a database on the server.  If the embedded database does not suit your needs, you can configure the JDBC datastore to point to a traditional database using simple JDBC properties.  

The following sample datastore section of the `streamflow.yml` configuration file outlines each of the JDCB datastore properties:

```
datastore:
    
```

## MongoDB Datastore

## Building a Custom Datastore