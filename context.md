Absolutely. Give OpenCode the following as a **project context/instructions file**. It contains the decisions we've made so far and gives it enough context to work on your existing GitHub Pages site without inventing experience.

 # Project Context: Transform Existing Technical Blog into Consulting Website

 ## 1\. Objective

 Transform the existing GitHub Pages technical blog:

 `http://ayushedu.github.io/`

 into the user's primary professional website and consulting landing site.

 Do **not** create a new website/domain from scratch.

 The site should continue functioning as a technical blog while adding a professional consulting presence.

 The primary objective is to support the user's transition into:

 > **Independent Technical Consultant — Distributed Systems & Data Platforms**

 The immediate business objective is to obtain the user's first paid consulting engagement, initially targeting approximately **₹5,000–₹10,000**.

 The site should eventually support higher-value consulting engagements.

---

 # 2\. User Professional Profile

 The user has approximately **18 years of software engineering experience** and has worked primarily as a senior/principal-level individual contributor.

 Current positioning:

 > **Principal Engineer & Independent Technical Consultant**

 Primary consulting specialization:

 - Distributed systems
- Event-driven architecture
- Kafka
- Cassandra
- Scala
- Akka/Pekko
- Spark/PySpark
- Data platforms
- ETL
- Performance optimization
- Scalability
- Production troubleshooting
- Architecture reviews

 The user is also transitioning into:

 - AI
- LLMs
- RAG
- AI platform engineering

 However, the website must **not overstate current LLM expertise**.

 The user's LLM experience is currently developing.

---

 # 3\. Core Professional Positioning

 Preferred positioning:

 > **I help engineering teams design, troubleshoot, and scale distributed systems, event-driven platforms, and data-intensive applications.**

 Secondary positioning:

 > **18 years of experience building distributed, event-driven and data-intensive systems across telecom, analytics, machine learning, big data and enterprise platforms.**

 Future positioning can evolve toward:

 > **Distributed Systems + AI/LLM Platform Engineering**

 Do not currently describe the user as an "LLM expert" or "AI architect" with extensive production experience.

---

 # 4\. Target Customers

 The consulting site should appeal to:

 - CTOs
- VP Engineering
- Heads of Engineering
- Engineering Managers
- Technical Founders
- Principal/Staff Engineers
- Architects

 Target companies:

 - US startups
- European startups
- Global enterprises
- SaaS companies
- AI startups
- Data-intensive companies
- Companies modernizing legacy systems
- Companies using Kafka/event-driven systems
- Companies experiencing scalability or performance problems

---

 # 5\. Problems the User Can Help Solve

 The website should focus on customer problems rather than merely listing technologies.

 Examples:

 ### Distributed systems

 > Our distributed system is slow, unreliable or difficult to scale.

 ### Kafka

 > Kafka consumer lag keeps increasing.

 > Our event-processing pipeline isn't keeping up.

 > We aren't sure how to partition our Kafka topics.

 ### Cassandra

 > Cassandra queries have become slow at scale.

 > We're seeing tombstones or poor read performance.

 > We need help reviewing our Cassandra data model.

 ### Data platforms

 > Our ETL pipeline doesn't scale.

 > Our Spark jobs are too slow.

 > Our data-processing architecture needs redesigning.

 ### Architecture

 > We need an independent architecture review.

 > We are unsure how to redesign a legacy system.

 ### Production troubleshooting

 > We have a difficult production problem and can't identify the real bottleneck.

---

 # 6\. Important Technical Experience

 ## Ciena

 ### Event-Driven ETL Platform

 - Designed and led development of scalable event-driven ETL systems.
- Built high-throughput aggregation pipelines.
- Scala
- Akka/Pekko
- Kafka
- Cassandra
- REST APIs
- Linux
- Docker
- Kubernetes

 The Ciena projects were containerized using Docker.

 The event-driven ETL platform also ran on Kubernetes.

 ### Stitcher Platform

 - Designed and implemented distributed event-streaming engines.
- Modeled complex Layer-3 network topologies using hierarchical data models.
- Built end-to-end service modelling solutions for multi-domain Layer-3 networks.
- Scala
- Akka
- Kafka
- Cassandra
- Distributed systems
- REST APIs
- Docker

---

 # 7\. Consulting Case Study #1 — Kafka / Cassandra Performance

 This should become a public sanitized case study.

 Do NOT mention confidential company information.

 ## Problem

 Commit rate was too slow.

 ## Investigation

 The investigation found multiple contributing factors:

 - synchronous thread blocking
- inefficient Kafka topic configuration
- single Kafka partition
- Cassandra cache operations involving quick reads/writes/deletes

 The important point is that the bottleneck crossed several system layers.

 Conceptual flow:

