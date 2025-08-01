---
title: watsonx Orchestrate Introduction
date: 2025-07-31 09:01:00 +/-0000
layout: post
author: [adamdeleeuw, brianinnes]
image: /assets/img/2025-7-31-watsonx-Orchestrate-Agent-Document-Comparison/watsonxassistant_lifecycle_1x1_16x9.jpeg
---

[IBM watsonx Orchestrate](https://www.ibm.com/products/watsonx-orchestrate) helps to build, deploy and manage AI agents which use generate AI to automating workflows and processes. Agents are defined without the need to write code, and they can call upon tools or other agents to complete their tasks.

If you've looked at watsonx Orchestrate in the past, you will find it has evolved significantly since July 2025 with a simplified experience, new terminology and options for both agentic and deterministic behaviour:

* Agentic - Agents can dynamically plan, and sequence the tools and other agents using AI to fulfil a user's request more flexibly.

* Deterministic - A human defined flow sequences the tools and other agents required using repeatable logic to fulfil a user’s request more predictably.


## The Anatomy of an Agent

![anatomyOfAnAgent](/assets/img/2025-7-31-watsonx-Orchestrate-Introduction.md/anatomyOfAnAgent.png)

An agent can be defined using the watsonx Orchestrate no-code Agent Builder, or via a declarative approach (yaml file) using the watsonx Agent Developer Tool. An agent is is defined by:

* A model - foundation models (including LLMs) provide the capabilities for agent to plan, and execute tasks. You can select from the third-party and IBM models hosted on watsonx.ai, or you can connect to models hosted by other providers using a gateway.
* An agent style - how the agent will follow its instructions and behave during tasks. The following styles can be selected:
    * Default - uses the built in reasoning capabilities of the LLM to understand your prompt, determine the best action and make use of tools.
    * ReAct (Reasoning and Acting) - an iterative loop where the agent thinks, acts, and observes continuously.
    * Planner - the agent emphasizes upfront planning followed by stepwise execution.
* Profile - describes what the agent does so other people and other agents know when to use it.
* Knowledge - uploading documents allows the agent follow a RAG pattern. A limited number of documents can uploaded via watsonx Orchestrate, and they are indexed to an in-memory vector database. Alternatively, the agent can connect to dedicated vectors databases such as Milvus, Elasticsearch or others.
* Toolset - tools and agent can be added to help the agent take actions. The difference between tools and agents is really down to granularity. A tool might be a fine grained service which calls a single API that could be reused by multiple agents, whereas an agent would be a more coarse grained 'module'. This is not a new concept to IT architects!
* Behaviour - defines how the agent should react to requests, what its purpose is, what tools it should use and how it should respond.
* Channels - watson Orchestrate provides a web chat UI, but the agent can also be connected to other channels such as Teams, Facebook messenger etc.

## Catalog

![catalog](/assets/img/2025-7-31-watsonx-Orchestrate-Introduction.md/catalog.png)

## Agents & tools

Tools - add from catalog, local instance, import external tool, create new flow

Agents - add from catalo, local instance, import external agent

## Flows

![flow](/assets/img/2025-7-31-watsonx-Orchestrate-Introduction.md/flow.png)

## Agent Developer Kit

## Assistants

![assistant](/assets/img/2025-7-31-watsonx-Orchestrate-Introduction.md/assistant.png)