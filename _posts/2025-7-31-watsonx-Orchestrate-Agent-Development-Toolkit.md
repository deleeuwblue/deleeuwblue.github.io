---
title: watsonx Orchestrate Agent Development Kit
date: 2025-07-31 09:02:00 +/-0000
layout: post
author: [adamdeleeuw, brianinnes]
image: /assets/img/2025-7-31-watsonx-Orchestrate-Agent-Document-Comparison/watsonxassistant_lifecycle_1x1_16x9.jpeg
---

## Orchestrate Development

As seen in the previous blogs in this series, watsonx Orchestrate Agents and Tools can be developed using the web user interface (UI).  However, the UI doesn't provide access to all features of Orchestrate.  In addition, working in the UI does not allow a more structured, change controlled development practice that many enterprises require.

The [IBM watsonx Orchestrate Agent Development Kit](https://developer.watson-orchestrate.ibm.com) (ADK) is a toolkit that allows you to develop and test agentic applications.  There are two primary parts to the ADK:

- Command Line Interface (CLI)
- watsonx Orchestrate Developer Edition

### ADK Command Line Interface

The CLI allows you to manage a watsonx Orchestrate instance.  This can be a SaaS instance running on AWS or the IBM Cloud, an instance running on premises or the Developer Edition.

![CLI options](/assets/img/2025-7-31-watsonx-Orchestrate-Agent-Development-Toolkit/adk_cli.png)

### IBM watsonx Orchestrate Developer Edition

The Developer Edition is a version of IBM watsonx Orchestrate that runs locally to allow development and testing of agentic applications.  

You need to have access to either watsonx Orchestrate or watsonx.ai to get the credentials needed to install the Developer Edition. It uses container technology to run.  At the time of writing the supported container runtimes are:

- [Docker](https://docs.docker.com/engine/install/) : the preferred runtime for Linux and Windows- using Windows Subsystem for Linux.  [Docker Desktop](https://docs.docker.com/get-started/introduction/get-docker-desktop/) can also be used, but many enterprise users no longer have access to Docker Desktop as it requires a paid subscription.
- [Colima](https://github.com/abiosoft/colima) : an alternate option for MacOS users
- [Rancher Desktop](https://www.rancher.com/products/rancher-desktop) : an alternate option for MacOS users

[Docker compose](https://docs.docker.com/compose/install/) is also required.  The [documentation](https://developer.watson-orchestrate.ibm.com/getting_started/wxOde_setup) provides further details on installing the Developer Edition

Once the prerequisites are installed and the [environment variables declared](https://developer.watson-orchestrate.ibm.com/getting_started/wxOde_setup#pulling-and-installing-watsonx-orchestrate-developer-edition) are set, the `orchestrate` CLI will pull the containers and start the Developer Edition.

## Managing watsonx Orchestrate

The ADK CLI can be used to [add multiple environments](https://developer.watson-orchestrate.ibm.com/environment/initiate_environment#creating-an-environment) to the ADK, and then [activate one of the environments](https://developer.watson-orchestrate.ibm.com/environment/initiate_environment#activating-an-environment).  The active environment is the environment that all ADK commands will be sent to.

The ADK is used to [start](https://developer.watson-orchestrate.ibm.com/environment/manage_local_environment#starting-the-server) and [stop](https://developer.watson-orchestrate.ibm.com/environment/manage_local_environment#stopping-the-server) the local Developer Edition and for [launching the web UI](https://developer.watson-orchestrate.ibm.com/environment/manage_local_environment#launching-the-local-ui). 

Instead of working in the web UI, agents and tools can be created in an editor before using the ADK to import the assets into the active watsonx Orchestrate instance.  This allows for version control of agents and tools.

If there are already agents and tools defined in watsonx Orchestrate, they can be exported using the web UI to be subsequently managed using the ADK.

The ADK offers some capabilities that are not available in the web UI :

- [adding credentials](https://developer.watson-orchestrate.ibm.com/connections/overview) to tools that use OpenAPI and external agents.  This capability is available from the Settings link at the bottom of the web UI on a production installation of IBM watsonx Orchestrate, but ADK is the only way to add credentials to the Developer Edition.
- [importing additional models](https://developer.watson-orchestrate.ibm.com/llm/managing_llm) for agent development.
- [copilot assistance](https://developer.watson-orchestrate.ibm.com/copilot/overview) which uses AI help guide the creation of the prompts used to defined the agent's behaviour.
- [evaluation framework](https://developer.watson-orchestrate.ibm.com/evaluate/overview) to help verify agent behaviour by comparing to a ground truth data set.

### Monitoring IBM watsonx Orchestrate Developer Edition

The developer edition has monitoring capability that can be enabled by adding command line options to the `orchestrate start` command.  There is `-l` or `--with-langfuse` to use Langfuse or `-i` or `--with-ibm-telemetry` to use IBM's native observability framework.  Only one of these switches can be used but they provide an additional web user interface where the behaviour of agents can be inspected.  

![Langfuse example](/assets/img/2025-7-31-watsonx-Orchestrate-Agent-Development-Toolkit/langfuse.png)

These monitoring capabilities can provide insight into how an agentic application is working.  It shows the data passed from agent to agent or agent to tool, which helps debug and tune the applications to deliver the required functionality.

### Staying up to date

Development of IBM watsonx Orchestrate and the ADK is ongoing with updates being regularly delivered, so it is important to stay up to date.

You can check the version of the ADK using `orchestrate --version` and compare it to the [available releases](https://pypi.org/project/ibm-watsonx-orchestrate/#history) and upgrade to the latest using command `pip install --upgrade ibm-watsonx-orchestrate==<required version>`.

After upgrading you will need to restart the Development Edition with `orchestrate server stop` and `orchestrate server start -e .env -d -l` or with your preferred command line options.
