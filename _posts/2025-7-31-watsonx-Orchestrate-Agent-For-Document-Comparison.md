---
title: watsonx Orchestrate Agent for Document Comparison
date: 2025-07-31 09:00:00 +/-0000
layout: post
author: [adamdeleeuw, brianinnes]
image: /assets/img/2025-7-31-watsonx-Orchestrate-Agent-Document-Comparison/watsonxassistant_lifecycle_1x1_16x9.jpeg
---

## Introduction

This short series of articles provides an introduction to the latest incarnation of watsonx Orchestrate, which has recently been redesigned for all new deployments since July 2025.

* [watsonx Orchestrate Introduction](https://deleeuw.me.uk/posts/watsonx-Orchestrate-Introduction/) covers the key components for building an agent, including defining the agent's purpose, knowledge, tools, flows and behaviour.

* [watsonx Orchestrate Agent Development Kit](https://deleeuw.me.uk/posts/watsonx-Orchestrate-Agent-Development-Toolkit) explains how to install a development environment for local agent development and testing.

* This post provides an end to end example of building a simple agent with tools.

* Finally, [watsonx Orchestrate Publish from ADK to SaaS](https://deleeuw.me.uk/posts/watsonx-Orchestrate-Publish-From-ADK-To-SaaS) shows how to publish a locally developed agent to a watson Orchestrate SaaS tenant, and how users can interact with it.


## Use Case - Document Comparison Task

Our use case relates to software company who use a system integrators to contract in developers to create software components. The software company need to compare their documented in-house coding standards to those of the system integrators. This document comparison task can be automated with a watsonx Orchestrate agent.

The two documents to compare are:
[in-house standards](https://github.com/deleeuwblue/wxo_doc_comparison_agent/blob/main/data/pythonNamingConeventions_SoftwareCompany.docx)
[system integrator](https://github.com/deleeuwblue/wxo_doc_comparison_agent/blob/main/data/pythonNamingConeventions_SystemIntegrator.docx)

## Solution Approach

Development of the agent takes the following steps:

* Installation of the watsonx Agent Development Kit.
* Definition of tools:
  * document_extraction_tool - takes a Word document and converts it to markdown using a Python based tool.
  * reference_coding_standard_tool - returns the master template for coding standards used by the software company. For our sample, the tool simply returns the document as a markdown string. In reality the latest document could be verified in a file sharing system/object storage, and be converted only when the document has changed.
* Definition of the agent which can request the System Integrator coding standard document to compare, convert it to markdown and use AI to make a comparison.
* Publish the agent to a production environment.

## watsonx Orchestrate Agent Development Kit

Please see blog [watsonx Orchestrate Agent Development Kit](https://deleeuw.me.uk/posts/watsonx-Orchestrate-Agent-Development-Toolkit) to learn how to install the ADK, and how it can be used to develop agents and tools.

After installing the latest ADK, it is good practice to set up the following folder structure:

```
.
└── adk-project/
    ├── agents/
    ├── tools/
    ├── knowledge/
    ├── flows/
    └── ...
```

The code for this sample can be found in the [repo](https://github.com/deleeuwblue/wxo_doc_comparison_agent)

## Building the Tools

Create a folder for each tool so that each tool can have a unique `requirements.txt` file:

```sh
mkdir -p tools/reference_coding_standard_tool
mkdir -p tools/reference_coding_standard_tool
```

### reference_coding_standard_tool

This will be implemented as a Python which simply returns some hardcoded markdown as a String.

In the folder `tools/reference_coding_standard_tool` add the following Python file, `reference_coding_standard_tool.py`, which implements the tool:

```python
from ibm_watsonx_orchestrate.agent_builder.tools import tool, ToolPermission

@tool(name="reference_coding_standard_tool", 
      description="This tool returns the software company coding standards document as markdown.", permission=ToolPermission.READ_ONLY)

def get_coding_standards_document() -> str:
  """
  Returns the software company coding standards document as markdown
  
  :returns: software company coding standards document
  """

  return """
  <DOCUMENT MARKDOWN GOES HERE>
  """
```

The important part here is the `@tool` annotation. The description define when the agent should use the tool. The function definition defines the input and output parameters which the agent must send to the tool and receive in response. Notice the Python DocStrings define the variables which is important for the agent to understand what data to gather before calling the tool.

Import the tool using the ADK using this command:

```sh
orchestrate tools import -f tools/reference_coding_standard_tool/reference_coding_standard_tool.py -k python
```

### document_extraction_tool

In the folder `tools/document_extraction_tool` add the following Python file, `document_extraction_tool.py`, which implements the tool:

Import the tool using the ADK using this command:

```sh
orchestrate tools import -f tools/document_extraction_tool/document_extraction_tool.py -r tools/document_extraction_tool/requirements.txt -k python
```

## Building the Agent

This sample uses a single agent which can access the two tools described. The goal is for the agent to prompt for the System Integrator coding standard document, use the tool to convert it to markdown, then use generative AI to make a comparison to the In-House coding standards.

This agent could be created using the Agent Builder UI, but we will use the ADK and a yaml file.

In the folder `agents` add the following yaml file, `coding_standards_comparison_agent.yaml`:

```yaml
kind: native
name: coding_standards_comparison_agent
display_name: coding_standards_comparison_agent
description: 'The coding standards comparison agent compares in-house coding standards to those from a system integrator to determine if they are similar and highlight the differences.'
context_access_enabled: true
context_variables: []
llm: watsonx/meta-llama/llama-3-405b-instruct  
style: default
instructions: |-
  You are a code standard analysis agent working for a software company.

  Instructions:
  You need to compare two coding standards documents:
  1. Upload the coding System Integrator's coding standards document from the user and convert it to Markdown using the document_extraction_tool tool. Do not ask the user for the file, use the tool.
  2. Retrieve the Software Company's In-House coding standards document using the reference_coding_standard_tool tool. Do not ask the user for the file, use the tool.
  3. Compare the two documents
  4. Create a summary showing only the sections which are missing in the System Integrator's coding standards document

  Output:
  Use a markdown table to present only the differences. Do not summarise each document.
guidelines: []
collaborators: []
tools:
- reference_coding_standard_tool
- document_extraction_tool
knowledge_base: []
spec_version: v1
```

Import the agent with this command:

```sh
orchestrate agents import -f agents/coding_standards_comparison_agent.yaml
```

## Testing the Agent

TODO

## Publish to a Production Environment

Please see blog [watsonx Orchestrate Publish from ADK to SaaS](https://deleeuw.me.uk/posts/watsonx-Orchestrate-Publish-From-ADK-To-SaaS) for an overview of production deployments publication from ADK.