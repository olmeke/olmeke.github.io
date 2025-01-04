---
layout: post
title:  "Week 1 - \"Fixing Security Vulnerabilities with AI in OSS-Fuzz\""
date:   2025-01-03 19:50:15 -0700
categories: paper review
---

✨ New Year, New Habit! ✨

To stay up-to-date with the ever-evolving fields of Offensive Security and AI, I’m embarking on a journey to read and share my thoughts on one paper every week. This week’s pick:

**“Fixing Security Vulnerabilities with AI in OSS-Fuzz”** \
*by Yuntong Zhang, Jiawei Wang, Dominic Berzin, Martin Mirchev, Dongge Liu, Abhishek Arya, Oliver Chang, and Abhik Roychoudhury.*

#### TL;DR

Adapting AutoCodeRover (an agent to resolve general github issues) to handle fuzzer inputs and C code results in a surprisingly effective patching agent, CodeRover-S.

#### The problem

Fuzzers are great at finding bugs (this is a good thing!), but they overwhelm developers with large numbers of bug reports. For instance, the authors cite a study revealing that 10% of bugs reported by Google’s OSS-Fuzz remain unresolved after 90 days—leaving software vulnerable to potential exploitation.

#### The solution: CodeRover-S

This paper demonstrates how advances in Generative AI (GenAI) can aid in patching security vulnerabilities. Building on AutoCodeRover (an AI agent designed for resolving generic GitHub issues), the authors adapt it to patch fuzzer-identified bugs through the following contributions:

1️⃣ Leveraging Fuzzer Outputs:
Since the fuzzer already provides a crashing input, the authors simplify the workflow by removing the reproducer generator. However, as other details about the bug are sparse, they introduce dynamic call traces to analyze the execution path and locate buggy code.

2️⃣ Improved Type Handling:
To fix frequent type errors, context about all relevant types is added, improving the patches’ syntactic and semantic correctness.

3️⃣ C/C++ Compatibility:
The original AutoCodeRover was built for Python projects. CodeRover-S incorporates infrastructure to tackle C/C++ projects—the primary focus of OSS-Fuzz.

#### The results

In evaluations using OSS-Fuzz vulnerability data, CodeRover-S significantly outperforms other tools:

* CodeRover-S: 52.6% patching success rate
* VulMaster: 30.9%
* Agentless: <1%

Further analysis by the authors shows that CodeRover-S excels at fixing buffer overflows and bounds-checking bugs but struggles with use-after-free and uninitialized-use vulnerabilities.

Additionally, the authors observed limitations in commonly used metrics like CodeBLEU, which failed to distinguish between effective and ineffective patches—a clear call for better evaluation methods in this space.

#### My Thoughts

This paper beautifully highlights how GenAI can improve security by addressing vulnerabilities faster and more effectively. However, there are a few questions that come to mind:

🤔 **Patch Verification:** Beyond compiling and re-executing the crashing input, what other methods could confirm patch correctness? Options like directed fuzzing, static analysis, or leveraging LLMs for secondary review could be worth exploring.

🤔 **Agent Specialization vs. Generalization**: It’s impressive that a slightly modified general-purpose agent like CodeRover-S performs so well. This makes me excited to see how specialized agents, tailored for specific bug types or languages, might push performance even further. At the same time, the potential of general agents to adapt across diverse challenges is equally inspiring. I’m looking forward to seeing advancements in both directions!

🤔 **Failure Analysis**: Understanding why the agent fails in certain cases is an intriguing area for exploration. What specific bug characteristics or codebase traits make them harder to patch? Could patterns in these failures guide improvements in context or training? While this paper offers some insights, I’m curious how future work might address these questions to further refine AI-based patching.
