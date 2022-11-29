Title: Gora Core Module

# Overview 
This is the main documentation for Gora + Hadoop compatibility which comes in the
form of <b>Gora Shims</b>. 

According to our great friends over at [Wikipedia](https://en.wikipedia.org/wiki/Shim_%28computing%29),
Shim's are described as

    ...a small library that transparently intercepts API calls and changes the arguments passed, handles the operation itself, or redirects the operation elsewhere. Shims typically come about when the behavior of an API changes, thereby causing compatibility issues for older applications which still rely on the older functionality. In such cases, the older API can still be supported by a thin compatibility layer on top of the newer code.

Shims functionality in Gora is provided across the following Gora modules

 * <b>gora-shims-distribution</b>, 
 * <b>gora-shims-hadoop</b>,
 * <b>gora-shims-hadoop1</b>, and 
 * <b>gora-shims-hadoop2</b>

As explained within the [Gora Core](./gora-core.html) module documentation,
every module in Gora depends on [Gora Core](./gora-core.html). 
In turn, [Gora Core](./gora-core.html) depends upon the 
<b>gora-shims-distribution</b> module.

This page therefore describes how to use the Gora Shims layers when building Gora
into your software stack. 

[TOC]

# gora-shims-distribution

## Description

This module provides the structure for using Gora Hadoop shims. The module only defines direct dependencies
on all of the other shims modules but contains no actual Java code itself. 

**This module is the only dependency which needs to be defined for users to take advantage of Gora Shims. The dependency
should be used as below.**

## Dependency Definition

[Click Here](https://search.maven.org/#artifactdetails|org.apache.gora|gora-shims-distribution|0.6|bundle)

# gora-shims-hadoop

## Description

This module contains functional Java code for enabling dynamic selection of the Hadoop environment based upon the 
presence of **org.apache.hadoop.util.VersionInfo**. This is a two-part process where we first obtain the [Hadoop
Major version](https://github.com/apache/gora/blob/master/gora-shims-hadoop/src/main/java/org/apache/gora/shims/hadoop/HadoopShimFactory.java#L79-L94).
We then [select the appropriate Gora Shim](https://github.com/apache/gora/blob/master/gora-shims-hadoop/src/main/java/org/apache/gora/shims/hadoop/HadoopShimFactory.java#L56-L77) 
based on detecting the Hadoop Major version. This code looks the following:


    /**
     * Get the Hadoop major version number.
     *
     * @return The major version number of Hadoop.
     */
    public String getMajorVersion() {
      String vers = VersionInfo.getVersion();
      String[] parts = vers.split("\\.");
      if (parts.length < 2) {
        throw new RuntimeException("Unable to parse Hadoop version: "
        + vers + " (expected X.Y.* format)");
      }
      return parts[0];
    }

    /**
     * Get the Hadoop shim for the Hadoop version on the class path. In case it
     * fails to obtain an appropriate shim (i.e. unsupported Hadoop version), it
     * throws a {@link RuntimeException}.
     *
     * Note that this method is potentially costly.
     *
     * @return A newly created instance of a {@link HadoopShim}.
     */
    public HadoopShim getHadoopShim() {
      String version = getMajorVersion();
      String className = HADOOP_VERSION_TO_IMPL_MAP.get(version);
      try {
        Class<?> class1 = Class.forName(className);
        return HadoopShim.class.cast(class1.newInstance());
      } catch (Exception e) {
        throw new RuntimeException(
          "Could not load Hadoop shim for version " + version
          + ", className=" + className, e);
      }
    }

## Dependency Definition

[Click Here](https://search.maven.org/#artifactdetails|org.apache.gora|gora-shims-hadoop|0.6|bundle)

# gora-shims-hadoop1

## Description

This module provides all functionality for Hadoop 1.X support in Gora. The Hadoop version is specified within
the project [pom.xml](https://github.com/apache/gora/blob/master/pom.xml) within the **${hadoop1-version}** variable.

The actual [code implementation](https://github.com/apache/gora/blob/master/gora-shims-hadoop1/src/main/java/org/apache/gora/shims/hadoop1/HadoopShim1.java) 
is very simple. Essentially it consists of two Java methods

     /**
      * {@inheritDoc}
      */
     public Job createJob(Configuration configuration) throws IOException {
       return new Job(configuration);
     }
     /**
      * {@inheritDoc}
      */
     public JobContext createJobContext(Configuration configuration) {
       return new JobContext(configuration, null); 
     }


## Dependency Definition

[Click Here](https://search.maven.org/#artifactdetails|org.apache.gora|gora-shims-hadoop1|0.6|bundle)


# gora-shims-hadoop2

## Description


This module provides all functionality for Hadoop 2.X support in Gora. The Hadoop version is specified within
the project [pom.xml](https://github.com/apache/gora/blob/master/pom.xml) within the **${hadoop2-version}** variable.

The actual [code implementation](https://github.com/apache/gora/blob/master/gora-shims-hadoop1/src/main/java/org/apache/gora/shims/hadoop1/HadoopShim1.java) 
is very simple. Essentially it consists of two Java methods

    /**
     * {@inheritDoc}
     *
     * Use the Hadoop 2.x way of creating a {@link Job} object.
     */
    public Job createJob(Configuration configuration) throws IOException {
      Job instance = Job.getInstance(configuration);
      return instance;
    }
    /**
     * {@inheritDoc}
     *
     * Use the Hadoop 2.x way of creating a {@link JobContext} object.
     */
    public JobContext createJobContext(Configuration configuration) {
      return new JobContextImpl(configuration, null);
    }

## Dependency Definition

[Click Here](https://search.maven.org/#artifactdetails|org.apache.gora|gora-shims-hadoop2|0.6|bundle)

