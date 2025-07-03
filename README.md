# langchain-localai-openai-v1-patch
A patch for langchain-community's LocalAIEmbeddings to support openai>=1.0.0 and resolve compatibility issues.

## Overview
This repository provides a patched version of the LocalAIEmbeddings class, originally found in langchain-community. The primary goal of this patch is to ensure full compatibility with the openai Python library version 1.0.0 and newer, addressing the AttributeError: module 'openai' has no attribute 'error' that can occur when using older langchain-community versions with modern openai installations.

LocalAIEmbeddings uses the openai client internally due to LocalAI's OpenAI-compatible API. This patch updates the internal client instantiation and error handling to align with the changes introduced in openai v1.x.

## Features

- OpenAI v1.x Compatibility: Ensures LocalAIEmbeddings works seamlessly with openai library versions 1.0.0 and above.
- Fixes AttributeError: Resolves the common module 'openai' has no attribute 'error' issue.
- Drop-in Replacement: Designed to be easily integrated into existing LangChain projects.

## Installation
You can install this package directly from GitHub using pip:

```bash
pip install git+https://github.com/reidlai-oss/langchain-localai-patch.git
```

Prerequisites
Ensure you have the following packages installed in your environment:

- `python >=3.9`
- `langchain-core>=0.1.0`
- `langchain-community>=0.0.0`
- `openai>=1.0.0`
- `pydantic>=2.0.0`
- `tenacity>=8.0.0`

It's highly recommended to use a virtual environment (like venv or poetry) for your projects.

## Usage
After installing the langchain-localai-patch package, you can import and use the LocalAIEmbeddings class just like you would with the official langchain-community version.

```python
# In your Python code (e.g., your main application script)

# Import the patched LocalAIEmbeddings
from langchain_localai_patch.localai import LocalAIEmbeddings
# Or, if you added it to __init__.py:
# from langchain_localai_patch import LocalAIEmbeddings

import os

# Configure your LocalAI endpoint
localai_api_base = os.getenv("LOCALAI_API_BASE", "http://localhost:8080/v1")
openai_api_key = os.getenv("OPENAI_API_KEY", "dummy") # LocalAI often doesn't need a real key

# Initialize the embeddings model
embeddings = LocalAIEmbeddings(
    model="nomic-embed-text-v1.5", # Or any other model supported by your LocalAI instance
    openai_api_base=localai_api_base,
    openai_api_key=openai_api_key
)

# Example: Embed a query
query = "What is the capital of France?"
query_embedding = embeddings.embed_query(query)
print(f"Query embedding length: {len(query_embedding)}")
# print(f"Query embedding: {query_embedding[:5]}...") # Print first 5 elements for brevity

# Example: Embed multiple documents
documents = [
    "Paris is the capital and most populous city of France.",
    "The Eiffel Tower is a famous landmark in Paris.",
    "Berlin is the capital of Germany."
]
document_embeddings = embeddings.embed_documents(documents)
print(f"Number of document embeddings: {len(document_embeddings)}")
print(f"First document embedding length: {len(document_embeddings[0])}")
```
## Contributing
Contributions are welcome! If you find issues or have improvements, please feel free to open a pull request or an issue on this repository.

##  License
This project is licensed under the MIT License - see the LICENSE file for details.

Disclaimer: This is a community-maintained patch. While it aims to resolve specific compatibility issues, always test thoroughly in your environment.