```
Application
    ↓
Synchronous thread blocking
    ↓
Kafka
    ↓
Single partition
    ↓
Cassandra
    ↓
Cache read/write/delete
```

 Technologies:

 - Scala
- Akka/Pekko
- Kafka
- Cassandra
- Docker

 The case study should emphasize the diagnostic process and architectural reasoning.

 Do not invent numerical performance improvements because exact numbers have not been provided.

---

 # 8\. Consulting Case Study #2 — Kafka Lag / Cassandra Tombstones

 This should become another public sanitized case study.

 ## Problem

 Kafka consumer lag was increasing.

 ## Investigation

 The investigation identified Cassandra performance degradation.

 Expired rows were being deleted in a way that produced a large number of Cassandra tombstones.

 The tombstones degraded read-query performance, which affected downstream processing and contributed to increasing Kafka lag.

 ## Solution

 The Cassandra data model was changed to use a week-based partitioning/data lifecycle strategy.

 Instead of deleting individual expired rows, the system could remove the previous week's data using the appropriate primary-key operation.

 Conceptual flow:

```
Kafka lag
    ↓
Processing throughput
    ↓
Cassandra read performance
    ↓
Tombstones
    ↓
Data lifecycle/deletion strategy
    ↓
Data model redesign
```

 Technologies:

 - Scala
- Akka/Pekko
- Kafka
- Cassandra
- Docker

 Do not invent metrics or claim specific percentage improvements.

---

 # 9\. Consulting Case Study #3 — Reporting System Failing at Scale

 Historical project at ValueFirst.

 ## Problem

 An MIS/reporting system worked with small datasets but became unusable in production with large customer datasets.

 ## Solution

 Several changes were made:

 - restricted reporting data to a one-month window instead of querying indefinitely
- made report generation asynchronous
- users could submit a report and return later to retrieve the generated file
- added MySQL indexing
- divided database/data handling for large customers

 Technologies:

 - Java
- MySQL
- BIRT
- Servlets

 Key architectural lesson:

 > Scaling often requires changing the data-access model and user workflow, not merely optimizing queries.

 Again, do not invent numerical performance metrics.

---

 # 10\. Data / ML Experience

 The user has significant historical machine-learning and text-analytics experience.

 At EXL/Inductis:

 A customer text analytics dashboard originally used:

 - Excel
- Python
- relatively small datasets

 The user helped move the platform toward large-scale processing using:

 - Spark
- PySpark
- Hadoop
- Scala
- Django

 Historical experience includes:

 - text analytics
- Word2Vec
- Spark MLlib
- scikit-learn
- Pandas
- NumPy
- clustering
- segmentation
- predictive analytics

 The website can mention this as part of the user's evolution toward AI.

---

 # 11\. Other Historical Experience

 The user's career includes:

 ### ValueFirst

 - HTTP/XML APIs
- Java middleware
- SMS messaging
- reporting
- BIRT
- MySQL
- Linux

 ### Alcatel-Lucent / Nokia

 - Hadoop
- Cassandra
- Spark
- Hive
- HDFS
- YARN
- Python
- Scala
- telecom analytics
- charging analytics
- alarm correlation
- Drools
- automation

 ### Accenture

 - Marketing analytics
- JavaScript
- ExtJS

 Do not clutter the consulting homepage with the entire career history.

 A separate `/about` or `/experience` page can contain more detail.

---

 # 12\. Leadership Experience

 The user has experience with:

 - mentoring engineers
- architecture/design reviews
- leading technical discussions
- making decisions across teams
- customer interaction
- working with architects
- presenting designs to senior leadership
- coordinating teams
- knowledge-transfer sessions
- defining engineering standards
- production incidents
- technical roadmaps

 The user has also conducted Spark MLlib training for an external company as a side engagement.

 This supports consulting/training credibility.

---

 # 13\. Current Technical Skill Ratings

 These are self-assessed and should NOT necessarily appear on the public website.

 | Skill | Rating |
| --- | --- |
| Scala | 4/5 |
| Java | 3/5 |
| Python | 3/5 |
| Kafka | 3/5 |
| Cassandra | 3/5 |
| Akka/Pekko | 4/5 |
| Spark/PySpark | 4/5 |
| Distributed Systems | 4/5 |
| Event-Driven Architecture | 4/5 |
| ETL/Data Platforms | 4/5 |
| Docker | 3/5 |
| Kubernetes | 3/5 |
| System Design | 3/5 |
| Machine Learning | 4/5 |
| NLP/Text Analytics | 4/5 |
| LLM/RAG | 1/5 |

