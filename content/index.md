Title: Welcome to Apache Gora&trade;

# Welcome to the Apache Gora project!

<div class="hero-unit">
  <div class="row">
    <!--div class="span10">
      <img src="/resources/img/gora-logo.png" />
    </div-->
    <div class="span10">
      <p>The Apache Gora&trade; open source framework provides an in-memory data model and 
         persistence for big data. Gora supports persisting to column stores, key value stores, 
         document stores, distributed in-memory key/value stores, in-memory data grids, in-memory caches,
        distributed multi-model stores, and hybrid in-memory architectures.</p>
       <p>Gora also enables analysis of data with extensive Apache Hadoop MapReduce&trade;,
        Apache Spark&trade;, Apache Flink&trade; and Apache Pig&trade; support. Gora uses the <a href="https://www.apache.org/licenses/LICENSE-2.0.html">Apache Software License v2.0</a>. 
        Gora graduated from the Apache Incubator in January 2012 to become a top-level Apache project.
        You can find the Gora DOAP <a href="./current/doap_Gora.rdf">here</a>.
      </p>
      <p>
        <a href="/downloads.html" class="btn btn-primary btn-large">Download</a>
        <a href="/about.html" class="btn btn-primary btn-large">About</a>
        <a href="/current/quickstart.html" class="btn btn-primary btn-large">Quickstart</a>
        <a href="/current/index.html" class="btn btn-primary btn-large">Overview</a>
        <a href="/current/tutorial.html" class="btn btn-primary btn-large">Tutorial</a>
        <a href="/credits.html" class="btn btn-primary btn-large">Credits</a>
        <a href="/contribute.html" class="btn btn-primary btn-large">Contribute</a>
        <a href="/current/api/javadoc.html" class="btn btn-primary btn-large">Javadoc</a>
      </p>
    </div>
    <div class="span10">
      <a class="twitter-timeline"  href="https://twitter.com/ApacheGora"  data-widget-id="473948188378812419">Tweets by @ApacheGora</a>
    <script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+"://platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");</script>
    </div>
  </div>
</div>

## News

### 15 August, 2019: Apache Gora 0.9 Release
The Apache Gora Release management team is pleased to announce the immediate availability of
Apache Gora 0.9.

