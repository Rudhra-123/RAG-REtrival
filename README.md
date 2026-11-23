# RAG  - Spring AI

A demonstration of RAG (Retrieval-Augmented Generation) technology using Spring AI with Ollama and PGVector.

## 📋 Overview

This project demonstrates the implementation of a RAG (Retrieval-Augmented Generation) system that allows users to ask questions about PDF documents and receive contextualized answers using artificial intelligence. The system uses a vector database to store and search for relevant information within the documents.

### Current Configuration
- **Analyzed Document**: "La Fortune des Rougon" by Émile Zola
- **Chat Model**: Llama 3.1 8B via Ollama
- **Embedding Model**: nomic-embed-text
- **Vector Database**: PGVector (PostgreSQL)

## 🛠️ Technologies Used

- **Java 21**
- **Spring Boot 3.5.3**
- **Spring AI 1.0.0**
- **Ollama** (for LLM models)
- **PostgreSQL** with PGVector extension
- **Docker Compose** (for infrastructure)

## 📦 Main Dependencies

- `spring-ai-starter-model-ollama` - Integration with Ollama
- `spring-ai-pdf-document-reader` - PDF reading and processing
- `spring-ai-starter-vector-store-pgvector` - Vector database
- `spring-ai-starter-model-chat-memory` - Conversation memory management
- `spring-ai-advisors-vector-store` - Advisors for vector search

## 🚀 Prerequisites

1. **Java 21** or higher
2. **Maven 3.6+**
3. **Docker** and **Docker Compose**
4. **Ollama** installed locally

### Installing Ollama

```bash
# macOS
brew install ollama

# Start the service
ollama serve

# Download required models
ollama pull llama3.1:8b
ollama pull nomic-embed-text
```

## 🏃‍♂️ Quick Start

### 1. Clone the repository

```bash
git clone <repository-url>
cd spring-ai-RAG-demo
```

### 2. Start PostgreSQL with PGVector

```bash
cd src/main/docker
docker-compose up -d
```

### 3. Check Ollama service

```bash
curl http://localhost:11434/api/tags
```

### 4. Build and Run the Application

```bash
mvn clean compile

# First run: fill the vector store and next continue with console interaction
mvn spring-boot:run -Dspring-boot.run.arguments="--fillVectorStore"

# Next runs: start the application with console interaction
mvn spring-boot:run
```

## 💬 Usage

Once the application is started, you can interact with it via the console:

```
Ask a question: Who are the main characters of the novel? ?

[The AI responds based on the content of the document...]

Ask a question: exit
```

### Example questions

- "What is the historical context of the novel?"
- "Describe the main character"
- "What are the main themes of the work?"
- "Summarize the first chapter"

## ⚙️ Configuration

### Changing the source document

To analyze another PDF document, edit the `application.yml` file:

```yaml
rag:
  system-prompt: "You are an expert on the provided document and you answer questions based on the information given"
  document-path: "classpath:your-document.pdf"
```

### Changing the chat model

```yaml
spring:
  ai:
    ollama:
      chat:
        options:
          model: llama3.2:3b  # Lower memory model
          # model: llama3.1:8b  # Default model
          temperature: 0.1
```

### Database Configuration

Ensure the database connection settings in `application.yml` match your PostgreSQL setup:

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/vector_db
    username: postgres_user
    password: postgres_password
```

## 🏗️ Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Document PDF  │────│  Text Splitter  │────│  Embeddings     │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                                        │
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│  User Question  │────│   Chat Client   │────│   PGVector DB   │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                │
                       ┌─────────────────┐
                       │   Ollama LLM    │
                       └─────────────────┘
```

## 🔧 Development

### Project Structure

```
src/
├── main/
│   ├── java/com/zenika/demo/rag/
│   │   └── RagDemoApplication.java      # Application entry point
│   ├── resources/
│   │   ├── application.yml              # Configuration
│   │   └── *.pdf                        # Documents source
│   └── docker/
│       └── compose.yml                  # Docker Compose for PostgreSQL with PGVector
```

### Main Features

1. **Document Ingestion**: Reading and splitting PDFs into chunks
2. **Embeddings Generation**: Converting text into vectors
3. **Vector Storage**: Saving into PGVector
4. **Semantic Search**: Retrieving relevant context
5. **Answer Generation**: Using an LLM to respond

## 🐛 Troubleshooting

### Common Issues

1. **Ollama service not running**
   ```bash
   # Check if Ollama is running
   ollama serve
   ```

2. **Database connection issues**
   ```bash
   # Restart PostgreSQL with PGVector
   docker-compose down && docker-compose up -d
   ```

3. **Models not found**
   ```bash
   # Download required models
   ollama pull llama3.1:8b
   ollama pull nomic-embed-text
   ```

4. **Unsufficient memory for model**
   - Use a smaller model (`llama3.2:3b`)
   - Increase memory: `-Xmx4g`

### Checking Logs

```bash
mvn spring-boot:run

# Check PostgreSQL logs
docker-compose logs pgvector

# List Ollama models
ollama list
```

## 📚 References

- [Spring AI Documentation](https://docs.spring.io/spring-ai/reference/)
- [Ollama](https://ollama.ai/)
- [PGVector](https://github.com/pgvector/pgvector)
- [Spring Boot](https://spring.io/projects/spring-boot)

## 🤝 Contribution

This project is an educational demonstration. Contributions are welcome to:
- Add new document types
- Improve system prompts
- Optimize performance
- Add tests
