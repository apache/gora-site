Title: Gora Pig Module

## Overview

This is the main documentation for the gora-pig module. gora-pig module enables loading/storing data through Apache Gora in Pig scripts.

[TOC]

## Introduction

Apache Pig is a platform for analyzing large data sets that consists of a high-level language for expressing data analysis programs. With the module gora-pig we allow to operate on the data from Pig scripts using Apache Gora as storage.
The objective of this document is to describe the approach taken to implement a Pig adapter for Gora and show how to load/store the data.

Warning: Not all Gora modules are adapted to be used under Pig, since they have to implement loading the mapping defined from gora properties with the key "gora.mapping". At this moment are adapted **gora-hbase** and **gora-kudu**.


### Data models

Apache Gora is an Object Datastore Mapper which has its own data model inheriting Avro data types, and Apache Pig has its own data model. Because of this, it is needed an adaptation between both data models.

The following tables shows the different types and a possible conversions between Gora and Pig types.

#### Primitive/Simple types

|Gora	|Pig|
|-------|-----|
|null| null|
|boolean|boolean|
|int (32-bit)|int (32-bit)|
|long (64-bit)|long (64-bit)|
|float (32-bit)|float (32-bit)|
|double (64-bit)|double (64-bit)|
|bytes (8-bit)|bytearray|
|string (unicode)|chararray (string UTF-8)|
|-|datetime|
|-|biginteger|
|-|bigdecimal|

#### Complex types

|Gora	|Pig|
|-------|-----|
|record|tuple|
|enum|int|
|array|bag|
|map<String, 'b>|map<chararray, 'b>|
|union|[the non-null type]|
|fixed|-|

Since `datetime`, `biginteger` and `bigdecimal` aren't handled by Apache Gora, it isn't possible to persist those types.

For unions, only nullable fields (`union:[null, type]`) are handled. Fixed type is not handled.

Notice that Gora's records are converted into Pig's tuples, and arrays into bags (index matters). When persisting, those types are the expected when checking the schemas.

##Reading from datastores

The storage GoraStorage is the responsible for loading and persisting entities. The simplest syntax to load data is the following:

        register gora/*.jar;
        webpage = LOAD '.' USING org.apache.gora.pig.GoraStorage('{
          "persistentClass": "admin.WebPage",
          "fields": "baseUrl,status,content"
        }') ;

It loads the fields `baseUrl`, `status` and `content` **(must not have spaces!)** for the entity `WebPage`.

The files `gora.properties`, `gora-xxx-mapping.xml` and support files are provided through the classpath to Pig client. They must be included inside one of the registered `*.jar` files.

The complete `LOAD` options allows to configure the options for each storage and avoid using the global configuration files when multiple different stores are used:


        webpage = LOAD '.' USING org.apache.gora.pig.GoraStorage('{
          "persistentClass": "admin.WebPage",
          "keyClass": "java.lang.String",
          "fields": "*",
          "goraProperties": "",
          "mapping": "",
          "configuration": {}
        }') ;


### Full options for LOAD

The configuration options are the following:

  - **persistentClass** (mandatory): The full name of the persistent class including the namespace.
  - **keyClass**: The full name of the key class. **By now only `java.lang.String` is supported**.
  - **fields** (mandatory): Comma-separated list of field names (without spaces!) or '*' to load all fields.
  - **goraProperties**: String with gora.properties configuration. Each line must be separated by \\n.
  - **mapping**: XML mapping for the entities loaded. Each line must be separated by \\n and escaped quotes as \\"
  - **configuration**: object with a map from keys to values that will be added to the configuration.

In JSON Strings, line feeds must be escaped as \\n.

An example of Gora properties value is:

        "gora.datastore.default=org.apache.gora.hbase.store.HBaseStore\\ngora.datastore.autocreateschema=true\\ngora.hbasestore.scanner.caching=4"

An example of mapping is:

        "<?xml version=\\"1.0\\" encoding=\\"UTF-8\\"?>\\n<gora-odm>\\n<table name=\\"webpage\\">\\n<family name=\\"f\\" maxVersions=\\"1\\"/>\\n</table>\\n<class table=\\"webpage\\" keyClass=\\"java.lang.String\\" name=\\"admin.WebPage\\">\\n<field name=\\"baseUrl\\" family=\\"f\\" qualifier=\\"bas\\"/>\\n<field name=\\"status\\" family=\\"f\\" qualifier=\\"st\\"/>\\n<field name=\\"content\\" family=\\"f\\" qualifier=\\"cnt\\"/>\\n</class>\\n</gora-odm>"

The configuration options is a JSON object with string key-values like this:

        {
          "hbase.zookeeper.quorum": "hdp4,hdp1,hdp3",
          "zookeeper.znode.parent": "/hbase-unsecure"
        }

## Writing to datastores

To write a Pig relation to a datastore, the command is:

        STORE webpages INTO '.' USING org.apache.gora.pig.GoraStorage('{
          "persistentClass": "",
          "fields": "",
          "goraProperties": "",
          "mapping": "",
          "configuration": {}
        }') ;

All the fields listed in "fields" will be persisted. If a field listed is missing in the relation the process will fail with an exception. Only the fields listed will be updated if the element already exists.

## Deleting elements

To delete elements of a collection is `GoraDeleteStorage`. Given a relation with schema `(key:chararray)` rows, the following will delete all rows with that keys:

        STORE webpages INTO '.' USING org.apache.gora.pig.GoraDeleteStorage('{
          "persistentClass": "",
          "goraProperties": "",
          "mapping": "",
          "configuration": {}
        }') ;
