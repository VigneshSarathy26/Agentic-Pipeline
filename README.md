# Agentic-Pipeline

![Python](https://img.shields.io/badge/python-3.10+-blue.svg)
![Status](https://img.shields.io/badge/status-active-success.svg)
![License](https://img.shields.io/badge/license-MIT-blue.svg)

**Agentic-Pipeline** is a dynamic, multi-agent content generation and publishing pipeline. A crew of specialized AI agents (Coordinator, Researcher, Writer, Editor, SEO Reviewer, Scheduler) collaborates sequentially and asynchronously to produce and publish high-quality blog content directly to WordPress.

---

## 🏗️ Architecture & Folder Structure

The project is organized into distinct modules to separate agent logic, external tools, workflows, and the core API.

```text
├── agents/                  # Specialized autonomous agents
│   ├── coordinator.py       # Orchestrates the overall process
│   ├── researcher.py        # Gathers information and sources
│   ├── writer.py            # Drafts long-form content
│   ├── editor.py            # Polishes tone, clarity, and grammar
│   ├── seo_reviewer.py      # Optimizes keyword density and meta descriptions
│   └── scheduler.py         # Selects optimal publish times based on analytics
│
├── tools/                   # External API integrations and utilities
│   ├── serper_api.py        # Google Search capabilities (Serper API)
│   ├── wordpress_api.py     # WordPress REST API integration for CMS posting
│   ├── analytics_api.py     # Feeds traffic data to determine publish times
│   └── redis_queue.py       # Redis task queue for async agent handoffs
│
├── workflows/               # Pre-defined agent execution graphs
│   ├── content_pipeline_workflow.py
│   └── ab_headline_workflow.py
│
├── app/                     # Core application and API endpoints
│   ├── api/
│   │   └── v1/
│   │       └── topic_submit.py  # Endpoint to ingest new content briefs
│   ├── core/
│   └── __init__.py
│
├── data/                    # Static assets, inputs, and templates
│   ├── sample_topics/
│   └── seo_templates/
│
├── outputs/                 # Run logs and generated artifacts
│   └── runs/            
├── tests/                   # Test suite
│   ├── unit/
│   └── integration/
├── config/                  # Environment configurations 
│   ├── dev.yaml
│   ├── staging.yaml
│   └── prod.yaml
├── README.md                # Project documentation
└── requirements.txt         # Python dependencies
```

## 🛠️ Tech Stack

- **Orchestration:** CrewAI / AutoGen / LangChain
- **LLMs:** Anthropic Claude / OpenAI GPT-4o
- **Search Integration:** Serper API
- **CMS Integration:** WordPress REST API
- **Task Management:** Redis Queue
- **Language:** Python 
- **API Framework:** FastAPI or similar (for `app/api`)

## 🚀 How It Works

The entire pipeline operates asynchronously using specialized state handoffs effectively mimicking a professional editorial team:

1. **Ingestion:** A topic brief or outline is submitted via the `topic_submit.py` API endpoint.
2. **Coordination:** The **Coordinator Agent** receives the brief and initializes the multi-agent workflow in the Redis queue.
3. **Research:** The **Researcher Agent** utilizes the `serper_api.py` tool to scrape the web for context, statistics, references, and competitor analysis.
4. **Drafting:** The **Writer Agent** takes the research context and writes a cohesive, long-form draft.
5. **Editing:** The **Editor Agent** processes the draft for tone, structural clarity, brevity, and grammar.
6. **SEO Optimization:** The **SEO Reviewer Agent** optimizes headings, ensures keyword density matches the `seo_templates`, and generates meta descriptions.
7. **Publishing:** The **Scheduler Agent** evaluates historical traffic data via `analytics_api.py` to determine the best publish time, then hands the final artifact off to the `wordpress_api.py` to schedule or publish directly.

## 🧠 Key Agentic Patterns

- **Multi-Agent Orchestration:** Complex tasks are intentionally decomposed into focused agent roles rather than prompting a single model to do everything.
- **Role Specialization:** Each agent acts using highly tuned system prompts specific to their editorial function (e.g., editorial review requires a different personality than raw data pulling).
- **Asynchronous Handoffs:** Tasks are queued in Redis, allowing agents to decouple, report intermediate statuses, and work independently.

## 🎯 Stretch Goals

- [ ] **Image Generation Agent:** Automatically create cover images and inline graphics based on the article's final context.
- [ ] **Social Media Promotion Agent:** Draft and schedule automated promotional threads (Twitter/X, LinkedIn) post-publication.
- [ ] **A/B Headline Testing:** Dynamically test and swap out blog headlines utilizing the `ab_headline_workflow.py`.
