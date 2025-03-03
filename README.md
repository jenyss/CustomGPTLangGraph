# Custom GPT based on LangGraph with Search and Summarization Tools

This Assistant processes company documents and provides precise answers with citations. It supports PDFs, text files, Word, and Excel, extracting content and storing vector embeddings in ChromaDB for efficient retrieval. When a user asks a question, it performs a similarity search, retrieves the most relevant snippets, and ensures responses are grounded in documented sources.

If you have any questions or would like to collaborate, feel free to reach out to me on [LinkedIn](https://www.linkedin.com/in/jenya-stoeva-60477249/). You're more than welcome!

## How It Works

**At the beginning, there is code to (re)initialize Chroma DB. This is only necessary if you are experimenting with different ways of splitting documents, as you must clean the database before creating new embeddings to avoid duplications and other issues. If you are not planning to experiment with splitting and embedding, you only need to initialize the database once, and after that, you can skip executing the cell!**

1️⃣ Document Ingestion & Embedding (One-Time Process)

📂 **Supported Formats**: PDF, TXT, DOC, DOCX, XLS, XLSX  
* Extracts text from documents while preserving metadata (e.g., source, page number).  
* Splits text into chunks using **RecursiveCharacterTextSplitter** or **SemanticChunker**. You must select which one to use by commenting/uncommenting the ```splitter``` in the code. You can also define the ```chunk_size``` and ```chunk_overlap```.
  
```
# Define recursive text splitter
splitter = RecursiveCharacterTextSplitter(
    chunk_size=1500,
    chunk_overlap=150,
    separators=["\n\n\n", "\n\n", "\n", ". ", " ", ""]
)

# Define semantic text splitter
# splitter = SemanticChunker(
#     embeddings=OpenAIEmbeddings(),
#     breakpoint_threshold_type="percentile",  # Method to determine breakpoints
#     breakpoint_threshold_amount=85.0  # Threshold value
# )
```
* Converts text chunks into vector embeddings using **OpenAI’s text-embedding-3-large** model.
* Stores embeddings in **ChromaDB**, ensuring documents are easily searchable.  

2️⃣ Query Processing (Every Time a User Asks a Question)

🔍 When a user asks something, the `search_docs(query)` function:  
* Converts the query into an embedding.  
* Performs **similarity search** in ChromaDB.  
* Retrieves the **top-k most relevant document chunks**. Current configuration is ```k=10``` and you can change it as you wish.
* Extracts relevant snippets from the retrieved content.  
* Returns results with **citations** (document name & page number).  

3️⃣ AI Assistant Handles User Queries

🤖 **Built with GPT-4o** and follows strict response guidelines:  
* Queries the **SearchDocsTool** first before answering.  
* Uses the **SummarizationTool** for high-level overviews.  Supports summarization Options: ```"stuff", "map_reduce", "refine"```. You can switch between these depending on your needs!
* Provides **precise responses with citations** (no hallucinated information).  
* If no relevant documents are found, it explicitly states: *"I don't have sufficient documentation to answer this question accurately."*

## Intallation

<b>Prerequisites</b>

* Access to <b>JupyterLab, Google Colab</b>, or another interactive computing environment to run this Jupyter Notebook.

### Step 1: Clone the Repository

Clone this repository to your local machine:
```
git clone <REPOSITORY_URL>
cd <PROJECT_FOLDER>
```

### Step 2: Open Jupyter Notebook in JupyterLab

Ensure that ```<PROJECT_FOLDER>``` is accessible in JupyterLab by setting it as your working directory in JupyterLab.
 * In JupyterLab, use the "Open from Path" option to load ```CustomGPTLangGraph.ipynb```.
 * Similarly, load ```.env``` and populate the variable keys with appropriate values.
 * The first cell in the Notebook installs the required libraries: **pip install langchain_experimental PyMuPDF langchain langgraph chromadb openai python-dotenv langchain-chroma**

### Step 3: Run the Jupyter Notebook

To execute the notebook, select each cell and press ```Shift + Enter```.
