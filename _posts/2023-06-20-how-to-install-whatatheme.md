---
title: How to Install and use Gentopia?
layout: post
post-image: https://gentopia-ai.github.io/Gentopia-AI-Homepage/assets/images/Install.png
description: This post will guide you to install Gentopia, follow the easy steps to set it up.
tags:
- how to
- setup
---

# Installation
Please follow the instructions below to set up Gentopia and GentPool on your system.
We recommend using a virtual environment.
```bash
conda create --name gentenv python=3.10
conda activate gentenv
```

## Install Gentopia from PyPI
To install the basic framework, 
```bash
pip install gentopia
```
Additionally, if you want to use open LLMs like `llama` on huggingface, together with 8-bit/4-bit quantization tricks, install with 
```bash
pip install gentopia[huggingface]
```
NOTE:  We are still in early development (gentopia `v0.x.x` is considered early access), you may want to frequently 
```bash
pip install --upgrade gentopia
```

## Install Gentopia from source
Alternatively, if you are a true hacker who modifies the source code frequently, we recommend installing from source.
```bash
git clone git@github.com:Gentopia-AI/Gentopia.git
cd Gentopia
pip install -e .
```

## Install GentPool
We recommend using GentPool as a space to build your agent because you can easily call other public agents for interaction, 
and access our unique benchmark eval to test your agent.
```bash
git clone git@github.com:Gentopia-AI/GentPool.git
```
Create a .env file under GentPool (ignored by git) and put your API Keys inside. They will be registered as environmental variables at run time.
```bash
cd GentPool
touch .env
echo "OPENAI_API_KEY=<your_openai_api_key>" >> .env
echo "WOLFRAM_ALPHA_APPID=<your_wolfram_alpha_api_key>" >> .env
```
... and so on if you plan to use other service keys.

## Download public GentBench data

GentBench is our unique benchmark eval for agents. It tests a wide range of agent capability beyond vanilla LLMs.
See [here]() for more details.

GentBench is half-public and half-private (both will be updated and expanded). 
You have full access to the public data for testing, fine-tuning or so, but private benchmark will only be used to evaluate agents registered in GentPool.
This prevents overfitting and gives you a sense of generalizability.
To download the public benchmark, make sure you've installed [Git-LFS](https://git-lfs.com/) and GentPool, then
```bash
cd GentPool
git lfs fetch --all
git lfs pull
```
Then you will see downloaded tasks under `GentPool/benchmark/`.
