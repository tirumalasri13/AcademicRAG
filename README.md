AcademicRAG --- Embedding Model Comparison System
AcademicRAG is a Retrieval-Augmented Generation (RAG) research platform
for exploring how different embedding models retrieve information from
academic PDF documents. Upload research papers, ask natural-language
questions, and compare retrieved passages and generated answers from
three embedding models using a Groq-hosted language model.

The project explores this research question:
How do MiniLM, E5-Large, and BGE-Large compare for academic document
question answering in terms of retrieval relevance, answer quality,
and latency?

AcademicRAG brings document ingestion, semantic search, answer
generation, and evaluation into one interface. When run locally,
embedding models use your machine's resources; Groq is used for LLM
answer generation.

Repository:
github.com/tirumalasri13/AcademicRAG
Features
- Upload and manage academic PDF documents.
- Build and compare indexes using MiniLM, E5-Large, and BGE-Large.
- Ask natural-language questions about uploaded documents.
- Compare model answers side by side.
- Inspect retrieved text chunks and similarity scores.
- Compare retrieval and response latency.
- Visualize chunk relevance with a similarity heatmap.
- Evaluate answers against reference answers using ROUGE-1, ROUGE-L,
  and BERTScore.
- Run batch evaluation using a JSON question-and-answer dataset.
- View query history.
- Export query history and batch evaluation results as CSV.
- Explore comparison charts, including bar, bubble, radar, and line
  charts.
- Switch between dark and light themes.
Technology Stack
  Area               Technologies
  Backend            Python, FastAPI, Uvicorn
Frontend           HTML, CSS, JavaScript, Chart.js
PDF processing     PyMuPDF
Text chunking      LangChain text splitters
Embeddings         Sentence-Transformers, Hugging Face
Vector search      FAISS
LLM                Groq API (openai/gpt-oss-20b)
Evaluation         ROUGE, BERTScore
Data and exports   JSON, CSV
How AcademicRAG Works
1. Document upload: Upload academic PDFs through the Document
   Manager.
2. Text extraction: The backend extracts text from the uploaded
   documents.
3. Chunking: Extracted text is split into smaller passages for
   retrieval.
4. Embedding: Each passage is encoded using MiniLM, E5-Large, and
   BGE-Large.
5. Indexing: FAISS indexes are created for semantic similarity
   search.
6. Retrieval: A user's question is encoded and relevant passages
   are retrieved for each model.
7. Generation: Retrieved context is sent to the configured Groq LLM
   to generate an answer.
8. Comparison and evaluation: The interface displays answers,
   retrieved chunks, similarity scores, latency, and optional
   evaluation metrics.
The Three Embedding Models
MiniLM-L6-v2
- Hugging Face model:
  sentence-transformers/all-MiniLM-L6-v2
- A compact model used as the speed-oriented baseline.
- Produces 384-dimensional embeddings.
- Useful for comparing retrieval speed against larger models.
E5-Large-v2
- Hugging Face model:
  intfloat/e5-large-v2
- Produces 1024-dimensional embeddings.
- Uses query and passage prefixes as part of its retrieval workflow.
- Provides a larger-model comparison point for semantic retrieval.
BGE-Large-v1.5
- Hugging Face model:
  BAAI/bge-large-en-v1.5
- Produces 1024-dimensional embeddings.
- Uses retrieval-oriented query formatting.
- Provides another high-capacity model for comparing retrieval quality
  and resource use.
Actual speed and retrieval quality depend on the machine, documents,
query, and evaluation dataset. The project is intended to measure
these differences rather than assume one model is always best.

System Requirements
For local execution, prepare:
- Python 3.10 or later.
- pip.
- Git (or download the repository as a ZIP file).
- An internet connection for initial installation and model downloads.
- Sufficient free disk space for Python packages, model weights,
  uploaded PDFs, and generated indexes.
- Adequate RAM for the embedding models. Larger models such as
  E5-Large and BGE-Large can require several gigabytes of memory
  during model loading and indexing. A machine with 16 GB RAM is a
  practical starting point, but actual requirements vary.
