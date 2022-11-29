Title: Gora Tutorial

# Gora Tutorial

Author : Enis Söztutar, enis [at] apache [dot] org

## Introduction
This is the official tutorial for Apache Gora. For this tutorial, we 
will be implementing a system to store our web server logs in Apache HBase,
and analyze the results using Apache Hadoop and store the results either in HSQLDB or MySQL.

In this tutorial we will first look at how to set up the environment and 
configure Gora and the data stores. Later, we will go over the data we will use and
define the data beans that will be used to interact with the persistency layer. 
Next, we will go over the API of Gora to do some basic tasks such as storing objects, 
fetching and querying objects, and deleting objects. Last, we will go over an example 
program which uses Hadoop MapReduce to analyze the web server logs, and discuss the Gora 
MapReduce API in some detail.

## Table of Content
[TOC]

## Introduction to Gora
The Apache Gora open source framework provides an in-memory data 
model and persistence for big data. Gora supports persisting to 
column stores, key value stores, document stores and RDBMSs, and 
analyzing the data with extensive Apache Hadoop MapReduce support. In Avro, the 
beans to hold the data and RPC interfaces are defined using a JSON 
schema. In mapping the data beans to data store specific settings, 
Gora depends on mapping files, which are specific to each data store. 
Unlike other OTD (Object-to-Datastore) mapping implementations, in Gora the data bean to data store 
specific schema mapping is explicit. This has the advantage that, 
when using data models such as HBase and Cassandra, you can always 
know how the values are persisted.
      
Gora has a modular architecture. Most of the data stores in Gora, 
has it's own module, such as gora-hbase, gora-cassandra,
and gora-sql. In your projects, you need to only include 
the artifacts from the modules you use. You can consult the [quick start](/current/quickstart.html)
for setting up your project.

## Setting up Gora
As a first step, we need to download and compile the Gora source code. The source codes 
for the tutorial is in the gora-tutorial module. If you have
already downloaded Gora, that's cool, otherwise, please go
over the steps at the [quickstart](/current/quickstart.html) guide for
how to download and compile Gora.

