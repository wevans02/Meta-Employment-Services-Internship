# Teams RAG Bot & Secure Embedding Pipeline

A production-grade Retrieval-Augmented Generation (RAG) system integrated directly into Microsoft Teams. The architecture splits into two primary pipelines: a low-latency, hardware-accelerated user query interface and a secure, OAuth 2.0-protected web scraping and embedding processor.

## System Architecture

```mermaid
graph TD
    %% Styling
    classDef client fill:#3b5998,stroke:#333,stroke-width:2px,color:#fff;
    classDef security fill:#f96,stroke:#333,stroke-width:2px;
    classDef engine fill:#9cf,stroke:#333,stroke-width:2px;
    classDef storage fill:#bfb,stroke:#333,stroke-width:1px;

    %% Query Pipeline
    Teams[MS Teams Interface] -->|User Query| BotAPI[Teams Bot API Gateway]
    BotAPI -->|Vector Search| FAISS[FAISS Vector DB]
    
    subgraph Local Hardware Acceleration
        FAISS -->|SIMD Instruction Parallelism| CPU[Local Host CPU AVX Extensions]
        CPU -->|~0.2s Context Retrieval| FAISS
    end
    
    FAISS -->|Relevant Context| ContextEngine[Context Aggregator]
    ContextEngine -->|Payload + Context| LocalLLM[Ollama Core]
    LocalLLM -->|Synthesized Summary| Teams

    %% Ingestion Pipeline
    Scraper[Custom Data Scraper] -->|Trigger Embedding Request| OAuthCheck{OAuth 2.0 Identity Verification}
    OAuthCheck -->|Valid Enterprise Credential| OllamaEmbed[Ollama Embedding Model]
    OAuthCheck -->|Unauthorized| Deny[Drop Request]
    
    OllamaEmbed -->|Generate Dense Vectors| Atlas[(MongoDB Atlas Cloud)]

    class Teams client;
    class OAuthCheck security;
    class BotAPI,LocalLLM,ContextEngine,OllamaEmbed engine;
    class FAISS,Atlas storage;
```

## Key Achievements & Metrics
*   **Ultra-Low Latency:** Achieved query response times of `~0.2s` across 5.5k corporate documents by utilizing CPU-level SIMD (Single Instruction, Multiple Data) parallelism via FAISS vector indexing.
*   **Enterprise Security:** Implemented an absolute OAuth 2.0 boundary on the ingestion loop, ensuring only authenticated internal scraper agents can register embedded data into MongoDB Atlas.

## Technical Implementation & Decisions
*   **Vector Engine Choice:** Selected FAISS for vector similarity matching due to its highly efficient C++ backend capable of squeezing out maximum operations per clock cycle on localized computing hardware.
*   **Transition Roadmap:** Currently structurally isolated on on-premises host CPU instruction sets; mapped to seamlessly scale into a containerized cloud infrastructure environment without core logical refactoring.
