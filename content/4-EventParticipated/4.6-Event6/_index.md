---
title: "Agentic AI Build Week Day 2"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 4.6. </b> "
---

# Summary Report: “Agentic AI Build Week Day 2”

### Event Objectives

- Join the second day of **Agentic AI Build Week**, focused on building, deploying, and operating AI agents in production-oriented environments
- Understand how AI coding tools, AWS, Amazon Bedrock, Kiro, Codex, Claude Code, Bedrock AgentCore, Agora ConvoAI, and TinyFish can support practical agentic workflows
- Observe key patterns such as spec-driven development, real-time voice agents, multi-agent orchestration, memory, tool access, web automation, and auditability
- Connect with builders, mentors, founders, and teams building agentic AI products during the hackathon week

### Agenda Timeline

| Time | Session | Speaker / Organization |
|---|---|---|
| 08:45 - 10:30 | From Spec to Production Code - Kiro, Claude Code & Codex on AWS | Diep Nguyen / AWS |
| 10:30 - 12:00 | Physical AI Party: Build Voice Agents with Agora ConvoAI | Derek Zheng / Agora |
| 13:00 - 14:30 | Production Multi-Agent AI on AWS - Bedrock AgentCore | Thi Nguyen / AWS |
| 14:30 - 16:00 | Enable Track Live-Demo Workshop | Huy Vo / TinyFish |
| 18:30 - 21:00 | Agentic AI Build Week Builder Night | GenAI Fund - Official |

### Workshop Sources