Now, after the source code for Gora is at hand, let's have a look at the files under the 
directory gora-tutorial. 
    
    $ cd gora-tutorial
    $ tree
    
    |-- conf
    |   |-- gora-hbase-mapping.xml
    |   |-- gora-sql-mapping.xml
    |   `-- gora.properties
    |
    |-- pom.xml
    | 
    `-- src
        |-- examples
        |   `-- java
        |-- main
        |   |-- avro
        |   |   |-- metricdatum.json
        |   |   `-- pageview.json
        |   |-- java
        |   |   `-- org
        |   |       `-- apache
        |   |           `-- gora
        |   |               `-- tutorial
        |   |                   `-- log
        |   |                       |-- KeyValueWritable.java
        |   |                       |-- LogAnalytics.java
        |   |                       |-- LogAnalyticsSpark.java
        |   |                       |-- LogManager.java
        |   |                       |-- TextLong.java
        |   |                       `-- generated
        |   |                           |-- MetricDatum.java
        |   |                           `-- Pageview.java
        |   `-- resources
        |       `-- access.log.tar.gz
        `-- test
            |-- conf
            `-- java

Since gora-tutorial is a top level module of Gora, it depends on the directory
structure imposed by Gora's main build scripts (pom.xml for Maven). The Java source code resides in directory 
<code>src/main/java/</code>, avro schemas in <code>src/main/avro/</code>, and data in <code>src/main/resources/</code>.

## Setting up HBase
For this tutorial we will be using [HBase](https://hbase.apache.org) to 
store the logs. For those of you not familiar with HBase, it is a NoSQL
column store with an architecture very similar to Google's BigTable.

If you don't already have already HBase setup, you can go over the steps at 
[HBase Overview](https://hbase.apache.org/book/quickstart.html)
documentation. Gora aims to support the most recent HBase versions however if you
find compatibility problems please [get in touch](../mailing_lists.html).
So download an [HBase release](https://www.apache.org/dyn/closer.cgi/hbase/). 
After extracting the file, cd to the hbase-${dist} directory and start the HBase server. 
 
    $ bin/start-hbase.sh

and make sure that HBase is available by using the Hbase shell. 

    $ bin/hbase shell

## Configuring Gora
Gora is configured through a file in the classpath named gora.properties. 
We will be using the following file <code>gora-tutorial/conf/gora.properties</code>
      
      gora.datastore.default=org.apache.gora.hbase.store.HBaseStore
      gora.datastore.autocreateschema=true
      
This file states that the default store will be HBaseStore,
and schemas(tables) should be automatically created.
More information for configuring different settings in <code>gora.properties</code> 
can be found [here](/current/gora-conf.html).

## Modeling the data
For this tutorial, we will be parsing and storing the logs of a web server. 
Some example logs are at <code>src/main/resources/access.log.tar.gz</code>, which 
belongs to the (now shutdown) server at http://www.buldinle.com/. 
Example logs contain 10,000 lines, between dates 2009/03/10 - 2009/03/15.
The first thing, we need to do is to extract the logs.

    $ tar zxvf src/main/resources/access.log.tar.gz -C src/main/resources/

You can also use your own log files, given that the log 
format is [Combined Log Format](https://httpd.apache.org/docs/current/logs.html).
Some example lines from the log are:

    88.254.190.73 - - [10/Mar/2009:20:40:26 +0200] "GET / HTTP/1.1" 200 43 "http://www.buldinle.com/" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; GTB5; .NET CLR 2.0.50727; InfoPath.2)
    78.179.56.27 - - [11/Mar/2009:00:07:40 +0200] "GET /index.php?i=3&amp;a=1__6x39kovbji8&amp;k=3750105 HTTP/1.1" 200 43 "http://www.buldinle.com/index.php?i=3&amp;a=1__6X39Kovbji8&amp;k=3750105" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 2.0.50727; OfficeLiveConnector.1.3; OfficeLivePatch.0.0)
    78.163.99.14 - - [12/Mar/2009:18:18:25 +0200] "GET /index.php?a=3__x7l72c&amp;k=4476881 HTTP/1.1" 200 43 "http://www.buldinle.com/index.php?a=3__x7l72c&amp;k=4476881" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; InfoPath.1)

The first fields in order are: User's ip, ignored, ignored, Date and 
time, HTTP method, URL, HTTP Method, HTTP status code, Number of bytes 
returned, Referrer, and User Agent.

## Defining data beans
Data beans are the main way to hold the data in memory and persist in Gora. Gora 
needs to explicitly keep track of the status of the data in memory, so 
we use [Apache Avro](https://avro.apache.org) for defining the beans. Using 
Avro gives us the possibility to explicitly keep track of an object's persistent state 
and a way to serialize an object's data. 
Defining data beans is a very easy task, but for the exact syntax please
consult the [Avro Specification](https://avro.apache.org/docs/current/spec.html).
First, we need to define the bean Pageview to hold a
single URL access in the logs. Let's go over the class at <code>src/main/avro/pageview.json</code>

     {
      "type": "record",
      "name": "Pageview",
      "namespace": "org.apache.gora.tutorial.log.generated",
      "fields" : [
        {"name": "url", "type": "string"},
        {"name": "timestamp", "type": "long"},
        {"name": "ip", "type": "string"},
        {"name": "httpMethod", "type": "string"},
        {"name": "httpStatusCode", "type": "int"},
        {"name": "responseSize", "type": "int"},
        {"name": "referrer", "type": "string"},
        {"name": "userAgent", "type": "string"}
      ]
    }

Avro schemas are declared in JSON. 
[Records](https://avro.apache.org/docs/current/spec.html#schema_record)
are defined with type "record", with a name as the name of the class, and a 
namespace which is mapped to the package name in Java. The fields 
are listed in the "fields" element. Each field is given with its type. 
    
## Compiling Avro Schemas
The next step after defining the data beans is to compile the schemas 
into Java classes. For that we will use the [GoraCompiler](/current/compiler.html). 
Invoke the Gora compiler from the top-level Gora directory with:

    $ bin/gora goracompiler

results in:

    $ Usage: GoraCompiler <schema file> <output dir> [-license <id>]
       <schema file>     - individual avsc file to be compiled or a directory path containing avsc files
       <output dir>      - output directory for generated Java files
       [-license <id>]   - the preferred license header to add to the
		           generated Java file. Current options include; 
		  ASLv2   (Apache Software License v2.0) 
		  AGPLv3  (GNU Affero General Public License)
		  CDDLv1  (Common Development and Distribution License v1.0)
		  FDLv13  (GNU Free Documentation License v1.3)
		  GPLv1   (GNU General Public License v1.0)
		  GPLv2   (GNU General Public License v2.0)
		  GPLv3   (GNU General Public License v3.0)
 		  LGPLv21 (GNU Lesser General Public License v2.1)
		  LGPLv3  (GNU Lesser General Public License v2.1)


so we will issue :

    $ bin/gora goracompiler gora-tutorial/src/main/avro/pageview.json gora-tutorial/src/main/java/

to compile the Pageview class into <code>gora-tutorial/src/main/java/org/apache/gora/tutorial/log/generated/Pageview.java</code>. 
This will use the default license header which is ASLv2 for licensing the generated data beans.
However, the tutorial java classes are already committed and present within SVN, so you do not need to do that now.
    
The Gora compiler extends Avro's SpecificCompiler to convert a JSON definition 
into a Java class. Generated classes extend the Persistent interface. 
Most of the methods of the Persistent interface deal with bookkeeping for 
persistence and state tracking, so most of the time they are not used explicitly by the
user. Now, let's look at the internals of the generated class Pageview.java.

    public class Pageview extends PersistentBase {

    private Utf8 url;
    private long timestamp;
    private Utf8 ip;
    private Utf8 httpMethod;
    private int httpStatusCode;
    private int responseSize;
    private Utf8 referrer;
    private Utf8 userAgent;

    ...

    public static final Schema _SCHEMA = Schema.parse("{\"type\":\"record\", ... ");
      public static enum Field {
      URL(0,"url"),
      TIMESTAMP(1,"timestamp"),
      IP(2,"ip"),
      HTTP_METHOD(3,"httpMethod"),
      HTTP_STATUS_CODE(4,"httpStatusCode"),
      RESPONSE_SIZE(5,"responseSize"),
      REFERRER(6,"referrer"),
      USER_AGENT(7,"userAgent"),
      ;
      private int index;
      private String name;
      Field(int index, String name) {this.index=index;this.name=name;}
      public int getIndex() {return index;}
      public String getName() {return name;}
      public String toString() {return name;}
      };
    public static final String[] _ALL_FIELDS = {"url","timestamp","ip","httpMethod"
      ,"httpStatusCode","responseSize","referrer","userAgent",};
  
    ...
    }

We can see the actual field declarations in the class. Note that Avro uses Utf8 
class as a placeholder for string fields. We can also see the embedded Avro 
Schema declaration and an inner enum named Field. The enum and 
the _ALL_FIELDS fields will come in handy when we query the datastore for specific fields.  

## Defining data store mappings
Gora is designed to flexibly work with various types of data modeling, 
including column stores(such as HBase, Cassandra, etc), SQL databases, flat files(binary, 
JSON, XML encoded), and key-value stores. The mapping between the data bean and 
the data store is thus defined in XML mapping files. Each data store has its own 
mapping format, so that data-store specific settings can be leveraged more easily.
The mapping files declare how the fields of the classes declared in Avro schemas 
are serialized and persisted to the data store.
    
### HBase mappings
HBase mappings are stored at file named <code>gora-hbase-mapping.xml</code>. 
For this tutorial we will be using the file <code>gora-tutorial/conf/gora-hbase-mapping.xml</code>.
      
    <gora-otd>
      <table name="Pageview"> <!-- optional descriptors for tables -->
        <family name="common"> <!-- This can also have params like compression, bloom filters -->
        <family name="http"/>
        <family name="misc"/>
      </table>

      <class name="org.apache.gora.tutorial.log.generated.Pageview" keyClass="java.lang.Long" table="Pageview">
       <field name="url" family="common" qualifier="url"/>
       <field name="timestamp" family="common" qualifier="timestamp"/>
       <field name="ip" family="common" qualifier="ip" />
       <field name="httpMethod" family="http" qualifier="httpMethod"/>
       <field name="httpStatusCode" family="http" qualifier="httpStatusCode"/>
       <field name="responseSize" family="http" qualifier="responseSize"/>
       <field name="referrer" family="misc" qualifier="referrer"/>
       <field name="userAgent" family="misc" qualifier="userAgent"/>
      </class>
  
      ...
  
    </gora-otd>  

Every mapping file starts with the top level element <code>&lt;gora-otd&gt;</code>. 
Gora HBase mapping files can have two type of child elements, table and 
class declarations. All of the table and class definitions should be 
listed at this level.
      
The table declaration is optional and most of the time, Gora infers the table 
declaration from the class sub elements. However, some of the HBase 
specific table configuration such as compression, blockCache, etc can be given here, 
if Gora is used to auto-create the tables. The exact syntax for the file can be found 
[here](/current/gora-hbase.html).
      
In Gora, data store access is always 
done in a key-value data model, since most of the target backends support this model.
DataStore API expects to know the class names of the key and persistent classes, so that 
they can be instantiated. The key value pair is declared in the class element.
The name attribute is the fully qualified name of the class, 
and the keyClass attribute is the fully qualified class name of the key class.
      
Children of the <code>class</code> element are <code>field</code> 
elements. Each field element has a name and family attribute, and 
an optional qualifier attribute. name attribute contains the name 
of the field in the persistent class, and family declares the column family 
of the HBase data model. If the qualifier is not given, the name of the field is used 
as the column qualifier. Note that map and array type fields are stored in unique column 
families, so the configuration should be list unique column families for each map and 
array type, and no qualifier should be given. The exact data model is discussed further 
at the [gora-hbase](/current/gora-hbase.html) documentation. 

## Basic API

### Parsing the logs
Now that we have the basic setup, we can see Gora API in action. As you can notice below the API 
is pretty simple to use. We will be using the class LogManager (which is located at
<code>gora-tutorial/src/main/java/org/apache/gora/tutorial/log/LogManager.java</code>) for parsing 
and storing the logs, deleting some lines and querying. 

First of all, let us look at the constructor. The only real thing it does is to call the 
init() method. init() method constructs the 
DataStore instance so that it can be used by the LogManager's methods.

      public LogManager() {
        try {
         init();
        } catch (IOException ex) {
        throw new RuntimeException(ex);
        }
      }
      private void init() throws IOException {
        dataStore = DataStoreFactory.getDataStore(Long.class, Pageview.class, new Configuration());
      }

DataStore is probably the most important class in the Gora API. 
DataStore handles actual object persistence. Objects can be persisted, 
fetched, queried or deleted by the DataStore methods. Every data store that Gora supports, defines its own subclass 
of the DataStore class. For example gora-hbase module defines HBaseStore, and 
gora-sql module defines SqlStore. However, these subclasses are not explicitly 
used by the user.

DataStores always have associated key and value(persistent) classes. Key class is the class of the keys of the 
data store, and the value is the actual data bean's class. The value class is almost always generated by 
Avro schema definitions using the Gora compiler.

Data store objects are created by DataStoreFactory. It is necessary to 
provide the key and value class. The datastore class is optional, 
and if not specified it will be read from the configuration (<code>gora.properties</code>).

For this tutorial, we have already defined the avro schema to use and compiled
our data bean into Pageview class. For keys in the data store, we will be using Longs. 
The keys will hold the line of the pageview in the data file.

Next, let's look at the main function of the LogManager class.

    public static void main(String[] args) throws Exception {
      if(args.length > 2) {
        System.err.println(USAGE);
        System.exit(1);
      }
    
      LogManager manager = new LogManager();
    
      if("-parse".equals(args[0])) {
        manager.parse(args[1]);
      } else if("-query".equals(args[0])) {
      if(args.length == 2) 
        manager.query(Long.parseLong(args[1]));
      else 
        manager.query(Long.parseLong(args[1]), Long.parseLong(args[2]));
      } else if("-delete".equals(args[0])) {
        manager.delete(Long.parseLong(args[1]));
      } else if("-deleteByQuery".equalsIgnoreCase(args[0])) {
        manager.deleteByQuery(Long.parseLong(args[1]), Long.parseLong(args[2]));
      } else {
        System.err.println(USAGE);
        System.exit(1);
      }
    
      manager.close();
    }

We can use the example log manager program from the command line (in the top level Gora directory): 

    $ bin/gora logmanager 

which lists the usage as:

    LogManager -parse <input_log_file>
           -get <lineNum>
           -query <lineNum>
           -query <startLineNum> <endLineNum>
           -delete <lineNum>
           -deleteByQuery <startLineNum> <endLineNum>

So to parse and store our logs located at <code>gora-tutorial/src/main/resources/access.log</code>, we will issue:

    $ bin/gora logmanager -parse gora-tutorial/src/main/resources/access.log

This should output something like:

    10/09/30 18:30:17 INFO log.LogManager: Parsing file:gora-tutorial/src/main/resources/access.log
    10/09/30 18:30:23 INFO log.LogManager: finished parsing file. Total number of log lines:10000

Now, let's look at the code which parses the data and stores the logs.

    private void parse(String input) throws IOException, ParseException {
      BufferedReader reader = new BufferedReader(new FileReader(input));
      long lineCount = 0;
      try {
        String line = reader.readLine();
        do {
          Pageview pageview = parseLine(line);
        
          if(pageview != null) {
            //store the pageview 
            storePageview(lineCount++, pageview);
          }
        
          line = reader.readLine();
        } while(line != null);
      
      } finally {
      reader.close();  
      }
    }

The file is iterated line-by-line. Notice that the parseLine(line)
function does the actual parsing converting the string to a Pageview object 
defined earlier.
    
    private Pageview parseLine(String line) throws ParseException {
      StringTokenizer matcher = new StringTokenizer(line);
      //parse the log line
      String ip = matcher.nextToken();
      ...
    
      //construct and return pageview object
      Pageview pageview = new Pageview();
      pageview.setIp(new Utf8(ip));
      pageview.setTimestamp(timestamp);
      ...
    
      return pageview;
    }

parseLine() uses standard StringTokenizers for the job 
and constructs and returns a Pageview object.

### Storing objects in the DataStore
If we look back at the parse() method above, we can see that the 
Pageview objects returned by parseLine() are stored via 
storePageview() method. 

The storePageview() method is where magic happens, but if we look at the code,
we can see that it is dead simple.
 

    /** Stores the pageview object with the given key */
    private void storePageview(long key, Pageview pageview) throws IOException {
      dataStore.put(key, pageview);
    }

All we need to do is to call the put() method, which expects a long as key and an instance of Pageview
as a value.

### Closing the DataStore
DataStore implementations can do a lot of caching for performance. 
However, this means that data is not always flushed to persistent storage all the times. 
So we need to make sure that upon finishing storing objects, we need to close the datastore 
instance by calling it's close() method. 
LogManager always closes it's datastore in it's own close() method.  

    private void close() throws IOException {
      //It is very important to close the datastore properly, otherwise
      //some data loss might occur.
      if(dataStore != null)
      dataStore.close();
    }

If you are pushing a lot of data, or if you want your data to be accessible before closing 
the data store, you can also the flush()
method which, as expected, flushes the data to the underlying data store. However, the actual flush 
semantics can vary by the data store backend. For example, in SQL flush calls commit()
on the jdbc Connection object, whereas in Hb=Base, <code>HTable#flush()</code> is called.
Also note that even if you call flush() at the end of all data manipulation operations, 
you still need to call the close() on the datastore.

## Persisted data in HBase
Now that we have stored the web access log data in HBase, we can look at
how the data is stored at HBase. For that, start the HBase shell.

    $ cd ../hbase-${version}
    $ bin/hbase shell

If you have a fresh HBase installation, there should be one table.
    
    hbase(main):010:0> list

    AccessLog                                                                                                     
    1 row(s) in 0.0470 seconds

Remember that AccessLog is the name of the table we specified at 
gora-hbase-mapping.xml. Looking at the contents of the table:

    hbase(main):010:0> scan 'AccessLog', {LIMIT=>1}

    ROW                          COLUMN+CELL                                                                      
     \x00\x00\x00\x00\x00\x00\x0 column=common:ip, timestamp=1285860617341, value=88.240.129.183                  
     0\x00                                                                                                        
     \x00\x00\x00\x00\x00\x00\x0 column=common:timestamp, timestamp=1285860617341, value=\x00\x00\x01\x1F\xF1\xAEl
     0\x00                       P                                                                                
     \x00\x00\x00\x00\x00\x00\x0 column=common:url, timestamp=1285860617341, value=/index.php?a=1__wwv40pdxdpo&amp;k=2
     0\x00                       18978                                                                            
     \x00\x00\x00\x00\x00\x00\x0 column=http:httpMethod, timestamp=1285860617341, value=GET                       
     0\x00                                                                                                        
     \x00\x00\x00\x00\x00\x00\x0 column=http:httpStatusCode, timestamp=1285860617341, value=\x00\x00\x00\xC8      
     0\x00                                                                                                        
     \x00\x00\x00\x00\x00\x00\x0 column=http:responseSize, timestamp=1285860617341, value=\x00\x00\x00+           
     0\x00                                                                                                        
     \x00\x00\x00\x00\x00\x00\x0 column=misc:referrer, timestamp=1285860617341, value=http://www.buldinle.com/inde
     0\x00                       x.php?a=1__WWV40pdxdpo&amp;k=218978                                                  
     \x00\x00\x00\x00\x00\x00\x0 column=misc:userAgent, timestamp=1285860617341, value=Mozilla/4.0 (compatible; MS
     0\x00                       IE 6.0; Windows NT 5.1)

The output shows all the columns matching the first line with key 0. We can see 
the columns common:ip, common:timestamp, common:url, etc. Remember that 
these are the columns that we have described in the <code>gora-hbase-mapping.xml</code> file.

You can also count the number of entries in the table to make sure that all the records
have been stored.

    hbase(main):010:0> count 'AccessLog'
      ... 
      10000 row(s) in 1.0580 seconds

## Fetching objects from data store
Fetching objects from the data store is as easy as storing them. There are essentially 
two methods for fetching objects. First one is to fetch a single object given it's key. The 
second method is to run a query through the data store.

To fetch objects one by one, we can use one of the overloaded 
<code>get()</code> methods. 
The method with signature <code>get(K key)</code> returns the object corresponding to the given key fetching all the 
fields. On the other hand <code>get(K key, String[] fields)</code> returns the object corresponding to the 
given key, but fetching only the fields given as the second argument.

When run with the argument -get LogManager class fetches the pageview object 
from the data store and prints the results.
            
    /** Fetches a single pageview object and prints it*/
    private void get(long key) throws IOException {
      Pageview pageview = dataStore.get(key);
      printPageview(pageview);
    }

To display the 42nd line of the access log :

    $ bin/gora logmanager -get 42 

    org.apache.gora.tutorial.log.generated.Pageview@321ce053 {
      "url":"/index.php?i=0&amp;a=1__rntjt9z0q9w&amp;k=398179"
      "timestamp":"1236710649000"
      "ip":"88.240.129.183"
      "httpMethod":"GET"
      "httpStatusCode":"200"
      "responseSize":"43"
      "referrer":"http://www.buldinle.com/index.php?i=0&amp;a=1__RnTjT9z0Q9w&amp;k=398179"
      "userAgent":"Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)"
    }

## Querying objects
DataStore API defines a Query interface to query the objects at the data store. 
Each data store implementation can use a specific implementation of the Query interface. Queries are 
instantiated by calling <code>DataStore#newQuery()</code>. When the query is run through the datastore, the results 
are returned via the Result interface. Let's see how we can run a query and display the results below in the 
the LogManager class.
 
    /** Queries and prints pageview object that have keys between startKey and endKey*/
    private void query(long startKey, long endKey) throws IOException {
      Query<Long, Pageview> query = dataStore.newQuery();
      //set the properties of query
      query.setStartKey(startKey);
      query.setEndKey(endKey);
    
      Result<Long, Pageview> result = query.execute();
    
      printResult(result);
    }

After constructing a Query, its properties 
are set via the setter methods. Then calling <code>query.execute()</code> returns
the <code>Result</code> object.

Result interface allows us to iterate the results one by one by calling the 
<code>next()</code> method. The <code>getKey()</code> method returns the current key and <code>get()</code>
returns current persistent object.

    private void printResult(Result<Long, Pageview> result) throws IOException {
    
      while(result.next()) { //advances the Result object and breaks if at end
        long resultKey = result.getKey(); //obtain current key
        Pageview resultPageview = result.get(); //obtain current value object
      
        //print the results
        System.out.println(resultKey + ":");
        printPageview(resultPageview);
      }
    
      System.out.println("Number of pageviews from the query:" + result.getOffset());
    }

With these functions defined, we can run the Log Manager class, to query the 
access logs at HBase. For example, to display the log records between lines 10 and 12 
we can use:
    
    bin/gora logmanager -query 10 12
 
Which results in:

    10:
    org.apache.gora.tutorial.log.generated.Pageview@d38d0eaa {
      "url":"/"
      "timestamp":"1236710442000"
      "ip":"144.122.180.55"
      "httpMethod":"GET"
      "httpStatusCode":"200"
      "responseSize":"43"
      "referrer":"http://buldinle.com/"
      "userAgent":"Mozilla/5.0 (X11; U; Linux x86_64; en-US; rv:1.9.0.6) Gecko/2009020911 Ubuntu/8.10 (intrepid) Firefox/3.0.6"
    }
    11:
    org.apache.gora.tutorial.log.generated.Pageview@b513110a {
      "url":"/index.php?i=7&amp;a=1__gefuumyhl5c&amp;k=5143555"
      "timestamp":"1236710453000"
      "ip":"85.100.75.104"
      "httpMethod":"GET"
      "httpStatusCode":"200"
      "responseSize":"43"
      "referrer":"http://www.buldinle.com/index.php?i=7&amp;a=1__GeFUuMyHl5c&amp;k=5143555"
      "userAgent":"Mozilla/5.0 (Windows; U; Windows NT 5.1; tr; rv:1.9.0.7) Gecko/2009021910 Firefox/3.0.7"
    }

## Deleting objects
Just like fetching objects, there are two main methods to delete 
objects from the data store. The first one is to delete objects one by 
one using the <code>DataStore#delete(K key)</code> method, which takes the key of the object. 
Alternatively we can delete all of the data that matches a given query by 
calling the <code>DataStore#deleteByQuery(Query query)</code> method. By using <code>#deleteByQuery</code>, we can 
do fine-grain deletes, for example deleting just a specific field 
from several records. 
Continueing from the LogManager class, the api's for both are given below.

    /**Deletes the pageview with the given line number */
    private void delete(long lineNum) throws Exception {
      dataStore.delete(lineNum);
      dataStore.flush(); //write changes may need to be flushed before they are committed 
    }
  
    /** This method illustrates delete by query call */
    private void deleteByQuery(long startKey, long endKey) throws IOException {
      //Constructs a query from the dataStore. The matching rows to this query will be deleted
      Query<Long, Pageview> query = dataStore.newQuery();
      //set the properties of query
      query.setStartKey(startKey);
      query.setEndKey(endKey);
    
      dataStore.deleteByQuery(query);
    }    

And from the command line :

    bin/gora logmanager -delete 12
    bin/gora logmanager -deleteByQuery 40 50

## MapReduce Support
Gora has first class MapReduce support for [Apache Hadoop](https://hadoop.apache.org). 
Gora data stores can be used as inputs and outputs of jobs. Moreover, the objects can 
be serialized, and passed between tasks keeping their persistency state. For the 
serialization, Gora extends Avro DatumWriters.

### Log analytics in MapReduce
For this part of the tutorial, we will be analyzing the logs that have been 
stored at HBase earlier. Specifically, we will develop a MapReduce program to 
calculate the number of daily pageviews for each URL in the site.

We will be using the LogAnalytics class to analyze the logs, which can
be found at <code>gora-tutorial/src/main/java/org/apache/gora/tutorial/log/LogAnalytics.java</code>.
For computing the analytics, the mapper takes in pageviews, and outputs tuples of 
&lt;URL, timestamp&gt; pairs, with 1 as the value. The timestamp represents the day 
in which the pageview occurred, so that the daily pageviews are accumulated. 
The reducer just sums up the values, and outputs MetricDatum objects 
to be sent to the output Gora data store.

### Setting up the environment
We will be using the logs stored at HBase by the LogManager class. 
We will push the output of the job to an HSQL database, since it has a zero conf 
set up. However, you can also use MySQL or HBase for storing the analytics results. 
If you want to continue with HBase, you can skip the next sections. 

### Setting up the database
First we need to download HSQL dependencies. For that, ensure that the hsqldb 
dependency is available in the Maven pom.xml.
Ofcourse MySQL users should uncomment the mysql dependency instead. 

    <!--<dependency org="org.hsqldb" name="hsqldb" rev="2.0.0" conf="*->default"/>-->

Then we need to run Maven so that the new dependencies can be downloaded.

    $ mvn 
   
If you are using Mysql, you should also setup the database server, create the database 
and give necessary permissions to create tables, etc so that Gora can run properly.

### Configuring Gora
We will put the configuration necessary to connect to the database to 
<code>gora-tutorial/conf/gora.properties</code>.

   
#### JDBC properties for gora-sql module using HSQL

    gora.sqlstore.jdbc.driver=org.hsqldb.jdbcDriver
    gora.sqlstore.jdbc.url=jdbc:hsqldb:hsql://localhost/goratest

#### JDBC properties for gora-sql module using MySQL

    gora.sqlstore.jdbc.driver=com.mysql.jdbc.Driver
    gora.sqlstore.jdbc.url=jdbc:mysql://localhost:3306/goratest
    gora.sqlstore.jdbc.user=root
    gora.sqlstore.jdbc.password=      

As expected the jdbc.driver property is the JDBC driver class,
and jdbc.url is the JDBC connection URL. Moreover jdbc.user
and jdbc.password can be specific is needed. More information for these 
parameters can be found at [gora-sql](/current/gora-sql.html) documentation. 

### Modelling the data - Data Beans for Analytics  
For web site analytics, we will be using a generic MetricDatum
data structure. It holds a string metricDimension, a long 
timestamp, and a long metric fields. The first two fields 
are the dimensions of the web analytics data, and the last is the actual aggregate 
metric value. For example we might have an instance {metricDimension="/index", 
timestamp=101, metric=12}, representing that there have been 12 pageviews to 
the URL "/index" for the given time interval 101.

The avro schema definition for MetricDatum can be found at 
<code>gora-tutorial/src/main/avro/metricdatum.json</code>, and the compiled source 
code at <code>gora-tutorial/src/main/java/org/apache/gora/tutorial/log/generated/MetricDatum.java</code>.

    {
      "type": "record",
      "name": "MetricDatum",
      "namespace": "org.apache.gora.tutorial.log.generated",
      "fields" : [
        {"name": "metricDimension", "type": "string"},
        {"name": "timestamp", "type": "long"},
        {"name": "metric", "type" : "long"}
      ]
    }

### Data store mappings
We will be using the SQL backend to store the job output data, just to 
demonstrate the SQL backend. 

Similar to what we have seen with HBase, gora-sql plugin reads configuration from the 
<code>gora-sql-mappings.xml</code> file. 
Specifically, we will use the <code>gora-tutorial/conf/gora-sql-mappings.xml</code> file.    

    <gora-otd>
      ...
      <class name="org.apache.gora.tutorial.log.generated.MetricDatum" keyClass="java.lang.String" table="Metrics">
        <primarykey column="id" length="512"/>
        <field name="metricDimension" column="metricDimension" length="512"/>
        <field name="timestamp" column="ts"/>
        <field name="metric" column="metric/>
      </class>
    </gora-otd>
     
SQL mapping files contain one or more class elements as the children of gora-orm. 
The key value pair is declared in the class element. The name attribute is the 
fully qualified name of the class, and the keyClass attribute is the fully qualified class 
name of the key class. 

Children of the class element are field elements and one 
primaryKey element. Each field element has a name 
and column attribute, and optional  jdbc-type, length and scale attributes. 
name attribute contains the name of the field in the persistent class, and 
column attribute is the name of the 
column in the database. The primaryKey holds the actual key as the primary key field. Currently, 
Gora only supports tables with one primary key.

## Constructing the job 
In constructing the job object for Hadoop, we need to define whether we will use 
Gora as job input, output or both. Gora defines 
its own GoraInputFormat, and GoraOutputFormat, which 
uses DataStore's as input sources and output sinks for the jobs. 
Gora{In|Out}putFormat classes define static methods to set up the job properly.
However, if the mapper or reducer extends Gora's mapper and reducer  classes, 
you can use the static methods defined in GoraMapper and 
GoraReducer since they are more convenient. 
        
For this tutorial we will use Gora as both input and output. As can be seen from the 
<code>createJob()</code> function, quoted below, we create the job 
as normal, and set the input parameters via 
<code>GoraMapper#initMapperJob()</code>, and <code>GoraReducer#initReducerJob()</code>. 

<code>GoraMapper#initMapperJob()</code> takes a store and an optional query to fetch the data from. 
When a query is given, only the results of the query is used as the input of the job, if not all the records are used. 
The actual Mapper, map output key and value classes are passed to <code>initMapperJob()</code> 
function as well. <code>GoraReducer#initReducerJob()</code> accepts 
the data store to store the job's output as well as the actual reducer class.
initMapperJob and initReducerJob functions have also overriden methods that take the data store class 
rather than data store instances.

      public Job createJob(DataStore<Long, Pageview> inStore
          , DataStore<String, MetricDatum> outStore, int numReducer) throws IOException {
        Job job = new Job(getConf());

        job.setJobName("Log Analytics");
        job.setNumReduceTasks(numReducer);
        job.setJarByClass(getClass());

        /* Mappers are initialized with GoraMapper.initMapper() or 
         * GoraInputFormat.setInput()*/
        GoraMapper.initMapperJob(job, inStore, TextLong.class, LongWritable.class
            , LogAnalyticsMapper.class, true);

        /* Reducers are initialized with GoraReducer#initReducer().
         * If the output is not to be persisted via Gora, any reducer 
         * can be used instead. */
        GoraReducer.initReducerJob(job, outStore, LogAnalyticsReducer.class);
    
        return job;
      }

### Gora mappers and using Gora an input
Typically, if Gora is used as job input, the Mapper class extends  
GoraMapper. However, currently this is not forced by the API so other class hierarchies can be used instead. 
The mapper receives the key value pairs that are the results of the input query, and emits
the results of the custom map task. Note that output records from map are independent 
from the input and output data stores, so any Hadoop serializable key value class can be used. 
However, Gora persistent classes are also Hadoop serializable. Hadoop serialization is 
handled by the PersistentSerialization class. Gora also defines a StringSerialization class, to serialize strings easily. 

Coming back to the code for the tutorial, we can see that LogAnalytics 
class defines an inner class LogAnalyticsMapper which extends 
GoraMapper. The map function receives Long keys which are the line 
numbers, and Pageview values as read from the input data store. The map simply 
rolls up the timestamp up to the day (meaning that only the day of the timestamp is used), 
and outputs the key as a tuple of &lt;URL,day&gt;.

    private TextLong tuple;

    protected void map(Long key, Pageview pageview, Context context) 
      throws IOException ,InterruptedException {
      
      Utf8 url = pageview.getUrl();
      long day = getDay(pageview.getTimestamp());
      
      tuple.getKey().set(url.toString());
      tuple.getValue().set(day);
      
      context.write(tuple, one);
    };

### Gora reducers and using Gora as output
Similar to the input, typically, if Gora is used as job output, the Reducer extends 
GoraReducer. The values emitted by the reducer are persisted to the output data store 
as a result of the job. 

For this tutorial, the LogAnalyticsReducer inner class, 
which extends GoraReducer, is used as the reducer. The reducer 
just sums up all the values that correspond to the &lt;URL,day&gt; tuple. 
Then the metric dimension object is constructed and emitted, which 
will be stored at the output data store. 

    protected void reduce(TextLong tuple
        , Iterable<LongWritable> values, Context context) 
      throws IOException ,InterruptedException {
      
      long sum = 0L; //sum up the values
      for(LongWritable value: values) {
        sum+= value.get();
      }
      
      String dimension = tuple.getKey().toString();
      long timestamp = tuple.getValue().get();
      
      metricDatum.setMetricDimension(new Utf8(dimension));
      metricDatum.setTimestamp(timestamp);
      
      String key = metricDatum.getMetricDimension().toString();
      metricDatum.setMetric(sum);
      
      context.write(key, metricDatum);
    };

### Running the job 
Now that the job is constructed, we can run the Hadoop job as usual. Note that the run function 
of the LogAnalytics class parses the arguments and runs the job. We can run the program by 

    $ bin/gora loganalytics [<input data store> [<output data store>]]
  
### Running the job with SQL 
Now, let's run the log analytics tools with the SQL backend(either Hsql or MySql). The input data store will be 

    org.apache.gora.hbase.store.HBaseStore 
    
and output store will be 

    org.apache.gora.sql.store.SqlStore
    
Remember that we have already configured the database 
connection properties and which database will be used at the Setting up the environment section.
      
    $ bin/gora loganalytics org.apache.gora.hbase.store.HBaseStore  org.apache.gora.sql.store.SqlStore

Now we should see some logging output from the job, and whether it finished with success. To check out the output
if we are using HSQLDB, below command can be used.
  
    $ java -jar gora-tutorial/lib/hsqldb-2.0.0.jar

In the connection URL, the same URL that we have provided in gora.properties should be used. If on the other hand 
MySQL is used, than we should be able to see the output using the mysql command line utility. 
      
The results of the job are stored at the table Metrics, which is defined at the <code>gora-sql-mapping.xml</code> 
file. Running a select query over this data confirms that the daily pageview metrics for the web site is indeed stored.
To see the most popular pages, run:
     
&gt; SELECT METRICDIMENSION, TS, METRIC  FROM metrics order by metric desc
   
<table>
<tr><th>METRICDIMENSION</th> <th>TS</th> <th>METRIC</th></tr>
<tr><td>/</td> <td>	1236902400000</td> <td>	220</td></tr>
<tr><td>/</td> <td>	1236988800000</td> <td>	212</td></tr>
<tr><td>/</td> <td>	1236816000000</td> <td>	191</td></tr>
<tr><td>/</td> <td>	1237075200000</td> <td>	155</td></tr>
<tr><td>/</td> <td>	1241395200000</td> <td>	111</td></tr>
<tr><td>/</td> <td>	1236643200000</td> <td>	110</td></tr>
<tr><td>/</td> <td>	1236729600000</td> <td>	95</td></tr>
<tr><td>/index.php?a=3__x8g0vi&amp;k=5508310</td> <td>	1236816000000</td> <td>	45</td></tr>
<tr><td>/index.php?a=1__5kf9nvgrzos&amp;k=208773</td> <td>	1236816000000</td> <td>	37</td></tr>
<tr><td>...</td> <td>...</td> <td>...</td></tr>
      </table>

As you can see, the home page (/) for various days and some other pages are listed. 
In total 3033 rows are present at the metrics table. 

### Running the job with HBase
Since HBaseStore is already defined as the default data store at <code>gora.properties</code>
we can run the job with HBase as:

    $ bin/gora loganalytics

The outputs of the job will be saved in the Metrics table, whose layout is defined at 
<code>gora-hbase-mapping.xml</code> file. To see the results:

    hbase(main):010:0> scan 'Metrics', {LIMIT=>1}

    ROW                          COLUMN+CELL
     /?a=1__-znawtuabsy&amp;k=96804_ column=common:metric, timestamp=1289815441740, value=\x00\x00\x00\x00\x00\x00\x00
     1236902400000               \x09
     /?a=1__-znawtuabsy&amp;k=96804_ column=common:metricDimension, timestamp=1289815441740, value=/?a=1__-znawtuabsy&amp;
     1236902400000               k=96804
     /?a=1__-znawtuabsy&amp;k=96804_ column=common:ts, timestamp=1289815441740, value=\x00\x00\x01\x1F\xFD \xD0\x00
     1236902400000
    1 row(s) in 0.0490 seconds

## Spark Backend
Log analytics example will be implemented via GoraSparkEngine at this tutorial to explain Spark backend of Gora.
Data will be read from Hbase, map/reduce methods will be run and result will be written into Solr (version: 4.10.3).
All the process will be done over Spark.

Persist data into Hbase as described at [Log analytics in MapReduce](/current/tutorial.html#log-analytics-in-mapreduce).

To write result into Solr, create a schemaless core named as Metrics. To do it easily, you can rename default core of collection1 to Metrics which is at
`solr-4.10.3/example/example-schemaless/solr` folder and edit `solr-4.10.3/example/example-schemaless/solr/Metrics/core.properties` as follows:

    name=Metrics

Then run start command for Solr:

    solr-4.10.3/example$ java -Dsolr.solr.home=example-schemaless/solr/ -jar start.jar

Read data from Hbase, generate some metrics and write results into Solr with Spark via Gora. Here is how to initialize in and out data stores:

    public int run(String[] args) throws Exception {
      DataStore<Long, Pageview> inStore;
      DataStore<String, MetricDatum> outStore;
      Configuration hadoopConf = new Configuration();
      if (args.length > 0) {
        String dataStoreClass = args[0];
        inStore = DataStoreFactory.getDataStore(dataStoreClass, Long.class, Pageview.class, hadoopConf);
        if (args.length > 1) {
          dataStoreClass = args[1];
        }
        outStore = DataStoreFactory.getDataStore(dataStoreClass, String.class, MetricDatum.class, hadoopConf);
        } else {
          inStore = DataStoreFactory.getDataStore(Long.class, Pageview.class, hadoopConf);
          outStore = DataStoreFactory.getDataStore(String.class, MetricDatum.class, hadoopConf);
      }
     ...
    }

Pass input data store’s key and value classes and instantiate a GoraSparkEngine:

    GoraSparkEngine<Long, Pageview> goraSparkEngine = new GoraSparkEngine<>(Long.class, Pageview.class);

Construct a JavaSparkContext. Register input data store’s value class as Kryo class:

    SparkConf sparkConf = new SparkConf().setAppName("Gora Spark Integration Application").setMaster("local");
    Class[] c = new Class[1];
    c[0] = inStore.getPersistentClass();
    sparkConf.registerKryoClasses(c);
    JavaSparkContext sc = new JavaSparkContext(sparkConf);

You can get JavaPairRDD from input data store:

    JavaPairRDD<Long, Pageview> goraRDD = goraSparkEngine.initialize(sc, inStore);

When you get it, you can work on it as like you are writing a code for Spark! For example:

    long count = goraRDD.count();
    System.out.println("Total Log Count: " + count);

These are the functions of map and reduce phases for this example:

    /** The number of milliseconds in a day */
    private static final long DAY_MILIS = 1000 * 60 * 60 * 24;

    /**
    * map function used in calculation
    */
    private static Function<Pageview, Tuple2<Tuple2<String, Long>, Long>> mapFunc = new Function<Pageview, Tuple2<Tuple2<String, Long>, Long>>() {
      @Override
      public Tuple2<Tuple2<String, Long>, Long> call(Pageview pageview) throws Exception {
        String url = pageview.getUrl().toString();
        Long day = getDay(pageview.getTimestamp());
        Tuple2<String, Long> keyTuple = new Tuple2<>(url, day);
        return new Tuple2<>(keyTuple, 1L);
      }
    };

    /**
    * reduce function used in calculation
    */
    private static Function2<Long, Long, Long> redFunc = new Function2<Long, Long, Long>() {
      @Override
      public Long call(Long aLong, Long aLong2) throws Exception {
        return aLong + aLong2;
      }
    };

    /**
    * metric function used after map phase
    */
    private static PairFunction<Tuple2<Tuple2<String, Long>, Long>, String, MetricDatum> metricFunc = new PairFunction<Tuple2<Tuple2<String, Long>, Long>, String, MetricDatum>() {
      @Override
      public Tuple2<String, MetricDatum> call(
        Tuple2<Tuple2<String, Long>, Long> tuple2LongTuple2) throws Exception {
        String dimension = tuple2LongTuple2._1()._1();
        long timestamp = tuple2LongTuple2._1()._2();
        MetricDatum metricDatum = new MetricDatum();
        metricDatum.setMetricDimension(dimension);
        metricDatum.setTimestamp(timestamp);
        String key = metricDatum.getMetricDimension().toString();
        key += "_" + Long.toString(timestamp);
        metricDatum.setMetric(tuple2LongTuple2._2());
        return new Tuple2<>(key, metricDatum);
      }
    };

    /**
    * Rolls up the given timestamp to the day cardinality, so that data can be aggregated daily
    */
    private static long getDay(long timeStamp) {
      return (timeStamp / DAY_MILIS) * DAY_MILIS;
    }

Here is how to run map and reduce functions at existing JavaPairRDD:

    JavaRDD<Tuple2<Tuple2<String, Long>, Long>> mappedGoraRdd = goraRDD.values().map(mapFunc);
    JavaPairRDD<String, MetricDatum> reducedGoraRdd = JavaPairRDD.fromJavaRDD(mappedGoraRdd).reduceByKey(redFunc).mapToPair(metricFunc);

When you want to persist result into output data store, (in our example it is Solr), you should do it as follows:

    Configuration sparkHadoopConf = goraSparkEngine.generateOutputConf(outStore);
    reducedGoraRdd.saveAsNewAPIHadoopDataset(sparkHadoopConf);

That’s all! You can check Solr to verify the result.

## JCache caching dataStore

This tutorial is about exposing Apache Gora persistent dataStore over Apache Gora default caching dataStore JCache. This sample exhibits how caching can reduce read latency 
for consecutive reads when data beans are retrieved from intermediate cache as opposite to directly through the backend for consecutive iteration.

Start HBase.

    /hbase-0.98.19-hadoop2/bin$ ./start-hbase.sh

Start DistributedLogManager. ( Expose HBase dataStore over JCache dataStore )

    /gora/bin$ ./gora distributedlogmanager 

Persist Log Databeans to HBase either via the path <b> JCache DataStore -> HBase DataStore -> HBase </b> either via direct path <b> HBase DataStore -> HBase </b>

    -parse persistent|cache <-input_log_file-> - 

Benchmark dataBean read latency for two paths, path via <b> JCache DataStore <- HBase DataStore <- HBase </b> and path via <b> HBase DataStore <- HBase </b>

    -benchmark <-startLineNum-> <-endLineNum-> <-iterations->

## More Examples
Other than this tutorial, there are several places that you can find 
examples of Gora in action.

The first place to look at is the examples directories 
under various Gora modules. All the modules have a <code>/src/examples/</code> directory 
under which some example classes can be found. Especially, there are some classes that are used for tests under 
<code>gora-core/src/examples/</code>

Second, various unit tests of Gora modules can be referred to see the API in use. The unit tests can be found 
at <code>gora-core/src/test/</code>. 

The source code for the projects using Gora can also be checked out as a reference. [Apache Nutch](https://nutch.apache.org) is 
one of the first class users of Gora; so looking into how Nutch uses Gora is always a good idea. Gora is however also in use 
in other Apache projects such as [Apache Giraph](https://giraph.apache.org)

Please feel free to grab our [poweredBy](https://gora.apache.org/resources/img/powered-by-gora.png) sticker and embedded it in anything backed by Apache Gora.

## Feedback
At last, thanks for trying out Gora. If you find any bugs or you have suggestions for improvement, 
do not hesitate to give feedback on the dev@gora.apache.org [mailing list](../mailing_lists.html).
