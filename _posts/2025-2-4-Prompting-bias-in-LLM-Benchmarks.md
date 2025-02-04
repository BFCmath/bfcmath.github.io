---
title: Prompting bias in LLM Benchmarks
date: 2025-2-4 21:00:00 +0700  
categories: [Yapping, AI-Questioning]  
tags: [en, llm, benchmark, bias]  
description: Is the prompting bias in LLM benchmarks actually a problem? 
author: BFC  
image:  
  path: assets/local/llm_bias.png  
# render_with_liquid: false  
# toc: false  
comments: true  
---  
> This post contains many subjective opinions based on my personal research and understandings, so please take it with a grain of salt.

## Why am I writing this?

Today, with the arrival of OpenAI DeepResearch with impressive score on Humanity’s Last Exam and the beef between OpenAI and Deepseek users about the performance of these models. I started to think about the bias in LLM benchmarks.

We're always comparing models based on benchmarks, but here's the big question: **Is the prompting bias in LLM benchmarks actually a problem?**

## Simple observation

Imagine you're tackling a debugging task and you're too lazy to fix it yourself. I believe moddern SoFtWaRe EnGiNeErS are likely to use LLMs for help.  So, you type: "Help me fix this code:" followed by the actual code.

Oh, wait. You can also type the code first, then the instruction "Help me fix this code."! 

The question then becomes: "Should the instruction come before or after the code? Does this order affect how well the model performs?"

## Model design bias in LLM

Let's keep going with our debugging scenario. The same task can be formatted differently:

1. **First instruction:** "Help me fix this code:" followed by the code.
2. **Second instruction:** The code is presented first, then the instruction "Help me fix this code."

We know LLMs do some magic before answering, including **padding** and **truncating**. 
+ Padding can be **left-padding** or **right-padding**. 
+ Truncating can be **left-truncation** or **right-truncation**.

If the model uses left-truncation, you'd want to put the instruction first to ensure it doesn't get cut off. But with right-truncation, putting the instruction first might mean it gets ignored, leaving the model clueless about what to do with the code!

So, how you structure your prompt can really make or break the model's performance.

## Prompt design bias in LLM benchmarks

Now, let's talk about benchmarking. Many benchmarks are designed with implicit assumptions about prompt structure. For instance, if authors default to placing instructions after examples (e.g., code-first, task-later), models optimized for that format gain an unfair advantage.

If benchmarks only use one type of prompt structure, they might unfairly favor models optimized for that particular setup. That's not fair, right?

The example with padding and truncation is quite simple, but I bet there are even more complex scenarios that could skew model performance.

## Other types of prompting bias

As I do some research, I realize that the padding and truncation problem is called positional bias. But there are other types of prompting bias that can affect model performance:
+ **Linguistic and Cultural Biases**: Prompts phrased in ways that favor certain linguistic styles or cultural references can lead to outputs skewed towards those cultures or languages
+ **Task Framing Bias**: The way a task is framed in a prompt can influence the model's response. For instance, asking "Why are socially assistive robots effective?" versus "Are socially assistive robots effective?" 
+ **Example Bias in Few-Shot Prompting**
+ ...

## Is this a real problem?

Yes, and I'm not the only one who thinks so. Researchers have been studying this issue and proposing solutions to mitigate prompting bias in LLM benchmarks.

1. [**"Split and Merge: Aligning Position Biases in Large Language Model Evaluators"**](https://openreview.net/forum?id=1hLFLNu4uy)
2. [**"Judging the Judges: A Systematic Study of Position Bias in LLM-as-a-Judge"**](https://arxiv.org/abs/2406.07791)

## Personal thoughts

+ I think a model's performance should be evaluated based on many benchmarks, especially for LLMs.
+ For users, don't just rely on the number from benchmarks. Try it yourself and master prompt engineering!
