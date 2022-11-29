Title: About Apache Gora&trade;

# About Gora

[TOC]

## Why Gora?
Although there are various excellent ORM frameworks such as JPA, Apache OpenJPA, Hibernate, etc for relational databases, data modeling in 
NoSQL data stores differ profoundly from their relational cousins. Moreover, data-model agnostic 
frameworks such as JDO are not sufficient for use cases, where one needs to use the full power 
of the data models in column stores (for example). Gora fills this gap by giving the user an easy-to-use in-memory 
data model and persistence for big data framework with data store specific mappings and built 
in [Apache Hadoop&trade;](https://hadoop.apache.org) support.

The overall goal for Gora is to become the standard data representation and persistence framework 
for big data. The roadmap of Gora can be grouped as follows:

* Data Persistence : Persisting objects to Column stores such as [Apache HBase&trade;](https://hbase.apache.org), 
  [Apache Cassandra&trade;](https://cassandra.apache.org), [Hypertable](https://hypertable.org/); 
  key-value stores such as [Voldermort](https://www.project-voldemort.com/voldemort), [Redis](https://redis.io/), 
  etc; SQL databases, such as [MySQL](https://www.mysql.com/), [HSQLDB](https://hsqldb.org/), flat files 
  in local file system of [Hadoop HDFS](https://hadoop.apache.org/docs/stable/hdfs_user_guide.html);
* Data Access : An easy to use Java-friendly common API for accessing the data regardless of its location;
* Indexing : Persisting objects to [Apache Lucene](https://lucene.apache.org) and 
  [Apache Solr](https://lucene.apache.org/solr) indexes, accessing/querying the data with Gora API;
* Analysis : Accesing the data and making analysis through adapters for [Apache Pig](https://pig.apache.org), 
  [Apache Hive](https://hive.apache.org) and [Cascading](https://www.cascading.org/);
* MapReduce support : Out-of-the-box and extensive 
  [MapReduce](https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html) ([Apache Hadoop&trade;](https://hadoop.apache.org)) 
  support for data in the data store.

## Who is Gora For?

Gora is a framework primarily aimed towards

* <b>Hands on Developers</b> required to deal with data volumes which justify Big Data storage solutions classified under
  the NoSQL umbrella.
* Developers who seek a <b>Java friendly (REST-style) API</b> for mapping Java objects to and from
  NoSQL technologies.
* <b>Development and/or Testing Engineers</b> looking to quickly set up and deploy applications on top of
  Big Data storage mediums. This includes testing how applications are suited to underlying data stores
  as data stores are easily interchanged. 
* Developers interested in <b>technology agnostic storage methods</b> for addressing data storage tasks.
* <b>Decision Makers</b> looking to implement a flexible storage framework under the [most liberal
  open source license available](https://www.apache.org/licenses/LICENSE-2.0).

## Background
<b>ORM</b> stands for [Object Relation Mapping](https://en.wikipedia.org/wiki/Object-relational_mapping). It is a technology which abstacts the persistency layer 
(mostly Relational Databases) so that plain domain level objects can be used, without the cumbersome 
effort to save/load the data to and from the database. 

Gora extends this concept to introduce <b>Object-to-Datastore Mapping</b> where the underlying 
technological implementations rely mostly on non-relational data modeling. In essence
Gora provides storage abstraction for NoSQL technologies.

Gora differs from current solutions in that:

* Gora is specially focussed at NoSQL data stores, but also has limited support for SQL databases.
* The main use case for Gora is to access/analyze big data using [Apache Hadoop&trade;](https://hadoop.apache.org).
* Gora uses [Apache Avro](https://avro.apache.org) for bean definition, not byte code enhancement or annotations.
* Object-to-data store mappings are backend specific, so that full data model can be utilized.
* Gora is simple since it ignores complex SQL mappings.
* Gora will support persistence, indexing and anaysis of data, using [Apache Pig](https://pig.apache.org), 
  [Apache Lucene](https://lucene.apache.org), [Apache Hive](https://hive.apache.org), etc.

## What Platform(s) does Gora work on?
Gora [builds nightly](https://builds.apache.org/view/All/job/gora-trunk/) on Ubuntu. 

The software has been tested and verified to run on the following platforms:

* Mac OSX 10.9.3
* Linux Mint
* Ubuntu

Gora does publish <b>.zip</b> artifacts for Windows users, however there is no gurantee
of platform compatibility.

Please provide platform compatibility issues and/or feedback to our [mailing lists](./mailing_lists.html).

## Which Languages/Technologies do I need to know to use Gora?

* Gora is written in Java. 
* Configuration however requires a working knowledge of syntax for JSON and XML.
* You should be able to use the command line terminal.
* You should be able to use Apache Maven from the command line.
* You should be able to edit simple flat files using a text editor.