---

 # 14\. AI/LLM Direction

 The user is currently learning modern LLM systems.

 Current self-assessment:

 | Topic | Rating |
| --- | --- |
| Transformer fundamentals | 2/5 |
| Attention | 2/5 |
| Embeddings | 4/5 |
| RAG | 0/5 |
| Vector databases | 2/5 |
| Prompt engineering | 4/5 |
| Tool/function calling | 5/5 |
| Agents | 5/5 |
| LLM inference | 1/5 |
| LLM evaluation | 1/5 |
| Fine-tuning | 1/5 |
| LLM system design | 1/5 |

The user is learning from **LLMs From Scratch**.

 Long-term direction:

```
Distributed Systems
        +
Data Platforms
        +
ML/NLP
        +
LLMs/RAG
        ↓
AI Platform / GenAI Infrastructure
```

 The website can state:

 > Currently exploring production AI/LLM systems and applying distributed-systems and data-platform principles to AI workloads.

 Do not claim extensive production LLM experience yet.

---

 # 15\. Local Technical Environment

 The user has:

 - Ubuntu
- 32 GB RAM
- NVIDIA GPU
- 16 GB VRAM

 They intend to build hands-on AI/LLM projects locally.

 Potential future portfolio stack:

 - Python
- FastAPI
- local LLMs
- embedding models
- Qdrant/vector database
- PostgreSQL
- Redis
- Kafka
- Docker
- Kubernetes
- observability

 These should only be added to the public site as actual hands-on work is completed.

---

 # 16\. Existing Blog History

 There are TWO existing blogs.

 ## Primary/current blog

 `http://ayushedu.github.io/`

 This was the later blog after migrating away from WordPress.

 It contains technical posts related to:

 - Cassandra
- Scala
- Kafka/Akka
- Spark
- Python
- PySpark
- Django
- distributed/data technologies

 Latest existing post is from approximately July 2023.

 ## Older WordPress archive

 `https://adeduction.wordpress.com/`

 This contains technical posts dating back to approximately **2012**.

 It contains historical technical writing around areas including:

 - Cassandra
- Big Data
- Spark
- Django
- Java
- testing

 The WordPress blog should remain as an archive.

 Do NOT delete it.

 Do NOT migrate all posts immediately.

 Eventually selected high-value posts may be rewritten or migrated to the GitHub Pages site.

---

 # 17\. Website Strategy

 Use:

 `ayushedu.github.io`

 as the primary professional/consulting website.

 Do NOT create a new website or domain at this stage.

 The WordPress site remains the historical archive.

 The GitHub Pages site should become the central hub for:

 - professional identity
- consulting
- case studies
- technical writing
- projects
- contact information

---

 # 18\. Proposed Website Structure

 Recommended:

```
/
├── Home
├── Consulting
├── Case Studies
│   ├── Kafka / Cassandra Performance
│   ├── Kafka Lag / Cassandra Tombstones
│   └── Reporting System at Scale
├── Projects
├── Writing
├── About
└── Contact
```

 Possible future sections:

```
├── AI / LLM
├── System Design
└── Resources
```

 Do not over-engineer the website.

 The immediate goal is credibility and lead generation.

---

 # 19\. Homepage Requirements

 The homepage should NOT simply look like an old chronological technical blog.

 The top section should clearly communicate:

 ## Name

 Ayush Vatsyayan

 ## Professional identity

 > Principal Engineer & Independent Technical Consultant

 ## Specialization

 > Distributed Systems · Event-Driven Architecture · Data Platforms · Performance & Scalability

 ## Core statement

 > I help engineering teams design, troubleshoot, and scale distributed systems, event-driven platforms, and data-intensive applications.

 Primary calls to action:

 - `Consulting`
- `Case Studies`
- `Technical Writing`
- `GitHub`
- `Contact`

 The technical blog should remain visible but should no longer dominate the first screen.

---

 # 20\. Consulting Page

 Create:

 `/consulting`

 or equivalent based on the site's existing framework.

 The page should explain:

 ## What I help with

 ### Distributed Systems

 Architecture, scalability, reliability and production troubleshooting.

 ### Kafka / Event Streaming

 Partitioning, consumer lag, event processing, reliability and architecture.

 ### Cassandra

 Data modeling, query patterns, performance and data lifecycle.

 ### Data Platforms

 Spark/PySpark, ETL, data processing and migration.

 ### Architecture Reviews

 Independent technical review of existing or proposed architecture.

 ### Performance Investigations

 Identify bottlenecks across application, messaging and database layers.

 ### Technical Prototypes

 Build focused proof-of-concepts to validate architectural approaches.

