# Autonomous Agentic Grant Search & Application Engine

An automated AI workforce that eliminates manual regulatory discovery and document formulation. Built utilizing custom asynchronous orchestration loops, the agent autonomously crawls external web endpoints, parses complex compliance rules, and generates contextual long-form grant drafts.

## System Architecture

```mermaid
%%{init: { 'theme': 'dark', 'themeVariables': { 'lineColor': '#a1a1aa', 'labelColor': '#ffffff', 'primaryColor': '#1e293b', 'primaryTextColor': '#ffffff', 'actorLineColor': '#ffffff' }}}%%
graph LR
    %% Main Vertical Flow for Scraping & Generation
    subgraph Data Processing Pipeline [1. Ingestion & Generation]
        direction TB
        Start[Daily Cron Scheduler] --> CheckCache{Check MongoDB Cache}
        CheckCache -->|Found| Skip[Skip Execution]
        CheckCache -->|Not Found| Scraper[Web Scraper: Page Text]
        Scraper --> LLM[LLM API Engine]
        LLM --> Draft[Application Draft & Header]
        LLM --> Keywords[File Keywords]
        Draft --> Mongo[(MongoDB Atlas Storage)]
    end

    %% File Handling Pipeline
    subgraph File Engine [2. File Retrieval]
        direction TB
        Keywords --> Bamboo[BambooHR Asset Search]
        Bamboo --> Limit[Limit: Max 3 Files / Category]
        Limit --> Zip[Download & Zip Files]
    end

    %% Horizontal Move to Delivery & Cleanup
    Draft --> Teams[MS Teams Notification Bot]
    Zip -->|Attach Zip| Teams

    subgraph Delivery & Cleanup [3. Delivery & Reset]
        direction TB
        Teams --> Notify[Notify Staff & Provide Link]
        Notify --> Cleanup[Local File Cleanup]
        Cleanup --> Reset[Reset State for Next Day]
    end
```

## Key Achievements & Metrics
*   **90% Processing Efficiency:** Reduced manual regulatory submission preparation workflows by 90% by executing multi-tier legal parsing algorithms automatically.
*   **Multi-Domain Flexibility:** Designed the system logic engine to seamlessly output custom documents tailored automatically across 3 unique federal and corporate grant type templates.

## Technical Implementation & Decisions
*   **Custom Agent Orchestration:** Opted to write standard native Python loops instead of bringing in high-level frameworks (e.g., CrewAI or LangGraph). This decision reduced runtime execution overhead, completely avoided rigid structure constraints, and allowed micro-optimized handling of target string data streams.
*   **Human-In-The-Loop Design:** Engineered the system to act purely as an autonomous compiler that hands off high-fidelity application drafts directly to team stakeholders, removing algorithmic risk prior to official submission.
