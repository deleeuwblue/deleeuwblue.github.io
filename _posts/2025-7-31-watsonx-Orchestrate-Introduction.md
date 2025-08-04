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

watsonx Orchestrate provides a low-code approach to defining agents. It is complimented by an Agent Development Toolkit (ADK) which is more developer focused, see blog [watsonx Orchestrate Agent Development Toolkit](https://deleeuw.me.uk/posts/watsonx-Orchestrate-Agent-Development-Toolkit/)

## The Anatomy of an Agent

![anatomyOfAnAgent](/assets/img/2025-7-31-watsonx-Orchestrate-Introduction.md/anatomyOfAnAgent-sml.png)

An agent can be defined using the watsonx Orchestrate no-code Agent Builder, or via a declarative approach (yaml file) using the watsonx Agent Developer Kit. An agent is is defined by:

* A model - foundation models (including LLMs) provide the capabilities for agent to plan, and execute tasks. You can select from the third-party and IBM models hosted on watsonx.ai, or you can connect to models hosted by other providers using a gateway.
* An agent style - how the agent will follow its instructions and behave during tasks. The following styles can be selected:
    * Default - uses the built in reasoning capabilities of the LLM to understand your prompt, determine the best action and make use of tools.
    * ReAct (Reasoning and Acting) - an iterative loop where the agent thinks, acts, and observes continuously.
    * Planner - the agent emphasizes upfront planning followed by stepwise execution.
* Profile - describes what the agent does so other people and other agents know when to use it.
* Knowledge - uploading documents allows the agent follow a RAG pattern. A limited number of documents can uploaded via watsonx Orchestrate, and they are indexed to an in-memory vector database. Alternatively, the agent can connect to dedicated vectors databases such as Milvus, Elasticsearch or others.
* Toolset - [tools](#tools) and [collaborative agents](#collaborative-agents) can be added to help the agent take actions. The difference between tools and agents is really down to granularity. A tool might be a fine grained service which calls a single API that could be reused by multiple agents, whereas an agent would be a more coarse grained 'module'. This is not a new concept to IT architects!
* Behaviour - defines how the agent should react to requests, what its purpose is, what tools it should use and how it should respond.
* Channels - watson Orchestrate provides a web chat UI, but the agent can also be connected to other channels such as Teams, Facebook messenger etc.

## Catalog

![catalog](/assets/img/2025-7-31-watsonx-Orchestrate-Introduction.md/catalog.png)

The IBM watsonx Orchestrate Catalog provides a collection of prebuilt agents and tools to support a wide variety of business functions. At the time of writing there are over 100 agents and 400 tools. The main categories are HR, IT, Procurement, Productivity and Sales. An example of an agent is 'Salesforce Account Management' and example tools would be 'Creating leads in Salesforce', 'Creating opportunities in Salesforce' etc.

## Tools

When building an agent, tools can be added from a number of sources:

* Tools catalog - the pre-built tools described in section [Catalog](#catalog)
* Local instance - tools that have been created in the ADK, i.e. Python based tools, and uploaded to watsonx Orchestrate
* External tool from OpenAPI - upload an OpenAOI specification and select the operations to import as a tool. 
![openAPISpec](/assets/img/2025-7-31-watsonx-Orchestrate-Introduction.md/openAPISpec.png)
![importOpenAPISpec](/assets/img/2025-7-31-watsonx-Orchestrate-Introduction.md/importOpenAPISpec.png)
![addedTools1](/assets/img/2025-7-31-watsonx-Orchestrate-Introduction.md/addedTools1.png)
A clear and detailed tool description helps the AI agent understand when to use the tool, what inputs are required, and how to interpret the output. The description is imported from the OpenAPI specification but can be edited later.
![toolDescription](/assets/img/2025-7-31-watsonx-Orchestrate-Introduction.md/toolDescription.png)
* External tool from an Model Context Protocol (MCP) server - use the MCP open standard to allow allow agents to interact with external tools and data sources through MCP servers. Typically, MCP servers are found in GitHub repositories, for example https://github.com/appcypher/awesome-mcp-servers. The documentation will provide the command to start the MCP server with a simple command, for example ```npx -y time-mcp```. 
![addTimeMCPServer2](/assets/img/2025-7-31-watsonx-Orchestrate-Introduction.md/addTimeMCPServer2.png)
In this example, the npx command directly executes a published JavaScript package ```time-mcp``` without installing it. watsonx Orchestrate supports the installation and execution of Node and Python MCP servers, typically servers that use the npx and uvx commands. Once the MCP server is running, it's tools can be added to the agent.
![addMCPTools2](/assets/img/2025-7-31-watsonx-Orchestrate-Introduction.md/addMCPTools2.png)
![addedTools2](/assets/img/2025-7-31-watsonx-Orchestrate-Introduction.md/addedTools2.png)
* Flows - a set of linked activities, e.g. tools and code blocks, wired together in a pre-determined order. See [Flows](#flows) section for more details.

## Collaborative Agents

When building an agent, collaborator agents can be added from a number of sources:

* Agent catalog - the pre-built agents described in section [Catalog](#catalog)
* Local instance - any agent already created in the watsonx Orchestrate instance
* External agent from third-party platforms - add a connection to an agent which follows the [Agent Connect Framework (ACF)](https://connect.watson-orchestrate.ibm.com/acf/overview). ACF defines standard interfaces and communication patterns for agents to interact with each other and with the watsonx Orchestrate platform. It follows the [OpenAI API](https://platform.openai.com/docs/api-reference/chat) ```v1/chat/completions``` which has become the de-facto standard. The ACF specification is openly documented and can be implemented by anyone. Note that IBM also provides the similarly titled [Agent Connect](https://connect.watson-orchestrate.ibm.com/agent-connect/overview) which is a partner program designed to help ISVs integrate their agents with watsonx Orchestrate, using the aforementioned ACF specification.
* External agent via A2A - this protocol also allows external agents to be added, but it can only be configured by the watsonx Orchestrate ADK. This is similar to the ACF approach mentioned above.
* External agent from watsonx.ai - IBM's watsonx.ai not only provides LLMs, it also serves as a runtime platform for machine learning models, Python deployments, and more recently, agents using it's [AgentLab](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/fm-agent-lab.html?context=wx&pos=2) feature. It has a more pro-code approach to watsonx Orchestrate, and has the flexibility to host agents implemented in common AI agent frameworks, currently limited to LangGraph.
![externalAgents](/assets/img/2025-7-31-watsonx-Orchestrate-Introduction.md/externalAgents.png)
* watsonx Orchestrate assistants - if you're familiar with watsonx Assistant, you'll know it is well established with capabilities to create virtual assistants which follow a prescribed flow. Such virtual assistants often follow a prescribed flow defined using ```Actions``` triggered by example intents, and while they can appear 'intelligent' and use AI features like RAG, they are not agentic. watson Orchestrate now includes the capabilities of watsonx Assistant. If a virtual assistant can handle some of the tasks an agent requires, it can be developed as an assistant and added to the agent.
* watsonx Assistant assistants - watsonx Assistant is still available as a stand-alone offering, and these external assistants can be added to the agent.

![assistant](/assets/img/2025-7-31-watsonx-Orchestrate-Introduction.md/assistant.png)

## Flows

A flow defines a set of linked nodes that are designed to achieve a specific goal. In many cases, while it is technically possible to provide an agent with the necessary tools and let it make it's own plan, this isn't the best approach if the sequence of steps is entirely deterministic. 
![flow](/assets/img/2025-7-31-watsonx-Orchestrate-Introduction.md/flow.png)
A flow has a start and end node, and the following components can be added to the sequence:

* Tools - tools from the local instance can be added to the flow
* Flow controls - provide conditional logic including branches, and coming soon loops and parallel activities.
* Code block - execute Python code using pre-defined imported libraries.
* User activity - capture additional input from the user with chat.
* Document classifier - coming soon.
* Decision - coming soon. Likely to provide capabilities to make decision based on tabular data.
* Document extractor - coming soon. Likely to provide OCR capabilities to extract data from documents using LLM prompts or field locations.
* Generative prompts - coming soon. Likely to provide capabilities to call any LLM accessible via watsonx.ai.
![flow](/assets/img/2025-7-31-watsonx-Orchestrate-Introduction.md/flowNodesComingSoon.png)
Flows also define input and output data. The input fields can be marked as required which ensure the agent will prompt for a value before invoking the flow.
![flow](/assets/img/2025-7-31-watsonx-Orchestrate-Introduction.md/flowInputsAndDescription.png)
Nodes in the flow might require data. This can be mapped from either the flow inputs, or upstream nodes. Data mapping can happen automatically using AI, or can be manually defined using an expression language.
![flow](/assets/img/2025-7-31-watsonx-Orchestrate-Introduction.md/flowDataMapping.png)





