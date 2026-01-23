---
title: watsonx Orchestrate chat via API
date: 2025-07-31 09:03:00 +/-0000
layout: post
author: [adamdeleeuw, brianinnes]
image: /assets/img/2025-7-31-watsonx-Orchestrate-Agent-Document-Comparison/watsonxassistant_lifecycle_1x1_16x9.jpeg
---
watsonx Orchestrate includes a comprehensive [REST API](https://developer.watson-orchestrate.ibm.com/apis/orchestrate-agent/chat-with-orchestrate-assistant) which can be used to build your own chat UI. In this blog you'll learn how to do this for an agent deployed the watsonx Orchestrate Draft or Live environments.

The steps are for IBM Cloud. They will be similar for watsonx Orchestrate on AWS, apart from steps to generate authentication tokens.

## Pre-requisites

* Generate an [IAM API Key](https://www.ibm.com/docs/en/watsonx/watson-orchestrate/base?topic=api-generating-key-cloud)
* Find your watsonx Orchestrate [Instance URL](https://www.ibm.com/docs/en/watsonx/watson-orchestrate/base?topic=api-getting-endpoint)


## Generate a token

```sh
curl -X POST \
 "https://iam.cloud.ibm.com/identity/token" \
 --header "Content-Type: application/x-www-form-urlencoded" \
 --header "Accept: application/json" \
 --data 'grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=<IAM_API_KEY>'
 ```

## List all agents

```sh
curl --location 'https://<INSTANCE_URL>/v1/orchestrate/agents' \
 --header 'Authorization: Bearer <TOKEN>' \
 --header 'Accept: application/json'
```

Alternatively, use:

```sh
orchestrate agents list
```

Make a note your agent's "id".

## Determine all environments for your agent

```sh
curl --request GET \
  --url https://<INSTANCE_URL>/v1/orchestrate/agents/<AGENT_ID>/environment \
  --header 'Authorization: Bearer <TOKEN>'
```

If your agent is deployed to both Draft and Live, there will be two environments listed, each with a id. It's really important to include this when chatting via REST API (or the embedded web client), otherwise you will chat with the unpublished Draft agent by default.

## Chat with agent

```sh
curl --location \
  'https://<INSTANCE_URL>/v1/orchestrate/runs?stream=true&stream_timeout=120000&multiple_content=true' \
  --header 'Authorization: Bearer <TOKEN>' \
  --header 'accept: application/json' \
  --header 'Content-Type: application/json' \
  --data '{"message": {"role": "user", "content": "<Your message to the agent>"}, "agent_id": "<AGENT_ID>", "environment_id": "<ENVIRONMENT_ID>"}'
```

The environment_id is an optional parameter, but it is highly recommended to include it.

On subsequent requests, the --data should include a thread_id from the previous response to maintain context:

```sh
--data '{"message": {"role": "user", "content": "what is the top tourist attraction in London"}, "agent_id": "<AGENT_ID>", "thread_id": "<THREAD_ID>", "environment_id": "<ENVIRONMENT_ID>"}'
```