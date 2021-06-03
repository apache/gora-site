Title: Gora Cassandra Module

## Overview
This is the main documentation for the gora-cassandra module which
enables [Apache Cassandra](http://cassandra.apache.org) backend support for Gora. 

[TOC]

## gora.properties 

<table class="table">
  <thead>
   <tr>
    <th align="left">Property Key</th>
    <th align="left">Property Value</th> 
    <th align="left">Required</th>
    <th align="left">Description</th>
   </tr>
  </thead>
  <tbody>
   <tr>
    <td>gora.datastore.default=</td>
    <td>org.apache.gora.cassandra.store.CassandraStore</td>
    <td>Yes</td>
    <td>Implementation of the persistent Java storage class</td>
   </tr>
   <tr>
    <td>gora.cassandrastore.mapping.file=</td>
    <td>/path/to/gora-cassandra-mapping.xml</td>
    <td>No</td>
    <td>The XML mapping file to be used. If no value is used this defaults to <code>gora-cassandra-mapping.xml</code></td>
   </tr>
   <tr>
    <td>gora.cassandrastore.cassandraServers=</td>
    <td>localhost</td>
    <td>Yes</td>
    <td>This value should specify the host for a running Cassandra server or node. In this case the server happens to be running on localhost which is the default Cassandra server configuration.</td>
   </tr>
   <tr>
    <td>gora.cassandrastore.port=</td>
    <td>9042</td>
    <td>Yes</td>
    <td>This value should specify the cql port for a running Cassandra server or node. In this case the server happens to be running on 9042 port which is the default Cassandra server configuration.</td>
   </tr>
   <tr>
    <td>gora.cassandrastore.clusterName=</td>
    <td>Test Cluster</td>
    <td>No</td>
    <td>This value should specify the cassandra cluster name for a running Cassandra server or node. In this case the server has configured to run with Cassandra cluster name as 'Test Cluster' which is the default Cassandra server configuration.</td>
   </tr>
   <tr>
    <td>gora.cassandrastore.username=</td>
    <td>${username}</td>
    <td>No</td>
    <td>The authentication details for passing a <b>username</b> to the CassandraHostConfigurator. This will be required if security is required for Cassandra reads and writes.</td>
   </tr>
   <tr>
    <td>gora.cassandrastore.password=</td>
    <td>${password}</td>
    <td>No</td>
    <td>The authentication details for passing a <b>password</b> to the CassandraHostConfigurator. This will be required if security is required for Cassandra reads and writes.</td>
   </tr>
   <tr>
    <td>gora.cassandrastore.cassandraSerializationType=</td>
    <td>AVRO/NATIVE</td>
    <td>No</td>
    <td>The serialization type for persist into the cassandra data store. default value is Native serialization type</td>
   </tr>
  </tbody>
</table>

## Gora Cassandra mappings 
Say we wished to map some CassandraRecord data and store it into the CassandraStore.

    <gora-otd>

        <keyspace name="RecordKeySpace" durableWrite="false">
            <placementStrategy name="SimpleStrategy" replication_factor="1"/>
        </keyspace>

        <class name="org.apache.gora.cassandra.example.generated.AvroSerialization.CassandraRecord"
               keyClass="org.apache.gora.cassandra.example.generated.AvroSerialization.CassandraKey"
               keyspace="RecordKeySpace"
               table="CassandraRecord" allowFiltering="true" id="5a1c395e-b41f-11e5-9f22-ba0be0483c18">
            <field name="name" column="name" type="text"/>
            <field name="dataInt" column="age" type="int"/>
            <field name="salary" column="salary" type="bigint"/>
            <field name="dataDouble" column="testDouble" type="double"/>
            <field name="dataBytes" column="quotes" type="blob"/>
            <field name="arrayInt" column="listInt" type="list(int)"/>
            <field name="arrayString" column="listString" type="list(text)"/>
            <field name="arrayLong" column="listLong" type="list(bigint)"/>
            <field name="arrayDouble" column="listDouble" type="list(double)"/>
            <field name="mapInt" column="mapInt" type="map(text,int)"/>
            <field name="mapString" column="mapString" type="map(text,text)"/>
            <field name="mapLong" column="mapLong" type="map(text,bigint)"/>
            <field name="mapDouble" column="mapDouble" type="map(text,double)"/>
        </class>

        <cassandraKey name="org.apache.gora.cassandra.example.generated.AvroSerialization.CassandraKey">
            <partitionKey>
                <field name="url" column="urlData" type="text"/>
                <field name="timestamp" column="timestampData" type="bigint"/>
            </partitionKey>
            <clusterKey>
                <key column="timestampData" order="desc"/>
            </clusterKey>
        </cassandraKey>

    </gora-otd>


Here you can see that we require the definition of two child elements within the 
<code>gora-otd</code> mapping configuration, namely;

The <b>keyspace</b> element; where we specify: 

1. a parameter containing the Cassandra keyspace schema name e.g. <b>RecordKeySpace</b>, 

2. a parameter containing the durable write enabled property in the Cassandra keyspace e.g. <b>false</b>, More about durable write can be found [here](http://docs.datastax.com/en/cassandra/2.1/cassandra/dml/dml_durability_c.html).

3. the child element <b>placementStrategy</b> containing the Cassandra placementStrategy details, a parameter containing the Cassandra placement strategy name,  e.g. <b>SimpleStrategy</b>, gora-cassandra will use SimpleStrategy by default if no value for this attribute is specified. More about placement strategies can be found [here](http://docs.datastax.com/en/archived/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html).

4. a parameter containing a <b>replicationFactor</b> attribute with value integer. Again the replicationFactor value associated with the Keyspace tag
   will only apply if Gora creates the Keyspace and will have no effect if this is being used against 
   an existing keyspace. the default value for 'replicationFactor' is '1'. 


The <b>class</b> element specifying persistent fields which values should map to. This element contains; 

1. a parameter containing the Persistent class name e.g. <b>org.apache.gora.cassandra.example.generated.AvroSerialization.CassandraRecord</b>, 

2. a parameter containing the keyClass e.g. <b>org.apache.gora.cassandra.example.generated.AvroSerialization.CassandraKey</b> which specifies the keys which map to the field values, 

3. a parameter containing the keyspace e.g. <b>RecordKeySpace</b> which matches to the above keyspace definition,

4. a parameter containing the table name e.g. <b>CassandraRecord</b>,

5. a parameter containing the allow filtering e.g. <b>true</b>, More about allow filtering can be found [here](https://www.datastax.com/dev/blog/allow-filtering-explained-2).

6. a child element(s) <b>field</b> which represent fields which are to be persisted into Cassandra. These need to be configured such that they receive the following;

   finally a parameter <b>name</b> e.g. (name, dateOfBirth, ssn and salary respectively) which map to Gora field name, 

   a parameter containing the column name e.g. <b>name</b>, 

   a parameter containing the data type of the column e.g. <b>text</b>,

   an optional parameter <b>primarykey</b>, which indicates the primary key.


The <b>cassandraKey</b> element specifying composite key fields which is used in keyClass, This is optional, this element should be added only when composite keys are available;	

1. a child element(s) <b>partitionKey</b> which represent cassandra partition key
    
   a child element(s) <b>compositeKey</b> which represent cassandra composite partition key

   a child element(s) <b>field</b> which represent partition key fields which are to be persisted into Cassandra. These need to be configured such that they receive the following;

   a parameter <b>name</b> e.g. (name, dateOfBirth, ssn and salary respectively) which map to Gora field name, 

   a parameter containing the column name e.g. <b>name</b>, 

   a parameter containing the data type of the column <b>text</b>,

2. a child element(s) <b>clusterKey</b> which represent cassandra cluster key

   a child element(s) <b>key</b> which represents column key fields which needs to be add clustered key. 

   a parameter containing the column name e.g. <b>name</b>, 

   a parameter containing the Order type of the column to be applied e.g. <b>desc</b>,

