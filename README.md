# Commercial Production Systems Portfolio

This repository documents the production system designs, architectural patterns, and engineering decisions led during my tenure. Because the production source code is proprietary and confidential, this space serves as a transparent look into my technical problem-solving process, engineering maturity, and system design expertise.

---

## Project Directory

Click on any project title below to jump directly into its dedicated subfolder. Inside, you will find standalone **Mermaid.js architecture flowcharts**, deep-dives into tech stacks, and performance optimization choices.

### 1. [SIMD-Accelerated Teams RAG Bot & Secure Pipeline](./RAG-system/)
*   **Brief Summary:** A production-grade Retrieval-Augmented Generation (RAG) bot integrated into Microsoft Teams, optimized using low-level hardware parallelism and protected by enterprise security layers.
*   **Key Tech:** Microsoft Teams API, FAISS Vector DB, SIMD Vectorization, Ollama, OAuth 2.0, MongoDB Atlas.
*   **Core Metric:** Reached `~0.2s` query response times across 5.5k corporate documents.
*   *[Explore the Flowcharts & Design Docs for Project 1](./RAG-system/)*

---

### 2. [Autonomous Agentic Grant Search & Application Engine](./Agentic-AI-system/)
*   **Brief Summary:** An advanced AI agent workforce designed to autonomously crawl external web endpoints, parse complex regulatory compliance rules, and generate high-fidelity long-form grant drafts.
*   **Key Tech:** Custom Orchestration, Autonomous Scrapers, Context Extraction, Python.
*   **Core Metric:** Reduced manual application preparation workflows by 90% across 3 distinct grant types.
*   *[Explore the Flowcharts & Design Docs for Project 2](./Agentic-AI-system/)*

---

### 3. [On-Premises Data Sync & Real-Time BI Pipeline](./Custom-app-with-local-database/)
*   **Brief Summary:** A containerized on-premises synchronization pipeline bridging enterprise Microsoft cloud data with relational storage, optimized heavily using write-time premature calculations.
*   **Key Tech:** Python, Docker, PostgreSQL (On-Premises), Microsoft Lists API, Power BI DirectQuery.
*   **Core Metric:** Handles live tracking for 1000+ active enterprise clients across 8-10 core KPIs with zero dashboard rendering latency.
*   *[Explore the Flowcharts & Design Docs for Project 3](./Custom-app-with-local-database/)*

---

## How to Navigate This Portfolio
1. Review the brief project summaries above to understand the engineering scopes.
2. Click any of the project folder links to view the standalone **system architecture diagrams**.
3. Read through the "Technical Implementation & Decisions" sections in the subfolders to see how I handle real-world performance bottlenecks.
