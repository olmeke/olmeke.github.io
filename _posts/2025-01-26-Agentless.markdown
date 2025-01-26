---
layout: post
title:  "Week 4 - \"AGENTLESS: Demystifying LLM-based Software Engineering Agents\""
date:   2025-01-26 01:50:15 -0700
categories: paper review
---

## 🔍 Exploring the Power of Simplicity in LLM-Based Software Engineering
Following last week’s deep dive into a highly complex multi-agent system, I came across a fascinating paper that takes the opposite approach:

**AGENTLESS: Demystifying LLM-based Software Engineering Agents**

*By Chunqiu Steven Xia, Yinlin Deng, Soren Dunn, Lingming Zhang*

#### TL;DR

AGENTLESS argues for simple, robust solutions to software engineering challenges, achieving competitive performance to complex agent-based systems while keeping costs low.

#### The Problem 🤔

Most current solutions for automating patching or issue resolution with LLMs rely on Agents—systems that let LLMs control tools and make decisions autonomously. However, agents:

1️⃣ Struggle with limited context windows, making it impossible to capture all repository information and other context.

2️⃣ Are prone to error propagation, where small mistakes snowball into larger ones.

Given these and other limitations, the authors ask: Do we really need this complexity?

#### The Solution 🛠️

Instead of agents, AGENTLESS uses an LLM workflow built around a simple, three-step process:

1️⃣ **Localization:** This step reduces token counts and addresses the limited context size of LLMs through three levels:

* **File-level localization:** The system uses two indicators—(1) presenting the LLM with the file tree and issue report to identify relevant files, and (2) asking the LLM to exclude irrelevant folders, embedding remaining files, and retrieving those similar to the issue description. The union of these approaches identifies the files passed to the next level.
* **Class/Function-level localization:** Content outside function and class definitions, as well as comments, is stripped from each file. The LLM then identifies relevant classes or functions.
* **Line-level localization:** The LLM is presented with the full code of the remaining classes/functions and tasked with pinpointing specific lines needing modification.

2️⃣ **Repair (Patch Generation):** The LLM receives the identified lines of code, along with a few surrounding lines for context, and generates patches in a format similar to MarsCode Agent (see last week’s post). Multiple patch proposals are created for each identified location.

3️⃣ **Patch Validation:** This is a multi-step workflow comprising:

* **Reproducer generation:** Based on the issue description, AGENTLESS generates a test to reproduce the issue and verify fixes.
* **Regression test identification:** From all passing tests in the original repo, AGENTLESS identifies those that should not be affected by the patch.
* **Patch filtering:** Regression tests are run to retain patches with minimal failures. Reproduction tests are then applied to confirm issue resolution. If no patches succeed, the results of this step are ignored.
* **Patch ranking:** Normalized patches (via AST operations) are ranked using majority voting, with the most frequent patch selected.

This approach avoids reliance on tool-calling and decision-making capabilities of the LLM, reducing demands on its performance.

#### The Results 🚀

Compared to 26 open- and closed-source systems on the SWE-lite benchmark, AGENTLESS:

✅ Outperforms all open-source baselines and many closed-source systems for correct patches.

✅ Costs a mere $0.70 per issue, significantly lower than agent-based approaches.

✅ Simplifies workflows, showing that complexity isn’t always necessary.

The authors’ ablation study also reveals some interesting insights:

* Splitting patch locations across prompts performs better than merging them, even if localization suffers slightly.
* Reproduction tests play a surprisingly large role in patch selection.

In their analysis of SWE-lite, the authors identified notable challenges with the dataset itself:

* Around 10% of issues in the dataset lack sufficient information to perform the task.
* 5% of issues contain misleading solution steps in their descriptions.

To address these challenges, the authors created a filtered subset of SWE-lite, removing issues with misleading solutions, insufficient information, or those containing the exact patch. This adjustment excluded 51 problems, but interestingly, the rankings of different approaches remained largely unchanged.

#### My Thoughts 💭

This paper serves as an important reminder to start with simple solutions and see how far they can go before resorting to the “fancy, shiny, new thing.” Over-engineering may not always fit the task at hand, and AGENTLESS proves that simplicity can yield competitive results.

One aspect of the system that stands out is file localization, which relies heavily on filenames. This approach assumes the LLM has pre-training knowledge about the codebase, which might work well for open-source projects but could perform worse on internal or proprietary codebases. To address this, why not generate function/class summaries for suspicious files to give the LLM more context?

The ablation study showing that taking a performance hit in localization can actually improve overall outcomes is intriguing. Specifically, splitting patch locations across prompts, even at the cost of reduced localization accuracy, leads to better patch generation. However, the explanation that “LLMs get confused with too much context” feels a bit vague. It would be great to see more experiments digging deeper into this phenomenon.

The impact of reproduction test cases on patch selection is both logical and surprising. While it makes sense that these tests help identify the best patches, the magnitude of their influence was very surprising to me.

Lastly, I’m not entirely convinced by the dataset adjustments the authors made to SWE-lite. While improving ground truth and result verification is always valuable, handling insufficient or misleading information should be part of the system’s real-world applicability. In practice, developers may provide incomplete or incorrect bug reports. A robust system should detect and ignore misleading solution steps, flag insufficient information, and offer meaningful feedback. These are critical challenges for any system designed for real-world use and shouldn’t be sidestepped through dataset filtering.

Overall, this paper raises important questions about the trade-offs between simplicity and complexity in LLM-powered systems, and it highlights opportunities for further improvement in handling real-world scenarios.

