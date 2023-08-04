---
title: Motivation
layout: post

[//]: # (post-image: "https://raw.githubusercontent.com/thedevslot/WhatATheme/master/assets/images/SamplePost.png?token=AHMQUEPC4IFADOF5VG4QVN26Z64GG")
description: The motivation of Gentopia.
tags:
- motivation
---

Agent practitioners start to realize the difficulty in tuning a "well-rounded" agent with tons of tools or instructions in a single layer. Recent studies like TinyStories, Specializing Reasoning, Let's Verify SbS, ReWOO, etc. also point us towards an intuitive yet undervalued direction 👉

**An LLM is more capable if you create a context/distribution shift specialized to some target tasks.**

Sadly, there is no silver bullet for agent specialization. For example, you can:
* Simply add Let's think step by step. in your prompt for more accurate Math QA.
* Give a few-shot exemplar in your prompt to guide a better reasoning trajectory for novel plotting.
* Supervise fine-tuning (SFT) your 70B llama2 like this to match reasoning of 175B GPT-3.5.
* Tune your agent paradigm like this demo to easily half the execution time for Seach & Summarize.

Isn't it beautiful if one shares his effort in specialized intelligence, allowing others to reproduce, build on, or interact with it? 🤗 This belief inspires us to build Gentopia, designed for agent specialization, sharing, and interaction, to stackingly achieve collective growth towards greater intelligence..