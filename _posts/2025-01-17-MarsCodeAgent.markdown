---
layout: post
title:  "Week 3 - \"MarsCode Agent: AI-native Automated Bug Fixing\""
date:   2025-01-16 01:50:15 -0700
categories: paper review
---

This week brings us back to patching with a very informative paper from ByteDance and Harbin Institute of Technology, Shenzhen:

**MarsCode Agent: AI-native Automated Bug Fixing**

*by Yizhou Liu, Pengfei Gao, Xinchen Wang, Jie Liu, Yexuan Shi, Zhao Zhang, Chao Peng*

#### TL;DR

The authors design and implement a multi-agent system with advanced tools for code retrieval and analysis, aiming to automate fault localization and patching in complex real-world settings.

#### The Problem

Patching bugs and vulnerabilities is critical but often complex, requiring deep contextual understanding of codebases. While LLMs excel in simpler scenarios, real-world codebases demand tools capable of navigating intricate dependencies, relationships, and interactions between components.

#### The Solution

The authors introduce MarsCode Agent, a system with six specialized agents—Searcher, Planner, Reproducer, Tester, Programmer, and Editor—designed to collaborate on fault localization and patching. Each agent is equipped with specific tools, such as a code knowledge graph (to analyze relationships and dependencies within the codebase), language server protocol, file indexing, repository resets, bash command execution, and script execution, enabling them to work effectively in complex real-world settings.

The workflow adapts dynamically based on the complexity of the issue:

* The Searcher starts by analyzing the issue report and gathering relevant code snippets using tools like a code knowledge graph and language server protocol.
* The Planner evaluates this information and determines the repair path:
  * For simpler issues, the Planner assigns the Editor to propose patches. The Editor generates multiple potential solutions, which are normalized using abstract syntax trees (ASTs). An LLM votes to finalize the most appropriate patch.
  * For complex issues, the Planner triggers a dynamic debugging process. The Reproducer creates a script to replicate the bug, and the Tester collects runtime data (e.g., stack traces, program outputs). This information is passed to the Programmer, who iteratively refines a patch. The Tester validates each attempt, using a diff tool to finalize successful patches or returning error details for further refinement.

The paper also delves deeply into code editing with LLMs, emphasizing that traditional methods like generating diffs, specifying line numbers, or rewriting entire files often fail in real-world scenarios. To address this, the authors introduce MarsCode Agent AutoDiff, which uses explicit markers to show both the before and after states of code changes. This tool is paired with a static diagnostics checker for compiler warnings and errors, ensuring stability and accuracy in the generated patches.

MarsCode Agent further benefits from tools like repository resets and bash command execution, making it a robust system for tackling real-world software engineering challenges.

#### The Results

On the SWE-bench Lite benchmark:

* MarsCode Agent patched more bugs and localized patch locations more reliably than baselines (Agentless, Meatless, CodeR).
* Dynamic Path (28% of cases) outperformed Static Path in success rate (38.1% vs 32.4%).

#### My Take

I found this paper incredibly detailed, especially regarding how agents interact, what didn’t work, and the innovative tools they introduced!

However, the evaluation section of this paper is unusually short. I wish the authors had dug deeper into:

* The Planner’s accuracy — e.g., were complex cases incorrectly routed to the static path? I’d be interested how this impacts the overall performance of the system.
* Failures — why did the Programmer sometimes generate incorrect patches?

Lastly, I’d love more clarity on the Planner’s decision-making process for choosing between static and dynamic paths. The paper doesn’t provide detailed information on how the Planner evaluates an issue’s complexity or decides on the most appropriate path. Does the Planner rely on predefined heuristics, specific attributes in the issue report, or runtime trial and error to make this determination? Understanding these criteria would offer valuable insights into the system's adaptability and potential limitations.

