# Autonomous Agentic Grant Search & Application Engine

An automated AI workforce that eliminates manual regulatory discovery and document formulation. Built utilizing custom asynchronous orchestration loops, the agent autonomously crawls external web endpoints, parses complex compliance rules, and generates contextual long-form grant drafts.

## System Architecture

```mermaid
%%{init: { 'theme': 'dark', 'themeVariables': { 'lineColor': '#a1a1aa', 'labelColor': '#ffffff', 'primaryColor': '#1e293b', 'primaryTextColor': '#ffffff', 'actorLineColor': '#ffffff' }}}%%
graph TD
    %% Trigger & Cache Lookup
    Start[Daily Cron Scheduler / Wake Up] --> CheckCache{Check MongoDB Cache<br/>for Opportunity URL}
    
    CheckCache -->|URL Found| EndCached[Skip Execution & Sleep]
    CheckCache -->|URL Not Found| Scraper[Web Scraper: Collect Primary & Target Page Text]

    %% Main Processing Subgraph
    subgraph Core Automation Engine
        Scraper -->|Raw Page Text| LLM[LLM API Engine]
        
        LLM -->|Generate| Draft[Application Draft & Header]
        LLM -->|Extract| Keywords[File Search Keywords]
        
        Keywords --> BambooSearch[BambooHR Asset Search]
        
        subgraph Guardrail & Storage Pipeline
            BambooSearch -->|Apply Limit: Max 3 Files / Category| Downloader[Download & Zip Compress Files]
        Downloader -->|Cache Metadata & Draft| Mongo[(MongoDB Atlas Storage)]
        end
    end

    %% Delivery & Cleanup
    Draft --> TeamsNotifier[MS Teams Notification Bot]
    Downloader -->|Attach Compressed Assets| TeamsNotifier
    
    TeamsNotifier -->|Notify Users & Provide Link| EndUser[Staff / Reviewers]
    TeamsNotifier -->|Trigger Post-Run| Cleanup[Local Directory File Cleanup]
    Cleanup --> Sleep[Reset State for Next Day]
```

## Key Achievements & Metrics
*   **90% Processing Efficiency:** Reduced manual regulatory submission preparation workflows by 90% by executing multi-tier legal parsing algorithms automatically.
*   **Multi-Domain Flexibility:** Designed the system logic engine to seamlessly output custom documents tailored automatically across 3 unique federal and corporate grant type templates.

## Technical Implementation & Decisions
*   **Custom Agent Orchestration:** Opted to write standard native Python loops instead of bringing in high-level frameworks (e.g., CrewAI or LangGraph). This decision reduced runtime execution overhead, completely avoided rigid structure constraints, and allowed micro-optimized handling of target string data streams.
*   **Human-In-The-Loop Design:** Engineered the system to act purely as an autonomous compiler that hands off high-fidelity application drafts directly to team stakeholders, removing algorithmic risk prior to official submission.
