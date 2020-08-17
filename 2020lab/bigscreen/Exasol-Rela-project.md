#Exasol-project.md

## What ?

---
Exasol is happy to share the latest and greatest from our open source community with the world. Below, you can see a list of our Github projects and a link to the latest release.


Character: 看看开源和商业版的区别










NOTE: Inclusion in this list does not guarantee that the project is officially supported by Exasol. Please view the Readme of the various projects to see if it is officially supported or not.

## Practice -> Related Projects

1. Exasol on Docker

Project Name 	Description 	Download
Docker DB 	Dockerized version of the Exasol Database for testing purposes 	
https://github.com/exasol/docker-db/releases

 
2. Virtual Schemas

Project Name 	Description 	Download
Virtual Schemas 	Exasol Virtual Schemas are an abstraction layer that makes external data sources accessible in our data analytics platform through regular SQL commands 	
https://github.com/exasol/virtual-schemas/releases

Virtual Schemas for Exasol 	This Virtual Schema adapter should be used to connect to another Exasol database via Virtual Schema. This adapter is removed from the base repository above 	
https://github.com/exasol/exasol-virtual-schema/releases

 
3. Dialects and Connectors

Project Name 	Description 	Download
Kafka Connect JDBC Connector 	Exasol database dialect example setup for Kafka Confluent JDBC Connector 	
https://github.com/exasol/kafka-connect-jdbc-exasol

4. Spark Exasol Connector 	

This is a connector library that supports an integration between Exasol and Apache Spark. Using this connector, users can create Spark dataframes from Exasol queries and save Spark dataframes as Exasol tables. 	
https://github.com/exasol/spark-exasol-connector/releases

5. Power BI - Exasol Connector 	
The Exasol Microsoft Power BI Connector is a Certified Custom Connector now and is also shipped with Power BI desktop. The powerbi-exasol repository will contain the latest release which might contain modifications not yet included in the stable version. 	
https://github.com/exasol/powerbi-exasol

6. Exasol Hibernate Dialect 	 
This repository contains a custom dialect to support ORM via JPA and Hibernate 5 using the Exasol database. 	
https://github.com/exasol/hibernate-exasol


7. Django - Exasol 	
This project contains the necessary dialect to integrate Exasol into the Django framework. 	https://github.com/exasol/django-exasol

SQLAlchemy Dialect for Exasol 	This is an SQLAlchemy dialect for the Exasol database. 	
https://github.com/exasol/sqlalchemy-exasol


8. Exasol Informatica Connector Architecture 	
Template for system and software architecture done with OpenFastTrace. 	https://github.com/exasol/exasol-informatica-connector-architecture

 
10. Data Science

Project Name 	Description 	Download
Data Science Examples 	This repository contains a collection of examples and tutorials for Data Science and Machine Learning with Exasol. 	
https://github.com/exasol/data-science-examples

11. Script Language Containers 	
This project contains implementations for user defined functions (UDF's) that can be used in the Exasol database (version 6.0.0 or later) 	
https://github.com/exasol/script-languages/releases

12. R Package for Exasol 	
The Exasol R Package offers interface functionality such as connecting to, querying and writing into an Exasol Database (version 5 onwards).  	
https://github.com/exasol/r-exasol

13. Python Package for Exasol - PyExasol 	
PyEXASOL is a custom Python driver for Exasol created by @littlekoi. It helps to handle massive volumes of data commonly associated with this database. 	
https://github.com/exasol/pyexasol

13. Data Loading
Project Name 	Description 	Download
Cloud Storage ETL UDFs 	his repository contains helper code to create Exasol ETL UDFs in order to read from and write to public cloud storage services such as AWS S3, Google Cloud Storage and Azure Blob Storage. 	
https://github.com/exasol/cloud-storage-etl-udfs/releases

14. Hadoop ETL UDFs 	
Hadoop ETL UDFs are the main way to transfer data between Exasol and Hadoop (HCatalog tables on HDFS). The SQL syntax for calling the UDFs is similar to that of Exasol's native IMPORT and EXPORT commands, but with added UDF paramters for specifying the various necessary and optional Hadoop properties. 	
https://github.com/exasol/hadoop-etl-udfs


15. Database Migration 	
This project contains SQL scripts for automatically importing data from various data management systems into Exasol. 	https://github.com/exasol/database-migration


16. Utilities
Project Name 	Description 	Download
Exa Toolbox 	The EXA-toolbox is a collection of useful scripts and views that you can add to your Exasol installation. 	
https://github.com/exasol/exa-toolbox/blob/master/load_scripts_from_github.sql

17. Opendata Examples 	Contains SQL scripts for loading open datasets into Exasol 	
https://github.com/exasol/opendata-examples

18. Nagios Monitoring 	
The Exasol nagios monitoring container provides users a simple starting point for setting up a monitoring system for your Exasol database.  	
https://github.com/exasol/nagios-monitoring

19. EXAOperation XMLRPC API 	
This project contains Python script examples how to automate the administration of Exasol cluster using its XMLRPC API instead of EXAoperation's web interface 	
https://github.com/exasol/exaoperation-xmlrpc

20. SQL Statement Builder 	
The Exasol SQL Statement Builder abstracts programmatic creation of SQL statements and is intended to replace ubiquitous string concatenation solutions which make the code hard to read and are prone to error and security risks. 	
https://github.com/exasol/sql-statement-builder/releases

21. ETL Utilities 	
The Query Wrapper is a lightweight object-oriented Lua script used as a library to build procedural ETL jobs in Lua. 	
https://github.com/exasol/etl-utils

22. Compatibility Test Suite 	
This compatibility test suite contains test datasets and sample SQL statements that you can use as a standard against which you can test database clients: ETL tools, connectors, SQL clients. 	
https://github.com/exasol/compatibility-test-suite/releases

23. AWS Cost Reporting 	Collection of Exasol scripts to record and analyze the costs produced by an AWS account. 	
https://github.com/exasol/aws-cost-reporting

24. Websockets API 	The JSON over WebSockets client-server protocol allows customers to implement their own drivers for all kinds of platforms using a connection-based web protocol. 	
https://github.com/exasol/websocket-api

25. BucketFS Explorer 	
The BucketFS explorer is a tiny GUI written in Java/JavaFX that helps you working with BucketFS. 	bucketfsexplorer-0.0.1-SNAPSHOT-jar-with-dependencies.jar
Exasol Testcontainers 	This software is intended for use in automated integration tests of Java software that uses Exasol. It sets up and runs a disposable Docker container and lets users access the interfaces of the Exasol instance inside that container with minimum effort. 	
https://github.com/exasol/exasol-testcontainers/releases

26. Hamcrest Resultset Matcher 	
This project provides Hamcrest matcher that compares JDBC result set (java.sql.ResultSet) against each other or against Java structures. 	
https://github.com/exasol/hamcrest-resultset-matcher/releases


27. Test DB Builder for Java 	
Exasol's Test Database Builder for Java (TDDB) is a library that makes writing integration tests for database applications easier.
https://github.com/exasol/test-db-builder-java/releases

28. Compatibility Test Suite 	
This compatibility test suite contains test datasets and sample SQL statements that you can use as a standard against which you can test database clients: ETL tools, connectors, SQL clients. 	
https://github.com/exasol/compatibility-test-suite/releases

29. Integration Test - Docker 	
This project provides a command line interface to start a test environment with an Exasol Docker DB. It starts an Exasol Docker-DB container and an associated test container where EXAPlus CLI and the Exasol ODBC driver are already installed. 	
https://github.com/exasol/integration-test-docker-environment