---
title: How Large Language Models Process Code Comments
date: 2025-3-06 00:00:00 +0700
categories: [Yapping, AI-Questioning]  
tags: [en, llm]  
description: Do LLMs even care about comments in code or are they just skipping it?
author: BFC
image:
  path: assets/local/thought_about_research.png
# render_with_liquid: false
# toc: false
comments: true
---
> This post contains many subjective opinions based on my personal research and understandings, so please take it with a grain of salt.

## Comments in coding
Hey everyone! Let’s talk about something we all deal with in coding: comments. You know, those little notes we leave in our code—like `# This adds two numbers` —to explain what’s going on. For us developers, comments are a lifesaver. They help us figure out what a tricky line does, remind us why we wrote something a certain way, and make it easier for others to jump into our projects. Without them, code can feel like a puzzle with missing pieces, right?

But here’s where things get interesting. I’ve been playing around with Large Language Models (LLMs) lately—you know, tools like GPT-4 or Grok that help us write and understand code. And I’ve noticed something odd. Sometimes, when I ask these models to explain my code, they just skip over the comments like they’re not even there. Other times, when they’re writing code for me, they add comments without me even asking! It got me wondering: what’s going on with LLMs and comments? Do they actually get what those `#` lines mean, or are they just doing their own thing?

So, I decided to dig into this a bit more, and this post is my way of sharing what I’ve found. Hopefully, by the end, we’ll have a clearer picture of this whole comment mystery!

## Some Observations

So, let’s start with what I’ve noticed while messing around with these LLMs. I’ve been testing them out on some coding tasks—nothing fancy, just stuff like explaining snippets or generating functions—and the way they handle comments has been kind of all over the place. Here’s what stood out to me.

First off, I tried asking GPT-4 and GPT-4o to explain some code I wrote. I’d throw in a comment like `# This function adds two numbers` above a simple addition function, expecting them to at least mention it. But nope—they’d just dive straight into what the code does, like the comment wasn’t even there. It’s not that they got it wrong; they explained the code fine. It just felt weird that my little note got totally ignored. I started wondering if they even see those `#` lines or if I’m just talking to myself when I write them.

Then there’s GitHub Copilot, which I use a lot when I’m coding. I noticed something different with it. If I put a regular comment like `# Please make this add two numbers`, it doesn’t seem to care—it’ll generate whatever it wants. But if I switch it up and write something in triple quotes, like `"""This function adds two numbers"""`, suddenly it listens! It’ll churn out a function that matches what I described. That threw me off at first—why does it care about one but not the other? It’s like it’s picky about how I talk to it.

On the flip side, I’ve seen LLMs shine when they’re the ones writing code. Models like GPT, Grok, and Gemini—they’ll whip up a function and toss in comments without me even asking. Like, I’ll say “Write me a function to sort a list,” and they’ll come back with something clean, complete with lines like `# Sort the list in ascending order`. It’s pretty cool, honestly. It makes me think they do get comments, at least when they’re in the driver’s seat.

Now, I’ll be real—these are just my first impressions from playing around. Maybe I missed something, or maybe it depends on how I ask. But it’s got me curious enough to dig deeper. Why do some LLMs skip comments when I want an explanation, but then turn around and write them so naturally? That’s what I’m here to figure out, and it’s where we’ll head next.

## How LLMs Are Trained for Coding Tasks

Okay, now that we’ve seen how LLMs act with comments, let’s dig into what’s happening behind the scenes. How do these models even learn to deal with code—and comments—in the first place? It all comes down to how they’re trained, and there’s some pretty interesting stuff going on here. I’ve been looking into this, and it turns out there are a few key pieces that shape what they do

