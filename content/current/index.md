Title: Gora Module Overview

## Introduction

[TOC] 

This is the main entry point for Gora documentation. Here are some pointers for further info:

* First if you haven't already done so, make sure to check the [quick start guide](./quickstart.html).
* Basic information about gora modules can be found below.
* You can also take a look at the [API Documentation](./api/javadoc.html) which contains the javadoc 
  for all of the modules combined. 
* We are always looking for [Documentation contributions](../contribute.html).

You can find an abstract overview of how to configure Gora [here](./gora-conf.html).

## Gora Modules
Gora source code is organized in a modular architecture. The gora-core module 
is the main module which contains the core of the code. All other modules depend 
on the gora-core module. 
Each datastore backend in Gora resides in it's own module. The documentation for 
the specific module can be found at the module's documentation directory. 

It is wise so start with going over the documentation for the gora-core 
module and then the specific data store module(s) you want to use. The 
following modules are currently implemented in Gora.

* [gora-compiler](./compiler.html): A page dedicated to the GoraCompiler; a critical part of the Gora workflow;
* [gora-compiler-cli](./compiler-cli.html): A page dedicated to the GoraCompiler Command Line Interface; a utility module for working with the Gora Compiler;
* [gora-core](./gora-core.html): Module containing core functionality, AvroStore and DataFileAvroStore stores, GoraSparkEngine;
* [gora-accumulo](./gora-accumulo.html): Module for [Apache Accumulo](http://accumulo.apache.org) backend and AccumuloStore implementation;
* [camel-gora](./gora-camel.html): An [Apache Camel](http://camel.apache.org/) component that allows you to work with NoSQL databases using Gora;
* [gora-cassandra](./gora-cassandra.html): Module for [Apache Cassandra](http://cassandra.apacheorg) backend and CassandraStore implementation;
* [gora-dynamodb](./gora-dynamodb.html): Module for [Amazon DynamoDB](http://aws.amazon.com/dynamodb/) backend and DynamoDBStore implementation;
* [gora-hbase](./gora-hbase.html): Module for [Apache HBase](http://hbase.apache.org) backend and HBaseStore implementation;
* [gora-jcache](./gora-jcache.html): Module for [Hazelcast JCache](https://hazelcast.com/use-cases/caching/jcache-provider) caching and JCacheStore implementation;
* [gora-couchdb](./gora-couchdb.html): Module for [Apache CouchDB](http://couchdb.apache.org) backend and CouchDBStore implementation;
* [gora-metamodel](./gora-metamodel.html): Module for [Apache MetaModel](http://metamodel.incubator.apache.org) backend and query functionality;
* [gora-mongodb](./gora-mongodb.html): Module for [MongoDB](http://www.mongodb.org/) backend and MongoStore implementation;
* [gora-solr](./gora-solr.html): Module for [Apache Solr](http://lucene.apache.org/solr) backend and SolrStore implementation;
* [gora-aerospike](./gora-aerospike.html): Module for [Aerospike](http://www.aerospike.com/) backend and Aerospike implementation;
* [gora-ignite](./gora-ignite.html): Module for [Apache Ignite](https://ignite.apache.org/) backend and IgniteStore implementation;
* [gora-kudu](./gora-kudu.html): Module for [Apache Kudu](https://kudu.apache.org/) backend and KuduStore implementation;
* [gora-pig](./gora-pig.html): Module for loading/writing using Apache Gora in an [Apache Pig](https://pig.apache.org/) script;
* [gora-tutorial](./tutorial.html): The Gora LogManager tutorial;
* gora-sources-dist: Packaging module used to build and distribute Gora sources during project releases;

We currently have modules under development for several other storage mediums such 
as [Oracle NoSQL](http://www.oracle.com/technetwork/database/database-technologies/nosqldb/overview/index.html) 
and [Apache Lucene](http://lucene.apache.org). Consult the Gora source, located on [Github](https://github.com/apache/gora/)
for a complete list of modules.

## Gora Testing
Gora currently has two testing mechanisms
 * JUnit Tests: These are included for every module which provides a DataStore within Gora.
 * Integration Tests: A custom testing suite called GoraCI (Continuous Ingestion) which stress tests Gora functionality at scale.

### JUnit Tests
Unit tests in Gora are implemented using the popular [JUnit](http://junit.org) framework.
Each module which implements the [DataStore](https://builds.apache.org/view/All/job/gora-trunk/javadoc/index.html?org/apache/gora/store/DataStore.html)
interface similarly implements a [DataStoreTestBase](https://github.com/apache/gora/blob/master/gora-core/src/test/java/org/apache/gora/store/DataStoreTestBase.java) API
which test utilities for DataStores. The DataStoreTestBase class delegates actual test execution
to [DataStoreTestUtil](https://github.com/apache/gora/blob/master/gora-core/src/test/java/org/apache/gora/store/DataStoreTestUtil.java). 

The tests begin in a fairly trivial fashion testing functionality like datastore schema creation 
schema deletion, etc and continue in this manner getting progressively more complex 
as we begin testing some more advanced features within the Gora API. 
In addition to the unit tests contained within this class, the best place to look for
API functionality is at the examples directories under various Gora modules. Most 
modules contain a <code>/src/examples/</code> directory under which some example 
classes can be found. Specifically, there are some classes that are used for tests 
under [gora-core/src/examples/](https://github.com/apache/gora/tree/master/gora-core/src/examples).

### GoraCI Integration Testsing Suite

#### Background
Since Gora 0.5, the GoraCI suite has been part of the mainstream Gora codebase.

Credit for GoraCI can be handed to Keith Turner (Gora PMC member) for his foresight
in developing GoraCI which we have now extended from gora-accumulo to the entire suite
of Gora modules.

[Apache Accumulo](http://accumulo.apache.org) has a test suite that verifies that data is not lost
at scale.  This test suite is called 
[continuous ingest](http://svn.apache.org/viewvc/accumulo/tags/1.4.0/test/system/continuous/ScaleTest.odp?view=co).  
Essentially the test runs many ingest clients that continually create linked lists containing **25 million**
nodes. At some point the clients are stopped and a map reduce job is run to
ensure no linked list has a hole. A hole indicates data was lost.    

The nodes in the linked list are random.  This causes each linked list to
spread across the table.  Therefore if one part of a table loses data, then it
will be detected by references in another part of the table.

This project is a version of the test suite written using Apache Gora [1].
Goraci has been tested against Accumulo and HBase.  

#### The Anatomy of GoraCI tests

Below is rough sketch of how data is written.  For specific details look at the
[Generator code](https://github.com/apache/gora/blob/master/gora-goraci/src/main/java/org/apache/gora/goraci/Generator.java)

  1. Write out 1 million nodes 
  2. Flush the client 
  3. Write out 1 million that reference previous million 
  4. If this is the 25th set of 1 million nodes, then update 1st set of million
     to point to last 
  5. goto 1

The key is that nodes only reference flushed nodes.  Therefore a node should
never reference a missing node, even if the ingest client is killed at any
point in time.

When running this test suite w/ Accumulo there is a script running in parallel
called the Aggitator that randomly and continuously kills server processes.  
The outcome was that many data loss bugs were found in Accumulo by doing this. 
This test suite can also help find bugs that impact uptime and stability when 
run for days or weeks.  

This test suite consists the following 

 * a few Java programs 
 * a little helper script to run the java programs
 * a maven script to build it.  

When generating data, its best to have each map task generate a multiple of 25
million.  The reason for this is that circular linked list are generated every
25M.  Not generating a multiple in 25M will result in some nodes in the linked
list not having references.  The loss of an unreferenced node can not be
detected.

#### Building GoraCI

As GoraCI is packaged with the Gora master branch source it is automatically 
built every time you execute

    mvn install

The maven pom file has some profiles that attempt to make it easier to run
GoraCI against different Gora backends by copying the jars you need into <code>lib</code>.
Before packaging its important to edit <code>gora.properties</code> and set it correctly
for your datastore.  To run against Accumulo do the following.

    vim src/main/resources/gora.properties //set Accumulo properties
    mvn package -Paccumulo-1.4

To run against HBase, do the following.

    vim src/main/resources/gora.properties //set HBase properties
    mvn package -Phbase-0.92

To run against Cassandra, do the following.

    vim src/main/resources/gora.properties //set Cassandra properties
    mvn package -Pcassandra-1.1.2

For other datastores mentioned in <code>gora.properties</code>, you will need to copy the
appropriate deps into <code>lib</code>.  Feel free to update the pom with other profiles, [open
a ticket](https://issues.apache.org/jira/browse/GORA/) or just [send us a pull request](https://github.com/apache/gora/).

#### Java Class Description

Below is a description of the Java programs

  * [org.apache.gora.goraci.Generator](https://github.com/apache/gora/blob/master/gora-goraci/src/main/java/org/apache/gora/goraci/Generator.java) -
    A map only job that generates data.  As stated previously, its best to generate data in multiples of 25M.
  * [org.apache.gora.goraci.Verify](https://github.com/apache/gora/blob/master/gora-goraci/src/main/java/org/apache/gora/goraci/Verify.java)    -
    A map reduce job that looks for holes.  Look at the counts after running.  REFERENCED and UNREFERENCED are 
    ok, any UNDEFINED counts are bad. Do not run at the same time as the Generator.
  * [org.apache.gora.goraci.Walker](https://github.com/apache/gora/blob/master/gora-goraci/src/main/java/org/apache/gora/goraci/Walker.java)    -
    A standalong program that start following a linked list and emits timing info.
  * [org.apache.gora.goraci.Print](https://github.com/apache/gora/blob/master/gora-goraci/src/main/java/org/apache/gora/goraci/Print.java)     -
    A standalone program that prints nodes in the linked list
  * [org.apache.gora.goraci.Delete](https://github.com/apache/gora/blob/master/gora-goraci/src/main/java/org/apache/gora/goraci/Delete.java)    -
    A standalone program that deletes a single node
  * [org.apache.gora.goraci.Loop](https://github.com/apache/gora/blob/master/gora-goraci/src/main/java/org/apache/gora/goraci/Loop.java)      -
    Runs generation and verify in a loop

[goraci.sh](https://github.com/apache/gora/blob/master/gora-goraci/goraci.sh) is a helper script that you can use to run the above programs.  It
assumes all needed jars are in the <code>lib</code> dir.  It does not need the package name.
You can just run <code>goraci.sh Generator</code>, below is an example.

    $ ./goraci.sh Generator
    Usage : Generator <num mappers> <num nodes>

For Gora to work, it needs a <code>gora.properties</code> file on the classpath and a
<code>gora-$datastore-mapping.xml</code> mapping file on the classpath, the contents of both are datastore specific,
more details can be found here [2]. You can edit the ones in src/main/resources
and build the <code>goraci-${version}-SNAPSHOT.jar</code> with those. Alternatively remove
those and put them on the classpath through some other means.

#### Gora and Hadoop
Gora uses [Apache Avro](http://avro.apache.org) which uses a Json library that Hadoop has an old version of.
The two libraries  jackson-core and jackson-mapper need to be updated in
<code>$HADOOP_HOME/lib</code> and <code>$HADOOP_HOME/share/hadoop/lib/</code>.  Currently these are updated to
jackson-core-asl-1.4.2.jar and jackson-mapper-asl-1.4.2.jar.  For details see
[HADOOP-6945](https://issues.apache.org/jira/browse/HADOOP-6945). 

#### GoraCI and HBase
To improve performance running read jobs such as the Verify step, enable
scanner caching on the command line.  For example:

    $ ./gorachi.sh Verify -Dhbase.client.scanner.caching=1000 \
       -Dmapred.map.tasks.speculative.execution=false verify_dir 1000

Dependent on how you have your Hadoop and HBase setup deployed, you may need to
change the <code>gorachi.sh</code> script around some.  Here is one suggestion that may help
in the case where your Hadoop and HBase configuration are other than under the
Hadoop and HBase home directories.

    diff --git a/org.apache.gora.goraci.sh b/org.apache.gora.goraci.sh
    index db1562a..31c3c94 100755
    --- a/org.apache.gora.goraci.sh
    +++ b/org.apache.gora.goraci.sh
    @@ -95,6 +95,4 @@ done
     #run it
     export HADOOP_CLASSPATH="$CLASSPATH"
     LIBJARS=`echo $HADOOP_CLASSPATH | tr : ,`
     -hadoop jar "$GORACI_HOME/lib/org.apache.gora.goraci-0.0.1-SNAPSHOT.jar" $CLASS -libjars "$LIBJARS" "$@"
     -
     -
     +CLASSPATH="${HBASE_CONF_DIR}" hadoop --config "${HADOOP_CONF_DIR} jar "$GORACI_HOME/lib/org.apache.gora.goraci-0.0.1-SNAPSHOT.jar" $CLASS -files "${HBASE_CONF_DIR}/hbase-site.xml" -libjars "$LIBJARS" "$@"

You will need to define <code>HBASE_CONF_DIR</code> and </code>HADOOP_CONF_DIR</code> before you run your
**goraci** jobs.  For example:

    $ export HADOOP_CONF_DIR=/home/you/hadoop-conf
    $ export HBASE_CONF_DIR=/home/you/hbase-conf
    $ PATH=/home/you/hadoop-1.0.2/bin:$PATH ./goraci.sh Generator 1000 1000000

#### Concurrency

Its possible to run verification at the same time as generation.  To do this
supply the -c option to Generator and Verify.  This will cause Genertor to
create a secondary table which holds information about what verification can
safely verify.  Running Verify with the **-c** option will make it run slower
because more information must be brought back to the client side for filtering
purposes.  The Loop program also supports the -c option, which will cause it to
run verification concurrently with generation.

If verification is run at the same time as generation without the **-c** option,
then it will inevitably fail.  This is because verification mappers read
different parts of the table at different times and giving an inconsistent view
of the table.  So one mapper may read a part of a table before a node is
written, when the node is later referenced it will appear to be missing.  The
**-c** option basically filters out newer information using data written to the
secondary table.

#### Conclusions

This test suite does not do everything that the Accumulo test suite does,
mainly it does not collect statistics and generate reports.  The reports
are useful for assesing performance.

Below shows running a test of the test.  Ingest one linked list, deleted a node
in it, ensure the verifaction map reduce job notices that the node is missing.
Not all output is shown, just the important parts.

    $ ./goraci.sh Generator  1 25000000
    $ ./goraci.sh Print -s 2000000000000000 -l 1
      2000001f65dbd238:30350f9ae6f6e8f7:000004265852:ef09f9dd-75b1-4c16-9f14-0fa84f3029b6
    $ ./goraci.sh Print -s 30350f9ae6f6e8f7 -l 1
      30350f9ae6f6e8f7:4867fe03de6ea6c8:000003265852:ef09f9dd-75b1-4c16-9f14-0fa84f3029b6
    $ ./goraci.sh Delete 30350f9ae6f6e8f7
      Delete returned true
    $ ./goraci.sh Verify gci_verify_1 2 
      11/12/20 17:12:31 INFO mapred.JobClient:   org.apache.gora.goraci.Verify$Counts
      11/12/20 17:12:31 INFO mapred.JobClient:     UNDEFINED=1
      11/12/20 17:12:31 INFO mapred.JobClient:     REFERENCED=24999998
      11/12/20 17:12:31 INFO mapred.JobClient:     UNREFERENCED=1
    $ hadoop fs -cat gci_verify_1/part\* 30350f9ae6f6e8f7	2000001f65dbd238

The map reduce job found the one undefined node and gave the node that
referenced it.

Below are some timing statistics for running Goraci on a 10 node cluster. 

Store           | Task                   | Time    | Undef  | Unref | Ref        
----------------+------------------------+---------+--------+-------+------------
accumulo-1.4.0  | Generator 10 100000000 | 40m 16s |    N/A |   N/A |        N/A     
accumulo-1.4.0  | Verify /tmp/goraci1 40 |  6m  7s |      0 |     0 | 1000000000  
hbase-0.92.1    | Generator 10 100000000 |  2h 44m |    N/A |   N/A |        N/A     
hbase-0.92.1    | Verify /tmp/goraci2 40 |  6m 34s |      0 |     0 | 1000000000

HBase and Accumulo are configured differently out-of-the-box.  We used the Accumulo 
3G, native configuration examples in the [conf/examples](https://github.com/apache/gora/tree/master/gora-goraci/src/main/resources) directory.

To provide a comparable memory footprint, we increased the HBase jvm to "-Xmx4000m", 
and turned on compression for the ci table:

    create 'ci', {NAME=>'meta', COMPRESSION=>'GZ'}

We also turned down the replication of write-ahead logs to be comparable to Accumulo:

    <property>
      <name>hbase.regionserver.hlog.replication</name>
      <value>2</value>
    </property>

For the accumulo run, we set the split threshold to 512M:

    shell> config -t ci -s table.split.threshold=512M

This was done so that Accumulo would end up with 64 tablets, which is the
number of regions HBase had. The number of tablets/regions determines how
much parallelism there is in the map phase of the verify step.

Sometimes when this test suite is run against HBase data is lost.  This issue
is being tracked under [HBASE-5754](https://issues.apache.org/jira/browse/HBASE-5754)
