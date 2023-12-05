# Azure Cosmos DB

Azure Cosmos DB is a document database service that enables us to store, query, and index our data in a highly available, globally consistent, and scalable cloud-based NoSQL database.

It is fully managed which means availability, reliability, and security are all handled for us.

## Cosmos DB APIs

The standout feature of Cosmos DB is that you can configure it to use either SQL or NoSQL to interact with your data. It does so by providing different APIs for each type of data you want to store.

The APIs are:

* **NoSQL** and **SQL** – The Azure Cosmos DB API for NoSQL stores data in document format. This is the default and recommeneded API.
* **MongoDB** – The Azure Cosmos DB API for MongoDB stores data in a document structure, via BSON format. It's compatible with MongoDB wire protocol; however, it doesn't use any native MongoDB related code. 
* **Apache Gremlin** – The Azure Cosmos DB API for Gremlin allows users to make graph queries and stores data as edges and vertices.
* **Apache Cassandra** – The Azure Cosmos DB API for Cassandra stores data in column-oriented schema. Apache Cassandra offers a highly distributed, horizontally scaling approach to storing large volumes of data while offering a flexible approach to a column-oriented schema.
* **PostreSQL** - Azure Cosmos DB for PostgreSQL is a managed service for running PostgreSQL at any scale, with the Citus open source superpower of distributed tables. It stores data either on a single node, or distributed in a multi-node configuration.
* **Table** – The Azure Cosmos DB API for Table stores data in key/value format.

By providing these APIs, Cosmos DB makes it easy for us to migrate from an existing data store to Cosmos DB.

> [!NOTE]
> The APIs do not determine how the data is stored. The data is stored in a NoSQL database.

**Read more**

* [Read more about chosing an API](https://learn.microsoft.com/azure/cosmos-db/choose-api).

## Cosmos DB Concepts

* **Cosmos DB Accounts**  
  Account is the top level resource in Azure Cosmos DB. It is the entry point to our Cosmos DB instances.
* **Databases**  
  An account can contain multiple databases which have the same distribution needs. 
* **Containers**  
  Each database can contain multiple containers. You can also use databases to manage users, permissions, and throughput for underlying containers.  
  Each container is analogous to a table in SQL, collection in MongoDB, graph in Gremlin, and so on. Containers are fundamental units of scalability for storage and throughput. They are horizontally partitioned and replicated across multiple regions as defined by the account.
* **Items**  
  An item is a single unit of record. For example, a row in a table, a document in a collection, a node in a graph, and so on. Similar to an SQL table, a container can contain multiple resources.

## Working with Cosmos DB

* [Create a Cosmos DB database](create-db.md)