This release addresses 48 issues, for a breakdown please see the [release report](https://s.apache.org/0.9GoraReleaseNotes). 
Drop by our mailing lists and ask questions for information on any of the above.

Gora 0.9 provides support for the following projects

   - [Apache Avro](https://avro.apache.org) 1.8.2   
   - [Apache HBase](https://hbase.apache.org) 2.1.1
   - [Apache Cassandra](https://cassandra.apache.org) 3.11.0 (Datastax Java Driver 3.3.0)
   - [Apache Solr](https://solr.apache.org/) 8.0.0
   - [Apache Lucene](https://lucene.apache.org/core/) 8.0.0
   - [MongoDB](https://mongodb.com) (Mongo Java Driver) 3.5.0
   - [Apache Accumlo](https://accumulo.apache.org) 1.7.1
   - [Apache CouchDB](https://couchdb.apache.org) 1.4.2 
   - [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) (Amazon Java SDK) 1.10.55
   - [Infinispan](https://infinispan.org/) 7.2.5.Final
   - [JCache](https://www.jcp.org/en/jsr/detail?id=107) 1.0.0 with JCache provider [Hazelcast](https://hazelcast.com/) 3.6.4
   - [OrientDB](https://orientdb.com/orientdb/) 2.2.22
   - [Aerospike](https://www.aerospike.com/) 4.2.2  
   - [Apache Ignite](https://ignite.apache.org/) 2.6.0
   - [Apache Pig](https://pig.apache.org/) 0.16.0
   - [Apache Flink](https://flink.apache.org/) 1.6.2
   - [Apache Hadoop](https://hadoop.apache.org) 2.5.2
   - [Apache Spark](https://spark.apache.org) 2.2.1
   - [Test containers](https://testcontainers.viewdocs.io/testcontainers-java/) 1.4.2   

Gora is released as both source code downloads for which can be found at
our [downloads page](https://gora.apache.org/downloads.html), as well as Maven artifacts which can be found on
[Maven central](https://s.apache.org/0.9GoraMavenArtifacts). 

### 20 September, 2017: Apache Gora 0.8 Release
The Apache Gora team are pleased to announce the immediate availability of
Apache Gora 0.8.

This release addresses 35 issues, for a breakdown please see the [release report](https://s.apache.org/3YdY). 
Drop by our mailing lists and ask questions for information on any of the above.

Gora 0.8 provides support for the following projects

   - [Apache Avro](https://avro.apache.org) 1.8.1
   - [Apache Hadoop](https://hadoop.apache.org) 2.5.2
   - [Apache HBase](https://hbase.apache.org) 1.2.3
   - [Apache Cassandra](https://cassandra.apache.org) 3.11.0 (Datastax Java Driver 3.3.0)
   - [Apache Solr](https://lucene.apache.org/solr) 6.5.1
   - [MongoDB](https://mongodb.com) (driver) 3.5.0
   - [Apache Accumlo](https://accumulo.apache.org) 1.7.1
   - [Apache Spark](https://spark.apache.org) 1.4.1
   - [Apache CouchDB](https://couchdb.apache.org) 1.4.2 ([test containers](https://testcontainers.viewdocs.io/testcontainers-java/) 1.1.0)
   - [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) (driver) 1.10.55
   - [Infinispan](https://infinispan.org/) 7.2.5.Final
   - [JCache](https://www.jcp.org/en/jsr/detail?id=107) 1.0.0 with [Hazelcast](https://hazelcast.com/) 3.6.4 support.
   - [OrientDB](https://orientdb.com/orientdb/) 2.2.22
   - [Aerospike](https://aerospike.com/) 4.0.6

Gora is released as both source code downloads for which can be found at
our [downloads page](https://gora.apache.org/downloads.html), as well as Maven artifacts which can be found on
[Maven central](https://search.maven.org/#search|ga|1|gora). 

### 23 March, 2017: Apache Gora 0.7 Release
The Apache Gora team are pleased to announce the immediate availability of
Apache Gora 0.7.

This release addresses 80 issues, for a breakdown please see the [release report](https://s.apache.org/YrmC). 
Drop by our mailing lists and ask questions for information on any of the above.

Gora 0.7 provides support for the following projects

   - [Apache Avro](https://avro.apache.org) 1.8.1
   - [Apache Hadoop](https://hadoop.apache.org) 2.5.2
   - [Apache HBase](https://hbase.apache.org) 1.2.3
   - [Apache Cassandra](https://cassandra.apache.org) 2.0.2
   - [Apache Solr](https://solr.apache.org) 5.5.1
   - [MongoDB](https://mongodb.com) (driver) 3.4.2
   - [Apache Accumlo](https://accumulo.apache.org) 1.7.1
   - [Apache Spark](https://spark.apache.org) 1.4.1
   - [Apache CouchDB](https://couchdb.apache.org) 1.4.2 ([test containers](https://testcontainers.viewdocs.io/testcontainers-java/) 1.1.0)
   - [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) (driver) 1.10.55
   - [Infinispan](https://infinispan.org/) 7.2.5.Final
   - [JCache](https://www.jcp.org/en/jsr/detail?id=107) 1.0.0 with [Hazelcast](https://hazelcast.com/) 3.6.4 support.

Gora is released as both source code, downloads for which can be found at
our [downloads page](https://gora.apache.org/downloads.html), as well as Maven artifacts which can be found on
[Maven central](https://search.maven.org/#search|ga|1|gora). 

### 14 September, 2015: Apache Gora 0.6.1 Release
The Apache Gora team are pleased to announce the immediate availability of
Apache Gora 0.6.1.

This release addresses a modest [21 issues](https://s.apache.org/l69) with many 
improvements and bug fixes for the 
[gora-mongodb](https://gora.apache.org/current/gora-mongodb.html) module, 
resolution of a major bug whilst flushing data to Apache Solr, a [gora-gradle plugin](https://issues.apache.org/jira/browse/GORA-330)
and our [Gora Spark backend support](https://issues.apache.org/jira/browse/GORA-386). 
Drop by our mailing lists and ask questions for information on any of the above.

We provide Gora support for the following projects

   - Apache Avro 1.7.6
   - Apache Hadoop 1.2.1 and 2.5.2
   - Apache HBase 0.98.8-hadoop2 (although also tested with 1.X)
   - Apache Cassandra 2.0.2
   - Apache Solr 4.10.3
   - MongoDB 2.6.X
   - Apache Accumlo 1.5.1
   - Apache Spark 1.4.1

Gora is released as both source code, downloads for which can be found at
our [downloads page](https://gora.apache.org/downloads.html) as well as Maven artifacts which can be found on
[Maven central](https://search.maven.org/#search|ga|1|gora). 

### 19 February, 2015: Apache Gora 0.6 Release
The Apache Gora team are pleased to announce the immediate availability of
Apache Gora 0.6.

This release addresses a modest [47 issues](https://s.apache.org/gora-0.6) with some being
major improvements, new functionality and dependency upgrades. Most notably the release
involves key upgrades to Hadoop, HBase and Solr dependecies as well as some
extermely important bug fixes for the MongoDB module. 

Suggested Gora database support is as follows

   - Apache Avro 1.7.6
   - Apache Hadoop 1.2.1 and 2.5.2
   - Apache HBase 0.98.8-hadoop2
   - Apache Cassandra 2.0.2
   - Apache Solr 4.10.3
   - MongoDB 2.6.X
   - Apache Accumlo 1.5.1

Gora is released as both source code, downloads for which can be found at
our [downloads page](https://gora.apache.org/downloads.html) as well as Maven artifacts which can be found on
[Maven central](https://search.maven.org/#search|ga|1|gora). 

### 8 January, 2015: Gora upgrades to Hadoop 2.5.X and HBase 0.98.X
Today our Jira issue [GORA-375](https://issues.apache.org/jira/browse/GORA-375) was resolved
with the result that the Gora community can now upgrade their systems to operate on Hadoop 2.X
and HBase 0.98.
Specifically the versions which we recommend are as follows:
 
   - Apache Hadoop 2.5.2
   - Apache HBase 0.98.8-hadoop2

Best efforts have also been made to retain backwards compatibility for Hadoop 1.X users,
therefore we have upgraded to, and still support Hadoop 1.2.1 via the addition of our
Shim layers. For further details on our Shim layers and pluggable Hadoop support please 
see Jira issue [GORA-346](https://issues.apache.org/jira/browse/GORA-375). Additional
documentation for the use of Shim layers within Gora will soon be documented and linked to 
from our [current documentation](https://gora.apache.org/current/index.html#gora-modules).

### 20 September, 2014: Apache Gora 0.5 Release
The Apache Gora team are pleased to announce the immediate availability of
Apache Gora 0.5.

This release addresses no fewer than [44 issues](https://s.apache.org/0.5report) with many being
improvements and new functionality. Most notably the release includes the
addition of a new module for MongoDB, Shim ffunctionality to support
multiple Hadoop versions, improved authentication for Accumulo, better
documentation for many modules, and pluggable solrj implementations
supporting a default value of http for HttpSolrServer. Available options
include http (HttpSolrServer), cloud (CloudSolrServer), concurrent
(ConcurrentUpdateSolrServer) and loadbalance (LBHttpSolrServer).

Suggested Gora database support is as follows

   - Apache Avro 1.7.6
   - Apache Hadoop 1.0.1 and 2.4.0
   - Apache HBase 0.94.14
   - Apache Cassandra 2.0.2
   - Apache Solr 4.8.1
   - MongoDB 2.6
   - Apache Accumlo 1.5.1

Gora is released as both source code, downloads for which can be found at
our [downloads page](https://gora.apache.org/downloads.html) as well as Maven artifacts which can be found on
[Maven central](https://search.maven.org/#search|ga|1|gora). 

### 01 July, 2014: Apache Gora joins the [DARPA Open Catalog](https://www.darpa.mil/opencatalog/)

<img src="http://www.darpa.mil/opencatalog/Open-Catalog-Single-Big.png" alt="Open Catalog Logo">

Gora now features within the Defense Advanced Research Projects Agency (DARPA) Open Catalog 
as part of ongoing participation in the [XDATA Program](http://www.darpa.mil/opencatalog/XDATA.html).

XDATA is developing an open source software library for big data to help overcome the 
challenges of effectively scaling to modern data volume and characteristics. The 
program is developing the tools and techniques to process and analyze large sets of 
imperfect, incomplete data. Its programs and publications focus on the areas of 
analytics, visualization, and infrastructure to efficiently fuse, analyze and 
disseminate these large volumes of data.

Gora is being used by the [Jet Propulsion Laboratory](https://jpl.nasa.gov) team to execute
[extract-transform-load](https://en.wikipedia.org/wiki/Extract,_transform,_load)-type tasks
for mapping and integration of source heterogeneous data to and from target NoSQL solutions.

### 04 June, 2014: Apache Gora now supports [MongoDB](https://www.mongodb.org/)

<img src="https://engineering.linkedin.com/sites/default/files/mongodb-logo.png" alt="MongoDB Logo">

The Gora community is proud to announce support MongoDB amongst our growing datastore 
support arsenal. MongoDB is an open-source document database, and the leading NoSQL database. Written in C++, MongoDB features:

* Document-Oriented Storage

* JSON-style documents with dynamic schemas offer simplicity and power.
    
* Full Index Support

* Index on any attribute, just like you're used to.

* Replication & High Availability

* Mirror across LANs and WANs for scale and peace of mind.

* Auto-Sharding

* Scale horizontally without compromising functionality.
    
* Querying

* Rich, document-based queries.

* Fast In-Place Updates

* Atomic modifiers for contention-free performance.

* [MapReduce](https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html)

* Flexible aggregation and data processing.

* GridFS

* Store files of any size without complicating your stack.

The current supported version of is MongoDB is 2.6 with client version support at 2.12.2. 

### 29 May, 2014: Apache Gora now implemented as [Apache Camel](https://camel.apache.org) Component

<img src="https://camel.apache.org/images/apache-camel-100h.png" alt="Apache Camel Logo">

We recently heard some excellent news that Gora is now implemented as a 
[Camel Component](https://fisheye6.atlassian.com/browse/camel-git/components/camel-gora) within Camel trunk.

Apache Camel is a rule-based routing and mediation engine which provides a Java object based implementation of 
the Enterprise Integration Patterns using an API (or declarative Java Domain Specific Language) to configure 
routing and mediation rules. The domain specific language means that Apache Camel can support type-safe 
smart completion of routing rules in your IDE using regular Java code without huge amounts of XML configuration 
files; though XML configuration inside Spring is also supported.

You can use this module with the following Maven configuration

    <dependency>
      <groupId>org.apache.camel</groupId>
      <artifactId>camel-gora</artifactId>
      <version>x.x.x</version>
      <!-- use the same version as your Camel core version -->
    </dependency>

You can find more information on the history of the development on the relevant [Jira ticket](https://issues.apache.org/jira/browse/CAMEL-4817).

You can also view the [gora-camel](./current/gora-camel.html) documentation.

### 19 May, 2014: Apache Gora at [HBase London Meetup](https://www.meetup.com/HBase-London/events/179791252/)

<img src="https://hbase.apache.org/images/hbase_logo.png" alt="HBase London Meetup">

Keeping in trend with our continual drive to build the Gora community, Gora is featuring
in this months [HBase London Meetup](https://www.meetup.com/HBase-London/events/179791252/).

The meetup is focused on [data types in HBASE](https://blogs.apache.org/hbase/entry/data_types_schema) c.f. [HBASE-8693](https://issues.apache.org/jira/browse/HBASE-8693)
[HBASE-8089](https://issues.apache.org/jira/browse/HBASE-8089) with the second half of the meetup 
being dedicated to discussion and presentation on where Gora/gora-hbase fits in the mix!

<iframe src="https://prezi.com/embed/qox1rakc7ftu/?bgcolor=ffffff&amp;lock_to_path=0&amp;autoplay=0&amp;autohide_ctrls=0&amp;features=undefined&amp;disabled_features=undefined" width="550" height="400" frameBorder="0" webkitAllowFullScreen mozAllowFullscreen allowfullscreen></iframe>

### 06 May, 2014: Apache Gora Features in [Black Duck Software's](www.blackducksoftware.com/‎) [Open Source Delivers Blog](https://osdelivers.blackducksoftware.com/)

<img src="https://osdelivers.blackducksoftware.com/wp-content/uploads/2011/06/OSD_logo.png" alt="Open Source Delivers">

Gora has become the first Apache project to feature on Black Duck Software's Open Source 
Blog with an article titled [What does it really take to build a community around code?](https://osdelivers.blackducksoftware.com/2014/05/06/what-does-it-really-take-to-build-a-community-around-code/).

The article focuses on Gora's journey through the [Apache Incubator](https://incubator.apache.org) and tells the story of how the community
has evolved and grown over time and software releases. 

A huge thank you goes to our friends at Black Duck Software for reviewing and publishing
our story.

### 24 April, 2014: Apache Gora 0.4 Released

<p>The Apache Gora team are proud to announce the release of Gora 0.4. 
This release addresses no fewer than 60 issues. Major improvements within
the release scope comprise a complete upgrade to Apache Avro 1.7.X and
overhaul of the Gora persistency API (such improvements enable Gora to be
used to map much more expressive and complicated data structures than
previously available), upgrades to Apache HBase 0.94.13, Apache Cassandra
2.0.X and Apache Accumulo 1.5.X.
Users can also benefit from using Gora + Solr for object-to-datastore
mapping with the addition of the new Solr module which uses Solr 4.X.
A full list of changes in this release can be seen in
<a href="https://www.apache.org/dist/gora/0.4/CHANGES.txt">0.4-CHANGES.txt</a>.
You can grab the maven release artifacts from 
<a href="https://repo1.maven.org/maven2/org/apache/gora/">Maven Central</a> 
and can also get the Gora sources from our [downloads page](/downloads.html).
</p>

### 14th April, 2014: Gora adopted by [Apache Giraph](https://giraph.apache.org)

<img src="https://giraph.apache.org/images/ApacheGiraph.svg" alt="Apache Giraph" height="200" width="200">

After a successful [Google Summer of Code 2013](https://www.google-melange.com/gsoc/homepage/google/gsoc2013)
project, Gora has been adopted as a core persistence abstraction for the popular graph processing 
library in use at companies such as Facebook, Twitter, Linkedin and many more.

The integration of these two awesome Apache projects has as main motivation the 
possibility of turning Gora-supported-NoSQL data stores into Giraph-processable graphs, 
and to provide Giraph the ability to store its results into different data stores, 
letting users focus on the processing itself. For more information please see the 
[Giraph-Gora documentation](https://giraph.apache.org/gora.html).

### 7th - 10th April, 2014: Apache Gora at [ApacheCon NA 2014](https://events.linuxfoundation.org/events/apachecon-north-america)

A number of the Gora team were at ApacheCon this year spreading the word about the project and 
presenting on several topics. This included Renato's presentation on 
[Turning NoSQL data into Graphs - Playing with Apache Giraph and Apache Gora](https://sched.co/1bsMgqm)
and Lewis' presentation on [ Deploying Apache Gora as a Query Broker](https://sched.co/1bsM9es).

Please see below for the presentation slides and audio.

<iframe src="https://www.slideshare.net/slideshow/embed_code/33495268" width="550" height="400" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px 1px 0; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe>
<iframe src="https://prezi.com/embed/cgdekhla4evl/?bgcolor=ffffff&amp;lock_to_path=0&amp;autoplay=0&amp;autohide_ctrls=0&amp;features=undefined&amp;disabled_features=undefined" width="550" height="400" frameBorder="0" webkitAllowFullScreen mozAllowFullscreen allowfullscreen></iframe>
<iframe width="550" height="400" src="//www.youtube.com/embed/06PjlHJ2srY" frameborder="0" allowfullscreen></iframe>
<iframe width="550" height="400" src="//www.youtube.com/embed/DRlfSbSfVC4" frameborder="0" allowfullscreen></iframe>

### 16 December, 2013: Apache Gora at Dublin NoSQL Meetup 

<p>
  <a title="TCube" href="http://tcubedublin.com/events/dublin-nosql-meetup-apache-gora-and-the-oracle-nosql-database-customize/">
  <img src="./resources/img/tcube.jpeg"
  class="float-right" alt="TCube Logo"/>
  </a>
  <br/>
  To further promote this years GSoC project, this years student (Apostolos Giannakidis) and mentor 
  (Lewis McGibbney) will present <b>Apache Gora and the Oracle NoSQL database</b>
  at <a href="http://tcubedublin.com/">TCube</a> in the beautiful
  city of Dublin. To say we are looking forward to this one may possibly be the 
  understatement of 2013. Looking forward to seeing some familiar faces  and new ones as we
  continue to promote Gora to the masses. Please see slides and video links below
  enjoy!<br>
  <iframe src="https://prezi.com/embed/b5_vabnmelmy/?bgcolor=ffffff&amp;lock_to_path=0&amp;autoplay=0&amp;autohide_ctrls=0&amp;features=undefined&amp;disabled_features=undefined" width="550" height="400" frameBorder="0"></iframe>
  <iframe src="https://prezi.com/embed/tzkpitqbwvah/?bgcolor=ffffff&amp;lock_to_path=0&amp;autoplay=0&amp;autohide_ctrls=0&amp;features=undefined&amp;disabled_features=undefined" width="550" height="400" frameBorder="0"></iframe>
  <iframe src="https://prezi.com/embed/pijlrsp3jzqx/?bgcolor=ffffff&amp;lock_to_path=0&amp;autoplay=0&amp;autohide_ctrls=0&amp;features=undefined&amp;disabled_features=undefined" width="550" height="400" frameBorder="0"></iframe>
</p>

### 25 September 2013: Apache Gora successfully participates in Google Summer of Code 2013

<p>
  <a title="GSoC2013" href="https://www.google-melange.com/gsoc/homepage/google/gsoc2013">
  <img src="./resources/img/gsoc2013.jpg"
  class="float-right" alt="GSoC2013 Logo"/>
  </a>
  <br/>The jury has been out, the results are in and we are extremely proud to announce that the 
  <a href="https://google-melange.appspot.com/gsoc/proposal/review/google/gsoc2013/apgiannakidis/1">
  <b>Apache Gora support for Oracle NoSQL datastore</b></a> project has come out on top in this years 
  <a href="https://www.google-melange.com/gsoc/homepage/google/gsoc2013">Google Summer of Code 2013</a>. 
  Another long, hard summers worth of work has resulted in another successful project and code contribution, again extending
  datastore support within Apache Gora. A huge congratulations to this years student Apostolos Giannakidis for his work over 
  the summer, as a community we look forward to your continued presence within the project and beyond.<br/>
  Adding to the success of this project, the bloggershpere has been endorsing and promoting the project wholeheartedly.
  On <a href="http://www.giannakidis.info/">his own website</a>, Apostolos has made various posts documenting 
  and sharing his experiences through the summer. <a href="http://blog.corbinian.com/node/89">Corbinian, Inc.</a> have also been talking about the project.
  Additionally Apostolos was recognized by the <a href="http://www.cs.bham.ac.uk/sys/news/content/2013/06/17/student-project-accepted-in-google-summer-of-code/">University of Birmingham</a> 
  where he obtained his masters degree in Computer Science. Finally, the project featured as part of a Veteran Organization
  post on <a href="https://google-opensource.blogspot.co.uk/2013/10/google-summer-of-code-veteran-orgs.html">Google's Open Source Blog</a>.<br/>
  For links to more documentation and even more blog posts covering this project, as well as the <a href="https://issues.apache.org/jira/secure/attachment/12604408/GSoC2013_Final_Project_Report.pdf"><b>final report</b></a>, 
  please see the GORA-217 issue on our <a href="https://issues.apache.org/jira/browse/GORA-217">issue tracker</a>.<br/>
</p>

### 19 June, 2013: Apache Gora Features in HadoopShere

<p>
  <a title="HadoopShere" href="http://www.hadoopsphere.com/">
  <img src="./resources/img/hadoopsphere.png"
  class="float-right" alt="HadoopSphere Logo"/>
  </a>
  <br/>Gora recently featured a two-part guest post in <a href="http://www.hadoopsphere.com/">HadoopSphere</a>.
  The first post, entitled <a href="http://www.hadoopsphere.com/2013/06/in-memory-data-model-with-apache-gora.html">In memory data model with Apache Gora</a>
  simply introduces Gora to the HadoopSphere reader base. The second post, entitled <a href="http://www.hadoopsphere.com/2013/06/amazon-dynamodb-datastore-for-gora.html">Amazon DynamoDB datastore for Gora</a>
  brings the 0.3 release to the forefront of attention, focusing on important improvements within Gora 0.3 and of course the DynamoDB datastore
  which hails as the keynote of the 0.3 release. <b>Happy Reading!!!</b>.     
</p>

### 08 June, 2013: Apache Gora at CassandraSummit 2013

<p>
  <a title="CassandraSummit 2013" href="https://www.datastax.com/company/news-and-events/events/cassandrasummit2013">
  <img src="https://s.apache.org/Jt6"
  class="float-right" alt="CassandraSummit Logo"/>
  </a>
  <br/>
  <a href="https://www.datastax.com/company/news-and-events/events/cassandrasummit2013">C* Summit</a> is the premier global conference for the Apache Cassandra 
  community and yee ha, Gora is going along for the ride. Renato and Lewis are
  presenting <b>Taking Bytes from Cassandra Clients</b>; a technical discussion
  and overview of current development on implementing a pluggable 
  Cassandra client infrastructure (<a href="https://github.com/hector-client/hector">Hector-client</a>, 
  <a href="https://github.com/datastax/java-driver">Datastax java-driver, <a href="https://github.com/Netflix/astyanax/wiki">Netflix Astyanax</a>,
  <a href="https://github.com/zznate/intravert-ug">intravert-ug</a>, etc) adapted specifically for the gora-cassandra module.
  The presentation kicks off at 3:30-4:30pm in Track 4 in the Marina Room 
  Conference Center. Check out the full <a href="https://www.datastax.com/company/news-and-events/events/cassandrasummit2013#schedule">schedule</a> 
  for a taste of whats going on. You can follow the news on <a href="https://twitter.com/search?q=%23Cassandra13&src=hashhashtag">#Cassandra13</a>.
  See the slides and video below. Enjoy
  <iframe src="https://prezi.com/embed/u6qlkcdb6vbs/?bgcolor=ffffff&amp;lock_to_path=0&amp;autoplay=0&amp;autohide_ctrls=0&amp;features=undefined&amp;disabled_features=undefined" width="550" height="400" frameBorder="0"></iframe>
  <iframe width="550" height="400" src="//www.youtube.com/embed/Ps-j4aro99U" frameborder="0" allowfullscreen></iframe>
</p>

### 8 May, 2013: Apache Gora 0.3 Released

<p>The Apache Gora team are proud to announce the release of Gora 0.3. 
This point release offers users significant improvements to a number of modules
 including a number of bug fixes, however of significant interest to the 
DynamoDB community will be the addition of a gora-dynamodb datastore for 
mapping and persisting objects to Amazon's DynamoDB [0]. Additionally the 
release includes various improvements to the gora-core and gora-cassandra 
modules as well as a new Web 
Services API implementation which enables users to extend Gora to any cloud 
storage platform. A full list of changes in this release can be seen in
<a href="https://www.apache.org/dist/gora/0.3/CHANGES.txt">0.3-CHANGES.txt</a>.
You can grab the maven release artifacts from 
<a href="https://repo1.maven.org/maven2/org/apache/gora/">Maven Central</a> 
and can also get the Gora sources from our [downloads page](/downloads.html).
</p>

### 12 September, 2012: Apache Gora at ApacheCon EU 2012

<p>
  <a title="ApacheCon EU 2012" href="https://www.apachecon.eu/">
  <img src="https://www.apache.org/events/logos-banners/ApacheCon-2012-Europe/EU_speaker_wide_25.png"
  class="float-right" alt="ApacheCon Logo"/>
  </a>
  <br/>After an absence of several years, the <a href="https://apache.org/"><b>Apache Software Foundation</b></a> is pleased to announce that <a href="https://apachecon.com/"><b>ApacheCon</b></a> is returning to Europe in 2012! <a href="https://www.apachecon.eu/"><b>ApacheCon EU Community Edition 2012</b></a> will be held between the 5th and 8th of November, at the Rhein-Neckar-Arena in Sinsheim, Germany... and guess what? Gora is coming along for the ride! The <a href="https://www.apachecon.eu/proposals/91/">proposal</a> entitled <i><b>From Incubation to Continuous Ingestion - The Story of Apache Gora</b></i> has been included in the <a href="https://www.apachecon.eu/tracks/#big-data"><b>Big Data</b></a> track. We look forward to seeing you in November in Germany.
</p>
      
### 24 August, 2012: Apache Gora successfully participates in Google Summer of Code 2012

<p>
  <a title="GSoC2012" href="https://www.google-melange.com/gsoc/homepage/google/gsoc2012">
  <img src="./resources/img/gsoc2012.jpg"
  class="float-right" alt="GSoC2012 Logo"/>
  </a><br/>
The jury has been out, the results are in and we are extremely proud to announce that the <a href="https://google-melange.appspot.com/gsoc/proposal/review/google/gsoc2012/renato2099/1"><b>Gora - Amazon DynamoDB datastore for Gora</b></a> project has come out on top in this years <a href="https://www.google-melange.com/gsoc/homepage/google/gsoc2012">Google Summer of Code</a>. We can now bear the fruits of success in this years program as it marks a first for Gora and will surely reap long term benefits for the community as a whole. A huge congratulations to this years student Renato Javier MarroquiÂ­n Mogrovejo for his work over the summer, as a community we look forward to your continued presence within the project and beyond.   
</p>
      
### 07 August, 2012: Apache Gora 0.2.1 released

<p>The Apache Gora team are proud to announce the release of Gora 0.2.1. This point-oh! release offers users large improvements within the gora-cassandra module including a number of bug fixes, significant upgrades to Apache Cassandra and Hector Client API usage and a number of improvements to the gora-core API. The Maven artifacts for the project are published  
to <a href="https://repo1.maven.org/maven2/org/apache/gora/">Maven Central</a>.
See the <a href="https://www.apache.org/dist/gora/0.2.1/CHANGES.txt">0.2-CHANGES.txt</a> 
file for a full list of changes in this release or alternatively the <a href="https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12311172&#38;version=12322496">Jira release report</a>.
</p>
     
### 24 April, 2012: Apache Gora 0.2 released

<p>The Apache Gora team are proud to announce the release of Gora 0.2, our first since graduating from the Apache
Incubator. This release boasts an assortment of over 50 improvements over our previous incubating release with artifacts 
available both within Maven Central and from official Apache mirrors. The Maven artifacts for the project are published  
to <a href="https://repo1.maven.org/maven2/org/apache/gora/">Maven Central</a>.
See the <a href="https://www.apache.org/dist/gora/0.2-CHANGES.txt">0.2-CHANGES.txt</a> 
file for a full list of changes in this release. 
</p>
      
### 23 April, 2012: Apache Gora Amazon DynamoDB project accepted as Google Summer of Code project

<p>The Apache Gora team are very happy to announce conformation that the green light has been given for a Gora 
<a href="https://aws.amazon.com/dynamodb/">Amazon Dynamo DB</a> project within the remit of this years 
<a href="https://www.google-melange.com/gsoc/homepage/google/gsoc2012">Google Summer of Code</a>.
See the official project proposal <a href="https://www.google-melange.com/gsoc/proposal/review/google/gsoc2012/renato2099/1">here</a>. 
</p>
     
### 24 January, 2012: Apache Gora Graduates to Top Level Project at the Apache Software Foundation

<p>The biggest event within Apache Gora's history was revealed on 24/01/2012 when it was announced that Apache Gora 
was to be established as a Top Level Project entity within the Apache Software Foundation. This is excellent news for 
the Gora community and we are looking forward to tackling the benefits and challenges brought by our new Top Level status. 
</p>

### 15 January, 2012: Apache Gora announces full support for <a href="ext:hector"><b>Hector Client</b>:</a>

<p>Some time ago, the Apache Gora development team made the decision to support Hector as the 
primary Apache Cassandra client. This decision enables Gora to build on the expressiveness offered by the 
Hector API, some features include:
<li>high level, simple object oriented interface to Cassandra. </li>
<li>failover behavior on the client side.</li>
<li>connection pooling for improved performance and scalability.</li>
<li>JMX counters for monitoring and management.</li>
<li>configurable and extensible load balancing (round robin (the default), least active, and a phi-accrural style response time detector).</li>
<li>complete encapsulation of the underlying Thrift API and structs.</li>
<li>automatic retry of downed hosts.</li>
<li>automatic discovery of additional hosts in the cluster.</li>
<li>suspension of hosts for a short period of time after several timeouts.</li>
<li>simple ORM layer that works.</li>
<li>a type-safe approach to dealing with Apache Cassandra's data model.</li>
As an Apache community we value community over code, this move is definitely a step in the right direction towards
bringing together two diverse communities, whilst during process undoubtedly making both Apache Gora and Hector better 
technologies. Here at Gora we look forward to working with the Hector community.
</p>

### 24 September, 2011: Apache Gora 0.1.1-incubating release

<p>The Apache Gora project made its second incubating release. This 
release improves the Maven artifacts for the project and publishes them 
in useable form to <a href="https://repo1.maven.org/maven2/org/apache/gora/">Maven Central</a>.
See the <a href="https://www.apache.org/dist/incubator/gora/CHANGES-0.1.1-incubating.txt">CHANGES.txt</a> 
file for a full list of changes in this release.
</p>

### 06 April, 2011: Apache Gora 0.1-incubating release

<p>The Apache Gora project made its first incubating release. See the
<a href="https://archive.apache.org/dist/incubator/gora/0.1-incubating/CHANGES-0.1-incubating.txt">CHANGES.txt</a>
file for a full list of changes in this release.
</p>

### 26 September, 2010: Gora in Apache Incubator 

<p>Gora has been accepted to the Apache Incubator and started it's life
as a candidate Apache project. </p>
