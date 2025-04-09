# Retrieval Optimization Technique

> Designed to search and extract relavant information from dataset
> 

Done at three stages:

1. Pre-retrieval Optimization
2. During retrieval Optimization
3. Post-retrieval Optimization

### Pre-retrieval Optimization

Focuses on refining input to maximize the efficiency and accuracy of retrieval process

1. Sentence Window
    - Involves segmenting larger documents  or text into manageable window or chunks
    - Enhance focus on smaller, contextual rich segment
    - improve recall rate by preventing over-reliance on large, broad document
    - more precise matching
2. Query Expansion
    - Modify initial query by adding **synonym and related term**
    - helps in broaden the scope with relavant information by adding different terminology
    - address the issue of vocabulary mismatch
    - Technique: synonym based and knowledge graph based expansion
3. Query Rewriting
    - refines query to make it search friendly
    - reorder words clarity, adjust use intent

### During Retrieval Optimization

Crucial to employ techniques that balance precision and recall

1. Hybrid Search
    - integraete lexical analysis and sematic meaning
    - deeper contextual analysis
    - Technique: Apply BM25 or TFIDF for surface level matching and thrn apply semantic vector embedding for deeper meaning refinement

```python
# Hybrid Search

# For lexical matching
# Initialize the BM25 retriever
bm25_retriever.k = 2  # Retrieve top 2 results
print("type of bm25", type(bm25_retriever))

# For semantic matching
# Initialize retriever
docsearch = Chroma(persist_directory=persist_directory, embedding_function=embedding)
retriever_chromadb = docsearch.as_retriever(search_kwargs={"k": 5})

# Initialize the ensemble retriever
ensemble_retriever = EnsembleRetriever(
    retrievers=[bm25_retriever, retriever_chromadb], weights=[0.3, 0.7]
)
```

### Post Retrieval Optimization

Applied once the retrieval is done

1. Reranking
    - Apply additional algorithm to reorder the initial set of retrieved documents
    - Approach: use machine learning model or heuristic rule
    - prioritize document based on metrics like click through rates, content freshness or user engagement
    - Higher quality and relavant document

```python
# Re-Ranking
# Import and initialize the reranker for document compression
compressor = CohereRerank()
# CohereRerank is a model or tool designed to rerank documents based on their relevance.

# Create a ContextualCompressionRetriever for improved document retrieval
compression_retriever = ContextualCompressionRetriever(
    base_compressor=compressor,               # Use the reranker as the base compression mechanism
    base_retriever=docsearch.as_retriever()   # Use the existing document search tool as the base retriever
)
# The ContextualCompressionRetriever combines the base retriever's results with reranking
# to provide more contextually relevant and concise results.

# Initialize an empty DataFrame with specified columns
source_df = pd.DataFrame(columns=['Text', 'source', 'relevance_score'])

# Retrieve compressed documents relevant to the query using the contextual compression retriever
compressed_docs = compression_retriever.get_relevant_documents("What is the architecture of Transformers?")
# contains the reult in ranked form
```