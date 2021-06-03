Title: Quick Start

Introduction
=============

This is a quick start guide to help you setup the project.

[TOC]

Download
--------

First you need to check out the most stable Gora release through the official 
Apache Gora [release page](../downloads.html).  
For those who would like to use a development version Gora or simply wish to 
work with the bleeding edge, instructions for how to check out the source 
code using svn or git can be found on the [version control](../version_control.html) documentation. 

Setting up your project
-----------------------

More recently Gora began using Maven to manage it's dependencies and build lifecycle. 
Stable Gora releases are available on the central maven repository or ivy repositories 
and Gora-SNAPSHOT OSGi bundle artifacts are now pushed to  
<a href="https://repository.apache.org/index.html#nexus-search;quick~gora">Apache Nexus</a>.</p>

Compiling and Installing the project
---------------------

If you have the source code for Gora, you can install the project using

    $ cd gora 
    $ mvn clean install

You can also install individual modules by cd'ing to the module directory and running

    $ mvn clean install

If you want to use Gora as a dependency, you can manage it in a few ways. 

Using ivy to manage Gora
------------------------

If your project already uses ivy, then you can include gora dependencies
to your ivy by adding the following lines to your ivy.xml file: 

      <dependency org="org.apache.gora" name="gora-core" rev="${version}" conf="*->compile" changing="true">
      <dependency org="org.apache.gora" name="gora-dynamodb" rev="${version}" conf="*->compile" changing="true">
      <dependency org="org.apache.gora" name="gora-hbase" rev="${version}" conf="*->compile" changing="true">      
      ...etc

Note: The ${version} variable should be replaced by the most stable Gora release.
    
Only add the modules that you will use, and set the conf to point to the 
configurations (of your project) that you want to depend on Gora. The 
changing="true" attribute states that, Gora artifacts 
should not be cached, which is required if you want to change Gora's 
source and use the recompiled version.

Add the following to your ivysettings.xml

    <resolvers>
      ...
      <chain name="internal">
        <resolver ref="local">
      </chain>
      ...
    </resolvers>
    <modules>
      ...
      <module organisation="org.apache.gora" name=".*" resolver="internal">
      ...
    </modules>

This forces Gora to be built locally rather than look for it in other repositories.

Using Maven to manage Gora
--------------------------

If your project however uses maven, then you can include Gora dependencies
to your project by adding the following lines to your pom.xml file: 


	<dependency>
  		<groupId>org.apache.gora</groupId>
  		<artifactId>gora-core</artifactId>
  		<version>${version}</version>
	 </dependency>

	<dependency>
  		<groupId>org.apache.gora</groupId>
  		<artifactId>gora-accumulo</artifactId>
  		<version>${version}</version>
	</dependency>
    
	...etc

N.B. The ${version} variable should be replaced by the most stable Gora release.
    
Again, only add the modules that you will use.

Specifying Gora SNAPSHOT dependencies
-------------------------------------

If you want to depend on Gora development snapshots, e.g. to get access to recent bug fixes, 
you should add the following to your pom.xml:

    <repository>
      <id>apache-repo-snapshots</id>
      <url>https://repository.apache.org/content/repositories/snapshots/</url>
      <releases>
        <enabled>false</enabled>
      </releases>
      <snapshots>
        <enabled>true</enabled>
      </snapshots>
    </repository>


Managing Gora Jars Manually
---------------------------

You can include Gora jars manually, if you prefer so. After installing Gora 
first and generating the desired artifacts, copy all the jars in gora-[modulename]/lib/ 
and gora-[modulename]/target/gora-${modulename}.jar dir's to your desired 
location. Finally copy all the jars in gora-core/lib/ since all of the 
modules depend on gora-core. 

What's Next?
------------

After setting up Gora, you might want to check out the documentation. 
Most of the current documentation is linked to from the [overview](./index.html)
or is available on the [wiki](https://cwiki.apache.org/confluence/display/GORA/Index). 
