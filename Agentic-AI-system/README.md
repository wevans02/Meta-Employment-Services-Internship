# Autonomous Agentic Grant Search & Application Engine

An automated AI workforce that eliminates manual regulatory discovery and document formulation. Built utilizing custom asynchronous orchestration loops, the agent autonomously crawls external web endpoints, parses complex compliance rules, and generates contextual long-form grant drafts.

## System Architecture

```mermaid
%%{init: { 'theme': 'dark', 'themeVariables': { 'lineColor': '#a1a1aa', 'labelColor': '#ffffff', 'primaryColor': '#1e293b', 'primaryTextColor': '#ffffff', 'actorLineColor': '#ffffff' }}}%%
graph TD
    %% Left Column: Ingestion & Draft Generation Pipeline
    subgraph Ingestion & Persistence Pipeline
        Cron[Daily Cron Scheduler] --> CheckCache{Check MongoDB Cache}
        CheckCache -->|URL Found| Skip[Skip Execution & Sleep]
        CheckCache -->|URL Not Found| Scraper[Web Scraper: Collect Page Text]
        
        Scraper --> LLM[LLM API Engine]
        
        LLM -->|Generate Text| Draft[Application Draft & Header]
        LLM -->|Extract Search Terms| Keywords[File Keywords]
        
        Draft -->|Early Persistence| Mongo[(MongoDB Atlas Storage)]
    end

    %% Right Column: Asset Packaging & Delivery Pipeline
    subgraph Delivery & Packaging Pipeline
        Keywords --> Bamboo[BambooHR Asset Search]
        
        subgraph File Constraints Engine
            Bamboo --> Limit[Apply Limit: Max 3 Files / Category]
            Limit --> Zip[Download & Zip Compress Files]
        end
        
        Draft -->|Send Payload| Teams[MS Teams Notification Bot]
        Zip -->|Attach Zip Package| Teams
        
        Teams --> Notify[Notify Staff with Link]
        Notify --> Cleanup[Local Directory Cleanup & Reset]
    end
```

## Key Achievements & Metrics
*   **90% Processing Efficiency:** Reduced manual regulatory submission preparation workflows by 90% by executing multi-tier legal parsing algorithms automatically.
*   **Multi-Domain Flexibility:** Designed the system logic engine to seamlessly output custom documents tailored automatically across 3 unique federal and corporate grant type templates.

## Technical Implementation & Decisions
*   **Custom Agent Orchestration:** Opted to write standard native Python loops instead of bringing in high-level frameworks (e.g., CrewAI or LangGraph). This decision reduced runtime execution overhead, completely avoided rigid structure constraints, and allowed micro-optimized handling of target string data streams.
*   **Human-In-The-Loop Design:** Engineered the system to act purely as an autonomous compiler that hands off high-fidelity application drafts directly to team stakeholders, removing algorithmic risk prior to official submission.
