### Overview

In the previous section you set up a Maven project capable of compiling a StreamFlow Framework JAR.  In order to identify the Spouts and Bolts that are available in your Framework JAR, StreamFlow utilizes a single Framework configuration file.  The framework configuration is integral to building Components that can be used within the StreamFlow UI to dynamically build topologies.  Although the project you created in the last section will compile, the lack of a `framework.yml` configuration file will prevent StreamFlow from registering any Spouts or Bolts within your JAR.  

> **Important:** Even though you provide the source code in your framework project, Spouts and Bolts which have not been registered in the `framework.yml` will not be visible by the StreamFlow UI.

The framework configuration file is used to register all of the Spouts, Bolts, Resources, and Serializations that are present within the framework jar project.  This configuration can be defined using a YAML format or a JSON format.  Selection of the configuration format (YAML/JSON) is solely a personal preference as all settings are available in each format.  Although either format can be used, YAML is the recommended format as it is less verbose than JSON when formatting the configuration file.  

The `framework.yml` and `framework.json` configuration files must be located in a `STREAMFLOW-INF` folder at the root of the class path (e.g. `src/main/resources/STREAMFLOW-INF/framework.yml` or `src/main/resources/STREAMFLOW-INF/framework.json`).  The following sample `framework.yml` and `framework.json` files showcase the property and interface configuration for a component.  

> **Note:** The following YAML and JSON configurations are equivalent and you should only define either `framework.yml` OR `framework.json` in your project.

#### Sample framework.yml

#### Sample framework.json

