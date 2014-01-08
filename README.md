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
	* The main use of Elasticsearch is searching, and that is powered by the open source project Lucene. Lucene is an implementation of an inverted index. In a nutshell, an [inverted index](http://en.wikipedia.org/wiki/Inverted_index) is a data structure that maps data (in this case words) to its location (in this case documents). This data structure allows you to quickly find what documents contain certain data.
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
Elasticsearch (and the underlying engine, Lucene) are written in Java and requires [Java 6](http://www.oracle.com/technetwork/java/javase/downloads/index.html) or higher to run. You can download and install Java, or if you have it installed you can easily check your version with the following command:

`$java -version`

That is the only requirement for Elasticsearch.

Installation & Running
----------------------
[Download Elasticsearch](http://www.elasticsearch.org/download) and extract it onto your computer. After that, navigate to the location you installed it and run:

`$bin/elasticsearch -f`

or on a Windows machine:

`bin\elasticsearch.bat`

* You will see information about Elasticsearch starting up and at this point Elasticsearch will be running with default options. In the log messages displayed, you’ll see the IP and port that it is listening on. The port defaults to 9200. Elasticsearch can also be ran as a service, which is desirable when running on a server.

Teesting the New Instance
-------------------------
You can check to see that Elasticsearch is up and running. Once it is started open a new browser window and navigate to [http://localhost:9200](http://localhost:9200).

If everything is working properly you should see a JSON document with information describing the running instance of Elasticsearch.

Sidenote - cURL
---------------
cURL is a command line tool that can be used to transfer data across many protocols such as HTTP. It is often used to quickly interact with RESTful APIs. You will see it in much of the Elasticsearch documentation.

* The examples in this presentation make use of it as will many online examples. cURL is available for many platforms including Windows, Linux and OS X. It is a worthwhile tool to have around.

Documents
---------
Data that is added to Elasticsearch is called a document. Documents are represented in JSON format and you are not required to create a schema before adding it.

* The JSON document can represent different data,types (which Elasticsearch will infer for the schema) as well as nested objects, which is supported.

Indexes
-------
Elasticsearch stores data in an index. Elasticsearch can contain multiple indexes to separate data and an index can also be sharded across multiple nodes in a cluster. A search can span multiple indexes.

Indexes can be created and deleted within Elasticsearch.

`$curl -XPOST http://localhost:9200/library/`

* Indexes correspond to data on disk.
* Elasticsearch Indexes are made of shards which are made  of Lucene indexes
* Multiple indexes and shards allow for data partitioning amongst one or more nodes
* Located under the /data directory

Types
-----
A type is how you keep documents with different schemas separate. As an example, if you had an ecommerce site, you might create an index containing documents of a Product type and also a Review type. Each of these would have different data structures, but could be included together in a search.

* Schemas can be manually created or inferred
* Schemas define data types, analyzers, multifields, etc

Analysis
--------
Documents that are added must be analyzed so that they can be searched for. This analysis work is done by an Analyzer and can be configured per field in the document. 

There are multiple analyzers that are built in and are useful for different circumstances. It is also possible to turn off analysis for a field if you do not want the field to be indexed.

* New analyzers can be created by defining a tokenizer and filter which will be applied to the field.
* Standard - useful for most European languages
* Pattern - allows for text to be separated by regular expressions
* Keyword - indexes for an exact match only - same as not_analyzed

API Basics - CRUD Operations
----------------------------
It is possible to Create, Read, Update, and Delete documents within an index. In addition you can bulk load data into an index.

By default, Elasticsearch will store the entire source of a document in a special field named _source. This is required for certain operations, such as an update.

* Data isn’t actually updated in the index - the source is used to create a new document with the change merged in, and then the original document is removed.

Searching
---------
Once data is added to an index it can be searched for. Searching can be accomplished either through the the URL or via a GET with a JSON body.

The main way to query Elasticsearch is through their Query DSL (domain specific language). The DSL is a JSON document that describes how the search should be put together.

```
{
	“query” : {
		“term” : {
			“title” : “gatsby”
		}
	}
}
```

Search Types
------------
* Term
	* This looks for an exact match of a word in a specific field
* Terms
	* This looks for an exact match of a work in multiple fields
* Match
	* This parses the search term and constructs a query. As an example “great gatsby” would match a field that has “great” or “gatsby”. It is possible to modify the boolean operator to an ‘or’ or add fuzziness
* Multi Match
	* Same as match but looks across multiple fields
* Phrase Match
	* Similar to match, but it works with a phrase instead of breaking up the words
* Query String
	* This allows for the use of a Lucene query. This is handy if you have existing Lucene experience, or if the user wants to construct a complicated query.
* Prefix
	* Searches for matches at the beginning of a word.
* Fuzzy
	* Looks for a close match based on edit distance.
* etc

Filters
-------
Filters are a way to select a subset of data as part of a query. They can be used include or exclude data from the query. They do not impact the scoring of the results (how relevant the result is to the query). They should be used instead of a query if the criteria is not important to the score of the result.

```
{
	“filter” : {
		“bool” : {
			“must” : { 
				“term” : { “user” : “pzerkel” }
			}
		}
	}
}
```

Filter Types
------------
* Term
	* Similar to the Term query
* Terms
	* Similar to the Terms query
* Bool
	* Boolean combinations of other filters
* Range
	* Filters a field to be within a certain range
* Script
	* Allows for a parameterized script to be used as a filter
* etc

