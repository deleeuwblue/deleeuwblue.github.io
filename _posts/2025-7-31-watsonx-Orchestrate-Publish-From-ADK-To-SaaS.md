---
title: watsonx Orchestrate Publish An Agent From ADK to SaaS
date: 2025-07-31 09:03:00 +/-0000
layout: post
author: [adamdeleeuw, brianinnes]
image: /assets/img/2025-7-31-watsonx-Orchestrate-Agent-Document-Comparison/watsonxassistant_lifecycle_1x1_16x9.jpeg
---
[watsonx Orchestrate Agent Development Toolkit](https://deleeuw.me.uk/posts/watsonx-Orchestrate-Agent-Development-Toolkit/) provides tools to locally develop and test agents, tool and flows. When you are ready to put these agents into the hands of users, you will need a runtime instance of watsonx Orchestrate. This blog briefly describes the deployment options, and how to make the connection between ADK and runtime.

watsonx Orchestrate is available as both an on-premises software offering, and SaaS via IBM Cloud and AWS. With IBM Cloud, the provisioning is initiated from the IBM Cloud Catalog. For AWS, the AWS Marketplace is used to purchase a subscription. Once approved, the owner will receive an email with a link to an IBM the 

The provisioning process differs slightly between 

Accessing an an environment requires an API key:
https://developer.watson-orchestrate.ibm.com/environment/production_import 

The environment can then be added to the ADK and activated:
https://developer.watson-orchestrate.ibm.com/environment/initiate_environment




The ADK must be configured 

watsonx Orchestrate can be provisioned via the IBM Cloud Catalog.



How to publish to SaaS tenant
Users
Chat UI
Other channels?
REST API?
