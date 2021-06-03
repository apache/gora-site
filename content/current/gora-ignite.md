Title: Gora Ignite Module

## Overview
This is the main documentation for the gora-ignite module. <b>gora-ignite</b> module enables [Apache Ignite](https://ignite.apache.org/) backend support for Gora.

[TOC] 

## Gora Ignite Properties - gora.properties 

* <code>gora.datastore.default=org.apache.gora.ignite.store.IgniteStore</code> - Implementation of the persistent Java storage class for Ignite
* <code>gora.datastore.ignite.schema=PUBLIC</code> - Property pointing to the Schema of the Ignite instance 
* <code>gora.datastore.ignite.host=localhost</code> -  Property pointing to the host where the server is running
* <code>gora.datastore.ignite.port=10800</code> -  Property pointing to the port where the server is running
* <code>gora.datastore.ignite.user=username</code> - An optional property defining the username of the server if available
* <code>gora.datastore.ignite.password=password</code> - An optional property defining the password of the server if available
* <code>gora.datastore.ignite.additionalConfigurations=</code> - An optional property defining additional configurations for the Ignite connection, format and available parameters: [Ignite JDBC Parameters](https://apacheignite-sql.readme.io/docs/jdbc-driver#section-parameters)
 
## Gora Ignite mappings - gora-ignite-mapping.xml
You should then create a gora-ignite-mapping.xml which will describe how you want to store each of your Gora persistent objects and which primary keys you want to use:

    <gora-otd>
      <class name="org.apache.gora.examples.generated.Employee" keyClass="java.lang.String" table="Employee">
        <primarykey column="pkssn" type="VARCHAR" />
        <field name="ssn" column="ssn" type="VARCHAR"/>
        <field name="name" column="name" type="VARCHAR"/>
        <field name="dateOfBirth" column="dateOfBirth" type="BIGINT"/>
        <field name="salary" column="salary" type="INT"/>
        <field name="boss" column="boss" type="BINARY"/>
        <field name="webpage" column="webpage" type="BINARY"/>
      </class>
    </gora-otd>

Here you can see that we require the definition of child elements within the <code>gora-otd</code> mapping configuration.

Each <b>class</b> element should contain the following elements; 

1. a parameter defining the Persistent class name e.g. <b>org.apache.gora.examples.generated.Employee</b>, 

2. a parameter defining the keyClass e.g. <b>java.lang.String</b> which specifies the key which maps to the field values, 

3. a parameter defining the table e.g. <b>Employee</b> which will be used to persist each Gora object

In addition, within the class element we should define two type of child elements: a primary key (<b>primarykey</b> tag) and some fields (<b>field</b> tag).

The primary key element defines which column is used by Ignite to identify the records stored in the DataStore. It has two costumizable parameters: <b>column</b> which defines the column's name of the table to be used as identifier for the records. And <b>type</b> which defines the data type of the aforementioned column.

The fields elements define the actual mapping between persistent object's attributes and the table's columns. These mapping have three customizable parameters: <b>name</b> which correspond to the object attribute's name. <b>column</b> which defines the column's name of the table to be assosiated with the attribute. And <b>type</b> which defines the data type of that column.

Notice that complex structures such 3-union fields are mapped using Binary fields through [Avro](https://avro.apache.org/) serialization.


## Supported Data types
Description of supported <b>type</b> values:

| Type value | Description                     |
|------------|---------------------------------|
| BINARY     | Store as Byte[]                 |
| BOOLEAN    | Store as Boolean                |
| INT        | Store as Integer                |
| TINYINT    | Store as Byte                   |
| SMALLINT   | Store as Short                  |
| BIGINT     | Store as Long                   |
| DECIMAL    | Store as BigDecimal             |
| DOUBLE     | Store as Double                 |
| REAL       | Store as Float                  |
| VARCHAR    | Store as Unicode string         |

A more detailed list of data types supported by Ignite and its equivalents in Java refer to [Ignite JDBC Data types](https://apacheignite-sql.readme.io/docs/data-types)