- A Groq API key for answer generation.
Embedding model downloads and local inference use your machine's
resources. Groq API usage is subject to Groq account limits and
policies.
Setup and Installation
1. Clone the repository
git clone https://github.com/tirumalasri13/AcademicRAG.git
cd AcademicRAG
Alternatively, download the repository as a ZIP file from GitHub and
extract it.
2. Create and activate a virtual environment
Windows (PowerShell):
python -m venv venv
.\venv\Scripts\Activate.ps1
If PowerShell blocks activation, you can use Command Prompt:
venv\Scripts\activate.bat
macOS / Linux:
python3 -m venv venv
source venv/bin/activate
3. Install dependencies
pip install -r requirements.txt
The first installation may take several minutes because the project uses
packages such as PyTorch, Transformers, Sentence-Transformers, and
FAISS.

If installing PyTorch separately is necessary for your environment,
install the CPU build and then install the remaining requirements:
pip install torch --index-url https://download.pytorch.org/whl/cpu
pip install -r requirements.txt
4. Configure the Groq API key
Create a local .env file from the example.

Windows PowerShell:
Copy-Item .env.example .env
macOS / Linux:
cp .env.example .env
Open .env and set your own key:
GROQ_API_KEY=your_groq_api_key_here
HOST=0.0.0.0
PORT=8000
Get an API key from the Groq Console.
Replace the placeholder with your key. Do not commit .env or share
your API key. The repository's .gitignore excludes local environment
files.
5. Run the application
From the project root:
cd backend
python main.py
When the server starts, open http://localhost:8000.

Keep the terminal open while using the application. Stopping the process
will stop the local server.
First-Time Usage
1. Open Document Manager.
2. Upload one or more academic PDF files.
3. Select Build Index for All Models.
4. Wait for the embedding and indexing process to complete.
5. Open Ask & Compare.
6. Enter a question about the uploaded documents and run the
   comparison.
7. Review the answers, retrieved passages, similarity scores, and
   latency for each model.
8. To evaluate an answer against a reference, provide a ground-truth
   answer and calculate the available evaluation scores.
9. Use Batch Evaluation to evaluate a collection of
   question-and-answer pairs from a JSON file.
The first run of each embedding model may take longer because model
weights need to be downloaded. Subsequent runs can use the locally
cached model files.
Application Sections
Dashboard
Shows the embedding model cards, index status, and document/index
statistics.
Document Manager
Supports PDF upload and deletion. Uploaded documents are stored under
backend/data/papers/. Use the indexing action to extract text, split
it into chunks, generate embeddings, and create the FAISS indexes.
Ask & Compare
Runs a question through the three retrieval configurations. The page
presents generated answers, retrieved chunks, similarity information,
and latency so the results can be compared.
Batch Evaluation
Accepts a JSON dataset of questions and reference answers. It runs the
evaluation workflow and presents aggregate metrics and visual
comparisons. Results can be exported as CSV.
Query History
Displays recent queries and provides CSV export functionality.
Sample QA Dataset Format
Batch evaluation uses a JSON array containing question and answer
fields:
[
  {
    "question": "What is the attention mechanism?",
    "answer": "The attention mechanism allows a model to focus on relevant parts of an input when producing an output."
  },
  {
    "question": "What are the limitations of BERT?",
    "answer": "BERT has a maximum input sequence length and can require substantial computational resources."
  },
  {
    "question": "How does positional encoding work in transformers?",
    "answer": "Positional encoding adds information about token positions to token embeddings."
  }
]
A sample dataset is included at backend/data/qa_dataset.json.
Evaluation Metrics
- ROUGE-1: Measures unigram overlap between a generated answer and
  its reference answer.
- ROUGE-L: Measures the longest common subsequence overlap between
  the generated answer and reference.
- BERTScore: Uses contextual embeddings to estimate semantic
  similarity between generated and reference text.
- Latency: Measures the time taken to complete the query workflow,
  including retrieval and LLM generation.
