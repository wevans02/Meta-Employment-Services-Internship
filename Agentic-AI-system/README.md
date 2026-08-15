# Autonomous Agentic Grant Search & Application Engine

An automated AI workforce that eliminates manual regulatory discovery and document formulation. Built utilizing custom asynchronous orchestration loops, the agent autonomously crawls external web endpoints, parses complex compliance rules, and generates contextual long-form grant drafts.

## System Architecture

```mermaid
%%{init: { 'theme': 'dark', 'themeVariables': { 'lineColor': '#a1a1aa', 'labelColor': '#ffffff', 'primaryColor': '#1e293b', 'primaryTextColor': '#ffffff', 'actorLineColor': '#ffffff' }}}%%
graph LR
    %% Column 1: Ingestion & Deduplication
    subgraph Ingestion [1. Ingestion & Cache]
        direction TB
        Cron[Daily Cron Scheduler] --> CheckCache{MongoDB Cache?}
        CheckCache -->|Found| Skip[Skip & Sleep]
        CheckCache -->|Not Found| Scraper[Web Scraper]
    end

    %% Column 2: LLM Engine & Early Storage
    subgraph Core [2. LLM Core & DB]
        direction TB
        Scraper --> LLM[LLM API Engine]
        LLM --> Draft[Application Draft]
        LLM --> Keywords[File Keywords]
        Draft --> Mongo[(MongoDB Atlas)]
    end

    %% Column 3: Asset Packaging
    subgraph Packaging [3. Asset Packaging]
        direction TB
        Keywords --> Bamboo[BambooHR Search]
        Bamboo --> Limit[Limit: Max 3/Cat]
        Limit --> Zip[Zip Compress]
    end

    %% Column 4: Delivery & Cleanup
    subgraph Delivery [4. Delivery & Reset]
        direction TB
        Draft --> Teams[MS Teams Bot]
        Zip --> Teams
        Teams --> Notify[Notify Staff]
        Notify --> Cleanup[Cleanup & Reset]
    end
```

## Key Achievements & Metrics
*   **90% Processing Efficiency:** Reduced manual regulatory submission preparation workflows by 90% by executing multi-tier legal parsing algorithms automatically.
*   **Multi-Domain Flexibility:** Designed the system logic engine to seamlessly output custom documents tailored automatically across 3 unique federal and corporate grant type templates.

## Technical Implementation & Decisions
*   **Custom Agent Orchestration:** Opted to write standard native Python loops instead of bringing in high-level frameworks (e.g., CrewAI or LangGraph). This decision reduced runtime execution overhead, completely avoided rigid structure constraints, and allowed micro-optimized handling of target string data streams.
*   **Human-In-The-Loop Design:** Engineered the system to act purely as an autonomous compiler that hands off high-fidelity application drafts directly to team stakeholders, removing algorithmic risk prior to official submission.
