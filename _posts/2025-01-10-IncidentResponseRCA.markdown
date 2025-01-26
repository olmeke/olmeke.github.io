---
layout: post
title:  "Week 2 - \"Exploring LLM-Based Agents for Root Cause Analysis\""
date:   2025-01-10 01:50:15 -0700
categories: paper review
---

This week, I came across a fascinating paper that bridges offensive security and incident response:

**“Exploring LLM-Based Agents for Root Cause Analysis”**

*by Devjeet Roy, Xuchao Zhang, Rashi Bhave, Chetan Bansal, Pedro Las-Casas, Rodrigo Fonseca, and Saravan Rajmohan*

#### TL;DR

ReAct agents equipped with specialized tools show significant promise for zero-shot out-of-domain root cause analysis (RCA)—reducing costs and manual labor for cloud incident response.

#### The Problem

Root cause analysis (RCA) for incident response is a time-consuming and costly process for cloud providers, requiring skilled engineers with team-specific experience that can take years to develop. Engineers must collect additional diagnostic data dynamically, and use deep domain knowledge to identify and address root causes, making the process labor-intensive and difficult to automate. How can the reasoning capabilities of LLMs be leveraged to reduce this burden while maintaining accuracy?

#### The Solution

The paper explores the use of ReAct agents, which are agents designed to interleave reasoning (thinking) with tool usage (acting). ReAct agents are instructed to first think through a problem, then call an external tool, observe the result, and either continue reasoning or provide a final answer. This iterative process allows for more sophisticated reasoning and improved performance compared to simple tool-using models.

The authors design the following tools

* (generic) Q&A tool about the incident: Allows the agent to query specific details in Q&A-style instead of relying on summaries that might exclude relevant information.
* (generic) Historical incident data retrieval: Enables similarity-based retrieval using incident titles and descriptions, with results either provided directly to the agent or summarized by another LLM.
* (team-specific) Database query tool: Uses an SQL-like language and includes Q&A capabilities through another LLM for interpreting query results.
* (team-specific) Internal knowledge base article (KBA) Q&A tool: Leverages a vector store and LLM to query team-specific knowledge bases.
* (team-specific) Planning tool: Similar to the Q&A tool but explicitly encourages the agent to devise high-level plans.
* (team-specific) Human interaction tool: Allows the agent to request information or assistance from a human engineer.

This set of tools demonstrates the distinction between the general capabilities of the agent, provided by generic tools, and what a specialized version tailored to a specific team’s needs could look like, as explored in the evaluation section.

#### The Results

Evaluation was conducted on a dataset of over 100k internal Microsoft incidents, with ReAct agents tested in both general and team-specific settings. Key findings include:

* Automated Metrics: In the general setting, ReAct agents underperformed compared to baselines (RAG, CoT, and IR-CoT) across all automated metrics (both semantic and lexical). Notably, these metrics were critiqued in last week’s paper for potentially being insufficiently meaningful, as they fail to reasoning traces.
* Manual Evaluation: ReAct agents significantly reduced hallucinations compared to baselines, demonstrating better factual accuracy, even if this was not fully reflected in the automated scores.
* Case Study: In the team-specific setting, the agent reliably performed simpler RCA tasks autonomously but required human assistance for complex cases. For more complex scenarios, the agent showed strong planning abilities but often ran out of iterations before completing tasks, highlighting the need for scalable long-term memory. The case study also emphasized the critical role of knowledge base articles (KBAs) for accessing team-specific information and enabling effective RCA workflows.

#### My Thoughts

This paper highlights the potential of general-purpose agents with minimal adjustments—primarily customizing the tools and providing more context/budget for the agent to operate. However, since the general setting didn’t perform as well, it seems that many teams will need to maintain their own specialized versions. I’m unclear on how much effort this would require per team, and a comment from the authors on the scalability of such an approach would have been very interesting.

That said, the tools described in the paper seem like they could be generalized to define a core set of functionalities, making it more about configuring the tools for each team’s specific data sources rather than implementing entirely new solutions.

Additionally, the inclusion of a dedicated planning tool is intriguing, but it feels like a "backdoor" approach. A more explicit integration of long-term planning and mechanisms to update plans dynamically could enhance the agent’s capabilities.

Finally, the limitations caused by the agent running out of actions in complex cases could likely be addressed with newer LLMs that offer longer context windows. Re-evaluating this approach with such models would be interesting to explore its true potential.

