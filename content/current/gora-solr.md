Title: Gora HBase Module

## Overview
This is the main documentation for the gora-solr module. gora-solr 
module enables [Apache Solr](https://solr.apache.org) backend support for Gora. 

[TOC]

## gora.properties 
* <code>gora.datastore.default=org.apache.gora.solr.store.SolrStore</code> - Implementation of the storage class 
* <code>gora.datastore.autocreateschema=true</code> - Create the table if doesn't exist
* <code>gora.solrstore.solr.url=https://localhost:9876/solr</code> - The URL of the Solr server.
* <code>gora.solrstore.solr.config</code> -  The <code>solrconfig.xml</code> file to be used.
* <code>gora.solrstore.solr.schema</code> - The <code>schema.xml</code> file to be used.
* <code>gora.solrstore.solr.batchSize</code> - A batch size unit (ArrayList) of SolrDocument's to be used for writing to Solr. A default value of <b>100</b> is used if this value is absent. This value must be of type <b>Integer</b>.
* <code>gora.solrstore.solr.solrjserver</code> - The solrj implementation to use. This has a default value of <b>http</b> for <i>[HttpSolrServer]()</i>. Available options include <b>http</b> (<i>[HttpSolrServer](https://solr.apache.org/docs/4_8_1/solr-solrj/index.html?org/apache/solr/client/solrj/impl/HttpSolrServer.html)</i>), <b>cloud</b> (<i>[CloudSolrServer](https://solr.apache.org/docs/4_8_1/solr-solrj/index.html?org/apache/solr/client/solrj/impl/CloudSolrServer.html)</i>), <b>concurrent</b> (<i>[ConcurrentUpdateSolrServer](https://solr.apache.org/docs/4_8_1/solr-solrj/index.html?org/apache/solr/client/solrj/impl/ConcurrentUpdateSolrServer.html)</i>) and <b>loadbalance</b> (<i>[LBHttpSolrServer](https://solr.apache.org/docs/4_8_1/solr-solrj/index.html?org/apache/solr/client/solrj/impl/LBHttpSolrServer.html)</i>). This value must be of type <b>String</b>.
* <code>gora.solrstore.solr.commitWithin</code> - A batch commit unit for SolrDocument's used when making (commit) calls to Solr. A default value of 1000 is used if this value is absent. This value must be of type <b>Integer</b>.
* <code>gora.solrstore.solr.resultsSize</code> - The maximum number of results to return when we make a call to <code>org.apache.gora.solr.store.SolrStore#execute(Query)</code>. This value must be of type <b>Integer</b>.
 
## Gora Solr mappings
Say we wished to map some Employee data and store it into the SolrStore.

    <gora-otd>
        <class name="org.apache.gora.examples.generated.Employee" keyClass="java.lang.String" table="Employee">
          <primarykey column="ssn"/>
          <field name="name" column="name"/>
          <field name="dateOfBirth" column="dateOfBirth"/>
          <field name="salary" column="salary"/>
          <field name="boss" column="boss"/>
          <field name="webpage" column="webpage"/>
        </class>
    </gora-otd>

Here you can see that we require the definition of only one child element within the 
<code>gora-otd</code> mapping configuration, namely;

The class element where we specify of persistent fields which values should map to. This contains;

1. a parameter containing the Persistent class <b>name</b> e.g. <code>org.apache.gora.examples.generated.Employee</code>, 

2. a parameter containing the <b>keyClass</b> e.g. <code>java.lang.String</code> which specifies the keys which map to the field values, 

3. a parameter containing the <b>Table name</b> e.g. <code>Employee</code>,

4. finally nested child element(s) mapping fields which are to be persisted into Solr. <b>We must provide a primary key for each object that we wish to persist into Solr.</b> Additional object fields need to be configured such that they receive;

   a parameter containing the <b>name</b> e.g. (name, dateOfBirth, ssn, salary, boss and webpage respectively), 

   a parameter containing the <b>column family</b> to which they belong e.g. (all info in this case), 

## Solr Schema.xml

<code>schema.xml</code> is an essential aspect of defining a storage and query model for your Solr data.

The Solr community maintain their own documentation relating to schema.xml, this can be found at [https://wiki.apache.org/solr/SchemaXml](https://wiki.apache.org/solr/SchemaXml).

    <schema name="testexample" version="1.5">

      <fields>

        <!-- Common Fields -->
        <field name="_version_" type="long" indexed="true" stored="true"/>

        <!-- Employee Fields -->
        <field name="ssn"         type="string" indexed="true" stored="true" required="true" multiValued="false" /> 
        <field name="name"        type="string" indexed="true" stored="true" />
        <field name="dateOfBirth" type="long" stored="true" /> 
        <field name="salary"      type="int" stored="true" /> 
        <field name="boss"        type="binary" stored="true" />
        <field name="webpage"     type="binary" stored="true" />
    
      </fields>

      <uniqueKey>ssn</uniqueKey>

      <types>

        <fieldType name="string" class="solr.StrField" sortMissingLast="true" />
        <fieldType name="int" class="solr.TrieIntField" precisionStep="0" positionIncrementGap="0"/>
        <fieldType name="long" class="solr.TrieLongField" precisionStep="0" positionIncrementGap="0"/>
        <fieldtype name="binary" class="solr.BinaryField"/>
  
      </types>  

    </schema>

## Solr solrconfig.xml

Similar to <code>schema.xml</code> above, <code>solrconfig.xml</code> documentation is also maintained by the Solr community.

Please see an example configuration below but also please refer to [https://wiki.apache.org/solr/SolrConfigXml](https://wiki.apache.org/solr/SolrConfigXml). 

    <config>
      <luceneMatchVersion>LUCENE_40</luceneMatchVersion>
      <dataDir>${solr.data.dir:}</dataDir>
      <directoryFactory name="DirectoryFactory" 
                    class="${solr.directoryFactory:solr.NRTCachingDirectoryFactory}"/> 
      <codecFactory class="solr.SchemaCodecFactory"/>
      <schemaFactory class="ClassicIndexSchemaFactory"/>
      <indexConfig>
        <lockType>${solr.lock.type:native}</lockType>
      </indexConfig>

      <jmx />

      <updateHandler class="solr.DirectUpdateHandler2">
        <updateLog>
          <str name="dir">${solr.ulog.dir:}</str>
        </updateLog>
      </updateHandler>
  
      <query>
        <maxBooleanClauses>1024</maxBooleanClauses>
        <filterCache class="solr.FastLRUCache"
                 size="512"
                 initialSize="512"
                 autowarmCount="0"/>
        <queryResultCache class="solr.LRUCache"
                     size="512"
                     initialSize="512"
                     autowarmCount="0"/>
        <documentCache class="solr.LRUCache"
                   size="512"
                   initialSize="512"
                   autowarmCount="0"/>
        <enableLazyFieldLoading>true</enableLazyFieldLoading>
        <queryResultWindowSize>20</queryResultWindowSize>
        <queryResultMaxDocsCached>200</queryResultMaxDocsCached>
        <listener event="newSearcher" class="solr.QuerySenderListener">
          <arr name="queries">
          </arr>
        </listener>
        <listener event="firstSearcher" class="solr.QuerySenderListener">
          <arr name="queries">
            <lst>
              <str name="q">static firstSearcher warming in solrconfig.xml</str>
            </lst>
          </arr>
        </listener>
        <useColdSearcher>false</useColdSearcher>
        <maxWarmingSearchers>2</maxWarmingSearchers>
      </query>

      <requestDispatcher handleSelect="false" >
        <requestParsers enableRemoteStreaming="true" 
                    multipartUploadLimitInKB="2048000"
                    formdataUploadLimitInKB="2048"
                    addHttpRequestToContext="false"/>
        <httpCaching never304="true" />
      </requestDispatcher>

      <requestHandler name="/select" class="solr.SearchHandler">
        <lst name="defaults">
          <str name="echoParams">explicit</str>
          <int name="rows">10</int>
          <str name="df">ssn</str>
        </lst>
      </requestHandler>

      <requestHandler name="/query" class="solr.SearchHandler">
        <lst name="defaults">
          <str name="echoParams">explicit</str>
          <str name="wt">json</str>
          <str name="indent">true</str>
          <str name="df">ssn</str>
        </lst>
      </requestHandler>

      <requestHandler name="/get" class="solr.RealTimeGetHandler">
        <lst name="defaults">
          <str name="omitHeader">true</str>
        </lst>
      </requestHandler>

      <requestHandler name="/update" class="solr.UpdateRequestHandler">
      </requestHandler>
    </config>

