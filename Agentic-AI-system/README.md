# Autonomous Agentic Grant Search & Application Engine

An automated AI workforce that eliminates manual regulatory discovery and document formulation. Built utilizing custom asynchronous orchestration loops, the agent autonomously crawls external web endpoints, parses complex compliance rules, and generates contextual long-form grant drafts.

## System Architecture

```mermaid
%%{init: { 'theme': 'dark', 'themeVariables': { 'lineColor': '#a1a1aa', 'labelColor': '#ffffff', 'primaryColor': '#1e293b', 'primaryTextColor': '#ffffff', 'actorLineColor': '#ffffff' }}}%%
graph TD
    %% Step 1: Trigger & Cache Check
    subgraph Step1 [1. Deduplication & Ingestion]
        Start[Daily Cron Scheduler] --> CheckCache{Check MongoDB Cache<br/>for Opportunity URL}
        CheckCache -->|URL Found| Skip[Skip Execution & Sleep]
        CheckCache -->|URL Not Found| Scraper[Web Scraper: Collect Page Text]
    end

    %% Step 2: Generation & State Persistence
    subgraph Step2 [2. LLM Generation & DB Persistence]
        Scraper --> LLM[LLM API Engine]
        LLM --> Draft[Application Draft & Header]
        LLM --> Keywords[File Keywords]
        Draft -->|1. Save State Immediately| Mongo[(MongoDB Atlas Storage)]
    end

    %% Step 3: Asset Retrieval & Compression
    subgraph Step3 [3. File Retrieval & Packaging]
        Keywords --> Bamboo[BambooHR Asset Search]
        Bamboo --> Limit[Apply Limit: Max 3 Files / Category]
        Limit --> Zip[Download & Zip Compress Files]
    end

    %% Step 4: Notification & Cleanup
    subgraph Step4 [4. Delivery & Cleanup]
        Draft -->|2. Send Text| Teams[MS Teams Notification Bot]
        Zip -->|3. Attach Zip| Teams
        Teams --> Notify[Notify Staff with Link]
        Notify --> Cleanup[Local File Cleanup & Reset State]
    end
```

## Key Achievements & Metrics
*   **90% Processing Efficiency:** Reduced manual regulatory submission preparation workflows by 90% by executing multi-tier legal parsing algorithms automatically.
*   **Multi-Domain Flexibility:** Designed the system logic engine to seamlessly output custom documents tailored automatically across 3 unique federal and corporate grant type templates.

## Technical Implementation & Decisions
*   **Custom Agent Orchestration:** Opted to write standard native Python loops instead of bringing in high-level frameworks (e.g., CrewAI or LangGraph). This decision reduced runtime execution overhead, completely avoided rigid structure constraints, and allowed micro-optimized handling of target string data streams.
*   **Human-In-The-Loop Design:** Engineered the system to act purely as an autonomous compiler that hands off high-fidelity application drafts directly to team stakeholders, removing algorithmic risk prior to official submission.
