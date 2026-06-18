---
title: "Agentic Build Day and Community Activity"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

<!--{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy it verbatim** into your report, including this warning.
{{% /notice %}}-->

# Summary Report: “Agentic Build Day and Community Activity”

### Event Objectives

- Provide a 3-hour technical deep-dive into AWS's **agentic services**
- Walk through a real-world reference architecture: an **AI-powered marketing campaign platform** that orchestrates multiple specialized agents to automate audience segmentation, campaign creation, and promotions end to end
- Give attendees a working mental model of how AWS agentic services fit together
- Combine technical content with a hands-on **community activity** (making no-sew plush toys for children)

### Speakers & Hosts

- **Diem My / Dinh Nguyen** – Community Activity hosts
- **My Nguyen** – Why Agentic AI & the MarTech use case
- **Dai Truong / Thi Nguyen** – Kiro Workshop Guidance
- AWS GenAI Builders Club team – technical sessions

### Agenda Timeline

| Time | Session | Speaker |
|---|---|---|
| 13:00 – 14:00 | 🧸 Make No-Sew Plush Toys for Children (Community Activity) | Diem My / Dinh Nguyen |
| 14:00 – 14:15 | Why Agentic AI & the MarTech use case we'll explore | My Nguyen |
| 14:15 – 15:00 | AWS Agentic Services in Action: AgentCore Runtime, Memory, MCP Gateway & Agent-to-Agent (A2A) Communication | — |
| 15:00 – 15:15 | ☕ Tea Break | — |
| 15:15 – 15:40 | The Strands Agents Framework & how it ties everything together | — |
| 15:40 – 16:10 | Full Workflow Walkthrough: tracing the code from user prompt to campaign creation | — |
| 16:10 – 16:30 | Security Patterns, Session Persistence & Infrastructure as Code | — |
| 16:30 – 16:45 | Key Takeaways & Source Code Tour | — |
| 16:45 – 17:00 | Kiro Workshop Guidance | Dai Truong / Thi Nguyen |

### Key Highlights

#### Community Activity – No-Sew Plush Toys for Children

- A warm, hands-on opener where attendees crafted **no-sew plush toys** to donate to children
- A reminder that a builders' community is about people, not just technology

#### My Nguyen – Why Agentic AI & the MarTech use case

- Why **agentic AI** is different from traditional prompt-response models
- Introduction to the **MarTech** reference use case: an AI-powered marketing campaign platform
- How multiple specialized agents collaborate to automate audience segmentation, campaign creation, and promotions

#### AWS Agentic Services in Action

- **AgentCore Runtime**: the execution environment for running agents
- **Memory**: how agents retain and recall context across interactions
- **MCP Gateway**: connecting agents to tools and data through the Model Context Protocol
- **Agent-to-Agent (A2A) Communication**: how specialized agents coordinate with each other

#### The Strands Agents Framework

- Overview of the **Strands Agents** framework
- How it ties the agentic services together into a cohesive workflow
- The role of orchestration in multi-agent systems

#### Full Workflow Walkthrough

- Tracing the code end to end — from a **user prompt** to a **created campaign**
- Seeing how the reference architecture executes in practice

#### Security Patterns, Session Persistence & Infrastructure as Code

- **Security patterns** for agentic workloads (IAM, least privilege)
- **Session persistence** to maintain state across agent runs
- **Infrastructure as Code** (CDK) to make the architecture reproducible

#### Key Takeaways & Source Code Tour

- A recap of the core concepts and how the pieces connect
- A guided tour of the **full source code** for self-paced exploration

#### Dai Truong / Thi Nguyen – Kiro Workshop Guidance

- Guidance on using **Kiro** for the workshop and building agentic features
- How to apply the spec-driven workflow to real projects

### Key Takeaways

- **Agentic AI** moves beyond single prompts toward systems of collaborating agents
- AWS provides building blocks — **AgentCore Runtime, Memory, MCP Gateway, A2A** — that fit together
- The **Strands Agents** framework is the glue that orchestrates multi-agent workflows
- Production agentic systems still need **security, session persistence, and IaC** to be reliable
- Having a complete **reference architecture + source code** is the fastest way to learn by adapting

### Applying to Work

- Use the **MarTech reference architecture** as a template for any multi-step automation use case
- Experiment with **AgentCore Memory** to give internal assistants persistent context
- Connect existing tools through an **MCP Gateway** instead of hardcoding integrations
- Apply **A2A communication** to break a complex single-agent prompt into specialized agents
- Adopt **CDK / Infrastructure as Code** so agentic deployments are reproducible and reviewable
- Try **Kiro** for spec-driven development of agentic features

### Prerequisites

- Familiarity with AWS (Lambda, IAM, CDK) and basic Python
- No prior experience with AI agents required

### Event Experience

Attending **Agentic Build Day and Community Activity** on May 29, 2026 was a great mix of meaningful community work and a focused technical deep-dive. Key moments included:

#### Starting with the community
- The event opened with a **no-sew plush toy** activity for children, hosted by **Diem My** and **Dinh Nguyen** — a heartwarming way to connect with other builders before diving into code.

#### Learning the agentic stack end to end
- **My Nguyen** framed the day with a clear **MarTech use case**, making the abstract idea of "agentic AI" concrete.
- The deep-dive into **AgentCore Runtime, Memory, MCP Gateway, and A2A** finally connected services I had only read about separately.
- Seeing the **Strands Agents** framework tie everything together clarified how orchestration actually works in practice.

#### Following real code
- The **full workflow walkthrough** — from user prompt to campaign creation — was the most valuable part, turning theory into a traceable execution path.
- The session on **security, session persistence, and IaC** was a good reminder that demos are easy but production needs guardrails.

#### Hands-on with Kiro
- **Dai Truong** and **Thi Nguyen** wrapped up with **Kiro Workshop Guidance**, showing how to apply a spec-driven workflow to build agentic features.

#### What I left with
- A working mental model of how AWS agentic services fit together
- A reference architecture I can adapt for my own use cases
- Access to the full source code to explore at my own pace

#### Some event photos

![Agentic Build Day](/images/4-EventParticipated/event-3/1780046299367.jpg)

![Agentic Build Day](/images/4-EventParticipated/event-3/1780046308292.jpg)

![Agentic Build Day](/images/4-EventParticipated/event-3/1780046311474.jpg)

![Agentic Build Day](/images/4-EventParticipated/event-3/1780050160542.jpg)

> Overall, **Agentic Build Day** balanced heart and engineering — giving back to the community in the first hour, then handing us a complete, adaptable blueprint for building real agentic systems on AWS.
