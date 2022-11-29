Title: Gora DynamoDB Module

## Overview
This is the main documentation for the gora-dynamodb module. 
gora-dynamodb module enables [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) backend support for Gora.

[TOC]

## Gora DynamoDB Properties - gora.properties

    gora.datastore.default=org.apache.gora.dynamodb.store.DynamoDBStore
    gora.datastore.autocreateschema=true
    preferred.schema.name=Person
    gora.dynamodb.mapping.file=/path/to/gora-dynamodb-mapping.xml
    gora.dynamodb.client=sync
    gora.dynamodb.consistent.reads=true
    gora.dynamodb.endpoint=http://dynamodb.ap-northeast-1.amazonaws.com/
    gora.dynamodb.serialization.type=dynamo

| Property Key | Property Value | Required | Description
|------|------|------|------
| gora.datastore.default | org.apache.gora.dynamodb.store.DynamoDBStore | Yes | Implementation of the storage class |
| gora.datastore.autocreateschema | true | No | Create the table if it doesn’t exist |
| preferred.schema.name | Person | Yes | Name of the DynamoDB table/schema |
| gora.dynamodb.mapping.file | /path/to/gora-dynamodb-mapping.xml | No | The XML mapping file to be used. Defaults to gora-dynamodb-mapping.xml |
| gora.dynamodb.client | sync | No | DynamoDB client type. It could be sync or async. |
| gora.dynamodb.consistent.reads | true | No | Default is eventual consistence i.e. false. |
| gora.dynamodb.endpoint | http:\//dynamodb.us-east-1.amazonaws.com/ | Yes | Set to geographically closest service endpoint. For accepted list, see [here](#Accepted) |
| gora.dynamodb.serialization.type | dynamo | No | Data store serialization type. It could be 'dynamo' or 'avro' |

#### Accepted list of endpoints
- http:\//dynamodb.ap-northeast-1.amazonaws.com/
- http:\//dynamodb.ap-northeast-2.amazonaws.com/
- http:\//dynamodb.eu-west-1.amazonaws.com/
- http:\//dynamodb.us-east-1.amazonaws.com/
- http:\//dynamodb.us-west-1.amazonaws.com/
- http:\//dynamodb.us-west-2.amazonaws.com/


## Gora DynamoDB mapppings - gora-dynamodb-mapping.xml

Say we wished to map some user data and store it into DynamoDB.

    <gora-otd>
        <table name="Person" readcunit="1" writecunit="1" package="org.apache.gora.dynamodb.example.generated">
        <attribute name="ssn" type="N" key="hash"/>
        <attribute name="date" type="S" key="hashrange"/>
        <attribute name="firstName" type="S"/>
        <attribute name="lastName" type="S"/>
        <attribute name="salary" type="N"/>
        <attribute name="visitedplaces" type="SS"/>
    </gora-otd>

Within the `gora-otd` mapping configuration, only the 'table' child element is required.

### Table
- a parameter containing the DynamoDB table `name` (String) e.g. Person
- a parameter containing the read capacity - `readcunit` (Number) e.g. 1 More about them [here](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Limits.html#default-limits-throughput)
- a parameter containing the write capacity - `writecunit` (Number) e.g. 1 
- a parameter containing the name of the `package` having the table (String)

### Attributes
- a parameter containing the `name` e.g. name, dateOfBirth, ssn and salary 
- a parameter containing the column `type` to which they belong e.g. (B/L/M/N/S/SS). For more, refer [here](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_AttributeValue.html)
- an optional parameter `key`. The key can be a hash key (partition key/primary key) or a hashrange key (sort key) (in case of composite primary key). The key parameter is left blank for non-key attributes. For more, refer [here](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html#HowItWorks.CoreComponents.PrimaryKey)


