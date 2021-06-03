Title: Gora HBase Module

## Overview
This is the main documentation for the gora-hbase module. gora-hbase 
module enables [Apache HBase](http://hbase.apache.org) backend support for Gora.

[TOC] 

## gora.properties 
* <code>gora.datastore.default=org.apache.gora.hbase.store.HBaseStore</code> - Implementation of the storage class 
* <code>gora.datastore.autocreateschema=true</code> - Create the table if doesn't exist
* <code>gora.datastore.scanner.caching=1000</code> - HBase client cache that improves the scan in HBase (default 0)
* <code>hbase.client.autoflush.default=false</code> -  HBase autoflushing. Enabling autoflush decreases write performance. Available since Gora 0.2. Defaults to disabled.
 
## Gora HBase mappings
Say we wished to map some Employee data and store it into the HBaseStore.

    <gora-otd>
      <table name="Employee">
        <family name="info" 
                compression="$$$" 
                blockCache="$$$" 
                blockSize="$$$" 
                bloomFilter="$$$" 
                maxVersions="$$$" 
                timeToLive="$$$" 
                inMemory="$$$" />
      </table> 

      <class name="org.apache.gora.examples.generated.Employee" keyClass="java.lang.String" table="Employee">
        <field name="name" family="info" qualifier="nm"/>
        <field name="dateOfBirth" family="info" qualifier="db"/>
        <field name="ssn" family="info" qualifier="sn"/>
        <field name="salary" family="info" qualifier="sl"/>
        <field name="boss" family="info" qualifier="bs"/>
        <field name="webpage" family="info" qualifier="wp"/>
      </class>
    </gora-otd>

Here you can see that we require the definition of two child elements within the 
<code>gora-otd</code> mapping configuration, namely;

The table element; where we specify: 

1. a parameter relating to the HBase table name (String) e.g. name=<b>"Employee"</b>, 

2. a nested element containing the type and definition of families we wish to create within HBase. In this case we create one family <b>info</b> which could have a combination of any of the following parameters;

   <b>name</b> (String): family name e.g. info

   <b>compression</b> (String): the compression option to use in HBase. Please see <a href="http://hbase.apache.org/book/compression.html">HBase documentation</a>.

   <b>blockCache</b> (boolean):  an LRU cache that contains three levels of block priority to allow for scan-resistance and in-memory ColumnFamilies. Please see <a href="https://hbase.apache.org/book/regionserver.arch.html#block.cache">HBase documentation</a>.

   <b>blockSize</b> (Integer): The blocksize can be configured for each ColumnFamily in a table, and this defaults to 64k. Larger cell values require larger blocksizes. There is an inverse relationship between blocksize and the resulting StoreFile indexes (i.e., if the blocksize is doubled then the resulting indexes should be roughly halved). Please see <a href="http://hbase.apache.org/book/perf.schema.html#schema.cf.blocksize">HBase documentation</a>. 

   <b>bloomFilter</b> (String): Bloom Filters can be enabled per-ColumnFamily. We use <code>HColumnDescriptor.setBloomFilterType(NONE | ROW | ROWCOL)</code> to enable blooms per Column Family. Default = NONE for no bloom filters. If ROW, the hash of the row will be added to the bloom on each insert. If ROWCOL, the hash of the row + column family name + column family qualifier will be added to the bloom on each key insert. Please see <a href="http://hbase.apache.org/book/perf.schema.html#schema.bloom">HBase documentation</a>.

   <b>maxVersions</b> (Integer): The maximum number of row versions to store is configured per column family via <code>HColumnDescriptor</code>. The default for max versions is <b>3</b>. This is an important parameter because HBase does not overwrite row values, but rather stores different values per row by time (and qualifier). Excess versions are removed during major compaction's. The number of max versions may need to be increased or decreased depending on application needs. Please see <a href="http://hbase.apache.org/book/schema.versions.html">HBase documentation</a>.

   <b>timeToLive</b> (Integer): ColumnFamilies can set a TTL length in seconds, and HBase will automatically delete rows once the expiration time is reached. This applies to all versions of a row - even the current one. The TTL time encoded in the HBase for the row is specified in UTC. Please see <a href="https://hbase.apache.org/book/ttl.html">HBase documentation</a>.

   <b>inMemory</b> (Boolean): ColumnFamilies can optionally be defined as in-memory. Data is still persisted to disk, just like any other ColumnFamily. In-memory blocks have the highest priority in the Block Cache, but it is not a guarantee that the entire table will be in memory. Please see <a href="http://hbase.apache.org/book/perf.schema.html#cf.in.memory">HBase documentation</a>.

The class element where we specify of persistent fields which values should map to. This contains;

1. a parameter containing the Persistent class name e.g. <b>org.apache.gora.examples.generated.Employee</b>, 

2. a parameter containing the keyClass e.g. <b>java.lang.String</b> which specifies the keys which map to the field values, 

3. a parameter containing the Table name e.g. <b>Employee</b> which matches to the above Table definition,

4. finally nested child element(s) mapping fields which are to be persisted into HBase. These fields need to be configured such that they receive;

   a parameter containing the <b>name</b> e.g. (name, dateOfBirth, ssn and salary respectively), 

   a parameter containing the column <b>family</b> to which they belong e.g. (all info in this case), 

   an optional parameter <b>qualifier</b>, which enables more granular control over the data to be persisted into HBase.
