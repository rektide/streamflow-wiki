### Overview

StreamFlow frameworks are compiled as a single JAR file which contains all of your Storm component implementations and the `framework.yml` configuration file.  

In order for a framework jar to be compatible with Storm, it is necessary to include additional goals in your maven pom.xml to ensure that all transient dependent libraries are compiled into the jar.  This requirement allows Storm topologies to be completely self-contained when executing on remote clusters.  The Maven Shade plugin is used to accomplish this task which includes all dependent libraries in the framework jar during packaging.  In general, any dependencies in your pom which are not `provided` will be compiled into your framework jar file.  Due to this behavior, it is important to list the storm dependency as a `provided` dependency to avoid compiling the Storm libraries in your framework.  All other dependencies should use the default scope of `compile` to ensure that those dependencies are included to prevent Class not found exceptions at runtime. 

This section will outline the process to build a StreamFlow framework Maven project from scratch.  Before continuing please verify that Maven is correctly installed on your development workstation.

### Framework Project Directory Structure

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

### Generating a Framework Project

Open up a new command line prompt and execute the following command.  

> **Note:** Replace `{project-package}` with the groupId you would like to use for the new project.  Maven will also use this to generate the default package structure for your project.  Replace `{project-name}` with the artifactId you would would like to use for your project.  Finally, replace `{project-version}` with the version you would like to use for your project.

    mvn archetype:generate -DgroupId={project-package} \
       -DartifactId={project-name} \
       -Dversion={project-version} \
       -DarchetypeArtifactId=maven-archetype-quickstart \
       -DinteractiveMode=false

If successful, you will find the your new Maven project folder in your current directory using the `{project-name}` you provided in the command.  While the Maven archetype does a good job of creating the base project structure, there are a few changes we will need to make to ensure the project is a valid StreamFlow framework.

### Updating the Project Configuration

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


### Building the Project

Now that you have modified the project configuration, you can rebuild the project at any time by executing the following command from within the root folder of your framework project:

    mvn clean install

Once the command completes you can find your StreamFlow Framework JAR in the `target` directory of your project.  

At this point you should have a properly configured project.  Please continue to the [Framework Configuration](Framework-Configuration) section to learn how to configure your `framework.yml` file to expose Spouts, Bolts, Resources, and Serializations.
