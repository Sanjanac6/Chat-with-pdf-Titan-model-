# Chat With PDF - Generative AI Application 📄🤖

### Built Using 🚀:
- **Amazon Bedrock**
- **Langchain**
- **Python**
- **Docker**
- **Amazon S3**

### Models Used 🧠:
- **Amazon Titan Embedding G1 - Text**
- **Anthropic Claude 2.1**

---

## 📚 Introduction
In this tutorial, we'll build a **chatbot-like application** using:
- **Amazon Bedrock**
- **Docker**
- **Python**
- **Langchain**
- **Streamlit**

We’ll leverage the **Retrieval-Augmented Generation (RAG)** concept to provide context from a knowledge base to the Large Language Model (LLM) along with user queries.

### 🛠️ Features Covered:
1. **Architecture Overview** 🏗️
2. Building **2 Applications**: **ADMIN** and **USER** 🖥️
3. Creating and running **Docker Images** 🐳

---

## 🏛️ Architecture

### **ADMIN Application** 📤:
- Web application for Admin users to **upload PDF files**.
- Steps:
  1. **Split PDF text into chunks** 📄✂️.
  2. Generate **vector representations** of chunks using **Amazon Titan Embedding Model** 🔗.
  3. Store the vector index locally using **FAISS** 📊.
  4. **Upload the vector index to Amazon S3** 🗂️ (alternatives: OpenSearch, Pinecone, PgVector).

#### Docker Commands 🐳:
- **Build Docker Image**:  
  ```bash
  docker build -t pdf-reader-admin .
  ```
- **Run ADMIN Application**:  
  ```bash
  docker run -e BUCKET_NAME=<YOUR S3 BUCKET NAME> -v ~/.aws:/root/.aws -p 8083:8083 -it pdf-reader-admin
  ```

### **USER Application** 📥:
- Web application for users to **query/chat with the PDF** 📚🤔.
- Steps:
  1. At startup, **download the vector index from S3** and build a local FAISS index 🗃️.
  2. Use **Langchain's RetrievalQA** for:
     - **Converting user queries into vector embeddings** (Amazon Titan Embedding Model, same as ADMIN) 🧬.
     - Performing **similarity search** on FAISS to retrieve relevant chunks 🔍.
     - Using **Prompt Template**, feed the question and context to the **Claude model** 📝.
  3. Display the **LLM's response** to the user 💬.

#### Docker Commands 🐳:
- **Build Docker Image**:  
  ```bash
  docker build -t pdf-reader-client .
  ```
- **Run USER Application**:  
  ```bash
  docker run -e BUCKET_NAME=<YOUR S3 BUCKET NAME> -v ~/.aws:/root/.aws -p 8084:8084 -it pdf-reader-client
  ```

---

## 📝 Notes:
- 🗂️ The **Docker volume mount** is needed only in local development.
- 🔒 For **ECS** or **EKS**, IAM roles are used instead.

---

## 📦 Key Components
- **Admin Application**: Handles PDF uploads, vectorization, and index management.
- **User Application**: Provides a chat interface for querying the uploaded PDFs.

Happy Building! 🚀🎉
