Title: Gora Module Overview

## Introduction
This is the main entry point for Gora documentation. Here are some pointers for further info:

* First if you haven't already done so, make sure to check the [quick start guide](./quickstart.html).
* Basic information about gora modules can be found below.
* You can also take a look at the [API Documentation](./api/javadoc.html) which contains the javadoc 
  for all of the modules combined. We are always looking for [Documentation contributions](../contribute.html).

You can find an abstract overview of how to configure Gora [here](./gora-conf.html).

## Gora Modules
Gora source code is organized in a modular architecture. The gora-core module 
is the main module which contains the core of the code. All other modules depend 
on the gora-core module. 
Each datastore backend in Gora resides in it's own module. The documentation for 
the specific module can be found at the module's documentation directory. 

It is wise so start with going over the documentation for the gora-core 
module and then the specific data store module(s) you want to use. The 
following modules are currently implemented in gora.

* [gora-compiler](./compiler.html): A page dedicated to the GoraCompiler; a critical part of the Gora workflow.
* [gora-core](./gora-core.html): Module containing core functionality, AvroStore and DataFileAvroStore stores;
* [gora-accumulo](./gora-accumulo.html): Module for [Apache Accumulo](http://accumulo.apache.org) backend and AccumuloStore implementation;
* [gora-cassandra](./gora-cassandra.html): Module for [Apache Cassandra](http://cassandra.apacheorg) backend and CassandraStore implementation;
* [gora-dynamodb](./gora-dynamodb.html): Module for [Amazon DynamoDB](http://aws.amazon.com/dynamodb/) backend and DynamoDBStore implementation;
* [gora-hbase](./gora-hbase.html): Module for [Apache HBase](http://hbase.apache.org) backend and HBaseStore implementation;
* [gora-sql](./gora-sql.html): Module for [HSQLDB](http://hsqldb.org/) and [MySQL](http://www.mysql.com/) backend and SqlStore implementation;
* [gora-mongodb](./gora-mongodb.html): Module for [MongoDB](http://www.mongodb.org/) backend and MongoStore implementation;
* [gora-aerospike](./gora-aerospike.html): Module for [Aerospike](http://www.aerospike.com/) backend and Aerospike implementation;
