Title: Gora MongoDB Module

## Overview
This is the main documentation for the gora-mongodb module. gora-mongodb 
module enables [MongoDB](http://www.mongodb.org) backend support for Gora.

This module has been tested with MongoDB Server [2.4.x](http://docs.mongodb.org/master/release-notes/2.4/)
and [2.6.x](http://docs.mongodb.org/master/release-notes/2.6/) series.
It will connect to remote MongoDB server(s) using standard [Java MongoDB Driver](http://docs.mongodb.org/ecosystem/drivers/java/)

[TOC]

## gora.properties
Here is a following sample <code>gora.properties</code> file to enable MongoStore:

    # MongoDBStore properties
    gora.datastore.default=org.apache.gora.mongodb.store.MongoStore
    gora.mongodb.override_hadoop_configuration=false
    gora.mongodb.mapping.file=/gora-mongodb-mapping.xml
    gora.mongodb.servers=localhost
    gora.mongodb.db=sample

Description of supported properties:

| Property                                   | Example value                            | Required ? | Description                                                                                                                   |
|--------------------------------------------|------------------------------------------|------------|-------------------------------------------------------------------------------------------------------------------------------|
|                     gora.datastore.default | org.apache.gora.mongodb.store.MongoStore |     Yes    | Implementation of the persistent Java storage class for MongoDB                                                               |
| gora.mongodb.override_hadoop_configuration | false                                    |     No     | If true, it will allow properties to be overriden by configuration coming from Hadoop                                         |
|                  gora.mongodb.mapping.file | /gora-mongodb-mapping.xml                |     No     | The XML mapping file to be used. If no value is used this defaults to gora-mongodb-mapping.xml                                |
|                       gora.mongodb.servers | localhost:27017                          |     Yes    | This value should specify the host:port for a running MongoDB node. Multiple values have to be separated by a coma character. |
|                            gora.mongodb.db | mytestdatabase                           |     Yes    | This value should specify the database for storage of documents.                                                              |
|                         gora.mongodb.login | login                                    |     No     | Login that will be used to authenticate against MongoDB server. If blank, driver won't try authentication.                    |
|                        gora.mongodb.secret | password                                 |     No     | Secret that will be used to authenticate against MongoDB server.                                                              |
|           gora.mongodb.authentication.type | SCRAM-SHA-1                              |     No     | Authentication mechanism type that will be used to authenticate against MongoDB server.                                       |

## Gora MongoDB mappings
You should then create a <code>gora-mongodb-mapping.xml</code> which will describe <b>how</b> you want to
store each of your Gora persistent objects:

    <gora-otd>
    
        <class name="org.apache.gora.examples.generated.Employee" keyClass="java.lang.String" document="employees">
            <field name="name" docfield="name" type="string"/>
            <field name="dateOfBirth" docfield="dateOfBirth" type="int64"/>
            <field name="ssn" docfield="ssn" type="string"/>
            <field name="salary" docfield="salary" type="int32"/>
            <field name="boss" docfield="boss" type="document"/>
            <field name="webpage" docfield="webpage" type="document"/>
        </class>
    
    </gora-otd>
    
Each <b>class</b> element specifying persistent fields which values should map to. This element contains; 

1. a parameter containing the Persistent class name e.g. <b>org.apache.gora.examples.generated.Employee</b>, 

2. a parameter containing the keyClass e.g. <b>java.lang.String</b> which specifies the keys which map to the field values, 

3. a parameter containing the MongoDB collection e.g. <b>employees</b> which will be used to persist each Gora object,

4. finally a child element(s) <b>field</b> which represent all fields which are to be persisted into MongoDB.
   These need to be configured such that they receive the following;

    a <b>name</b> attribute e.g. (name, dateOfBirth, ssn and salary respectively) which map to Gora field name, 

    a <b>docfield</b> attribute containing the field's name in mapped Mongo document, 

    a <b>type</b> attribute which allow transformation of Gora types into native MongoDB types.
    MongoDB use [BSON](bsonspec.org) is a binary serialization format to store documents
    and make remote procedure calls. 

    Description of supported <b>type</b> values:

| Type value | Description                     |
|------------|---------------------------------|
| BINARY     | Store as binary data            |
| BOOLEAN    | Store as boolean value          |
| INT32      | Store as signed 32-bit integer  |
| INT64      | Store as signed 64-bit integer  |
| DOUBLE     | Store as floating point         |
| STRING     | Store as UTF-8 string           |
| DATE       | Store as UTC datetime (ISODate) |
| LIST       | Store as Array                  |
| DOCUMENT   | Store as embedded document      |
| OBJECTID   | Store as ObjectId (12-byte)     |
