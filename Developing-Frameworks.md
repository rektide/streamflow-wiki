## Overview

Frameworks contain all of the business logic for StreamFlow.  A StreamFlow Framework is simply a JAR file which contains a framework configuration file and implementations of Storm Spouts and Bolts.  The framework configuration (`framework.yml`) is a simple YAML file which registers each Spout and Bolt and any properties that associated with them.  The StreamFlow UI utilizes these registered Spouts and Bolts to allow users to dynamically create new topologies in a drag and drop interface.

This section describes the process to develop Spouts and Bolts for Storm that are compatible with StreamFlow. A major goal of StreamFlow is to you to import your existing Spout and Bolt implementations without requiring you to import custom StreamFlow base classes.  This goal was meant to allow users to easily transition their existing code to StreamFlow without the need to heavily refactor their code.  StreamFlow does *NOT* require extension of custom base classes which allows you to utilize your existing Spouts and Bolts with minimal or no code changes required.

While your code does not need to import custom Storm libraries, your Spouts and Bolts can benefit from the dynamic property injection system.  This injection system allows a Storm component to expose a set of configurable properties which can be edited in the StreamFlow UI and automatically injected into the component at runtime.  These properties allow Storm components to be more flexible and interoperable.

The following sections will help you get started building your first StreamFlow framework and explain in detail all of the options available to you.  Follow the various links below to learn more about configuring and implementing your own StreamFlow Framework.

## Project Setup
See [Framework Project Setup](Framework-Project-Setup) to learn how to generate and configure your Framework project.

## Framework Configuration
See [Framework Configuration](Framework-Configuration) to learn how to register Spouts, Bolts, Resources, and Serializations within your Framework project.

## Component Implementation
See [Component Implementation](Component-Implementation) to learn how to modify your Spouts and Bolts to take advantage of StreamFlow specific features.

## Resource Implementation
See [Resource Implementation](Resource-Implementation) to learn how to build Resources to simplify your configuration and reuse commonly used logic.

## Serialization Implementation
See [Serialization-Implementation](Serialization-Implementation) to learn how to register your Kryo serializations so they can be loaded by Storm at runtime.

## Framework Logging
See [Framework Logging](Framework-Logging) to learn how to configure and use the StreamFlow SLF4J logger in your Spouts and Bolts.

## Framework Metrics
See [Framework Metrics](Framework-Metrics) to learn how to report and view metrics using Yammer metrics in your Spouts and Bolts.