---

 # 21\. Initial Consulting Offer

 Create a clear initial offer around:

 ## Distributed Systems Architecture & Troubleshooting Session

 Potential format:

 - 60–90 minute technical session
- review architecture/problem
- discuss logs/code/design if appropriate
- identify likely bottlenecks
- provide written findings
- provide prioritized recommendations
- follow-up Q&A

 Initial experimental price:

 **₹5,000**

 Do not guarantee that the issue will be completely fixed for ₹5,000.

 The offer is for diagnosis/review.

 Possible wording:

 > Have a distributed-system performance or scalability problem you can't pin down? I offer focused technical review sessions to help identify bottlenecks and determine practical next steps.

 Do not promise specific outcomes that haven't been established.

---

 # 22\. Consulting Positioning

 Do NOT position the user as:

 - generic freelancer
- cheap developer
- generic Scala programmer
- generic Python developer
- prompt engineer
- LLM expert

 Preferred:

 > **Principal Engineer & Independent Technical Consultant**

 The customer buys:

 - engineering judgment
- architecture
- troubleshooting
- system understanding
- production experience
- technical decision-making

 not merely coding hours.

---

 # 23\. Public Case Study Rules

 All historical work must be presented as **sanitized case studies**.

 Never expose:

 - proprietary company information
- customer names unless already public and explicitly appropriate
- internal architecture details
- confidential metrics
- proprietary code
- internal project names
- sensitive telecom/network details

 Use wording such as:

 > "In a production event-driven platform..."

 rather than naming the employer unless the information is already clearly public and appropriate.

 Do not invent metrics.

 If an exact improvement percentage is not known, do not create one.

---

 # 24\. Existing Public Credibility

 The user has a Stack Overflow profile:

 `https://stackoverflow.com/users/6065591/ayush-vatsyayan`

 Current known reputation:

 **2,646**

 The profile has substantial historical reach and technical contributions, especially around:

 - Python
- Spark
- PySpark
- Spark SQL
- DataFrame
- Django

 The Stack Overflow profile should remain the user's existing profile.

 Do not create a new profile.

 Eventually the website should link to it.

---

 # 25\. Personal Brand Ecosystem

 The desired ecosystem is:

```
                    LinkedIn
                       |
                       |
              ayushedu.github.io
                       |
        +--------------+--------------+
        |              |              |
      Blog          Case Studies    Projects
        |              |              |
        +--------------+--------------+
                       |
                    GitHub
                       |
                    YouTube
                       |
               Consulting Leads
```

 Stack Overflow is an additional credibility signal.

---

 # 26\. LinkedIn Positioning

 The user's current LinkedIn experience entry is:

 > **Independent Technical Consultant — Distributed Systems & Data Platforms**

 Employment type:

 > **Freelance**

 Company:

 > **Self-employed**

 Dates:

 > **2026 – Present**

 The website should use the same terminology.

---

 # 27\. Content Strategy

 The user intends to build credibility through:

 - YouTube
- technical blogging
- GitHub projects
- LinkedIn

 Content should focus on real technical problem solving rather than generic tutorials.

 Potential content pillars:

 ### Distributed Systems

 - scalability
- reliability
- failure modes
- architecture tradeoffs
- performance

 ### Kafka

 - consumer lag
- partitioning
- ordering
- retries
- idempotency
- event-driven design

 ### Cassandra

 - data modeling
- tombstones
- partitioning
- query patterns
- performance

 ### Spark

 - performance
- data processing
- architecture

 ### AI + Distributed Systems

 Future content:

 - RAG architectures
- Kafka-based ingestion for RAG
- vector databases
- embedding pipelines
- LLM gateways
- AI observability
- AI system design
- production AI infrastructure

---

 # 28\. Website Design Requirements

 Keep the design:

 - professional
- technical
- clean
- minimal
- credible
- fast
- mobile-friendly

 Avoid:

 - flashy startup-style marketing
- fake testimonials
- fake client logos
- stock-photo-heavy design
- exaggerated AI claims
- "10x developer" language
- generic motivational content

 The site should feel like:

 > **A senior/principal engineer's technical consulting site.**

 Not:

 > **A generic freelance agency website.**

---

 # 29\. Existing Content Preservation

 Before modifying the site:

 1. Inspect the current repository.
2. Identify the static-site generator/theme/framework.
3. Preserve existing posts.
4. Preserve existing URLs/permalinks where practical.
5. Avoid breaking old links.
6. Do not delete technical writing.
7. Improve navigation.
8. Add consulting pages around the existing content.

 If a migration is required, minimize URL breakage.

