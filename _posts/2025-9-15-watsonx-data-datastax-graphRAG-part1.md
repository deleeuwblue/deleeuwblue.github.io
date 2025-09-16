---
title: Building GraphRAG with watsonx.data, DataStax and watsonx Orchestrate - part 1
date: 2025-09-15 09:03:00 +/-0000
layout: post
author: [adamdeleeuw, brianinnes]
image: /assets/img/2025-9-15-watsonx-data-datastax-graphRAG/leadspace_article.jpeg
---
DataStax's distributed database and vector capabilities are now [integrated into IBM watsonx.data](https://www.ibm.com/products/datastax). In this two part series, we explore how to use DataStax to build a GraphRAG application, using watsonx.data and watsonx Orchestrate.

## Introduction to GraphRAG

Retrieval-augmented generation (RAG) is a popular pattern for AI applications. RAG primarily relies on semantic similarity searches using a vector store to provide additional context for an LLM, which is prompted to answer the user's question. This approach is very effective for providing LLMs with up to date data, or internal data which is not part of the LLM's training. However, what this approach may lack is a the concept of relationships or structured associations between documents. Even though the knowledge base may contain all the information required to answer the question, the RAG system might not be able to find it all, especially when the information is stored to the vector store as multiple documents, or chunks of documents.

This is where GraphRAG comes in. For example, a semantic search for query "Who developed the theory of relativity?" might result in a document explaining the theory, but without an explicit mention of Albert Einstein. With GraphRAG, the document could be augmented with a "developed by" relationship to another document all about Albert Einstein. 

## A DataStax Approach to GraphRAG

DataStax bring some useful technologies and opinions to help build GraphRAG applications. Take a look at the following blogs:

- [Building Graph-Based RAG Applications Just Got Easier](https://thenewstack.io/building-graph-based-rag-applications-just-got-easier/)
- [Scaling Knowledge Graphs by Eliminating Edges](https://thenewstack.io/scaling-knowledge-graphs-by-eliminating-edges/)
- [Boost LLM Results: When to Use Knowledge Graph RAG](https://thenewstack.io/boost-llm-results-when-to-use-knowledge-graph-rag/)

They explain how RAG can be improved with the introduction of a knowledge graph to better capture the relationships between documents. Certain types of documents are ideal candidates:

- Documents which already link and cross reference each other, including wikis and knowledge bases
- Documents which have sections, definitions of terms and glossaries which provide more complete information

Given documents which are clearly and directly related, it makes sense to ensure the RAG system considers these connections using a knowledge graph. Connections that could be utilised include HTML links from one document to another, references to other sections, and documents that share common terms or keywords.

However, the blogs from DataStax explain how constructing, maintaining and navigating these knowledge graphs can be difficult. It can involve manually extracting structured relationships from the documents and storing them to a dedicated graph database. The blogs advocate recent advancements which simplify these workflows using LLM tools to format documents into graph-ready data, and code libraries which dynamically perform graph-based queries over vector stores where the documents include *meta-data* representing relationships, thus eliminating the need for dedicated graph databases. The next section provides a simple example.

## Tools: LangChain Retriever

The [LangChain Retriever](https://python.langchain.com/docs/integrations/retrievers/graph_rag/?vector-store=astra-db) combines vector and knowledge graph traversal and is compatible with DataStax.

The Langchain sample uses a dataset is based on animals which includes meta-data keywords and entities. In the example below, the aadvark has entities for `number_of_legs` and `habitat`. However, not all animals share the same entities, others may have `origin`, `diet`, `activity` etc.

```python
(id='aardvark', 
    metadata={
        'type': 'mammal', 
        'number_of_legs': 4, 
        'keywords': ['burrowing', 'nocturnal', 'ants', 'savanna'], 
        'habitat': 'savanna', 
        'tags': [{'a': 5, 'b': 7}, {'a': 8, 'b': 10}]}, 
    page_content='the aardvark is a nocturnal mammal known for its burrowing habits and long snout used to sniff out ants.')
```

A vector embedding is created for the `page_content` text using an embeddings service such as watsonx.ai. The text, embedding and meta-data are stored to a vector store, for example DataStax.

A GraphRetriever is defined with a reference to the vector store, and the edges (in this case the meta-data entities `habitat` and `origin`) from which to dynamically generate an in-memory knowledge graph for the specified depth, starting at the most relevant document(s) retrieved with semantic search.

```python
traversal_retriever = GraphRetriever(
    store = vector_store,
    edges = [("habitat", "habitat"), ("origin", "origin")],
    strategy = Eager(start_k=1, k=5, max_depth=2),
)
```

In this case, the `Eager` strategy was applied which selects all discovered nodes at each traversal step. The values have the following meaning:

- `start_k` - the number of similarity search results to retrieve which provide the starting point(s) for traversal
- `k` - the number of nodes to retrieve in each level of traversal
- `max_depth` - the maximum levels of traversal

You can learn more at the [DataStax documentation](https://datastax.github.io/graph-rag/reference/graph_retriever/) for this Langchain component.

For a query "what animals could be found near a capybara?", the results without the knowledge graph include animals such as guinea pigs and boars. With the addition of the `GraphRetriever` approach, the results are improved as all the animals share a `habitat` meta-data value of `wetlands`.

## Tools: DataStax Unstructured

In the animals data above, the meta-data was clearly defined. Accurate meta-data for each document is essential for GraphRAG as it is used to build the knowledge graph. The [Unstructured](https://docs.unstructured.io/welcome) tool enables you to build an automated pipeline to load unstructured data from systems of record, transform it using ETL processes and use custom LLM prompts to automatically generate meta-data key-value pairs.

## DataStax Deployment Options

[IBM watsonx.data](https://www.ibm.com/products/watsonx-data) integrates data fabric and data lakehouse technologies. It can also include Milvus and DataStax databases.

DataStax provides a NoSQL database with vector capabilities. It has historically been available in a couple of form-factors for on-premises deployment (HCD/DSE), or as SaaS via [Astra](https://accounts.datastax.com/). It is possible to provision a free database using Astra if you wish to try out GraphRAG.

## Creating a GraphRAG Agent with watsonx Orchestrate and DataStax

TODO BRIAN

In this section we provide more comprehensive steps to build a DataStax GraphRAG application, and incorporate this into an AI Agent using watsonx Orchestrate. See [part 2](https://deleeuw.me.uk/posts/2025-9-15-watsonx-data-datastax-graphRAG-part2/).
