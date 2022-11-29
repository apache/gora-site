Title: Gora CouchDB Module

## Overview
This is the main documentation for the gora-couchdb module. gora-couchdb module enables [Apache CouchDB](https://couchdb.apache.org/) backend support for Gora.

[TOC] 

## gora.properties 
* <code>gora.datastore.default=org.apache.gora.couchdb.store.CouchDBStore</code> - Implementation of the storage class 
* <code>gora.datastore.couchdb.server=localhost</code> - Property pointing to the host where the server is running
* <code>gora.datastore.couchdb.port=5984</code> - Property pointing to the port where the server is running
* <code>gora.datastore.mapping.file=gora-couchdb-mapping.xml</code> -  The XML mapping file to be used. If no value is used this defaults to.
 
## Gora CouchDB mappings
You should then create a gora-couchdb-mapping.xml which will describe how you want to store each of your Gora persistent objects:

    <gora-otd>
        <class name="org.apache.gora.examples.generated.Employee" keyClass="java.lang.String" table="Employee">
       		<field name="name"/>
    		<field name="dateOfBirth"/>
    		<field name="ssn"/>
    		<field name="salary"/>
    		<field name="boss"/>
    		<field name="webpage"/>
  		</class>		
    </gora-otd>		

Here you can see that we require the definition of two child elements within the <code>gora-otd</code> mapping configuration, namely;

The class element where we specify of persistent fields which values should map to. This contains; 

1. a parameter containing the Persistent class name e.g. : <b>org.apache.gora.examples.generated.Employee </b>,

2. a parameter containing the keyClass e.g. <b>java.lang.String</b> which specifies the keys which map to the field values,

3. finally nested child element(s) mapping fields which are to be persisted into CouchDB. These fields need to be configured such that they receive; a parameter containing the name e.g. (name, dateOfBirth, ssn and salary respectively),
