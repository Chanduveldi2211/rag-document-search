# 📚 RAG Document Search

<p align="center">
  <img src="https://img.shields.io/badge/Java-17-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white" />
    <img src="https://img.shields.io/badge/Spring_Boot-3.x-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white" />
      <img src="https://img.shields.io/badge/LangChain4j-0.27-1C3C3C?style=for-the-badge" />
        <img src="https://img.shields.io/badge/PostgreSQL-pgvector-316192?style=for-the-badge&logo=postgresql&logoColor=white" />
          <img src="https://img.shields.io/badge/OpenAI-Embeddings-412991?style=for-the-badge&logo=openai&logoColor=white" />
            <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" />
</p>p>

> A **Retrieval-Augmented Generation (RAG)** system built with Java 17 and LangChain4j. Upload PDF, DOCX, or TXT documents and query them with natural language — the AI retrieves relevant context and generates accurate, grounded answers.
>
> ---
>
> ## 🧠 How RAG Works
>
> ```
> User Query
>     │
>     ▼
> ┌─────────────────┐
> │  Query Embedding │  ← OpenAI text-embedding-ada-002
> └────────┬────────┘
>          │
>          ▼
> ┌─────────────────┐
> │  Vector Search  │  ← PostgreSQL pgvector similarity search
> │  (pgvector)     │
> └────────┬────────┘
>          │  Top-K relevant chunks
>          ▼
> ┌─────────────────┐
> │  LLM Generation │  ← GPT-4 with retrieved context
> │  (GPT-4)        │
> └────────┬────────┘
>          │
>          ▼
>     Grounded Answer
> ```
>
> ---
>
> ## ✨ Features
>
> - 📄 **Multi-format Support** — PDF, DOCX, TXT, Markdown document ingestion
> - - 🔢 **Vector Embeddings** — OpenAI `text-embedding-ada-002` for semantic search
>   - - 🗄️ **pgvector Storage** — PostgreSQL with pgvector extension for high-performance similarity search
>     - - 💬 **Natural Language Q&A** — ask questions and get AI-generated answers with source citations
>       - - 🔗 **Source Attribution** — every answer includes references to the source documents
>         - - 🌐 **REST API** — clean API for document upload and querying
>           - - 📊 **Admin Dashboard** — manage documents, view embeddings, monitor queries
>             - - ⚡ **Chunking Strategies** — configurable chunk size and overlap for optimal retrieval
>              
>               - ---
>
> ## 🛠️ Tech Stack
>
> | Component | Technology |
> |-----------|------------|
> | Language | Java 17 |
> | Framework | Spring Boot 3.2 |
> | AI Orchestration | LangChain4j 0.27 |
> | LLM | OpenAI GPT-4 |
> | Embeddings | OpenAI text-embedding-ada-002 |
> | Vector DB | PostgreSQL 15 + pgvector |
> | Document Parsing | Apache PDFBox, Apache POI |
> | Build Tool | Maven 3.9 |
> | Containerization | Docker & Docker Compose |
>
> ---
>
> ## 🚀 Getting Started
>
> ### Prerequisites
>
> - Java 17+
> - - Maven 3.9+
>   - - Docker & Docker Compose
>     - - OpenAI API Key
>      
>       - ### Quick Start
>      
>       - ```bash
>         # Clone the repository
>         git clone https://github.com/Chanduveldi2211/rag-document-search.git
>         cd rag-document-search
>
>         # Start PostgreSQL with pgvector
>         docker-compose up -d postgres
>
>         # Set environment variables
>         export OPENAI_API_KEY=your_api_key_here
>
>         # Run the application
>         mvn spring-boot:run
>         ```
>
> ### Upload a Document
>
> ```bash
> # Upload a PDF document
> curl -X POST http://localhost:8080/api/documents/upload \
>   -F "file=@/path/to/your/document.pdf" \
>   -F "name=My Document"
>
> # Response
> {
>   "documentId": "doc-123",
>   "status": "PROCESSED",
>   "chunks": 47,
>   "message": "Document successfully ingested and indexed"
> }
> ```
>
> ### Query Your Documents
>
> ```bash
> # Ask a question
> curl -X POST http://localhost:8080/api/search/query \
>   -H "Content-Type: application/json" \
>   -d '{"question": "What are the key findings in the report?"}'
>
> # Response
> {
>   "answer": "The key findings include...",
>   "sources": [
>     {"document": "My Document", "page": 3, "relevanceScore": 0.92},
>     {"document": "My Document", "page": 7, "relevanceScore": 0.87}
>   ],
>   "confidence": 0.91
> }
> ```
>
> ---
>
> ## 📁 Project Structure
>
> ```
> rag-document-search/
> ├── src/main/java/com/chanduveldi/rag/
> │   ├── controller/
> │   │   ├── DocumentController.java
> │   │   └── SearchController.java
> │   ├── service/
> │   │   ├── DocumentIngestionService.java
> │   │   ├── EmbeddingService.java
> │   │   ├── VectorSearchService.java
> │   │   └── RAGService.java
> │   ├── config/
> │   │   ├── LangChain4jConfig.java
> │   │   └── VectorStoreConfig.java
> │   ├── model/
> │   │   ├── Document.java
> │   │   ├── DocumentChunk.java
> │   │   └── SearchResult.java
> │   └── repository/
> │       └── DocumentChunkRepository.java
> ├── src/main/resources/
> │   ├── application.yml
> │   └── db/migration/
> │       └── V1__init_pgvector.sql
> ├── docker-compose.yml
> └── pom.xml
> ```
>
> ---
>
> ## ⚙️ Configuration
>
> ```yaml
> # application.yml
> app:
>   openai:
>     api-key: ${OPENAI_API_KEY}
>     embedding-model: text-embedding-ada-002
>     chat-model: gpt-4
>   rag:
>     chunk-size: 512
>     chunk-overlap: 50
>     top-k-results: 5
>     similarity-threshold: 0.75
>   document:
>     max-file-size: 50MB
>     supported-formats: pdf,docx,txt,md
> ```
>
> ---
>
> ## 📈 Performance
>
> | Metric | Value |
> |--------|-------|
> | Avg. Query Response Time | < 2 seconds |
> | Document Processing Speed | ~100 pages/minute |
> | Max Document Size | 50 MB |
> | Supported Languages | English, Spanish, French, German |
> | Embedding Dimensions | 1536 (OpenAI ada-002) |
>
> ---
>
> ## 🤝 Contributing
>
> Contributions are welcome! Please open an issue first to discuss what you'd like to change.
>
> ## 📄 License
>
> MIT License — see [LICENSE](LICENSE) for details.
>
> ---
>
> <p align="center">Made with ❤️ by <a href="https://github.com/Chanduveldi2211">Veldi Purna Chandu</a>a></p>p>