---

 # 30\. SEO

 Use natural technical keywords.

 Relevant terms:

 - distributed systems consultant
- Kafka consultant
- Cassandra consultant
- Scala consultant
- Spark consultant
- data platform consultant
- event-driven architecture consultant
- distributed systems architecture
- Kafka performance
- Cassandra performance
- Spark performance
- data engineering consultant
- technical architecture review

 Do not keyword-stuff.

 The content should primarily be useful to engineers and engineering leaders.

---

 # 31\. Contact / CTA

 The site should have a simple contact mechanism.

 Possible CTA:

 > **Have a difficult distributed-systems problem?**

 > Let's discuss the architecture, bottleneck or scalability challenge.

 Use the user's actual contact information if it exists in the repository/configuration.

 Do NOT invent an email address.

 If no contact information exists, create a clearly marked placeholder and ask the user to configure it.

---

 # 32\. Important Business Context

 The user has approximately 3 months of financial runway.

 Initial target:

 **₹1 lakh/month**

 Immediate validation target:

 **₹5,000–₹10,000 first paid engagement**

 The website's immediate role is therefore:

 > Convert existing professional credibility into consulting conversations.

 It is NOT primarily intended to:

 - make advertising revenue
- sell courses immediately
- generate massive SEO traffic
- become a large media site

---

 # 33\. What OpenCode Should NOT Do

 Do not:

 - invent clients
- invent consulting engagements
- invent revenue
- invent performance metrics
- invent testimonials
- invent customer logos
- claim production LLM experience that doesn't exist
- claim expertise the user hasn't demonstrated
- delete existing technical articles
- replace the entire site unnecessarily
- create a fake consulting company
- create a second personal identity
- rewrite historical articles without preserving their meaning
- fabricate credentials

---

 # 34\. Desired Immediate Deliverables

 First inspect the existing GitHub Pages repository.

 Then implement:

 ### Phase 1

 1. Professional homepage
2. Consulting page
3. About page
4. Case Studies landing page
5. Contact CTA
6. Improved navigation
7. Professional footer

 ### Phase 2

 Create initial case studies:

 1. Kafka / Cassandra performance
2. Kafka lag / Cassandra tombstones
3. Reporting platform scalability

 ### Phase 3

 Improve blog navigation:

 - Distributed Systems
- Kafka
- Cassandra
- Spark/Data
- Python
- AI/LLM

 Use categories/tags only if the existing framework supports them cleanly.

 ### Phase 4

 Add links to:

 - LinkedIn
- GitHub
- Stack Overflow
- YouTube

 Only use URLs actually provided/configured by the user.

---

 # 35\. Tone

 The writing should be:

 - technically confident
- factual
- understated
- experienced
- direct
- professional

 Avoid:

 > "I'm the world's leading..."

 > "Revolutionary..."

 > "10x..."

 > "AI guru..."

 Prefer:

 > "18 years of experience..."

 > "I help engineering teams..."

 > "I've worked on..."

 > "A recurring class of problems I've solved..."

 > "Currently exploring..."

---

 # 36\. Core Brand Statement

 Use this as the central conceptual message:

 > **18 years of building distributed, event-driven and data-intensive systems. I help engineering teams diagnose difficult production problems, review architecture, and build systems that scale.**

 Secondary future message:

 > **Now applying that experience to production AI/LLM systems.**

---

 # 37\. Important Strategic Context

 The user's strongest existing differentiator is NOT individual technology knowledge.

 It is the ability to investigate a difficult problem across system boundaries.

 Example:

```
Kafka symptom
      ↓
Application behavior
      ↓
Concurrency
      ↓
Messaging topology
      ↓
Cassandra behavior
      ↓
Data model
      ↓
Architecture change
```

 The website should communicate this systems-thinking capability.

 The user should compete on:

 - architecture
- debugging
- performance
- scalability
- reliability
- technical judgment

 rather than competing on raw coding speed with AI coding agents.

---

 # 38\. Long-Term Direction

 The consulting business may eventually evolve:

```
Independent Consultant
        ↓
Distributed Systems Specialist
        ↓
Architecture Consultant
        ↓
AI + Distributed Systems Consultant
        ↓
Fractional Principal Engineer
        ↓
Specialist Consulting Practice
```

 The website should be designed so this evolution does not require a complete rebrand.

---

 # 39\. First Priority

 Do not overbuild.

 The immediate objective is:

 > **Make the existing blog credible enough that a prospect from LinkedIn can click the link and understand within 30 seconds who the user is, what problems they solve, and how to contact them.**

 Then create one strong case study and begin outreach.

 The website is a sales-support asset, not the business itself.

 # End Context
