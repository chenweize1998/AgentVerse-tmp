<h1 align="center"> 🤖 AgentVerse 🪐 </h1>

<!--
<h3 align="center">
    <p>A Framework for Multi-LLM Environment Simulation</p>
</h3>
-->

<p align="center">
    <a href="https://www.python.org/downloads/release/python-3916/">
        <img alt="Python Version" src="https://img.shields.io/badge/python-3.9+-blue.svg">
    </a>
    <a href="https://github.com/psf/black">
        <img alt="Code Style: Black" src="https://img.shields.io/badge/code%20style-black-black">
    </a>
    
    
</p>

<p align="center">
<img src="./imgs/title.png" width="512">
</p>

**AgentVerse** is designed to facilitate the deployment of multiple LLM-based agents in various applications. AgentVerse primarily provides two frameworks: **task-solving** and **simulation**. 

- Task-solving: This framework assembles multiple agents as an automatic multi-agent system (AgentVerse-Tasksolving, Multi-agent as system) to collaboratively accomplish the corresponding tasks. 
Applications: software development system, consulting system, etc. 


- Simulation: This framework allows users to set up custom environments to observe behaviors among, or interact with, multiple agents.



---


# Contents
- [Contents](#contents)
- [🚀 Getting Started](#-getting-started)
  - [Installation](#installation)
  - [Environment Variables](#environment-variables)
  - [Simulation](#simulation)
    - [Framework Required Modules](#framework-required-modules)
    - [CLI Example](#cli-example)
    - [GUI Example](#gui-example)
  - [Task-Solving](#task-solving)
    - [Framework Required Modules](#framework-required-modules-1)
    - [CLI Example](#cli-example-1)
  - [Local Model Support](#local-model-support)
    - [1. Install the Additional Dependencies](#1-install-the-additional-dependencies)
    - [2. Launch the Local Server](#2-launch-the-local-server)
    - [3. Modify the Config File](#3-modify-the-config-file)




# 🚀 Getting Started

## Installation


**Manually Install (Recommended!)**

**Make sure you have Python >= 3.9**
```bash
git clone xxx.git --depth 1
cd AgentVerse
pip install -e .
```

If you want to use AgentVerse with local models such as LLaMA, you need to additionally install some other dependencies:
```bash
pip install -r requirements_local.txt
```

**Install with pip**

Or you can install through pip
```bash
pip install -U agentverse
```

## Environment Variables
You need to export your OpenAI API key as follows：
```bash
# Export your OpenAI API key
export OPENAI_API_KEY="your_api_key_here"
```

If you want use Azure OpenAI services, please export your Azure OpenAI key and OpenAI API base as follows：
```bash
export AZURE_OPENAI_API_KEY="your_api_key_here"
export AZURE_OPENAI_API_BASE="your_api_base_here"
```

## Simulation

### Framework Required Modules 
```
- agentverse 
  - agents
    - simulation_agent
  - environments
    - simulation_env
```

### CLI Example

You can create a multi-agent environments provided by us. Using the classroom scenario as an example. In this scenario, there are nine agents, one playing the role of a professor and the other eight as students.

```shell
agentverse-simulation --task simulation/nlp_classroom_9players
```

### GUI Example

We also provide a local website demo for this environment. You can launch it with

```shell
agentverse-simulation-gui --task simulation/nlp_classroom_9players
```
After successfully launching the local server, you can visit [http://127.0.0.1:7860/](http://127.0.0.1:7860/) to view the classroom environment.


## Task-Solving 


### Framework Required Modules 
```
- agentverse 
  - agents
    - simulation_env
  - environments
    - tasksolving_env
```

### CLI Example

To run the experiments with the task-solving environment proposed in our paper, you can use the following command:

To run AgentVerse on a benchmark dataset, you can try
```shell
# Run the Humaneval benchmark using gpt-3.5-turbo (config file `agentverse/tasks/tasksolving/humaneval/gpt-3.5/config.yaml`)
agentverse-benchmark --task tasksolving/humaneval/gpt-3.5 --dataset_path data/humaneval/test.jsonl --overwrite
```

To run AgentVerse on a specific problem, you can try
```shell
# Run a single query (config file `agentverse/tasks/tasksolving/brainstorming/gpt-3.5/config.yaml`). The task is specified in the config file.
agentverse-tasksolving --task tasksolving/brainstorming
```

To run the tool using cases presented in our paper, i.e., multi-agent using tools such as web browser, Jupyter notebook, bing search, etc., you can first build ToolsServer provided by [XAgent](https://github.com/OpenBMB/XAgent). You can follow their [instruction](https://github.com/OpenBMB/XAgent#%EF%B8%8F-build-and-setup-toolserver) to build and run the ToolServer.

After building and launching the ToolServer, you can use the following command to run the task-solving cases with tools:
```shell
agentverse-tasksolving --task tasksolving/tool_using/24point
```
We have provided more tasks in `agentverse/tasks/tasksolving/tool_using/` that show how multi-agent can use tools to solve problems.

Also, you can take a look at `agentverse/tasks/tasksolving` for more experiments we have done in our paper.

## Local Model Support
### 1. Install the Additional Dependencies
If you want to use local models such as LLaMA, you need to additionally install some other dependencies:
```bash
pip install -r requirements_local.txt
```

### 2. Launch the Local Server
Then modify the `MODEL_PATH` and `MODEL_NAME` according to your need to launch the local server with the following command:
```bash
bash scripts/run_local_model_server.sh
```
The script will launch a service for Llama 7B chat model.
The `MODEL_NAME` in AgentVerse currently supports several models including `llama-2-7b-chat-hf`, `llama-2-13b-chat-hf`, `llama-2-70b-chat-hf`, `vicuna-7b-v1.5`, and `vicuna-13b-v1.5`. If you wish to integrate additional models that are [compatible with FastChat](https://github.com/lm-sys/FastChat/blob/main/docs/model_support.md), you need to:
1. Add the new `MODEL_NAME` into the `LOCAL_LLMS` within `agentverse/llms/__init__.py`. Furthermore, establish
2. Add the mapping from the new `MODEL_NAME` to its corresponding Huggingface identifier in the `LOCAL_LLMS_MAPPING` within the `agentverse/llms/__init__.py` file.

### 3. Modify the Config File
In your config file, set the `llm_type` to `local` and `model` to the `MODEL_NAME`. For example
```yaml
llm:
  llm_type: local
  model: llama-2-7b-chat-hf
  ...
```

You can refer to `agentverse/tasks/tasksolving/commongen/llama-2-7b-chat-hf/config.yaml` for a more detailed example.


