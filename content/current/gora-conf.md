Title: Gora Configuration

## gora.properties

Gora reads necessary configuration from a properties file name 
<code>gora.properties</code>. 

The file is searched in the classpath, which is 
obtained using the <code>ClassLoader</code> of the [DataStoreFactory](http://gora.apache.org/current/api/apidocs-0.4/index.html?org/apache/gora/store/DataStoreFactory.html)
 class.

The following properties are recognized:

## Common Properties

<table>
  <tr><th align="left">Property</th> <th align="left">Required</th> <th align="left">Default</th> <th align="left">Explanation</th></tr>
  <tr><td><code>gora.datastore.default</code></td><td>No</td> <td> – </td> <td>The full classname of the default data store implementation to use </td></tr>
  <tr><td><code>gora.datastore.autocreateschema</code></td><td>No</td><td>true</td><td>Whether to create schemas automatically</td></tr>
</table>

<code>gora.datastore.default</code> is perhaps the most important property in this file. 
This property configures the default [DataStore](http://gora.apache.org/current/api/apidocs-0.4/index.html?org/apache/gora/store/DataStore.html) implementation to use. 
However, other data stores can still be instantiated thorough the API. 
Data store implementation in Gora distribution include:

<table>
  <caption>DataStore implementations</caption>
  <tr><th align="left">DataStore Implementation</th> <th align="left">Full Class Name</th> <th align="left">Module Name</th> <th align="left">Explanation</th></tr>
  <tr><td><b>AvroStore</b></td> <td><code>org.apache.gora.avro.store.AvroStore</code></td> <td>gora-core</td> <td>An adapter DataStore for binary-compatible Apache Avro serializations. AvroDataStore supports Binary and JSON serializations. </td></tr>
  <tr><td><b>DataFileAvroStore</b></td> <td><code>org.apache.gora.avro.store.DataFileAvroStore</code></td> <td>gora-core</td> <td>DataFileAvroStore is file based store which uses Avro's DataFile{Writer,Reader}'s as a backend. This datastore supports mapreduce.</td></tr>
  <tr><td><b>AccumuloStore</b></td> <td><code>org.apache.gora.accumulo.store.AccumuloStore</code></td> <td>gora-accumulo</td> <td> DataStore for Apache Accumulo. </td></tr>
  <tr><td><b>HBaseStore</b></td> <td><code>org.apache.gora.hbase.store.HBaseStore</code></td> <td>gora-hbase</td> <td> DataStore for Apache HBase. </td></tr>
  <tr><td><b>CassandraStore</b></td> <td><code>org.apache.gora.cassandra.store.CasssandraStore</code></td> <td>gora-cassandra</td> <td> DataStore for Apache Cassandra. </td></tr>
  <tr><td><b>SolrStore</b></td> <td><code>org.apache.gora.solr.store.SolrStore</code></td> <td>gora-solr</td> <td> A DataStore implementation for Apache Solr.</td></tr>
  <tr><td><b>MemStore</b></td> <td><code>org.apache.gora.memory.store.MemStore</td> <td>gora-core</td> <td> Memory based DataStore implementation for tests. </td></tr>
  <tr><td><b>Dynamodb</b></td> <td><code>org.apache.gora.dynamodb.store.DyanmoDBStore</td> <td>gora-dynamodb</td> <td> Webservices-based datastore implementation for Amazon's DynamoDB. </td></tr>
  <tr><td><b>MongoStore</b></td> <td><code>org.apache.gora.mongodb.store.MongoStore</td> <td>gora-mongodb</td> <td> A DataStore implementation for MongoDB document storage. </td></tr>
</table> 

Some of the properties can be customized per datastore. The format of these 
properties is as follows: <code>gora.&lt;data_store_class&gt;.&lt;property_name&gt;</code>.
 
Note that <code>&lt;data_store_class&gt;</code> is the classname of the datastore 
implementation w/o the package name, for example <code>HbaseStore</code>. 
You can also use the string datastore instead of the specific 
data store class name, in which case, the property setting is effective 
to all data stores. The following properties can be set per data store.

## Per DataStore Properties

<table>
  <caption>DataStore Properties</caption>
  <tr><th align="left">Property</th> <th align="left">Required</th> <th align="left">Default Value</th> <th align="left">Explanation</th></tr>
  <tr><td><code>gora.&lt;data_store_class&gt;.autocreateschema</code> No true Whether to create schemas automatically for the specific data store</td></tr>
  <tr><td><code>gora.&lt;data_store_class&gt;.mapping.file</code> No gora-{accumulo|hbase|cassandra|sql|dynamodb}-mapping.xml The name of the mapping file</td></tr>
</table>

## Data store specific settings
Other than the properties above, some of the data stores have their 
own configurations. These properties are listed at the module documentations:

* [Gora Core Module](./gora-core.html) (incl. AvroStore, DataFileAvroStore and MemStore)
* [Gora HBase Module](./gora-hbase.html)
* [Gora Cassandra Module](./gora-cassandra.html)
* [Gora Solr Module](./gora-solr.html)
* [Gora Accumulo Module](./gora-accumulo.html)
* [Gora DynamoDB Module](./gora-dynamodb.html)
* [Gora MongoDB Module](./gora-mongodb.html)