- Similarity score: Represents the similarity between the query
  and retrieved passages according to the embedding model.
These metrics provide different views of system behavior. No single
metric fully captures answer correctness, so results should be
interpreted together and checked against the source documents.
Configuration
Project settings are maintained in backend/config.py. Depending on the
current implementation, configurable values include:
- Chunk size and chunk overlap.
- Default number of retrieved chunks (Top-K).
- Groq model and generation settings.
- Maximum PDF upload size.
- Paths used for uploaded documents and vector indexes.
The configured Groq model in this version is openai/gpt-oss-20b. If
you change the model or other settings, make sure the selected model is
available to your Groq account.
Troubleshooting
Invalid or missing Groq API key
- Confirm that the local file is named .env.
- Check that GROQ_API_KEY contains your valid key.
- Do not add quotes or extra spaces around the value.
- Confirm that the key has not been revoked.
- Never paste your key into GitHub, screenshots, or public issue
  reports.
Index not found when querying
Upload at least one PDF and build the indexes from the Document Manager
before querying.
PyTorch installation fails
Try installing the CPU version of PyTorch first, then rerun the
requirements installation:
pip install torch --index-url https://download.pytorch.org/whl/cpu
pip install -r requirements.txt
Out-of-memory error during indexing
Close other memory-intensive applications and try processing a smaller
document set. Larger embedding models require more memory than MiniLM.
Reducing the chunk size may also help, depending on the implementation.
Model download is slow or fails
Check your internet connection and retry. The first run downloads model
weights from Hugging Face. Downloads are cached locally, so they
generally do not need to be repeated on every run.
Port 8000 is already in use
Change the PORT value in .env (for example, PORT=8001) and restart
the application. Then open the matching local URL, such as
http://localhost:8001.

On Windows, inspect the process using port 8000 with:
netstat -ano | findstr :8000
Frontend shows a server-offline message
Make sure the FastAPI backend is running and check the terminal for
startup errors.
Project Structure
AcademicRAG/
├── frontend/
│   ├── index.html
│   ├── css/
│   │   ├── main.css
│   │   ├── components.css
│   │   └── animations.css
│   └── js/
│       ├── app.js
│       ├── api.js
│       ├── ui.js
│       ├── charts.js
│       └── history.js
├── backend/
│   ├── main.py
│   ├── config.py
│   ├── logger.py
│   ├── pipeline/
│   │   ├── pdf_parser.py
│   │   ├── chunker.py
│   │   ├── embedder.py
│   │   └── retriever.py
│   ├── services/
│   │   ├── groq_service.py
│   │   ├── evaluator.py
│   │   └── report_gen.py
│   ├── routes/
│   │   ├── upload.py
│   │   ├── documents.py
│   │   ├── query.py
│   │   └── evaluate.py
│   └── data/
│       └── qa_dataset.json
├── requirements.txt
├── .env.example
├── .gitignore
└── README.md
Uploaded PDFs, generated FAISS indexes, logs, and the local .env file
are runtime/local data and are not included in the GitHub repository.
Future Improvements
- Add support for more embedding models.
- Expand evaluation datasets and test across different academic
  domains.
- Add configurable retrieval strategies and reranking.
- Improve evaluation reporting and experiment reproducibility.
- Add support for additional document formats.
Credits and Tools
  Tool / Model                                                                    Purpose
  FastAPI                                        Backend API framework

  Uvicorn                                             ASGI server

  PyMuPDF                                      PDF text extraction

  LangChain                                         Text splitting utilities

  Sentence-Transformers                                 Embedding model interface

  FAISS                                                      Vector similarity search

  Groq                                               LLM inference API

  ROUGE Score                            Text-overlap evaluation

  BERTScore                              Semantic answer evaluation

  Chart.js                                            Data visualizations

  MiniLM-L6-v2   Lightweight embedding model

  E5-Large-v2                      Large embedding model
BGE-Large-v1.5                 Large embedding model
Author
Ampolu Tirumala Sri
- GitHub: @tirumalasri13
- Project: AcademicRAG