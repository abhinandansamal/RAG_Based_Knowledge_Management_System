# RAG_Based_Knowledge_Management_System

This project is a full-stack Python application that builds an interactive knowledge base from your documents. Users can upload `.txt` and `.pdf` files, which are then processed, vectorized, and stored. A conversational AI, powered by a Large Language Model (LLM) and Retrieval-Augmented Generation (RAG), can then answer questions based on the content of those documents, as shown below:

<img width="1260" height="861" alt="image" src="https://github.com/user-attachments/assets/ce138751-8f87-4c89-add8-7386e8788337" />

## Overview

The application leverages the following key components:

* **Flask:** A lightweight WSGI web application framework for serving the user interface and API endpoints.
* **Vector Store (Chroma):** A local vector database (persisted as `chroma.sqlite3` by default) used to store embeddings of the document chunks for efficient semantic search.
* **S3 Storage (Simulated):** A service (currently likely simulated as per the provided code without actual AWS credentials) for storing the uploaded documents.
* **LLM Service:** A service that interacts with a Large Language Model to generate answers based on the user's query and the retrieved relevant document chunks from the vector store.
* **LangChain:** Used for document loading (`TextLoader`, `PyPDFLoader`) and text splitting (`RecursiveCharacterTextSplitter`).

## ⚙️ Application Architecture
The application follows a clear, modular workflow:

**1. File Upload** (`/upload`):

* A user uploads a `.pdf` or `.txt` file via the **Flask** web interface.

* The file is saved to a cloud bucket using the **AWS S3** Storage Service.

* The document is loaded and split into text chunks using **LangChain**.

* The **Vector Store Service** generates embeddings (using OpenAI) for each chunk and stores them in the **Chroma** vector database.

**2. Question Answering** (`/query`):

* A user submits a question via the Flask API.

* The **LLM Service** takes the question.

* It queries the **Vector Store** to retrieve the most relevant document chunks (the "context").

* The service uses LangChain's `ConversationalRetrievalChain` to send the question and the context to the **OpenAI LLM** (gpt-3.5-turbo).

* The LLM generates an answer, which is returned to the user as a JSON response.

## 🔧 Technology Stack
* **Backend:** Flask
* **AI & RAG:** LangChain, OpenAI (gpt-3.5-turbo)
* **Vector Database:** ChromaDB (for semantic search and storage)
* **File Storage:** AWS S3 (via `boto3`)
* **Core:** Python 3.11

## ✨ Features
* **Document Upload:** Web interface to upload `.txt` and `.pdf` files.
* **Cloud Storage:** Integrates with AWS S3 for scalable file storage.
* **Document Processing:** Automatically loads, splits, and creates embeddings for document content.
* **Persistent Vector Store:** Uses ChromaDB to save and load the vector database from disk.
* **Conversational Q&A:** Leverages LangChain's `ConversationalRetrievalChain` and `ConversationBufferMemory` to answer questions with context and remember chat history.
* **API-Driven:** Clean RESTful endpoints for uploading documents and posting queries.
* **Modular Code:** Services are separated by concern (LLM, Storage, Vector Store).

## 🚀 Getting Started
Follow these steps to set up and run the project locally.

### 1. Prerequisites
* Python 3.11
* Conda (or another virtual environment manager)
* Git

### 2. Clone the Repository

```bash
git clone https://github.com/abhinandansamal/RAG_Based_Knowledge_Management_System.git
cd RAG_Based_Knowledge_Management_System
```

### 3. Create & Activate Conda Environment

```bash
conda create -n llmapp python=3.11 -y
conda activate llmapp
```

### 4. Install Dependencies

```bash
pip install -r requirements.txt
```
### 5. Configure Environment Variables
This application requires API keys and configuration to connect to OpenAI and AWS S3. You will need to set these in your environment.

The application reads from `config.py`.

**Required Variables:**

* `OPENAI_API_KEY`: Your API key from OpenAI.
* `AWS_ACCESS_KEY`: Your AWS IAM user access key.
* `AWS_SECRET_KEY`: Your AWS IAM user secret key.
* `AWS_BUCKET_NAME`: The name of the S3 bucket where files will be stored.
* `VECTOR_DB_PATH`

### 6. Run the Application

```bash
python app/main.py
```

The application will now be running on http://0.0.0.0:8080 (or http://localhost:8080).

<img width="1043" height="212" alt="image" src="https://github.com/user-attachments/assets/b17571c4-eddb-44e4-8066-a7570e77a508" />

## 🌐 API Endpoints
* `GET /`

   * Description: Renders the main web interface (`index.html`) for uploading files and asking questions.

* `POST /upload`

   * Description: Accepts file uploads. Processes the document, stores it in S3, and adds its content to the vector database.

   * Body: `multipart/form-data` with a `file` key.

   * Returns: JSON response indicating success or failure.

* `POST /query`

   * Description: Accepts a user's question, retrieves context, and returns an LLM-generated answer.

   * Body: JSON payload: `{ "question": "Your question here" }`

   * Returns: JSON response: `{ "response": "The AI's answer here" }`


## 📂 Project Structure

The code demonstrates a modular structure:

    ├── app/
    │   ├── __init__.py
    │   ├── config.py
    │   ├── main.py
    │   ├── models/
    │   │   ├── __init__.py
    │   │   └── vector_store.py
    │   ├── services/
    │   │   ├── __init__.py
    │   │   ├── llm_service.py
    │   │   └── storage_service.py
    │   ├── static/
    │   │   └── style.css
    │   ├── templates/
    │   │   └── index.html
    ├── data/
    │   ├── Attention_Is_All_You_Need.pdf
    │   └── Transformers_for_Low-Resource_Languages_Is_Féidir_Linn!.pdf
    ├── vector_db/
    │   ├── d7df5c68-44a9-4e9d-9188-88d5be934b98
    │   └── chroma.sqlite3
    ├── requirements.txt
    ├── README.md      # This file
    └── LICENSE        # MIT License


* **`app.py`:** Contains the main Flask application logic, routing, and request handling.
* **`models/vector_store.py`:** Likely handles the interaction with the Chroma vector database (creation, adding documents, querying).
* **`services/storage_service.py`:** Manages the storage of uploaded files (currently likely a simplified or simulated version).
* **`services/llm_service.py`:** Encapsulates the logic for interacting with the Large Language Model (embedding generation, question answering).
* **`config.py`:** Stores configuration parameters (like the vector database path).
