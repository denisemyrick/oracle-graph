# Oracle Graph

Graph is a native feature of Oracle Database.  Graphs help you

- Discover connections and explore relationships in data using graph queries to identify patterns
- Analyze your data with graph algorithms such as ranking, centrality, community detection and path finding, to uncover new insights based on how your data is connected
- Create a semantic layer represented as a knowledge graph to help capture better context about your data for AI and AI agents
- And much more.

Best of all, you can create graphs from your existing business data in relational tables, without moving data to a specialized database.   As a first class object in Oracle Database graphs benefit from scalability, security, high availability, disaster recovery, and other enterprise features.

Developers, data scientists and analysts can use graphs to address various use cases, including financial fraud detection, manufacturing traceability, recommendation systems, semantic data catalogs, and more.

Oracle Graph includes support for

- Property Graph: Create and query graphs using SQL (SQL:2023 standard includes syntax for property graphs)
- RDF Graph: Create, query and run entailment on RDF graphs using the W3C standards (RDF, RDFS, OWL, SPARQL)

## Get started with Graphs in Oracle AI Database 26ai

### Property Graph

All you need to get started is an Oracle AI Database.  This can be a developer instance (such as any of the Database Free options) or an enterprise instance.  You can create graphs from any kind of data stored in Oracle AI Database tables.

Property graphs are very helpful in following the flow of “things” in your data: The flow of money among a set of bank accounts, the flow of goods in a supply chain, the flow of variables in software.  They are also very helpful in representing hierarchy in data so that you can capture dependencies, such as which components a particular product depends on.

In this example we provide two files to represent a BANK GRAPH dataset, BANK_ACCOUNTS.csv and BANK_TRANSFERS.csv.  This script shows you how you can create the tables for this data.  You can run the SQL statements in your script from your favorite SQL tool.  After creating the tables load data using your favorite data load tool.

Once the data is loaded the script sets the primary key and foreign key constraints in the two tables.

Then you are ready to create a graph (as in the script) and run some graph queries!

Graphs enable you to find connections and explore relationships in your data. Oracle Graph is an AI-ready, integrated feature of Oracle's converged database that eliminates the need for a separate graph database and data movement. Analysts and developers can address various use cases, including financial fraud detection and manufacturing traceability, while gaining enterprise-grade security, ease of data ingestion, and strong support for operational workloads.

#### Graph Algorithms

Oracle Graph feature includes a specialized graph server (PGX) that can run graph algorithms in parallel at high speed.  A wide range of [algorithms](./built-in-algorithms/README.md) are available through Java and Python APIs.

### RDF Graph

RDF graphs are governed by standards from the W3C.  They are useful to have a formal way of representing knowledge in a domain, for automatically inferring new facts from existing facts, and managing rules about data in the data itself, rather than in application code.  Popular serialized representation of RDF graph is the “triple” format, where each relationship in the graph is represented as \<subject\> \<predicate\> \<object\>.  For example, the fact that John is a graduate student can be represented as

    <http://example.oracle.com/data#John>
    <http://www.w3.org/1999/02/22-rdf-syntax-ns#type>
    <http://example.oracle.com/onto#GradStudent>

And a fact about the data that all graduate students are students can be represented as
    <http://example.oracle.com/onto#GradStudent>
    <http://www.w3.org/2000/01/rdf-schema#subClassOf>
    <http://example.oracle.com/onto#Student>

Oracle Graph has comprehensive support for the W3C standard, including RDF, RDFS, OWL, and the query language SPARQL.

## Get started with Graphs in Oracle Database 19c

### Property Graph

In Oracle Database 19c, you can use PGQL to run graph queries.  PGQL 2.0 is identical in syntax to SQL, but requires client libraries to parse the graph query and translate it into a query that can run in the database.  (In Oracle AI Database 26ai, graph queries can be in SQL, so client libraries are not required.)
[26ai Graph Demos](./26ai-graph-demos)

### RDF Graph

RDF graphs have been supported in Oracle Database since the W3C created these standards in the mid-2000s.  
Refer to the <section on RDF Graphs in Oracle AI Database 26ai> for more details.

## Contributing

This project welcomes contributions from the community. Before submitting a pull request, please [review our contribution guide](./CONTRIBUTING.md)

## Security

Please consult the [security guide](./SECURITY.md) for our responsible security vulnerability disclosure process

## License

Copyright (c) 2019, 2026 Oracle and/or its affiliates.

Released under the Universal Permissive License v1.0 as shown at
<https://oss.oracle.com/licenses/upl/>.
