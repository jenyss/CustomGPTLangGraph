# Custom GPT based on LangGraph with Search and Summarization Tools

## How It Works

1️⃣ Document Ingestion & Embedding (One-Time Process)

📂 **Supported Formats**: PDF, TXT, DOC, DOCX, XLS, XLSX  
* Extracts text from documents while preserving metadata (e.g., source, page number).  
* Splits text into chunks using **RecursiveCharacterTextSplitter** (supports various separators).  
* Converts text chunks into vector embeddings using **OpenAI’s text-embedding-3-large** model.  
* Stores embeddings in **ChromaDB**, ensuring documents are easily searchable.  

2️⃣ Query Processing (Every Time a User Asks a Question)

🔍 When a user asks something, the `search_docs(query)` function:  
* Converts the query into an embedding.  
* Performs **similarity search** in ChromaDB.  
* Retrieves the **top-k most relevant document chunks**.  
* Extracts relevant snippets from the retrieved content.  
* Returns results with **citations** (document name & page number).  

3️⃣ AI Assistant Handles User Queries

🤖 **Built with GPT-4o** and follows strict response guidelines:  
* Queries the **SearchDocsTool** first before answering.  
* Uses the **SummarizationTool** for high-level overviews.  
* Provides **precise responses with citations** (no hallucinated information).  
* If no relevant documents are found, it explicitly states: *"I don't have sufficient documentation to answer this question accurately."*

