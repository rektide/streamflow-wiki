## Overview

Frameworks contain all of the business logic for StreamFlow.  A StreamFlow Framework is simply a JAR file which contains a framework configuration file and implementations of Storm Spouts and Bolts.  The framework configuration (`framework.yml`) is a simple YAML file which registers each Spout and Bolt and any properties that associated with them.  The StreamFlow UI utilizes these registered Spouts and Bolts to allow users to dynamically create new topologies in a drag and drop interface.

This section describes the process to develop Spouts and Bolts for Storm that are compatible with StreamFlow. A major goal of StreamFlow is to you to import your existing Spout and Bolt implementations without requiring you to import custom StreamFlow base classes.  This goal was meant to allow users to easily transition their existing code to StreamFlow without the need to heavily refactor their code.  StreamFlow does *NOT* require extension of custom base classes which allows you to utilize your existing Spouts and Bolts with minimal or no code changes required.

While your code does not need to import custom Storm libraries, your Spouts and Bolts can benefit from the dynamic property injection system.  This injection system allows a Storm component to expose a set of configurable properties which can be edited in the StreamFlow UI and automatically injected into the component at runtime.  These properties allow Storm components to be more flexible and interoperable.

The following sections will help you get started building your first StreamFlow framework and explain in detail all of the options available to you. 


##Project Setup

StreamFlow frameworks are compiled as a single JAR file which contains all of your Storm component implementations and the `framework.yml` configuration file.  

In order for a framework jar to be compatible with Storm, it is necessary to include additional goals in your maven pom.xml to ensure that all transient dependent libraries are compiled into the jar.  This requirement allows Storm topologies to be completely self-contained when executing on remote clusters.  The Maven Shade plugin is used to accomplish this task which includes all dependent libraries in the framework jar during packaging.  In general, any dependencies in your pom which are not `provided` will be compiled into your framework jar file.  Due to this behavior, it is important to list the storm dependency as a `provided` dependency to avoid compiling the Storm libraries in your framework.  All other dependencies should use the default scope of `compile` to ensure that those dependencies are included to prevent Class not found exceptions during runtime. 

This section will outline the process to build a StreamFlow framework Maven project from scratch.  Before continuing please verify that Maven is correctly installed on your development workstation.

