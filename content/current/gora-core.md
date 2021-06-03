Title: Gora Core Module

# Overview 
This is the main documentation for DataStore's contained within the 
<code>gora-core</code> module which (as it's name implies)
holds most of the core functionality for the gora project. 

Every module 
in gora depends on gora-core therefore most of the generic documentation 
about the project is gathered here as well as the documentation for <code>AvroStore</code>, 
<code>DataFileAvroStore</code> and <code>MemStore</code>. In addition to this, gora-core holds all of the 
core **MapReduce**, **GoraSparkEngine**, **Persistency**, **Query**, **DataStoreBase** and **Utility** functionality.

[TOC]

# AvroStore

## Description
AvroStore can be used for binary-compatible Avro serializations. It supports Binary and JSON serializations.

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
    <td>org.apache.gora.avro.store.AvroStore</td>
    <td>Yes</td>
    <td>Implementation of the persistent Java storage class</td>
   </tr>
   <tr>
    <td>gora.avrostore.input.path=</td>
    <td>*hdfs://uri/path/to/hdfs/input/path* || *file:///uri/path/to/local/input/path*</td>
    <td>Yes</td>
    <td>This value should point to the input directory on hdfs (if running Gora in a distributed Hadoop environment) or to some location input directory on the local file system (if running Gora locally).</td>
   </tr>
   <tr>
    <td>gora.avrostore.output.path=</td>
    <td>*hdfs://uri/path/to/hdfs/output/path* || *file:///uri/path/to/local/output/path*</td>
    <td>Yes</td>
    <td>This value should point to the output directory on hdfs (if running Gora in a distributed Hadoop environment) or to some location output location on the local file system (if running Gora locally).</td>
   </tr>
   <tr>
    <td>gora.avrostore.codec.type=</td>
    <td>BINARY || JSON</td>
    <td>No</td>
    <td>The property key specifying avro encoder/decoder type to use. Can take values <code>BINARY</code> or <code>JSON</code> but resolves to BINARY is one is not supplied.</td>
   </tr>
  </tboday>
</table>

## AvroStore XML mappings
In the stores covered within the gora-core module, no physical mappings are required.

# DataFileAvroStore

## Description
DataFileAvroStore is file based store which extends <codeAvroStore</code> to use Avro's <code>DataFile{Writer,Reader}</code>'s as a backend. 
This datastore supports MapReduce.

## gora.properties
DataFileAvroStore would be configured exactly the same as in AvroStore above with the following exception

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
    <td>org.apache.gora.avro.store.DataFileAvroStore</td>
    <td>Yes</td>
    <td>Implementation of the persistent Java storage class</td>
   </tr>
  </tboday>
</table>

## Gora Core mappings
In the stores covered within the gora-core module, no physical mappings are required.

# MemStore

## Description
Essentially this store is a ConcurrentSkipListMap in which operations run as follows
 * put(K key, T Object) - expect average log(n)
 * get(K key, String [] fields) - expect average log(n)
 * delete(K key) - expect average log(n)

## gora.properties
MemStore would be configured exactly the same as in AvroStore above with the following exception

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
    <td>org.apache.gora.memory.store.MemStore</td>
    <td>Yes</td>
    <td>Implementation of the Java class used to hold data in memory</td>
   </tr>
  </tboday>
</table>

## MemStore XML mappings
In the stores covered within the gora-core module, no physical mappings are required.

# GoraSparkEngine
## Description
GoraSparkEngine is Spark backend of Gora. Assume that input and output data stores are:

    DataStore<K1, V1> inStore;
    DataStore<K2, V2> outStore;

First step of using GoraSparkEngine is to initialize it:

    GoraSparkEngine<K1, V1> goraSparkEngine = new GoraSparkEngine<>(K1.class, V1.class);

Construct a `JavaSparkContext`. Register input data store’s value class as Kryo class:

    SparkConf sparkConf = new SparkConf().setAppName("Gora Spark Integration Application").setMaster("local");
    Class[] c = new Class[1];
    c[0] = inStore.getPersistentClass();
    sparkConf.registerKryoClasses(c);
    JavaSparkContext sc = new JavaSparkContext(sparkConf);

JavaPairRDD can be retrieved from input data store:

    JavaPairRDD<Long, Pageview> goraRDD = goraSparkEngine.initialize(sc, inStore);

After that, all Spark functionality can be applied. For example running count can be done as follows:

    long count = goraRDD.count();

Map and Reduce functions can be run on a `JavaPairRDD` as well. Assume that this is the variable after map/reduce is applied:

    JavaPairRDD<String, MetricDatum> mapReducedGoraRdd;

Result can be written as follows:

    Configuration sparkHadoopConf = goraSparkEngine.generateOutputConf(outStore);
    mapReducedGoraRdd.saveAsNewAPIHadoopDataset(sparkHadoopConf);

