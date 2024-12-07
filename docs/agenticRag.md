
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/osllmai/inDox/blob/master/Demo/openai_agenticrag.ipynb) 

# AgenticRag

## Overview

The `AgenticRag` class is designed to handle the interaction between a Language Learning Model (LLM) and a vector database. It facilitates the process of retrieving relevant information from the vector database to answer questions posed to the LLM. This class maintains a history of question-answer interactions and context, ensuring efficient and accurate responses.

## How It Works

1. **Context Grading**: The class first grades each context and filters them based on relevancy.
2. **Web Search Fallback**: If no relevant context is found, it performs a web search and generates an answer based on the search results.
3. **Answer Generation from Context**: If relevant contexts are found, it filters these contexts and attempts to generate an answer based on the filtered contexts.
4. **Hallucination Check**: After generating the answer, it checks for hallucination (i.e., ensuring the answer is plausible and accurate).
5. **Retry on Hallucination**: If the answer is found to be hallucinated, it attempts to generate the answer again.
6. **Return Answer**: If the answer is not hallucinated, it returns the answer.

## Class Definition

```python
class AgenticRag:
    def __init__(self, llm, vector_database, top_k: int = 5)
```

### Hyperparameters
- llm: The Language Learning Model used for generating responses. This could be any model that processes natural language input and produces relevant outputs.

- vector_database: The database that stores vectors representing pieces of information. It is used to retrieve the most relevant information based on the input queries.

- top_k: (int, default=5) The number of top relevant vectors to retrieve from the vector database for each query. This determines how many pieces of information are considered for generating the response.

## Usage

```python
# Create an instance of the AgenticRag class
agent = Indox.AgenticRag(llm=llm, vector_database=db, top_k=5)

# Run the agent with a query
answer = agent.run(query)
```
<p align="center">
  <img src="AgenticRag.png" alt="inDox AgenticRag">
</p>