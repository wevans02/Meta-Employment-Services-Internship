# On-Premises Data Sync & Real-Time BI Pipeline

A containerized synchronization engine that bridges Microsoft 365 cloud data stores with an isolated on-premises relational database. The system utilizes advanced write-time query pre-calculation to enable real-time operational dashboarding via Power BI DirectQuery with zero interface latency.

## System Architecture

```mermaid
%%{init: { 'theme': 'dark', 'themeVariables': { 'lineColor': '#a1a1aa', 'labelColor': '#ffffff', 'primaryColor': '#1e293b', 'primaryTextColor': '#ffffff', 'actorLineColor': '#ffffff' }}}%%
graph TD
    MSLists[Microsoft Lists Cloud API] -->|Continuous State Checks| DockerSync[Python Sync Tool inside Docker]
    
    subgraph On-Premises Isolated Environment
        DockerSync -->|1. Stream Cleaned Updates| Postgres[(PostgreSQL On-Premises DB)]
        
        subgraph Write-Time Optimization Engine
            Postgres -->|2. Trigger Immediate Sync Event| PreCalc[SQL Premature Computation Engine]
            PreCalc -->|3. Materialize Key KPIs & Aggregations| Postgres
        end
    end

    PowerBI[Power BI Premium Gateway] -->|4. Live DirectQuery Pull| Postgres
    PowerBI -->|5. Render Real-Time Dashboard| UI[Embedded Report App UI]
```

## Key Achievements & Metrics
*   **Zero-Lag Live Reporting:** Scaled database response times to feed live reporting streams for over 1,000+ active enterprise clients simultaneously.
*   **Instantaneous Loading:** Eliminated runtime rendering bottlenecks by embedding a live Power BI DirectQuery dashboard tracking 8-10 core operational KPIs simultaneously.

## Technical Implementation & Decisions
*   **Write-Time Premature Calculations:** Rather than letting Power BI execute expensive data aggregations on the fly (which causes severe interface slowdowns), all calculations are performed via optimized native SQL scripts inside PostgreSQL the exact moment the Docker container pushes an update. 
*   **Continuous Synchronization Container:** Implemented a lightweight, isolated Python daemon running within Docker to constantly ping and pull mutations from Microsoft Lists, ensuring the on-premises engine stays up-to-the-minute accurate.
