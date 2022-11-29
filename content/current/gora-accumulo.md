Title: Gora Accumulo Module

## Overview
This is the main documentation for the gora-accumulo module which
enables [Apache Accumulo](https://accumulo.apache.org) backend support for Gora. 

[TOC]

## gora.properties 
* <code>gora.datastore.default</code>=org.apache.gora.accumulo.store.AccumuloStore - Implementation of the storage class 
* <code>gora.accumulo.mapping.file</code>=gora-accumulo-mapping.xml                - The XML mapping file to be used 
* <code>gora.datastore.accumulo.mock</code>=true                                   - Mock Accumulo supplies mock implementations for much of the client API. It presently does not enforce users, logins, permissions, etc. It does support Iterators and Combiners. Note that MockAccumulo holds all data in memory, and will not retain any data or settings between runs. 
* <code>gora.datastore.accumulo.instance</code>=a14                                - An identifier for the Accumulo instance
* <code>gora.datastore.accumulo.zookeepers</code>=localhost                        - This value should specify the host:port for a running Zookeeper server or node. In this case the server happens to be running on localhost which is the default server configuration.
* <code>gora.datastore.accumulo.user</code>=root                                   - This relates to the name of the client which will be communicating with Accumulo. This is also used for authentication purposes.
* <code>gora.datastore.accumulo.password</code>=secret                             - This relates to the password of the client which will be communicating with Accumulo. This is also used for authentication purposes.

## Gora Accumulo mappings 
Say we wished to map some Employee data and store it into the AccumuloStore.

    <gora-otd>
      <table name="Employee">
        <family name="info" />
        <config key="table.file.compress.blocksize" value="32K"/>
      </table>

      <class name="org.apache.gora.examples.generated.Employee" keyClass="java.lang.String" table="Employee" 
          encoder="org.apache.gora.accumulo.encoders.BinaryEncoder">
        <field name="name" family="info" qualifier="nm"/>
        <field name="dateOfBirth" family="info" qualifier="db"/>
        <field name="ssn" family="info" qualifier="sn"/>
        <field name="salary" family="info" qualifier="sl"/>
        <field name="boss" family="info" qualifier="bs"/>
        <field name="webpage" family="info" qualifier="wp"/>
      </class>
    </gora-otd>

Here you can see that we require the definition of two child elements within the <code>gora-otd</code> mapping configuration, namely;

The **table** element; where we specify:

1. a parameter relating to the Accumulo table name (String) e.g. name="Employee",

2. a nested element containing the type and definition of any families we wish to create within Accumulo. In this case we create one family *info* which could have a combination of any of the following parameters;
  
   a **name** (String): family name e.g. info

   a **config** (key:value): which is a typical key/value-type configuration for Accumulo runtime configuration. A fully comprehensive list of options can be found [here](https://accumulo.apache.org/1.5/accumulo_user_manual.html#_table_configuration)   

The **class** element where we specify of persistent fields which values should map to. This contains;

1. a parameter containing the Persistent class **name** e.g. <code>org.apache.gora.examples.generated.Employee</code>,

2. a parameter containing the **keyClass** e.g. <code>java.lang.String</code> which specifies the keys which map to the field values,

3. a parameter containing the Table **name** e.g. <code>Employee</code> which matches to the above Table definition,

4. a parameter containing the **Encoder** to be used e.g. <code>org.apache.gora.accumulo.encoders.BinaryEncoder</code> which defines an appropriate [encoder](https://github.com/apache/gora/tree/master/gora-accumulo/src/main/java/org/apache/gora/accumulo/encoders) for for object field values. 

5. finally nested child element(s) mapping fields which are to be persisted into Accumulo. These fields need to be configured such that they receive;

   a parameter containing the **name** e.g. (name, dateOfBirth, ssn and salary respectively),

   a parameter containing the column **family** to which they belong e.g. (all info in this case),

   an optional parameter **qualifier**, which enables more granular control over the data to be persisted into HBase.
