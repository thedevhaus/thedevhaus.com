---
title: "Init Java Groovy Project Thru Gradle"
date: 2018-02-02T13:46:51+11:00
draft: false
weight: 10
#topics: ["Build Tools"]
#description: "Basic Tutorial"
tags : ["java", "groovy", "gradle"]
---


## Initialize a Java or Groovy project using gradle

Gradle does not have full fledged support for generating skeleton project structure (maven archetype). Although there is some support available for some default java/scala/groovy and related libraries.

The ```gradle init``` plugin supports various basic formats for generating the project structure. For example:

Here's an example:

```
# Create Java application directory structure
gradle init --type java-application

# This will generate basic project structure for a java command line application project with the standard defaults and will use JUnit as test framework.

gradle init --type java-library

# This will generate basic project structure for a java library project (JAR) with the standard defaults and will use JUnit as test framework.

Similarly the following types are supported:

gradle init --type groovy-library
gradle init --type scala-library

# The test framework by default is JUnit but can be overridden to use spock or testng. For example

gradle init --type groovy-library  --test-framework spock

# The mix and match of Java/Groovy can be used by using <code>groovy-library</code>

```

