---
title: Building GraphRAG with watsonx.data, DataStax and watsonx Orchestrate
date: 2025-09-15 09:03:00 +/-0000
layout: post
author: [adamdeleeuw, brianinnes]
image: /assets/img/2025-9-15-watsonx-data-datastax-graphRAG/leadspace_article.jpeg
---
DataStax's distributed database and vector AI capabilities are now [integrated into IBM watsonx.data Premium](https://www.ibm.com/products/datastax). This blog explores how to use DataStax to build a GraphRAG application, using watsonx.data and watsonx Orchestrate.

## Introduction to GraphRAG

What is it, why is is better than RAG.

Link to this article: https://thenewstack.io/building-graph-based-rag-applications-just-got-easier/

## DataStax Approach

Quick summary and link to this article: https://thenewstack.io/scaling-knowledge-graphs-by-eliminating-edges/

Basically, not storing the links is very efficient.

## Tools: DataStax Unstructured & LangChain Retriever

DataStax Unstructured 
https://docs.unstructured.io/welcome
In this example, links are used + unstructured: https://thenewstack.io/building-graph-based-rag-applications-just-got-easier/

LangChain Retriever combining Vector and Graph traversal
https://python.langchain.com/docs/integrations/retrievers/graph_rag/?vector-store=astra-db
In this example, entities are used + LangChain Retriever.

## Creating a GraphRAG Agent with watsonx Orchestrate and DataStax

Use the movie example and llangchain retriever: https://datastax.github.io/graph-rag/examples/movie-reviews-graph-rag/#the-strategy

Create API endpoint using LangChain Retriever example.
Create a flow which takes the user query, calls the API, then calls a prompt node with the context.

## DataStax Deployment Options

Quick explanation of Astra, on-prem versions, watsonx.data

SKIP THIS - Possible mention of watsonx RAG Accelerator?
https://community.ibm.com/community/user/blogs/thomas-schaeck/2024/09/04/qa-with-rag-accelerator-on-watsonxai


## Next Steps?

--> example links
![linkToImage](/assets/img/2025-7-31-watsonx-Orchestrate-Publish-From-ADK-To-SaaS/draftAgent.png)
[https://link](https://link)