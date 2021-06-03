Title: Gora Kudu Module

## Overview
This is the main documentation for the gora-kudu module. <b>gora-kudu</b> module enables [Apache Kudu](https://kudu.apache.org) backend support for Gora.

[TOC] 

## Gora Kudu Properties - gora.properties 

* <code>gora.datastore.default=org.apache.gora.kudu.store.KuduStore</code> - Implementation of the persistent Java storage class for Kudu
* <code>gora.datastore.kudu.masterAddresses=localhost:7051</code> - Comma-separated list of "host:port" pairs of the Kudu masters
* <code>gora.datastore.kudu.flushMode=AUTO_FLUSH_SYNC</code> -  Flush mode for the Kudu-client session. More details here: https://kudu.apache.org/apidocs/org/apache/kudu/client/SessionConfiguration.FlushMode.html.
* <code>gora.datastore.kudu.flushInterval=1000</code> -  Flush interval in ms, which will be used for the next scheduling.
* <code>gora.datastore.kudu.bossCount=1</code> - Maximum number of boss threads. Optional. If not provided, 1 is used.
* <code>gora.datastore.kudu.defaultAdminOperationTimeoutMs=1000</code> - Default timeout used for administrative operations (e.g. createTable, deleteTable, etc). Optional. If not provided, defaults to 30s.  A value of 0 disables the timeout.
* <code>gora.datastore.kudu.defaultOperationTimeoutMs=1000</code> - Default timeout used for user operations (using sessions and scanners). Optional. If not provided, defaults to 30s. A value of 0 disables the timeout.
* <code>gora.datastore.kudu.defaultSocketReadTimeoutMs=10000</code> - Default timeout to use when waiting on data from a socket. Optional. If not provided, defaults to 10s. A value of 0 disables the timeout.
* <code>gora.datastore.kudu.clientStatistics=true</code> - Client's collection of statistics. Statistics are enabled by default.
* <code>gora.datastore.kudu.workerCount=2</code> - Maximum number of worker threads. Optional. If not provided, (2 * the number of available processors) is used.
 
## Gora Kudu mappings - gora-kudu-mapping.xml
You should then create a gora-kudu-mapping.xml which will describe how you want to store each of your Gora persistent objects and which primary keys you want to use:

	<gora-otd>
		<table name ="Employee">
			<primaryKey column="pkssn" type="STRING" />
			<hashPartition numBuckets="8"/>
		</table>
		<class name="org.apache.gora.examples.generated.Employee" keyClass="java.lang.String" table="Employee">
			<field name="ssn" column="ssn" type="STRING"/>
			<field name="value" column="value" type="STRING"/>
			<field name="name" column="name" type="STRING"/>
			<field name="dateOfBirth" column="dateOfBirth" type="INT64"/>
			<field name="salary" column="salary" type="INT32"/>
			<field name="boss" column="boss" type="BINARY"/>
			<field name="webpage" column="webpage" type="BINARY"/>
		</class>
	</gora-otd>


Here you can see that we require the definition of two child elements within the <code>gora-otd</code> mapping configuration: a class and a table.

### Class

Each <b>class</b> element should contain the following elements;

1. a parameter defining the Persistent class name e.g. <b>org.apache.gora.examples.generated.Employee</b>,

2. a parameter defining the keyClass e.g. <b>java.lang.String</b> which specifies the key which maps to the field values,

3. a parameter defining the table e.g. <b>Employee</b> which will be used to persist each Gora object

In addition, within the class element we should define the fields to be mapped (field tag). The fields elements define the actual mapping between persistent object's attributes and the table's columns. These mapping have three customizable parameters: name which correspond to the object attribute's name. column which defines the column's name of the table to be associated with the attribute. And type which defines the data type of that column.

* The DECIMAL data type is parameterizable, so two additional attributes might be used in the XML mapping, scale and precision. Example:

		<field name="value" column="value" type="DECIMAL" scale=”1” precision=”1”/>

### Table

Each <b>table</b> element should contain the following elements;

1. A **`primaryKey`** element defining which column is used by Kudu to identify the records stored in the DataStore. It has two attributes: **`column`** which defines the column's name of the table to be used as identifier for the records, and **`type`** which defines the data type of the aforementioned column.

2. A partition definition for the table, which could be either a hash partition or a range partition. For more information about Partitioning in Kudu refer to [https://kudu.apache.org/docs/schema_design.html#partitioning](https://kudu.apache.org/docs/schema_design.html#partitioning)

asdhfjalkjhf

Example for Hash partition:

        <hashPartition numBuckets="8"/>

The numBuckets attribute specifies the number of hash buckets to be used as partitions.

Example for Range partition:

        <rangePartition lower="" upper ="1000"/>

        <rangePartition lower="1000" upper =""/>

The lower and upper attributes define the boundaries of each partition. This partition is applied only on the primary key column of the table. Take into consideration that the lower bound is inclusive and the upper one is not.

## Supported Data types
Description of supported <b>type</b> values:

| Kudu datatype | Java datatype                     |
|---------------|-----------------------------------|
|BOOL	|boolean|
|INT8	|byte|
|INT32	|short|
|INT16	|int|
|INT64	|long|
|UNIXTIME_MICROS	|java.sql.Timestamp|
|FLOAT	|float|
|DOUBLE	|double|
|DECIMAL	|BigDecimal|
|STRING	|String|
|BINARY	|byte[]|


For more details about supported data types refer to [https://kudu.apache.org/docs/schema_design.html#column-design](https://kudu.apache.org/docs/schema_design.html#column-design)
