---
title: How to create agents?
layout: post

[//]: # (post-image: "https://raw.githubusercontent.com/thedevslot/WhatATheme/master/assets/images/What%20is%20Jekyll%20and%20How%20to%20use%20it.png?token=AHMQUELVG36IDSA4SZEZ5P26Z64IW")
description: The quick start on how to create your own agent.
tags:
- create
- agent
---
# Quick Start
The best way to get started with Gentopia is to go through our short demo [tutorials](https://www.youtube.com/playlist?list=PLZ2Bx-k4vH2dyvW9md5SHx4GBQOGoIJWr).

Grab one of the following agent template and store it in `agent.yaml`, then jump to [Assembling Agents](quick_start.md#howto-assemble-your-agent) section. 
See [Agent Components](agent_components.md) for custom configurations.

## Vanilla Agent
`vanilla` agents are plain LLMs instances without any plugin. 
It is useful if you simply want to create context shift, such as granting identities or instructing an LLM to behave in a particular way.

```yaml
# Vanilla agent template
name: vanilla_template
version: 0.0.1
type: vanilla
description: A plain gpt-4 LLM.
target_tasks:
  - anything to do with an LLM 
llm:
  model_name: gpt-4
  params:
    temperature: 0.0
    top_p: 0.9
    repetition_penalty: 1.0
    max_tokens: 1024
prompt_template: !prompt VanillaPrompt

```

## ReAct Agent
`react` agents are by far the most common type of agents. It prompts LLMs to respond in multiple "Thought -> Action -> Observation" cycles.
A stop word "Observation: " is used to detect plugin calls, and "Final Answer: " is used to detect cycle end.
It prompts LLM at each tool-call with all previous history stored in a "scratchpad". 

```yaml
# ReAct Agent Template
name: react_template
version: 0.0.1
type: react
description: A react agent capable of online web search and browsing.
target_tasks:
  - web search
  - web browsing
llm:
  model_name: gpt-4
  params:
    temperature: 0.0
    top_p: 0.9
    repetition_penalty: 1.0
    max_tokens: 1024
prompt_template: !prompt ZeroShotReactPrompt
plugins:
  - name: google_search
  - name: web_page
```

## OpenAI Function Agent
`openai` agents are powered by [OpenAI function-call API](https://openai.com/blog/function-calling-and-other-api-updates).
It uses a similar sequential style as `react` agents, interleaving LLM response and tool-calls. 
According to OpenAI, it is syntax fine-tuned to generate function call message.

```yaml
# OpenAI agent template
name: openai_template
version: 0.0.1
type: openai
description: An openai agent capable of online web search and browsing.
target_tasks:
  - web search
  - web browsing
llm:
  model_name: gpt-4-0613
  params:
    temperature: 0.0
    top_p: 0.9
    repetition_penalty: 1.0
    max_tokens: 1024
prompt_template: !prompt VanillaPrompt
plugins:
  - name: google_search
  - name: web_page
```

## ReWOO Agent
`rewoo` agents are designed to improve efficiency of sequential agent types by prompting LLM to plan ahead and use hypothetical
tool responses to complete the reasoning process. Then the algorithm parses that "blueprint" into a Directed Acyclic Graph (DAG)
and executes it in parallel with topological order.

```yaml
# ReWOO agent template
name: rewoo_template
version: 0.0.1
type: rewoo
description: A rewoo agent capable of online web search and browsing.
target_tasks:
  - web search
  - web browsing
llm:
  Planner:
    model_name: gpt-4
    params:
      temperature: 0.0
      top_p: 0.9
      repetition_penalty: 1.0
      max_tokens: 1024
  Solver:
    model_name: gpt-3.5-turbo
    params:
      temperature: 0.0
      top_p: 0.9
      repetition_penalty: 1.0
      max_tokens: 1024
prompt_template:
  Planner: !prompt ZeroShotPlannerPrompt
  Solver: !prompt ZeroShotSolverPrompt
plugins:
  - name: google_search
  - name: web_page
```

## OpenAI Function Agent (with Memory)
`openai_memory` agents are specialized `openai` agents enabling memory retrieval from vector database.
Abusing index retrieval is highly expensive and lagging, so we generally don't encourage it unless you expect unavoidably long context. 
After tons of tuning, we present this novel [2-level memory]() architecture for fast and robust memmory retrieval.

```yaml
# OpenAI agent template
name: openai_memory_template
version: 0.0.1
type: openai_memory
description: An openai memory agent capable of online web search and browsing in unlimited context length.
target_tasks:
  - web search
  - web browsing
llm:
  model_name: gpt-4-0613
  params:
    temperature: 0.0
    top_p: 0.9
    repetition_penalty: 1.0
    max_tokens: 1024
prompt_template: !prompt VanillaPrompt
plugins:
  - name: google_search
  - name: web_page
memory:
  memory_type: chroma
  conversation_threshold: 2      # first-level memory
  reasoning_threshold: 2         # second-level memory
  params:
    index: main
    top_k: 2
```

## Howto: Assemble your Agent
While it's recommended to use Gentopia in [GentPool](gentpool.md), you may follow instructions below to run in your own python environment:

Suppose you have saved your agent config to `my_agent.yaml`, make sure you have set up `.env` environmental keys similar to [here](installation.md#install-gentpool). Then to chat with your agent in our built-in streaming CLI, run:

```python
import dotenv
from gentopia import chat
from gentopia.assembler.agent_assembler import AgentAssembler

dotenv.load_dotenv(".env")  # load environmental keys
agent = AgentAssembler(file='my_agent.yaml').get_agent()

chat(agent) 
```
To run that agent in your own app
```python
# AgentOutput contains response content, running cost and token usgae.
agent.run("Your instruction or message.")
```
To stream output in your app, inherit [BaseOutput](https://github.com/Gentopia-AI/Gentopia/blob/c26370cf8c9642d2ec101c2d827b56341058b482/gentopia/output/base_output.py#L12)
with your own output handler
```python
oh = Your_OutputHandler()
assert isinstance(oh, BaseOutput)
# Response content streams into your output handler.
agent.stream("Your instruction or message.", output_handler=oh)
```