---
title: "AWS First Cloud AI Journey - Community Day"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

<!--{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy it verbatim** into your report, including this warning.
{{% /notice %}}-->

# Summary Report: “AWS First Cloud AI Journey – Community Day”

### Event Objectives

- Connect the **FCAJ (First Cloud AI Journey)** community in Ho Chi Minh City
- Share practical experience working with **AI / GenAI** in real production workloads
- Update useful **AWS** services for AI workloads (CloudFront, Amazon Quick suite)
- Provide career inspiration through hackathon stories and enterprise case studies

### Speakers

- **Tinh Truong** – Platform Engineer at GoTymeX
- **Thinh Nguyen** – DevOps Engineer | FCAJ Ambassador
- **Anh Pham** – Cloud Consultant at G-AsiaPacific Vietnam
- **Uyen Le** – GenAI Engineer at VIB
- **Thao Nguyen** – GenAI Engineer at VIB
- **Mai Nguyen** – GenAI Engineer at VIB
- **Duc Dao** – Solutions Architect at Cloud Kinetics
- **Vy Lam** – Senior Business Systems Analyst at VPBank

### Agenda Timeline

| Time | Speaker | Session |
|---|---|---|
| 09:00 – 09:30 | **Tinh Truong** | Context Is Everything: Making AI Actually Work for You |
| 09:30 – 09:45 | **Anh Pham** | Friendly AI Assistant with Amazon Quick |
| 09:45 – 10:25 | **Thinh Nguyen** | From Edge To Origin: CloudFront as Your Foundation |
| 10:25 – 10:55 | **Uyen Le, Thao Nguyen, Mai Nguyen** | 36 hrs with LotusHacks – Building UTMorpho from Idea to Reality |
| 10:55 – 11:00 | — | Coffee Break |
| 11:00 – 11:30 | **Duc Dao** | Non-Determinism of "Deterministic" LLM Settings |
| 11:30 – 12:00 | **Vy Lam** | Enterprise-Grade Multi-Agent System: The Case of Startup Credit Scoring |

### Key Highlights

#### Tinh Truong – Context Is Everything: Making AI Actually Work for You

- Why AI fails without context, and what “context” really means
- From prompts to memory: how AI is evolving with the **Second AI Brain** concept
- How better context produces better results — practical mindset and tips
- Career insights and how students can start building with AI

#### Anh Pham – Friendly AI Assistant with Amazon Quick

- **Quick Chat Agent**: AI assistants for exploring and analyzing data
- **Quick Flows**: build intelligent workflows with natural language, no coding required
- **Quick Spaces**: collaborative spaces that turn individual insights into team knowledge
- **Quick Sight**: build dashboards and reports from raw data using natural language

#### Thinh Nguyen – From Edge To Origin: CloudFront as Your Foundation

- Amazon CloudFront for every workload type
- Cost optimization techniques with CloudFront
- Security capabilities at the edge
- Enhanced reliability and performance with CloudFront

#### Uyen Le, Thao Nguyen, Mai Nguyen – 36 hrs with LotusHacks: Building UTMorpho

- Why the team joined **LotusHacks**
- From zero to idea — the brainstorming journey
- Defining the problem and shaping **UTMorpho**
- Building under pressure during a 36-hour development sprint
- Challenges, failures, and turning points
- Product overview, demo, and key learnings

#### Duc Dao – Non-Determinism of "Deterministic" LLM Settings

- How LLMs choose the next token
- The common assumption that **Temperature = 0** guarantees determinism
- Reality: inference optimizations break that assumption
- Practical impacts on production systems and mitigation strategies

#### Vy Lam – Enterprise-Grade Multi-Agent System: Startup Credit Scoring

- The structural mismatch between banking systems and startup data
- **Single Agent**: when to use it and when not to
- The **Multi-Agent paradigm** for complex domains
- Blueprint of a Virtual Credit Committee
- Guardrails, compliance, operational ROI, and an implementation roadmap

### Key Takeaways

#### Mindset for working with AI

- **Context engineering** matters more than clever prompting
- Treat AI like a teammate that needs **memory, tools, and grounding**, not a search engine
- **Determinism is an illusion** — always design for variance, even with `temperature=0`

#### Architecture & AWS

- **CloudFront** is more than a CDN; it can be a security and performance foundation
- **Amazon Quick** suite lowers the barrier for non-engineers to build AI workflows
- For complex business domains, **multi-agent systems** with guardrails outperform a single agent

#### Career & community

- Hackathons like **LotusHacks** are a fast way to ship something real and learn under pressure
- Enterprise AI adoption requires **compliance, ROI tracking, and a clear roadmap**, not just demos
- Joining the **FCAJ community** is a shortcut to learning from people already doing this in production

### Applying to Work

- Add a **context layer** (memory, retrieval, tools) to internal AI assistants instead of relying on raw prompts
- Pilot **Amazon Quick Flows / Quick Sight** for internal reporting use cases
- Audit current LLM features for **non-determinism risks** and add guardrails or evaluation harnesses
- Evaluate **CloudFront** for static asset delivery and basic edge security on new projects
- Sketch a **multi-agent design** for any workflow that currently struggles as a single-prompt pipeline

### Event Experience

Attending **AWS First Cloud AI Journey – Community Day** on May 23, 2026 was an inspiring half-day filled with both deep technical content and real-world stories. Key moments included:

#### Learning from a diverse speaker lineup
- Speakers came from very different angles — banking (**VPBank, VIB**), platform engineering (**GoTymeX**), AWS partners (**G-AsiaPacific, Cloud Kinetics**) and the **FCAJ** community itself.
- Each talk hit a different layer of the AI stack: prompting, tooling, infrastructure, and enterprise architecture.

#### Strong technical takeaways
- **Tinh Truong** reframed how I think about prompting — context first, prompt second.
- **Duc Dao** broke the comfortable myth that `temperature=0` means reproducible output.
- **Vy Lam** showed how multi-agent systems are already being used in serious financial domains, not just demos.

#### Hands-on AWS exposure
- **Anh Pham** demoed the **Amazon Quick** suite, which lowers the bar for building internal AI tools.
- **Thinh Nguyen** went deep on **CloudFront** beyond CDN — security, cost, and reliability angles I had underused.

#### Inspiration from peers
- The **UTMorpho / LotusHacks** story from **Uyen Le, Thao Nguyen, Mai Nguyen** was a strong reminder that 36 hours is enough to ship something meaningful with the right team.

#### Networking
- Met other FCAJ/FCJ members at floor 26 of Bitexco, exchanged notes on internships, AWS learning paths, and current AI projects.

#### Some event photos

![FCAJ Community Day](/images/4-EventParticipated/event-2/fcaj-community.jpg)

![Nguyen Gia Hung](/images/4-EventParticipated/event-2/NguyenGiaHung.jpg)

![Tinh Truong – Platform Engineer @ GoTymeX](/images/4-EventParticipated/event-2/TinhTruong.jpg)

![Thinh Nguyen – DevOps Engineer | FCAJ Ambassador](/images/4-EventParticipated/event-2/ThinhNguyen.jpg)

![Anh Pham – Cloud Consultant @ G-AsiaPacific Vietnam](/images/4-EventParticipated/event-2/AnhPham.jpg)

![Uyen Le, Thao Nguyen, Mai Nguyen – GenAI Engineers @ VIB](/images/4-EventParticipated/event-2/UyenLe-ThaoNguyen-MaiNguyen.jpg)

![Duc Dao – Solutions Architect @ Cloud Kinetics](/images/4-EventParticipated/event-2/DucDao.jpg)

![Vy Lam – Senior Business Systems Analyst @ VPBank](/images/4-EventParticipated/event-2/VyLam.jpg)

> Overall, the event reinforced that **building useful AI products is mostly about context, guardrails, and team execution** — and that the FCAJ community in HCMC is a great place to learn this together.
