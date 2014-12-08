In the previous section you set up a Maven project capable of compiling a StreamFlow Framework JAR.  In order to identify the Spouts and Bolts that are available in your Framework JAR, StreamFlow utilizes a single Framework configuration file.  The framework configuration is integral to building Components that can be used within the StreamFlow UI to dynamically build topologies.  Although the project you created in the last section will compile, the lack of a `framework.yml` configuration file will prevent StreamFlow from registering any Spouts or Bolts within your JAR.  

> **Important:** Even though you provide the source code in your framework project, Spouts and Bolts which have not been registered in the `framework.yml` will not be visible by the StreamFlow UI.

The framework configuration file is used to register all of the Spouts, Bolts, Resources, and Serializations that are present within the framework jar project.  This configuration can be defined using a YAML format or a JSON format.  Selection of the configuration format (YAML/JSON) is solely a personal preference as all settings are available in each format.  Although either format can be used, YAML is the recommended format as it is less verbose than JSON when formatting the configuration file.  

The `framework.yml` and `framework.json` configuration files must be located in a `STREAMFLOW-INF` folder at the root of the class path (e.g. `src/main/resources/STREAMFLOW-INF/framework.yml` or `src/main/resources/STREAMFLOW-INF/framework.json`).  The following sample `framework.yml` and `framework.json` files outline the format of these configuration files.  Following the examples, each major section of the configuration will be covered in detail.

> **Note:** The following YAML and JSON configurations are equivalent and you should only define either `framework.yml` OR `framework.json` in your project.

### Sample framework.yml

```
# Framework Properties
name: sample-framework
label: Sample Framework
version: 1.0.0-SNAPSHOT
description: Spouts and Bolts implemented for demonstration purposes

# Framework Components
components: 
  - name: sample-bolt
    label: Sample Bolt
    type: storm-bolt
    description: Sample bolt used for demonstration purposes
    mainClass: streamflow.bolt.SampleBolt
    properties: 
      - name: field-one
        label: Field One
        description: Description of field one
        defaultValue: 10
        required: true
        type: number
      - name: field-two
        label: Field Two
        description: Description of field two
        defaultValue: second
        required: false
        type: select
        options:
            listItems:
              - first
              - second
              - third   
    inputs: 
      - key: default
        description: Takes any tuple as input
    outputs:
      - key: default
        description: Processed activity content

# Framework Resources      
resources:
  - name: file-resource
    label: File Resource
    description: File Resource for Testing
    resourceClass: streamflow.resource.FileResource
    properties:
      - name: file-resource
        label: File Resource
        description: Use this resource to upload and save files
        defaultValue: 
        required: true
        type: file

# Framework Serializations
serializations:
  - typeClass: streamflow.serializer.SampleType
    serializerClass: streamflow.serializer.SampleTypeSerializer
```

### Sample framework.json

```
{
    "name": "sample-framework",
    "label": "Sample Framework",
    "version": "1.0.0-SNAPSHOT",
    "description": "Spouts and Bolts implemented for demonstration purposes",
    "components": [
        {
            "name": "sample-bolt",
            "label": "Sample Bolt",
            "type": "storm-bolt",
            "description": "Sample bolt used for demonstration purposes",
            "mainClass": "streamflow.bolt.SampleBolt",
            "properties": [
                {
                    "name": "field-one",
                    "label": "Field One",
                    "description": "Description of field one",
                    "defaultValue": "10",
                    "required": true,
                    "type": "number"
                },
                {
                    "name": "field-two",
                    "label": "Field Two",
                    "description": "Description of field two",
                    "defaultValue": "second",
                    "required": false,
                    "type": "select",
                    "options": {
                        "listItems": [
                            "first",
                            "second",
                            "third"
                        ]
                    }
                }
            ],
            "inputs": [
                {
                    "key": "default",
                    "description": "Takes any tuple as input"
                }
            ],
            "outputs": [
                {
                    "key": "default",
                    "description": "Processed activity content"
                }
            ]
        }
    ],    
    "resources": [
        {
            "name": "file-resource",
            "label": "File Resource",
            "description": "File Resource for Testing",
            "resourceClass": "streamflow.resource.FileResource",
            "properties": [
                {
                    "name": "file-resource",
                    "label": "File Resource",
                    "description": "Use this resource to upload and save files",
                    "defaultValue": "",
                    "required": true,
                    "type": "file"
                }
            ]
        }
    ],
    "serializations": [
        {
            "typeClass": "streamflow.serializer.SampleType",
            "serializerClass": "streamflow.serializer.SampleTypeSerializer"
        }
    ]
}
```

## General Configuration
Framework properties define general information about a framework that is used to identify the framework. These properties are typically listed at the top of the framework configuration for clarity, although it can be located anywhere in the configuration.  

Let's look at a snippet of the framework properties from the above example and walk through each property in detail.

```
name: sample-framework
label: Sample Framework
version: 1.0.0-SNAPSHOT
description: Spouts and Bolts implemented for demonstration purposes
```

#### `name`
- **Description:** The name is the globally unique identifier for the framework.  You must ensure that no other frameworks use the same name, otherwise you the two frameworks will collide during framework upload.  To help protect against this, it is common to prefix your framework name with namespace style information (e.g. `streamflow.core.sample.framework`)
- **Default:** *None*

#### `label`
- **Description:** The label is the user friendly name that user's will see in the UI to identify your framework.
- **Default:** *None*

#### `version`
- **Description:** The version helps identify the version number of your framework to users.  Although you are free to enter any value for this field, we recommend incrementing your version using Semantic Versioning.
- **Default:** *None*

#### `description`
- **Description:** Helps users understand the purpose of your framework
- **Default:** *None*


## Component Configuration
The `components` section of the configuration is used to register Storm Spouts and Bolts in the framework.  The `components` property is an array which can be used to register multiple components in the configuration.  The components that are registered in this section will appear in the palette of the topology editor when the framework is uploaded to the StreamFlow server.

Let's look at a snippet of the components section from the above example and walk through each property in detail.

```

```

## Property Configuration

## Resource Configuration

## Serialization Configuration