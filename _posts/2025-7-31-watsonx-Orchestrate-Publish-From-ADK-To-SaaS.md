---
title: watsonx Orchestrate Publish An Agent From ADK to SaaS
date: 2025-07-31 09:03:00 +/-0000
layout: post
author: [adamdeleeuw, brianinnes]
image: /assets/img/2025-7-31-watsonx-Orchestrate-Agent-Document-Comparison/watsonxassistant_lifecycle_1x1_16x9.jpeg
---
[watsonx Orchestrate Agent Development Toolkit](https://deleeuw.me.uk/posts/watsonx-Orchestrate-Agent-Development-Toolkit/) provides tools to locally develop and test agents, tool and flows. When you are ready to put these agents into the hands of users, you will need a runtime instance of watsonx Orchestrate. This blog briefly describes the deployment options, and how to make the connection between ADK and runtime.

## Deployment

watsonx Orchestrate is available as both an on-premises software offering, and SaaS via IBM Cloud and AWS. With IBM Cloud, the provisioning is initiated from the IBM Cloud Catalog. For AWS, the AWS Marketplace is used to purchase a subscription. Once approved, the owner will receive an email with a link to an IBM SaaS Console where the adminstrator can trigger the deployment of a watsonx Orchestrate tenant to the desired AWS region.

Accessing the production watsonx Orchestrate environment from the watsonx Orchestrate ADK requires an API key:
https://developer.watson-orchestrate.ibm.com/environment/production_import 

The production watsonx Orchestrate environment can then be added to the ADK and activated:
https://developer.watson-orchestrate.ibm.com/environment/initiate_environment

## Export Agents and Tools from ADK

Agents and tools can be exported from the ADK to the production watsonx Orchestrate environment, in the same way as they were tested with the local watsonx Orchestrate environment:
https://developer.watson-orchestrate.ibm.com/tools/manage_tool
https://developer.watson-orchestrate.ibm.com/agents/manage_agent

## Publish Agent in Production watsonx Orchestrate Environment

Once deployed to the production watsonx Orchestrate environment, the agent will be in draft. It can be further tested via the Agent Builder UI.
![draftAgent](/assets/img/2025-7-31-watsonx-Orchestrate-Publish-From-ADK-To-SaaS/draftAgent.png)
To make it available to users via the chat UI or other channels, the agent needs to be deployed:
https://www.ibm.com/docs/en/watsonx/watson-orchestrate/base?topic=agents-deploying-agent
During deployment you are reminded to check the connections to any external applications used by the agent and its tools (the agent in the image below does not use any external connections):
![deployAgent](/assets/img/2025-7-31-watsonx-Orchestrate-Publish-From-ADK-To-SaaS/deployAgent.png)
In the chat UI, deployed agents will appear in the list:
![deployedAgents](/assets/img/2025-7-31-watsonx-Orchestrate-Publish-From-ADK-To-SaaS/deployedAgents.png)

## Configure Users and Roles

Managing users in watsonx Orchestrate ensures users can build and interact with AI agents. For this purpose, watsonx Orchestrate defines the roles user, builder and administrator. Learn more [here](https://www.ibm.com/docs/en/watsonx/watson-orchestrate/base?topic=users-roles-watsonx-orchestrate).

For IBM Cloud, access to watsonx Orchestrate is managed by IAM to provide the user with an appropriate access policy using a role. The access policy determines what actions a user can perform within the context of the service (i.e. watsonx Orchestrate) or a specific instance of a service. Each user must have both a 'platform role' and a 'service role'. A platform role defines what actions the user can take at a platform management level, for example creating & deleting instance, adding users etc. Service roles enables users to access the watsonx Orchestrate UI or call the watsonx Orchestrate APIs. The [documentation](https://www.ibm.com/docs/en/watsonx/watson-orchestrate/base?topic=users-managing-cloud#actions-mapped-to-roles) details exactly what actions are mapped to the platform and service roles. IAM can be managed using the IBM Cloud console, CLI or API.

In this example, a user has been service access to watsonx Orchestrate:
![userRoles1](/assets/img/2025-7-31-watsonx-Orchestrate-Publish-From-ADK-To-SaaS/userRoles1.png)
for a specific instance only:
![userRoles2](/assets/img/2025-7-31-watsonx-Orchestrate-Publish-From-ADK-To-SaaS/userRoles2.png)
assigning the following platform and service roles:
![userRoles3](/assets/img/2025-7-31-watsonx-Orchestrate-Publish-From-ADK-To-SaaS/userRoles2.png)

This defines the user access to IBM Cloud.

## Configure webChat UI

The webChat UI can be embedded into a website. Use the following ADK CLI command to generate the a script tag:

```
orchestrate channels webchat embed --agent-name=test_agent1
```

Paste the resulting script between <head></head> elements of a HTML file. 
![generatedScript](/assets/img/2025-7-31-watsonx-Orchestrate-Publish-From-ADK-To-SaaS/generatedScript.png)
The webchat UI will render:
![webChatUI](/assets/img/2025-7-31-watsonx-Orchestrate-Publish-From-ADK-To-SaaS/webChatUI.png)

There are various options to customise the webChatUI and enable security, meaning that user identity can be verified. Assuming the user has been authorised using an identity provider, the browser client can request a signed JWT representing the user's identity from the identity provider, which is passed to watsonx Orchestrate with every request. See the [documentation](https://www.ibm.com/docs/en/watsonx/watson-orchestrate/base?topic=agents-using-webchat) for more details.


