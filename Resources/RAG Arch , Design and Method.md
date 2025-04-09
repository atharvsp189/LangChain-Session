# RAG Arch., Design and Method

Dividing larger documents into smaller manageable pieces

### Why?

1. Memory Limitations
2. Efficient Retrieval
3. Contextual Relevance
4. Balances Efficiency and relevance

### Characteristics

- typically subset of a document
- Often Annotated with metadata
- each ideally should represent a cohesive unit of meaning ( single idea)

### Strategies

1. **Fixed-Length Chunking** 

*Description:* Split documents into equal-sized chunks (e.g., 512 tokens). 

*Pros:* Easy to implement, consistent. 

*Cons:* May split important context across chunks.

1. **Semantic Chunking** 

*Description:* Split based on semantic boundaries (e.g., paragraphs, sections, or topics). 

*Pros:* Maintains context integrity. 

*Cons:* Requires NLP tools for segmentation.

1. **Sliding Window Chunking** 

*Description:* Overlap chunks to preserve context between consecutive parts. 

*Pros:* Ensures continuity for related queries. 

*Cons:* May increase redundancy in retrieval.

1. **Specialized Chunking (e.g., Markdown, LaTeX)** 

*Description:* Parse and split documents based on structural syntax (e.g., Markdown headers, LaTeX sections). 

*Pros:* Ideal for technical documents, preserves logical structure. 

*Cons:* Requires format-specific parsers, may not generalize across formats.

1. **Recursive Chunking** 

*Description:* Iteratively divide chunks that are too large into smaller sub-chunks until they fit the desired size or semantic boundary. 

*Pros:* Balances granularity and context, ensures compliance with LLM token limits. 

*Cons:* Computationally expensive, may require multiple iterations for large documents.

### Query Operation

Query Decomposition

- Break queries into multiple sub queries
- for each sub query retrieve the data from vector db and some ranking algo above it

Query Rewriting

Read-Rewrite-Retrieve - 

- Sometime because of randomness in query provided LLM might not be able to provide right answer for the question
- Hence we use Query Rewriter tool to rewrite the query remove unnecessary things and get the right question

Lllamaindex Resource

https://docs.llamaindex.ai/en/stable/examples/low_level/fusion_retriever/

### Types of RAG

1. Direct Retrieval RAG
2. Retrieval with Reranking
    1. use Reranker to rank the retrieved docs and selected the best one
3. Iterative Retrieval RAG
    1. Query → Retrieval → LLM → Query Refinement → Retrieval (repeat) → Response
4. Hybrid/Fusion RAG
    1. Combine multiple retrieval method (semantic + lexical search)

### Query Routing

Query → Router → Select Retriever ( physics, chem etc. )→ LLM → Response

1. Logical Routing → some logic
2. Semantic Routing → intent/context

### Raptor

**Recursive Abstractive Processing for Tree Organized Retrieval (RAPTOR)** is an indexing and retrieval technique that involves:

1. Segmenting documents into shorter text chunks.
2. Embedding the chunks using an embedding model.
3. Clustering the chunk embeddings using a clustering algorithm.
4. Summarizing the text associated with each cluster using a large language model (LLM).

![image.png](image.png)