First up is the training data. These models—like GPT, Grok, or Copilot—get fed massive piles of code from places like GitHub, where developers share their work. That code usually comes with comments, since it’s written by real people who want others (or themselves later) to understand it. But here’s the catch: not all of that code has comments that perfectly match what the code does. [Code Needs Comments](https://arxiv.org/abs/2402.13013) points out that there’s often a shortage of what they call “aligned” code-comment pairs—where the comment actually explains the code spot-on. So, the models are learning from a mix of great comments, outdated ones, or sometimes barely any at all. That mix probably affects how much they lean on comments—or don’t.


Next, there’s this thing called tokenization, which is how they break down code into bite-sized pieces they can process. When I send a line like `# This adds two numbers` followed by `return a + b`, the model doesn’t see it as “comment” and “code” separately. It chops it all up into tokens—little chunks like `#`, `This`, `adds`, and so on—and treats them as one big sequence. That’s cool because it means comments aren’t invisible; they’re part of the input. But it also means the model has to figure out on its own what those `#` chunks mean, based on what it’s seen in training. If it’s not taught to pay special attention to them, it might not.


Then there’s the training process itself, and this is where it gets a bit more serious. These models aren’t just thrown at code and told “good luck.” They’re trained with specific goals in mind. For example, some are tuned to generate code—like when Grok writes a function with comments—because that’s what the data shows: code with explanations. Others, like when GPT-4 explains my code, might be more focused on understanding what the code does, not what I say it does in a comment. That focus comes from fine-tuning, where they tweak the model to get better at certain tasks. If explaining code is the goal, they might prioritize the actual lines that run over the notes I scribbled next to them.

What I’m getting at is that how LLMs handle comments isn’t random—it’s shaped by what they’re fed, how they process it, and what they’re trained to care about. It’s not super strict or formal like a textbook, but it’s definitely a system. And that system might explain why my comments sometimes feel ignored while other times they’re front and center. Let’s unpack that next.

## Why LLMs Exhibit Varied Comment Behaviors

Now that we’ve covered how LLMs are trained, it’s time to tackle the big question: why do they act so differently with comments depending on what I ask them to do? I’ve seen models like GPT-4 skip my comments when explaining code, yet others like Grok happily add them when writing code from scratch. It’s not random—there’s a logic to it, rooted in how these models are built and what they’re trained to focus on.

### Ignoring Comments in Explanations

When I ask GPT-4 or GPT-4o to explain a snippet like `return a + b `with a comment `# This adds two numbers`, they often describe the function’s output—something like “This returns the sum of two inputs”—without ever mentioning my note. Why? It comes down to what they’re trained to prioritize. [Large Language Models for Code Analysis](https://arxiv.org/abs/2310.12357) suggest that when models are fine-tuned for understanding tasks, they focus on the executable code itself. Comments might be part of the input, but they’re treated as secondary. The reasoning makes sense: code defines what actually happens, and comments can sometimes be outdated or wrong. If my comment said `# This multiplies two numbers` by mistake, relying on it could lead to an inaccurate explanation. So, these models seem designed to zero in on the functional truth over whatever I’ve written alongside it.

### Generating Comments in Code
On the flip side, when I ask Grok or Gemini to write code—like “sort a list”—they often come back with something like `# Sort the list in ascending order` above a tidy function. This happens because their training data is full of examples where code and comments go hand in hand. [Code Needs Comments](https://arxiv.org/abs/2402.13013) highlights that models trained on repositories like GitHub pick up patterns of well-commented code. Generating comments isn’t just a bonus—it’s part of what they’ve learned to mimic. They’re not guessing what the code does after writing it; they’re following a template they’ve seen thousands of times. It’s almost like they’ve been taught to “show their work” the way a good developer would.

### Implicit Processing: Do They Secretly Get It?

Here’s where it gets trickier. Even when GPT-4 skips my comments in an explanation, I’ve started to wonder: does it still see them? Research suggests yes. Since comments are tokenized along with code, they’re part of the context the model processes—it’s not like they’re stripped out before the model starts. [Large language models, explained with a minimum of math](https://www.understandingai.org/p/large-language-models-explained-with) talks about how hidden state vectors carry information across the input. So, my `# This adds two numbers` might quietly shape how GPT-4 understands the code, even if it doesn’t say so. It’s not explicit, though—if I want it to use the comment, I’ve found I need to nudge it with something like “Explain this, including the comment.” Without that, it sticks to the code-first approach it’s been tuned for.

### Docstrings vs. Comments: Copilot’s Pickiness

Then there’s Copilot, which threw me for a loop. When I write `# Make this add two numbers`, it often ignores me, but """This function adds two numbers""" gets a perfect response. This isn’t a fluke—it’s about how comments are treated in programming. In Python, triple quotes mark docstrings, which are more than just notes; they’re part of the code’s structure, stored and accessible at runtime. Regular `#` comments, though, are tossed out by the interpreter and don’t carry the same weight.[Best practices for writing code comments](https://stackoverflow.blog/2021/12/23/best-practices-for-writing-code-comments/) points out that tools like Copilot are often trained to see docstrings as instructions, while `#` lines are more like background chatter. It’s a design choice that explains why my wording matters so much with that tool.

What ties all this together is that LLMs don’t treat comments the same way across tasks. Their behavior—ignoring, generating, or subtly using them—comes from a mix of training priorities, data patterns, and how they’re meant to help us. It’s not perfect, and it’s definitely not what I expected at first, but it’s starting to make sense. Next, we’ll see how newer models are handling this and what it might mean down the line.

## Conclusion

Looking at how LLMs handle comments, it’s clear they don’t work the way I first thought. My tests with GPT-4 skipping comments or Copilot favoring docstrings show there’s a system behind it—tied to training data, token processing, and task goals. They’re not blind to comments; they just prioritize differently, focusing on code for explanations and mimicking patterns for generation. It’s a design shaped by what they’re built to do, not a flaw.

This is a great learning experience for me, and I hope it has been informative for you as well. If you have any thoughts or questions, feel free to share them in the comments below!