Before we start coding, it is worthwhile to discuss the overall project layout we will be setting up.  Below is the overall structure of a typical StreamFlow framework Maven project:

    framework-project
    |--pom.xml
    `--src
       `--main
          |--resources
          |  |--STREAMFLOW-INF
          |  |  `--framework.yml
          |  `--icons
          |     `--SampleIcon.png
          `--java
             `--streamflow
                `--example
                   |--spout
                   |  `--SampleSpout.java
                   |--bolt
                   |  `--SampleBolt.java
                   |--resource
                   |  `--SampleResource.java
                   `--serialization
                      `--SampleKryoSerialization.java

With this target project structure in mind, let's start by using Maven's archetype construct to build the base structure of our project.  

> **Note:** If you would prefer to build the directory structure manually or use an existing project feel free to do so.  The archetype is simply used to help build the base project structure for those building their project from scratch.

Open up a new command line prompt and execute the following command.  

> **Note:** Replace `{project-package}` with the groupId you would like to use for the new project.  Maven will also use this to generate the default package structure for your project.  Replace `{project-name}` with the artifactId you would would like to use for your project.  Finally, replace `{project-version}` with the version you would like to use for your project.

    mvn archetype:generate -DgroupId={project-package} \
       -DartifactId={project-name} \
       -Dversion={project-version} \
       -DarchetypeArtifactId=maven-archetype-quickstart \
       -DinteractiveMode=false

If successful, you will find the your new Maven project folder in your current directory using the `{project-name}` you provided in the command.  While the Maven archetype does a good job of creating the base project structure, there are a few changes we will need to make to ensure the project is a valid StreamFlow framework.

First, lets modify the `pom.xml` project configuration to add the Storm dependency and Shade plugin.  The Storm dependency will allow us to properly compile Storm components and the Shade plugin will ensure that all non `provided` dependencies are compiled into our final JAR file.

Open the `pom.xml` file in your text editor so it looks similar to the following:

> **Note:** Make sure to leave your `groupId`, `artifactId`, `version`, and `name` as is to fit your project.

    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0" 
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
             xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>

        <groupId>streamflow.sample</groupId>
        <artifactId>sample-framework</artifactId>
        <version>1.0.0</version>
        <name>Sample StreamFlow Framework</name>
        <packaging>jar</packaging>

        <!-- =========================================================== -->
        <!-- Builds all plugin dependency libraries into a single jar    -->
        <!-- =========================================================== -->
        <build>
            <plugins>
                <!-- Maven Shade Plugin (Includes all dependencies) -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-shade-plugin</artifactId>
                    <version>2.3</version>
                    <executions>
                        <execution>
                            <phase>package</phase>
                            <goals>
                                <goal>shade</goal>
                            </goals>
                            <configuration>
                                <createDependencyReducedPom>false</createDependencyReducedPom>
                                <filters>
                                    <filter>
                                        <artifact>*:*</artifact>
                                        <excludes>
                                            <exclude>META-INF/*.SF</exclude>
                                            <exclude>META-INF/*.DSA</exclude>
                                            <exclude>META-INF/*.RSA</exclude>
                                        </excludes>
                                    </filter>
                                </filters>
                            </configuration>
                        </execution>
                    </executions>
                </plugin>
            </plugins>
        </build>

        <dependencies>
            <!-- Storm Core Library (Provided) -->
            <dependency>
                <groupId>org.apache.storm</groupId>
                <artifactId>storm-core</artifactId>
                <scope>provided</scope>
            </dependency>

            <!-- Google Guice Dependency Injection (Provided) -->
            <dependency>
                <groupId>com.google.inject</groupId>
                <artifactId>guice</artifactId>
                <scope>provided</scope>
            </dependency>
            
            <!-- ADD ANY REQUIRED DEPENDENCIES HERE -->
        </dependencies>
    </project>

When implementing your Spouts and Bolts, don't forget to add any required dependencies to this `pom.xml` and use the default `compile` scope for those dependencies.

Now that you have modified the project configuration, you can rebuild the project at any time by executing the following command from within the root folder of your framework project:

    mvn clean install

Once the command completes you can find your StreamFlow Framework JAR in the `target` directory of your project.


##Framework Configuration

In the previous section you set up a Maven project capable of compiling a StreamFlow Framework JAR.  In order to identify the Spouts and Bolts that are available in your Framework JAR, StreamFlow utilizes a single Framework configuration file.  The framework configuration is integral to building Components that can be used within the StreamFlow UI to dynamically build topologies.  Although the project you created in the last section will compile, the lack of a `framework.yml` configuration file will prevent StreamFlow from registering any Spouts or Bolts within your JAR.  It is important to note that only Spout and Bolt implementations that have been registered in the `framework.yml` file will be visible by the StreamFlow UI.

The framework configuration file is used to register all of the Spouts, Bolts, Resources, and Serializations that are present within the framework jar project.  This configuration can be defined using a YAML format or a JSON format.  Selection of the configuration format (YAML/JSON) is solely a personal preference as all settings are available in each format.  Although either format can be used, YAML is the recommended format as it is less verbose than JSON in regards to formatting.  

The `framework.yml` and `framework.json` configuration files should be located in a `STREAMFLOW-INF` folder at the root of the class path (e.g. `src/main/resources/STREAMFLOW-INF/framework.yml` or `src/main/resources/STREAMFLOW-INF/framework.json`).  The following sample `framework.yml` and `framework.json` files showcase the property and interface configuration for a component.  

> **Note:** The following YAML and JSON configurations are equivalent and you should only define either `framework.yml` OR `framework.json` in your project.

#### Sample framework.yml

#### Sample framework.json





##Component Implementation

##Resource Implementation

##Serialization Implementation

##Framework Logging

##Framework Metrics