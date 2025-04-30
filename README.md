# RAG_Based_Knowledge_Management_System

This application allows users to upload `.txt` and `.pdf` documents, store their content in a vector database, and then ask questions against that content using a Large Language Model (LLM).

## Overview

The application leverages the following key components:

* **Flask:** A lightweight WSGI web application framework for serving the user interface and API endpoints.
* **Vector Store (Chroma):** A local vector database (persisted as `chroma.sqlite3` by default) used to store embeddings of the document chunks for efficient semantic search.
* **S3 Storage (Simulated):** A service (currently likely simulated as per the provided code without actual AWS credentials) for storing the uploaded documents.
* **LLM Service:** A service that interacts with a Large Language Model to generate answers based on the user's query and the retrieved relevant document chunks from the vector store.
* **LangChain:** Used for document loading (`TextLoader`, `PyPDFLoader`) and text splitting (`RecursiveCharacterTextSplitter`).

## Features

* **Document Upload:** Users can upload `.txt` and `.pdf` files through a web interface.
* **Document Processing:** Uploaded documents are processed by:
    * Loading the content.
    * Splitting the text into manageable chunks.
    * Generating embeddings for these chunks (implicitly done by the `VectorStore` and `LLMService`).
* **Storage:** Uploaded files are stored (currently likely simulated) using an S3-like storage service.
* **Vector Database:** The embeddings of the document chunks are stored in a local Chroma vector database for semantic search.
* **Question Answering:** Users can ask questions through an API endpoint. The application retrieves relevant document chunks based on the question's embedding and uses an LLM to generate an answer.
* **Logging:** The application includes basic logging for debugging and monitoring.


## How to run?

### STEPS:

### STEP 01 - Create a conda environment after opening the repository

```bash
conda create -n llmapp python=3.11 -y
```

```bash
conda activate llmapp
```

### STEP 02 - Install the requirements
```bash
pip install -r requirements.txt
```

```bash
# Finally run the following command
python app/main.py
```

Now,
```bash
open up your local host and port
```

## Endpoints

* **`/` (GET):** Renders the main web interface (`index.html`) for uploading files and asking questions.
* **`/upload` (POST):** Accepts file uploads (only `.txt` and `.pdf` are currently supported). Processes the document, stores it (simulated S3), and adds its content to the vector database. Returns a JSON response indicating success or failure.
* **`/query` (POST):** Accepts a JSON payload with a `question` key. Queries the vector database for relevant document chunks and uses the LLM service to generate a response. Returns a JSON response containing the `response`.

## Project Structure

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