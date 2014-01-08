An Overview of Elasticsearch and Nest
=====================================

This is a lunch presentation on the Elasticsearch project as well as Nest, 
a client library for .NET. Notes for the presentation are below.

Overview
--------
What is Elasticsearch?

At its core, Elasticsearch is an open, distributed, and document oriented full text search engine that indexes data in real time through a RESTful API.

* In other words, Elasticsearch is an application that allows for the indexing and advanced searching of structured information.

Features
--------
* Full text search engine
	* The main use of Elasticsearch is searching, and that is powered by the open source project Lucene. Lucene is an implementation of an inverted index. In a nutshell, an (inverted index)[http://en.wikipedia.org/wiki/Inverted_index] is a data structure that maps data (in this case words) to its location (in this case documents). This data structure allows you to quickly find what documents contain certain data.
* Open source
	* Elasticsearch is licensed as open source under the Apache 2 license. This is a permissive license that allows for commercial use and modification without royalties to the original authors.
* Distributed
	* Elasticsearch can grow horizontally as needed by adding additional hardware nodes. Additionally, nodes can cluster aware which allows for high availability. Data is distributed across nodes as new hardware comes online.
* Document oriented
	* When you have data to index in Elasticsearch you send it a document object in JSON format. This allows for multiple fields and relationships between data. Elasticsearch will infer a schema from the JSON you send it, but you can also customize that schema to enforce rules or change indexing and searching behavior.
* RESTful API
	* Elasticsearch runs as a service on one or more servers. It provides a REST based interface for interacting with the service. Anything that can handle standard HTTP verbs can talk to Elasticsearch! To simplify interacting with Elasticsearch, there are a number of client libraries available, including one for .NET.
* Real time
	* As data is added it is immediately available as the results of the search. Its overall speed makes it great for use on modern web applications.

Requirements
------------
Elasticsearch (and the underlying engine, Lucene) are written in Java and requires (Java 6)[http://www.oracle.com/technetwork/java/javase/downloads/index.html] or higher to run. You can download and install Java, or if you have it installed you can easily check your version with the following command:

`$java -version

That is the only requirement for Elasticsearch.
