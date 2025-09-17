---
title: watsonx Orchestrate Publish An Agent From ADK to SaaS
date: 2025-07-31 09:03:00 +/-0000
layout: post
author: [adamdeleeuw, brianinnes]
image: /assets/img/2025-7-31-watsonx-Orchestrate-Agent-Document-Comparison/watsonxassistant_lifecycle_1x1_16x9.jpeg
---
[watsonx Orchestrate Agent Development Kit (ADK)](https://deleeuw.me.uk/posts/watsonx-Orchestrate-Agent-Development-Toolkit/) is used to locally develop and test agents, tools and flows. When it's time to provide completed agents to end users, a runtime instance of watsonx Orchestrate is required. This blog briefly describes the watsonx Orchestrate deployment options, and how to publish from the ADK.

## Deployment

watsonx Orchestrate is available as both an on-premises software offering, and SaaS via IBM Cloud and AWS. With IBM Cloud, the provisioning is initiated from the IBM Cloud Catalog. For AWS, the AWS Marketplace is used to purchase a subscription. Once approved, the owner will receive an email with a link to an IBM SaaS Console where the adminstrator can trigger the deployment of a watsonx Orchestrate tenant to the desired AWS region.

Accessing the production watsonx Orchestrate environment from the watsonx Orchestrate ADK requires an API key:
[https://developer.watson-orchestrate.ibm.com/environment/production_import](https://developer.watson-orchestrate.ibm.com/environment/production_import)

The production watsonx Orchestrate environment can then be added to the ADK and activated:
[https://developer.watson-orchestrate.ibm.com/environment/initiate_environment](https://developer.watson-orchestrate.ibm.com/environment/initiate_environment)

For example, this screenshot shows two ADK environments, with a SaaS runtime being active:
![adkEnvs](/assets/img/2025-7-31-watsonx-Orchestrate-Publish-From-ADK-To-SaaS/adkEnvs.png)

## Export Agents and Tools from ADK

Agents and tools can be exported from the ADK to the production watsonx Orchestrate environment. This is a similar process to testing with a local watsonx Orchestrate environment:

[https://developer.watson-orchestrate.ibm.com/tools/manage_tool](https://developer.watson-orchestrate.ibm.com/tools/manage_tool)

[https://developer.watson-orchestrate.ibm.com/agents/manage_agent](https://developer.watson-orchestrate.ibm.com/agents/manage_agent)

## Publish Agent to Production watsonx Orchestrate Environment

Once deployed to the production watsonx Orchestrate environment, the agent will be in draft. It can be further tested via the Agent Builder UI.
![draftAgent](/assets/img/2025-7-31-watsonx-Orchestrate-Publish-From-ADK-To-SaaS/draftAgent.png)

To make it available to users, the agent needs to be deployed to 'live':
[https://www.ibm.com/docs/en/watsonx/watson-orchestrate/base?topic=agents-deploying-agent](https://www.ibm.com/docs/en/watsonx/watson-orchestrate/base?topic=agents-deploying-agent)

During deployment you are reminded to check the connections to any external applications used by the agent and its tools (the agent in the image below does not use any external connections):
![deployAgent](/assets/img/2025-7-31-watsonx-Orchestrate-Publish-From-ADK-To-SaaS/deployAgent.png)

Deployed agents will appear in the list via the Chat screen, which is also used for testing:
![deployedAgents](/assets/img/2025-7-31-watsonx-Orchestrate-Publish-From-ADK-To-SaaS/deployedAgents.png)

## Configure webChat UI

End users can access the watsonx Orchestrate agents via a variety of channels including web, Teams or Slack. For example, the webChat UI can be embedded into a website. Use the following ADK CLI command to generate the a script tag:

```sh
orchestrate channels webchat embed --agent-name=test_agent1
```

Paste the resulting script tag between <head></head> elements of a HTML file. 
![generatedScript](/assets/img/2025-7-31-watsonx-Orchestrate-Publish-From-ADK-To-SaaS/generatedScript.png)

The JavaScript is intended to be hosted via a webserver and may not work if you try to load it directly from the filesystem using a browser (depending on the browser's security restrictions). A fail-safe way to test is to launch a local webserver, for example:

```sh
cd <path-to-html-folder>
python3 -m http.server 1996
```

Direct your browser to ```localhost:1996``` and the webchat UI will render:
![webChatUI](/assets/img/2025-7-31-watsonx-Orchestrate-Publish-From-ADK-To-SaaS/webChatUI.png)

There are various options to customise the webChatUI for layout and colour. See the [documentation](https://www.ibm.com/docs/en/watsonx/watson-orchestrate/base?topic=agents-using-webchat) for more details.


Note the webChatUI is not providing authentication, this is something the hosting website would take care of. There are steps in the documentation for passing data between client and server securely, but that's something that will be covered in another blog.
