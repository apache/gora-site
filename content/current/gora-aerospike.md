Title: Gora Aerospike Module

## Overview
This is the main documentation for the gora-aerospike module. <b>gora-aerospike</b> module enables [Aerospike](https://aerospike.com/) backend support for Gora.

[TOC] 

## gora.properties 

* <code>gora.datastore.default=org.apache.gora.aerospike.store.AerospikeStore</code> - Implementation of the persistent Java storage class for Aerospike
* <code>gora.aerospikestore.server.ip=localhost</code> - Property pointing to the host where the server is running
* <code>gora.aerospikestore.server.port=3000</code> - Property pointing to the port where the server is running
* <code>gora.datastore.mapping.file=gora-aerospike-mapping.xml</code> -  The XML mapping file to be used. If no value is used this defaults to gora-aerospike-mapping.xml
* <code>gora.aerospikestore.server.username=user_name</code> - An optional property defining the username of the server if available
* <code>gora.aerospikestore.server.password=password</code> - An optional property defining the password of the server if available
 
## Gora Aerospike mappings
You should then create a gora-aerospike-mapping.xml which will describe how you want to store each of your Gora persistent objects along with the read and write policies in Aerospike:

    <gora-otd>
		<policy name="write" gen="NONE" recordExists="UPDATE" commitLevel="COMMIT_ALL" durableDelete="false"/>
  		<policy name="read" priority="DEFAULT" consistencyLevel="CONSISTENCY_ONE" replica="SEQUENCE" maxRetries="2"/>

		<class name="org.apache.gora.examples.generated.Employee" keyClass="java.lang.String" set="Employee" namespace = "test">
			<field name="name" bin="name"/>
			<field name="dateOfBirth" bin="dateOfBirth"/>
			<field name="ssn" bin="ssn"/>
			<field name="salary" bin="salary"/>
			<field name="boss" bin="boss"/>
			<field name="webpage" bin="webpage"/>
		</class>		
    </gora-otd>		

Here you can see that we require the definition of child elements within the <code>gora-otd</code> mapping configuration. We can define the classes and the policies.

Each <b>class</b> element should contain the following elements; 

1. a parameter defining the Persistent class name e.g. <b>org.apache.gora.examples.generated.Employee</b>, 

2. a parameter defining the keyClass e.g. <b>java.lang.String</b> which specifies the key which maps to the field values, 

3. a parameter defining the Aerospike set e.g. <b>Employee</b> which will be used to persist each Gora object,

4. a parameter defining the Aerospike namespace e.g. <b>test</b> which will be used to persist each Gora object,

In addition, within the class field we should specify the fields and for which bin each field value maps to. We do not need to explicitly specify the type of each field, as the type is automatically detected in Aerospike server when creating the bin values. Thus each <b>field</b> should contain the field name and the corresponding bin it gets mapped to.  e.g. <b> name="webpage" bin="webpage" </b>

Further, we can define the policies on reading and writing data from/to the server.

Write policy can have following fields and each field values are the default values supported by [Aerospike Write Policy API](https://www.aerospike.com/apidocs/java/com/aerospike/client/policy/WritePolicy.html)

1. gen - [generation policy](https://www.aerospike.com/apidocs/java/com/aerospike/client/policy/GenerationPolicy.html) (values: EXPECT_GEN_EQUAL, EXPECT_GEN_GT, NONE) 

2. recordExists - [record exists action](https://www.aerospike.com/apidocs/java/com/aerospike/client/policy/RecordExistsAction.html) (values: CREATE_ONLY, REPLACE, REPLACE_ONLY, UPDATE, UPDATE_ONLY)

3. commitLevel - [commit level](https://www.aerospike.com/apidocs/java/com/aerospike/client/policy/CommitLevel.html) (values: COMMIT_ALL, COMMIT_MASTER) 

4. durableDelete - [durable delete](https://www.aerospike.com/apidocs/java/com/aerospike/client/policy/WritePolicy.html#durableDelete) (values: true, false) 

5. expiration - [record expiration](https://www.aerospike.com/apidocs/java/com/aerospike/client/policy/WritePolicy.html#expiration) (values: 0, 10) 

Read policy can have following fields and each field values are the default values supported by [Aerospike Read Policy API](https://www.aerospike.com/apidocs/java/com/aerospike/client/policy/Policy.html)

1. priority - [priority policy](https://www.aerospike.com/apidocs/java/com/aerospike/client/policy/Priority.html) (values: DEFAULT, HIGH, LOW, MEDIUM) 

2. consistencyLevel - [consistency level](https://www.aerospike.com/apidocs/java/com/aerospike/client/policy/ConsistencyLevel.html) (values: CONSISTENCY_ALL, CONSISTENCY_ONE)

3. replica - [replica](https://www.aerospike.com/apidocs/java/com/aerospike/client/policy/Replica.html) (values: MASTER, MASTER_PROLES, RANDOM, SEQUENCE) 

4. socketTimeout - [socket timeout](https://www.aerospike.com/apidocs/java/com/aerospike/client/policy/Policy.html#socketTimeout) (values: timeout in milliseconds) 

5. totalTimeout - [total timeout](https://www.aerospike.com/apidocs/java/com/aerospike/client/policy/Policy.html#totalTimeout) (values: timeout in milliseconds) 

6. timeoutDelay - [timeout delay](https://www.aerospike.com/apidocs/java/com/aerospike/client/policy/Policy.html#timeoutDelay) (values: timeout in milliseconds) 

7. maxRetries - [max retries](https://www.aerospike.com/apidocs/java/com/aerospike/client/policy/Policy.html#maxRetries) (values: int of max num of retries) 
