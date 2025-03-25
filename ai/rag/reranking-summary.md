<!-- ********************* -->
# Reranking
<!-- ********************* -->

## Table of Contents

1. Introduction
2. Why Reranking Matters
3. How Reranking Works
4. End-to-End Retrieval + Reranking Flow
5. Understanding Pairwise Scoring
6. Comparison: Bi-encoder vs Cross-encoder
7. Common Tools and Models for Reranking
8. Use Cases and Applications
9. References

<!-- ********************* -->
# Introduction
<!-- ********************* -->

**Reranking** is a technique used in GenAI-powered retrieval systems to improve the relevance of retrieved documents. After an initial retrieval—often using vector similarity or keyword search—reranking is used to reorder the top N results to surface the most relevant content for the original query.

This is particularly useful in systems that use **Retrieval-Augmented Generation (RAG)**, where the quality of the retrieved context significantly affects the final generated answer.

<!-- ********************* -->
# Why Reranking Matters
<!-- ********************* -->

The initial retrieval stage, while fast, may not always provide the most relevant results. This is because:

- Vector search (e.g., using bi-encoders) focuses on overall semantic similarity but may miss nuanced contextual alignment.
- Keyword search may return documents with matching terms but irrelevant meanings.

Reranking provides a second, more precise filter, using more computationally intensive models to deeply understand the relationship between the query and the documents.

<!-- ********************* -->
# How Reranking Works
<!-- ********************* -->

The reranking process typically follows these steps:

1. **Initial Retrieval**: A fast vector search retrieves the top N candidate documents using bi-encoder embeddings.
2. **Pairwise Scoring**: A cross-encoder model takes each query-document pair and computes a relevance score.
3. **Sorting**: Documents are reordered based on these scores, with the highest-scoring items presented first.

This approach captures more subtle relationships between the query and each document, leading to improved results.

### Visual Analogy

Think of the initial vector search as casting a wide net to catch fish in a lake. Reranking is like sorting those fish by quality, size, and freshness before selecting the best ones to serve.

<!-- ********************* -->
# End-to-End Retrieval + Reranking Flow
<!-- ********************* -->

Here is an overview of how the full retrieval + reranking process typically works:

1. **User submits a query**: This could be a natural language question or phrase.
2. **Query embedding**: A bi-encoder model (like `all-MiniLM-L6-v2`) converts the query into a dense vector.
3. **Initial retrieval**: This vector is used to perform an approximate nearest neighbor (ANN) search in a vector database (e.g., FAISS, Pinecone, Weaviate), retrieving the top-N semantically similar documents.
4. **Reranking preparation**: Each of the top-N documents is paired with the original query.
5. **Cross-encoder scoring**: A cross-encoder model scores each query-document pair for relevance.
6. **Reordering**: Documents are sorted based on the relevance scores from the cross-encoder.
7. **Result usage**: The top reranked documents are either shown to the user or passed to a large language model (LLM) for generation.

This two-step pipeline balances **speed and scalability** (via the bi-encoder) with **precision and contextual understanding** (via the cross-encoder).

<!-- ********************* -->
# Understanding Pairwise Scoring
<!-- ********************* -->

Pairwise scoring is the core step in the reranking phase, where a **cross-encoder model** evaluates the relevance of each document with respect to the user's query.

Here’s how it works conceptually:

- Each document is **not** treated independently of the query. Instead, the query and a document are presented together as a pair to the model.
- The cross-encoder **reads both inputs jointly**, allowing it to understand the deep contextual relationships between the query and the document.
- It then assigns a **single relevance score** that reflects how well the document answers or relates to the query.

This is different from earlier stages of retrieval, where documents and queries are processed separately, which can lead to missing finer nuances. The strength of pairwise scoring lies in:

- **Rich attention mechanisms**: The model can focus on which words or concepts in the document align with the intent of the query.
- **Full-context awareness**: The cross-encoder has access to the entire query and document text at once, allowing for global reasoning.

While more computationally expensive, this level of understanding results in significantly more accurate and trustworthy rankings of documents.

<!-- ********************* -->
# Comparison: Bi-encoder vs Cross-encoder
<!-- ********************* -->

Below is a quick comparison of the two main encoder types used in retrieval pipelines:

| Aspect         | Bi-encoder (Vector Search) | Cross-encoder (Reranking)       |
|----------------|----------------------------|----------------------------------|
| Speed          | Fast                       | Slower                           |
| Accuracy       | Moderate                   | High                             |
| Input Method   | Query and doc separately   | Query and doc **together**       |
| Use Case       | First-pass search          | Reranking top-N docs             |

Bi-encoders are great for scalability, especially when working with millions of documents. Cross-encoders are more accurate but computationally expensive, so they are typically only used on a smaller set of results.

<!-- ********************* -->
# Common Tools and Models for Reranking
<!-- ********************* -->

Several models and tools are available for implementing reranking in production systems:

- **Cohere Rerank API**: Offers a managed reranking service using their in-house language models.
- **OpenAI Rerank endpoint**: Allows reranking using models like `text-embedding-ada-002` paired with fine-tuned models.
- **ColBERT (University of Waterloo)**: An efficient and scalable system combining dense and late interaction features.
- **SentenceTransformers**: Supports cross-encoder reranking models such as `cross-encoder/ms-marco-MiniLM-L-6-v2`.
- **BERT + CLS Token**: Fine-tuned models that compute similarity by using the [CLS] token's output.

These tools can be integrated with vector databases like Pinecone, Weaviate, or FAISS.

<!-- ********************* -->
# Use Cases and Applications
<!-- ********************* -->

Reranking is particularly useful in:

- **Search engines**: Improving the quality of search results.
- **Chatbots**: Enhancing context retrieval in RAG pipelines.
- **Legal/medical domain**: Precision in retrieving highly accurate information.
- **E-commerce**: Improving product search relevance.
- **Academic research**: Filtering high-quality papers for query-specific needs.

By using reranking, these systems can provide more relevant, trustworthy, and useful information to end users.

<!-- ********************* -->
# References
<!-- ********************* -->

1. [Cohere Rerank API](https://docs.cohere.com/docs/rerank): Documentation on how to use Cohere's managed reranking service.
2. [ColBERT: Efficient and Effective Passage Search via Contextualized Late Interaction over BERT](https://arxiv.org/abs/2004.12832): Research paper describing the ColBERT method.
3. [OpenAI API Documentation](https://platform.openai.com/docs): Includes info on embeddings and reranking use cases.
4. [SentenceTransformers Reranking Models](https://www.sbert.net/examples/applications/re-ranking/README.html): Examples and usage guides for cross-encoder reranking.
5. [Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401): Original RAG paper discussing the importance of accurate retrieval.

