# SingleStoreVectorDB
`SingleStoreVectorDB` is a document storage solution using SingleStore for storing text documents and their vector embeddings. It supports adding documents, performing similarity searches with cosine similarity, and managing vector indexes to optimize search performance.

**Note**: To use `SingleStoreVectorDB`, users need to have a running SingleStore instance and provide the necessary connection parameters when initializing the vector store.

To use `SingleStoreVectorDB` as the vector store:

```python
from indox.vector_stores import SingleStoreVectorDB
from indox.embeddings import HuggingFaceEmbedding

db = SingleStoreVectorDB(
    connection_params={
        'user': 'your_user',
        'password': 'your_password',
        'host': 'your_host',
        'database': 'your_database'
    },
    embedding_function=HuggingFaceEmbedding(),
    table_name="your_table"
)
```
## Hyperparameters
- **connection_params** [dict]: Connection parameters for SingleStore (e.g., user, password, host, database).
- **table_name** [str]: Name of the SingleStore table to store embeddings (default: "embeddings").
- **embedding_function** [Optional[Embeddings]]: Function to generate embeddings for documents.
- **vector_dimension** [int]: Dimensionality of vector embeddings (default: 768).
- **use_vector_index** [bool]: Whether to use vector indexing for fast similarity searches (default: True).
- **use_full_text_search** [bool]: Whether to enable full-text search along with vector search (default: False).
- **vector_index_options** [Optional[dict]]: Additional options for vector indexing (default: None).

## Usage
### Setting Up the Python Environment
### Windows

1. **Create the virtual environment:**
```bash
python -m venv indox
```
2**Activate the virtual environment:**
```bash
indox\Scripts\activate
```
### macOS/Linux
1. **Create the virtual environment:**
```bash
python -m venv indox
```
2. **Activate the virtual environment:**
```bash
source indox/bin/activate
```
### Get Started
**Import HuggingFace API Key**
```python
import os
from dotenv import load_dotenv

load_dotenv()
HUGGINGFACE_API_KEY = os.getenv('HUGGINGFACE_API_KEY')
```
**Import Essential Libraries**
```python
from indox import IndoxRetrievalAugmentation
from indox.llms import HuggingFaceModel
from indox.embeddings import HuggingFaceEmbedding
from indox.data_loader_splitter.SimpleLoadAndSplit import SimpleLoadAndSplit
from indox.vector_stores import MongoDB

indox = IndoxRetrievalAugmentation()
mistral_qa = HuggingFaceModel(api_key=HUGGINGFACE_API_KEY,model="mistralai/Mistral-7B-Instruct-v0.2")
embed = HuggingFaceEmbedding(model="multi-qa-mpnet-base-cos-v1",api_key=HUGGINGFACE_API_KEY)
```
**Setting Up Reference Directory and File Path**
```python
from indox.splitter import SemanticTextSplitter

!wget https://raw.githubusercontent.com/osllmai/inDox/master/Demo/sample.txt
file_path = "sample.txt"
with open(file_path, "r") as file:
    text = file.read()
splitter = SemanticTextSplitter(400)
content_chunks = splitter.split_text(text)
```
**Initialize SingleStoreDB**
```python
from indox.vector_stores import SingleStoreVectorDB

connection_params = {
    "host": "host",
    "port": port,
    "user": "user",
    "password": "password",
    "database": "databasename"
}

db = SingleStoreVectorDB(connection_params=connection_params,embedding_function=embed)

```
**Connecting VectorStore to Indox**
```python
db.add_texts(content_chunks)
```
**Querying the Database**
```python
query = "How cinderella reach her happy ending?"
retriever = indox.QuestionAnswer(vector_database=db, llm=mistral_qa, top_k=5, document_relevancy_filter=True)
answer = retriever.invoke(query=query)
```