- [From Spec to Production Code - Kiro, Claude Code & Codex on AWS](https://luma.com/1mxdg4em)
- [Physical AI Party: Build Voice Agents with Agora ConvoAI](https://luma.com/3j43oewt)
- [Production Multi-Agent AI on AWS - Bedrock AgentCore](https://luma.com/lptsgwm6)
- [Enable Track Live-Demo Workshop](https://luma.com/5a4h9zsx)
- [Agentic AI Build Week Builder Night](https://luma.com/gaf-u7gd)

### Key Highlights

#### From Spec to Production Code - Kiro, Claude Code & Codex on AWS

This session focused on the gap between an AI coding demo that works locally and a system that can be deployed with production controls. It covered structured specs with Kiro, engineering standards enforced through Kiro Hooks, and the deployment of Claude Code and Codex on Amazon Bedrock with IAM, usage policies, and CloudTrail.

Key points I noted:

- Spec-driven development reduces risk when using AI coding tools because requirements, design, and implementation stay connected
- Enterprise AI coding needs governance, logging, access control, and model flexibility, not only fast code generation
- Agentic coding is useful for multi-file refactoring, test generation, codebase reasoning, and implementation standardization

#### Physical AI Party: Build Voice Agents with Agora ConvoAI

Agora's workshop focused on real-time AI voice agents. Instead of treating voice AI as only speech-to-text plus a chatbot response, the session showed how voice, video, real-time communication, and automation can create systems that interact with the physical world.

Key points I noted:

- Voice agents must handle latency, turn-taking, interruption, audio quality, and continuous response flows
- Physical AI expands AI applications into robotics, smart devices, customer service, healthcare, and education
- A strong voice-agent prototype must be tested as a real-time system, not only judged by the quality of model answers

#### Production Multi-Agent AI on AWS - Bedrock AgentCore

The Bedrock AgentCore session focused on production multi-agent architecture. It showed why a single agent is often not enough for complex tasks and why memory, tool access, orchestration patterns, and managed agent infrastructure matter.

Key points I noted:

- AgentCore Runtime helps deploy multi-agent systems on managed AWS infrastructure and reduces runtime operations work
- Episodic and semantic memory help agents preserve context and learn from previous interactions
- Supervisor orchestration and collaborative orchestration should be selected based on workflow dependency and control requirements
- Gateway, model versioning, and routing become important when multiple agents or models operate in the same system

#### Enable Track Live-Demo Workshop

The TinyFish session focused on a practical issue: many agents can reason well but cannot reliably access live web data, authenticated portals, or current system state. The workshop demonstrated how a web-enabled agent can select sources, read clean data, perform actions, and return evidence.

Key points I noted:

- Production agents must distinguish between stale training data and live data from trusted sources
- Reliable web automation needs layers such as Search, Fetch, Browser, Vault, AgentQL, and Agent rather than simple scraping
- When agents interact with portals or authenticated sources, evidence, logging, and action control are mandatory
- Track-based demos across mobility, real estate, F&B, gaming, retail, and aviation showed that the same pattern can support many domains

#### Agentic AI Build Week Builder Night

Builder Night was an evening networking session for builders, founders, investors, and enterprise representatives in the AI ecosystem. Instead of a technical lecture, it created space for discussing product ideas, implementation direction, hackathon experience, and potential collaboration after the event.

Key points I noted:

- Direct conversations with builders helped me understand how teams choose problem statements, scope MVPs, and divide roles in agentic AI projects
- Networking connected the technical lessons from the day with product, market, and implementation realities
- The session showed how the Southeast Asia AI community discusses builder culture and enterprise adoption

### Key Takeaways

- Production AI agents should start from a clear spec before moving into code generation, orchestration, and deployment
- Governance is not a final add-on; IAM, logging, audit trails, usage policies, and network boundaries should be designed early
- Voice agents are real-time systems where latency and turn-taking directly affect user experience
- Multi-agent systems need memory, routing, orchestration, and control mechanisms to avoid becoming disconnected agents
- Agents that access web or portal data need evidence, clean sources, safe login handling, and control over risky actions
- A productive workshop day does more than introduce tools; it shapes product thinking and how builders collaborate

### Applying the Lessons to My Work

- For project, I can apply spec-driven development by writing clear requirements, design contracts, and acceptance criteria before using AI-assisted coding
- Document-processing modules should include logging, audit trails, and least-privilege access, especially when calling AI services or processing sensitive data
- If the system expands toward agents, each agent role should be clearly separated: ingestion, extraction, validation, review suggestion, and reporting
- For workflows that need external data, the system should prefer trusted sources, store evidence, and avoid allowing agents to act on unverified data
- The voice-agent and multi-agent criteria from this event can help evaluate real-time design, asynchronous workflow, and human-in-the-loop controls in the project

### Event Experience

Attending **Agentic AI Build Week Day 2** gave me a practical view of agentic AI. The sessions did not stop at concept introductions; they focused on how agents can run in production with governance, live data, memory, tool access, and risk control.

The strongest part of the day was how the sessions connected with each other. The morning started with structured specs and controlled AI coding, then moved into real-time voice agents. The afternoon covered multi-agent infrastructure and web automation, while the evening gave builders space to exchange ideas directly. This helped me see the path from a hackathon idea to an architecture that can keep evolving after the event.

#### Some event photos

![Opening remarks at Agentic AI Build Week Day 2](/images/4-EventParticipated/event-6/6C7A9572.jpg)

![Workshop space at AWS HCMC Office](/images/4-EventParticipated/event-6/6C7A0119.jpg)

![From Spec to Production Code session](/images/4-EventParticipated/event-6/1783570209032.jpg)

![Opening remarks from the organizing team](/images/4-EventParticipated/event-6/1783570210109.jpg)

![Kiro, Claude Code, and Codex on AWS workshop](/images/4-EventParticipated/event-6/1783578886626.jpg)

![Production agent session](/images/4-EventParticipated/event-6/1783581458134.jpg)

![TinyFish web-enabled agent demo](/images/4-EventParticipated/event-6/1783588020682.jpg)

![Bedrock AgentCore Observability session](/images/4-EventParticipated/event-6/1783607564112.jpg)

![Builder Night and evening networking](/images/4-EventParticipated/event-6/6C7A0154.jpg)

![Builders attending the workshop](/images/4-EventParticipated/event-6/6C7A9585.jpg)

![Workshop learning atmosphere](/images/4-EventParticipated/event-6/6C7A9590.jpg)

![Check-in photo at the Agentic AI Build Week booth](/images/4-EventParticipated/event-6/6C7A9656.jpg)

![Builder teams discussing during the workshop](/images/4-EventParticipated/event-6/6C7A9776.jpg)

![Mentor supporting a builder team during the hands-on session](/images/4-EventParticipated/event-6/6C7A9784.jpg)

![Moment from Agentic AI Build Week Day 2](/images/4-EventParticipated/event-6/6C7A9987.jpg)

> Overall, **Agentic AI Build Week Day 2** showed me that building AI agents is not only about choosing a powerful model. A good agentic system needs clear specs, reliable infrastructure, scoped permissions, traceable data sources, full logging, and human control over high-impact steps.
