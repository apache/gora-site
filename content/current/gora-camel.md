Title: Gora Configuration

# Introduction

**Camel-Gora** is an [Apache Camel](https://camel.apache.org/) component that allows you to work with NoSQL databases using the 
[Apache Gora](https://gora.apache.org/) framework. 

**N.B.** Camel-Gora is NOT a Gora module... but instead a Camel one. This documentation exists to provide detail on how 
Gora is being used in different settings.

**Available as of Camel 2.14**

[TOC]

## Maven Configuration

Maven users will need to add the following dependency to their pom.xml for this component:

    <dependency>
        <groupId>org.apache.camel</groupId>
        <artifactId>camel-gora</artifactId>
        <version>x.x.x</version>
        <!-- use the same version as your Camel core version -->
    </dependency>
    

## URI format

    gora:instanceName[?options]

 Hbase examples with mandatory options : 

 *XML*

	<to uri="gora:foobar?keyClass=java.lang.Long&amp;valueClass=org.apache.camel.component.gora.generated.Pageview&amp;dataStoreClass=org.apache.gora.hbase.store.HBaseStore"/>

 *Java DSL*

	to("gora:foobar?keyClass=java.lang.Long&valueClass=org.apache.camel.component.gora.generated.Pageview&dataStoreClass=org.apache.gora.hbase.store.HBaseStore"/>


## Configuratiion

 Using camel-gora needs some configuration. This mainly involve to configure the <code>AvroStore</code> through the <code>gora.properties</code> file and to define the relevant mappings as part of the *[gora-core](https://gora.apache.org/current/gora-core.html)* module.

 Extensive information for this configuration can be found in the apache [gora documentation](./index.html) and the [gora-conf](./gora-conf.html) page. 

## Supported Gora Operations

 Supported operations include : ***put**, **get**, **delete**, **getSchemaName**, **deleteSchema**, **createSchema**, **query**, **deleteByQuery**, **schemaExists***. 

 Some of the operations require arguments while some others no. The arguments to operations could be either the *body* of the *in* message or defined in a header property. Below there is a list with some additional info for each operation.
 
 
<table>
  <tr><th align="left">Property</th><th align="left">Description</th></tr>
   <tr>
	<td><tt><code>put</code></tt> 
	<td>*Inserts the persistent object with the given key.*</td>
   </tr>
   <tr>
	<td><tt><code>get</code></tt> 
	<td>*Returns the object corresponding to the given key fetching all the fields.*</td>
   </tr>
   <tr>
	<td><tt><code>delete</code></tt> 
	<td>*Deletes the object with the given key.*</td>
   </tr>
   <tr>
	<td><tt><code>getSchemaName</code></tt> 
	<td>*Returns the schema name given to this DataStore.*</td>
   </tr>
   <tr>
	<td><tt><code>deleteSchema</code></tt> 
	<td>*Deletes the underlying schema or table (or similar) in the datastore that holds the objects.*</td>
   </tr>
   <tr>
	<td><tt><code>createSchema</code></tt> 
	<td>*Creates the optional schema or table (or similar) in the datastore to hold the objects.*</td>
   </tr>
   <tr>
	<td><tt><code>query</code></tt> 
	<td>*Executes the given query and returns the results.*</td>
   </tr>
   <tr>
	<td><tt><code>deleteByQuery</code></tt> 
	<td>*Deletes all the objects matching the query.*</td>
   </tr>
   <tr>
	<td><tt><code>schemaExists</code></tt> 
	<td>*Returns whether the schema that holds the data exists in the datastore.*</td>
   </tr>
 </table> 


## Options


### Gora Headers

<table>
<tr><th align="left">Property</th><th align="left">Description</th></tr>
   <tr>
	<td><tt><code>GoraOperation</code></tt> 
	<td>*Used in order to define the operation to execute.*</td>
   </tr>
   <tr>
	<tr>
	<td><tt><code>GoraKey</code></tt> 
	<td>*Used in order to define the datum key for the operations need it.*</td>
   </tr>
 </table>

### Gora Configuration attributes

<table>
  <tr><th align="left">Property</th><th align="left">Type</th><th align="left">Description</th></tr>
   <tr>
	<td><tt><code>keyClass</code></tt> 
	</td><td>_String_</td>
	<td>*Key type.* *</td>
   </tr>
   <tr>
	<td><tt><code>valueClass</code></tt> 
	</td><td>_String_</td>
	<td> *Value type.* *</td>
   </tr>
   <tr>
	<td><tt><code>dataStoreClass</code></tt> 
	</td><td>_String_</td>
	<td> *DataStore type* *</td>
   </tr>
   <tr>
	<td><tt><code>hadoopConfiguration</code></tt> 
	</td><td>_Configuration_</td>
	<td> *Hadoop Configuration*</td>
   </tr>
   <tr>
	<td><tt><code>concurrentConsumers</code></tt> 
	</td><td>_int_</td>
	<td> *Concurrent Consumers (used only by consumers).*</td>
   </tr>
   <tr>
	<td><tt><code>flushOnEveryOperation</code></tt> 
	</td><td>_boolean_</td>
	<td> *Flush on every operation (used only by producers).*</td>
   </tr>
</table>

*NOTE: the gora configuration properties marked with asterisk are mandatory* 
   
### Gora Query attributes

<table>
   <tr><th align="left">Property</th><th align="left">Type</th><th align="left">Description</th></tr>
   <tr>
	<td><tt><code>startTime</code></tt> 
	</td><td>_long_</td>
	<td> *Start Time attribute.*</td>
   </tr>
   <tr>
	<td><tt><code>endTime</code></tt> 
	</td><td>_long_</td>
	<td> *End Time attribute.*</td>
   </tr>
   <tr>
	<td><tt><code>timeRangeFrom</code></tt> 
	</td><td>_long_</td>
	<td> *Time Range From attribute.*</td>
   </tr>
   <tr>
	<td><tt><code>timeRangeTo</code></tt> 
	</td><td>_long_</td>
	<td> *Time Range To attribute.*</td>
   </tr>
   <tr>
	<td><tt><code>limit</code></tt> 
	</td><td>_long_</td>
	<td> *Gora Query Limit attribute.*</td>
   </tr>
   <tr>
	<td><tt><code>timestamp</code></tt> 
	</td><td>_long_</td>
	<td> *Timestamp attribute.*</td>
   </tr>
   <tr>
	<td><tt><code>startKey</code></tt> 
	</td><td>_Object_</td>
	<td> *Start Key attribute.*</td>
   </tr>
   <tr>
	<td><tt><code>endKey</code></tt> 
	</td><td>_Object_</td>
	<td> *End Key attribute.*</td>
   </tr>
   <tr>
	<td><tt><code>keyRangeFrom</code></tt> 
	</td><td>_Object_</td>
	<td> *Key Range From attribute.*</td>
   </tr>
   <tr>
	<td><tt><code>keyRangeTo</code></tt> 
	</td><td>_Object_</td>
	<td> *Key Range To attribute.*</td>
   </tr>
   <tr>
	<td><tt><code>fields</code></tt> 
	</td><td>_String_</td>
	<td> *Fields attribute.*</td>
   </tr>
</table>

### Usage examples


**Create Schema** *(XML DSL)*:

	<setHeader headerName="GoraOperation">
	   <constant>CreateSchema</constant>
	</setHeader>
	
	<to uri="gora:foobar?keyClass=java.lang.Long&amp;valueClass=org.apache.camel.component.gora.generated.Pageview&amp;dataStoreClass=org.apache.gora.hbase.store.HBaseStore"/>

**SchemaExists** *(XML DSL)*:

 	  <setHeader headerName="GoraOperation">
      	<constant>SchemaExists</constant>
      </setHeader>
      
      <to uri="gora:foobar?keyClass=java.lang.Long&amp;valueClass=org.apache.camel.component.gora.generated.Pageview&amp;dataStoreClass=org.apache.gora.hbase.store.HBaseStore"/>


**Put** *(XML DSL)*:

	  <setHeader headerName="GoraOperation">
        <constant>put</constant>
      </setHeader>
           
	  <setHeader headerName="GoraKey">
        <constant>22222</constant>
      </setHeader>

      <to uri="gora:foo?keyClass=java.lang.Long&amp;valueClass=org.apache.camel.component.gora.generated.Pageview&amp;dataStoreClass=org.apache.gora.hbase.store.HBaseStore"/>

**Get** *(XML DSL)*:

	<setHeader headerName="GoraOperation">
       <constant>GET</constant>
    </setHeader>
    
    <setHeader headerName="GoraKey">
       <constant>10101</constant>
    </setHeader>
 
    <to uri="gora:bar?keyClass=java.lang.Long&amp;valueClass=org.apache.camel.component.gora.generated.Pageview&amp;dataStoreClass=org.apache.gora.hbase.store.HBaseStore"/>

**Delete** *(XML DSL)*:

	<setHeader headerName="GoraOperation">
      <constant>DELETE</constant>
    </setHeader>
    
	<setHeader headerName="GoraKey">
      <constant>22222</constant>
    </setHeader>

    <to uri="gora:bar?keyClass=java.lang.Long&amp;valueClass=org.apache.camel.component.gora.generated.Pageview&amp;dataStoreClass=org.apache.gora.hbase.store.HBaseStore"/>

**Query** *(XML DSL)*:

	<to uri="gora:foobar?keyClass=java.lang.Long&amp;valueClass=org.apache.camel.component.gora.generated.Pageview&amp;dataStoreClass=org.apache.gora.hbase.store.HBaseStore"/>

The full usage examples in the form of integration tests can be found at [camel-gora-examples](https://github.com/ipolyzos/camel-gora-examples/) repository.

### More resources

For more please information and in depth configuration refer to the [Apache Gora Documentation](./overview.html) and the [Apache Gora Tutorial](./tutorial.html).
