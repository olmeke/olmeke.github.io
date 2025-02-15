---
layout: post
title:  "Week 7 - \"Using AI Assistants in Software Development: A Qualitative Study on Security Practices and Concerns\""
date:   2025-02-14 01:50:15 -0700
categories: paper review
---

The rapidly increasing usage of AI assistants raises new questions for security researchers. This week’s paper examines the current state of security practices in AI-assisted software development and offers insights and recommendations for the future.

**Using AI Assistants in Software Development: A Qualitative Study on Security Practices and Concerns**

*By Jan H. Klemmer, Stefan Albert Horstmann, Nikhil Patnaik, Cordelia Ludden, Cordell Burton Jr., Carson Powers, Fabio Massacci, Akond Rahman, Daniel Votipka, Heather Richter Lipford, Awais Rashid, Alena Naiakshina, Sascha Fahl*

This study investigates three key questions:

1. How do security professionals use AI assistants?
2. What security concerns arise from AI-assisted development?
3. What long-term impact will AI have on secure software development?

#### TL;DR

AI assistants boost efficiency and will soon perform most mundane tasks. Current models need more focus on secure coding practices and overall code quality

#### Methodology

The authors conducted in-depth interviews with 27 professionals in security roles, including engineers, penetration testers, tech leads, and directors. The participants were sourced globally through the researcher’s networks and Upwork. Each interview lasted an average of 55 minutes, following a structured format to ensure consistency. The interviews were transcribed and the transcripts were analyzed according to standard practices.

Additionally, the study analyzed 68 Reddit posts and 122 comments to supplement the interview findings. This provided insight into the broader developer community's experiences with AI assistants.

#### Key Findings: How Developers Use AI Assistants

The study found that ChatGPT and GitHub Copilot dominate the AI assistant space, with productivity and time-saving as the primary motivations for their use. The most common use cases include:

* Threat Modeling: Generating draft threat models
* Vulnerability Detection: Identifying potential bugs in code
* Code Generation: Producing high-quality code drafts with minor modifications required
* Debugging & Research: Replacing search engines and Stack Overflow for quick answers
* Documentation & Task Management: Generating documentation and Jira stories

#### Security Concerns & Risks

Despite their benefits, AI assistants come with significant security concerns:

* Privacy Risks: Developers must avoid leaking sensitive user data or company IP to AI providers.
* Code Quality Issues: AI-generated code often requires rigorous peer review and testing. Code quality especially lacks for harder tasks
* Mistrust: Developers generally distrust AI output but worry that others may accept it without scrutiny.
* Insecure Defaults: AI-generated code does not follow secure coding practices by default.
* Model Poisoning: AI models could be manipulated to intentionally generate insecure code.

#### Company Policies & AI Regulations

Organizations approach AI assistant use differently:

* Some enforce strict bans due to security and quality concerns.
* Others allow usage but enforce privacy compliance, restricting sensitive data from AI prompts.
* A few have adopted custom/local AI models to mitigate privacy risks.
* Model providers implement guardrails, sometimes requiring "jailbreaking" for security-related tasks.

#### Hypothetical Scenarios: AI's Role in Secure Development

The study explored two key debates:

* Who is responsible for AI-generated security flaws? \
Most participants said the developer, but some pointed to company-level quality assurance failures.
* Who writes more secure code: humans or AI?
  * About half favored human developers.
  * Some argued that AI could be better.
  * A few believed a human-AI combination is optimal.

#### The Future of AI in Secure Development

Participants agreed that AI will play a growing role in security, but its impact will evolve over time:

* Short-term security dip: AI-generated code currently lacks security best practices.
* Long-term security improvement: AI-driven vulnerability detection may enhance security in the future.
* Developers' roles will shift: AI will automate mundane tasks, allowing developers to focus on complex challenges.
* AI output must improve: Developers widely agree that AI-generated code needs better quality and security standards.

#### Conclusions: Where Do We Go from Here?

**Mistrust & Productivity Paradox**

Despite widespread mistrust in AI assistants, developers continue using them for their productivity benefits. As AI improves, trust may increase.

**Comparing with Other Studies**

* This study found few real-world security incidents from AI assistants, while other research identified severe security flaws in AI-generated code.
* Participants were not impressed with AI’s security capabilities, in contrast to studies showing AI outperforming humans in some security tasks.

**Impact on Developer Communities**

AI assistants are becoming the go-to research tool, potentially reducing engagement in online communities like Stack Overflow. This may affect the quality of future AI training data, creating a negative loop.

#### The Road Ahead: Challenges & Open Questions

1. AI-generated output requires in-depth verification—a process still lacking standardization with some unsolved problems.
2. Model security and quality must improve to make AI-generated code truly reliable.
3. Prompt engineering is a crucial skill for maximizing AI assistant effectiveness.
4. Privacy-friendly solutions are needed to prevent data leaks.
5. Ethical debate across organizations required regarding AI’s role in security.
6. Future research should expand beyond coding assistants to general-purpose AI tools.

#### My Thoughts

Ultimately, the study confirms what many security professionals working with AI already suspect: AI assistants are powerful but far from perfect. While they boost productivity, their security shortcomings require vigilant human oversight.

While prompt engineering plays a crucial role in AI effectiveness, the study did not account for participants' varying skill levels in crafting prompts. This oversight leaves uncertainty about whether participants’ estimations of AI assistant performance stem from AI limitations or poor prompt formulation.

The participants of the study are mostly engineers and technical people. A broader perspective from management could have added valuable insights, as policies and regulations will shape AI’s future in software security. 

