<!-- ********************* -->
# Query Modification Techniques in Retrieval-Augmented Generation (RAG)
<!-- ********************* -->

Retrieval-Augmented Generation (RAG) systems are enhanced when query formulation is adapted to suit different information needs. This article explores several popular techniques for query modification: Multi-query, RAG Fusion, Query Decomposition, Step Back Prompting, and HYDE. Each technique is described in terms of its mechanics, usage example, and available tooling.

<!-- ********************* -->
# 1. Multi-query
<!-- ********************* -->

**Introduction:**
Multi-query RAG expands a single user query into multiple semantically diverse versions, improving retrieval recall and robustness.

**How it works:**
A language model is used to generate several paraphrases or reformulations of the original query. These reformulated queries are then sent to the retriever. The results are aggregated and ranked for relevance.

**Flow:**
User Query → Query Generator (LM) → Multiple Reformulations → Retriever → Merged Results → RAG Generator

**Example:**
- Original query: "What is the capital of the Mughal Empire?"
- Reformulated queries:
  - "Which city served as the political center of the Mughal Empire?"
  - "Main capital during the Mughal rule in India?"
  - "Where did Mughal emperors rule from?"

**Libraries:**
Available in frameworks like **LangChain** and **LlamaIndex**.

<!-- ********************* -->
# 2. RAG Fusion
<!-- ********************* -->

**Introduction:**
RAG Fusion uses multiple queries and combines their retrieved results intelligently to generate a final answer.

**How it works:**
Similar to Multi-query, but after retrieval, the documents are ranked collectively using re-ranking algorithms (e.g., Reciprocal Rank Fusion) before passing them to the generator.

**Flow:**
User Query → Query Generator → Multiple Queries → Retriever → Document Pool → Reranker → Generator

**Example:**
- Query: "How did the Cold War affect Africa?"
- Multiple rewrites explore economic, political, and military dimensions
- Retrieved documents are combined and sorted before answering

**Libraries:**
Integrated in **LangChain**, **Haystack**, and **LlamaIndex**.

<!-- ********************* -->
# 3. Query Decomposition
<!-- ********************* -->

**Introduction:**
Query decomposition breaks down complex or multi-faceted queries into simpler sub-questions to improve retrieval precision.

**How it works:**
The original complex question is analyzed and decomposed by a model or a rule-based system. Each sub-query is independently retrieved, and the results are then synthesized into a coherent answer.

**Flow:**
User Query → Decomposer (LM) → Sub-queries → Retrieval per Sub-query → Merge + Synthesize Results

**Example:**
- Query: "What were the causes and effects of the French Revolution?"
- Decomposed into:
  - "What caused the French Revolution?"
  - "What were the consequences of the French Revolution?"

**Libraries:**
Supported in **LangChain**, **DSPy**, and **AutoGPT-like agents**.

<!-- ********************* -->
# 4. Step Back Prompting
<!-- ********************* -->

**Introduction:**
Step Back Prompting introduces a higher-level abstraction or generalization before retrieval to access more contextually rich documents.

**How it works:**
Instead of directly querying the specific question, the system first asks a broader, meta-level question, retrieves documents for that, then uses them to answer the original question.

**Flow:**
User Query → Generate Broader Prompt → Retrieve General Documents → Use them to Re-answer Specific Query

**Example:**
- Query: "Why did Enron collapse?"
- Step back prompt: "What are common causes of corporate failures?"
- Retrieved documents give general insights, then applied to Enron

**Libraries:**
Still experimental, but doable via **LangChain** and custom prompt pipelines.

<!-- ********************* -->
# 5. HYDE (Hypothetical Document Embeddings)
<!-- ********************* -->

**Introduction:**
HYDE uses generated hypothetical answers to a query and embeds them as retrieval queries.

**How it works:**
Instead of embedding the query directly, a language model generates a potential answer. This generated answer is then embedded and used for nearest neighbor search in the vector database.

**Flow:**
User Query → Generate Hypothetical Answer → Embed Answer → Vector Search → Retrieve Documents

**Example:**
- Query: "What are the symptoms of vitamin D deficiency?"
- Hypothetical answer: "Symptoms include fatigue, bone pain, and muscle weakness."
- This answer is embedded and used to retrieve relevant documents.

**Libraries:**
Supported in **LlamaIndex**, **LangChain**, and **OpenAI cookbook** examples.

<!-- ********************* -->
# Reranking vs RAG Fusion
<!-- ********************* -->

**Overview:**
Reranking and RAG Fusion are often used together but serve different purposes in the RAG pipeline.

### Reranking
- **What it is:** A post-processing step that reorders retrieved documents based on relevance.
- **Use case:** Works on results from a single query or combined document set.
- **How:** Typically uses cross-encoders or scoring functions to refine document order.

### RAG Fusion
- **What it is:** A retrieval strategy that gathers documents from multiple query rewrites.
- **Use case:** Increases coverage and diversity of documents.
- **How:** Uses fusion methods (e.g., Reciprocal Rank Fusion) to combine and sort documents.

### Combined Flow:
1. User query → Multiple rewrites (RAG Fusion)
2. Retrieve documents for each query
3. Merge results
4. Apply reranking
5. Generate answer

**TL;DR Comparison:**

| Feature         | Reranking                         | RAG Fusion                                      |
|----------------|------------------------------------|------------------------------------------------|
| **Focus**       | Reorder docs based on relevance   | Improve retrieval by using multiple queries    |
| **Input**       | One list of documents              | Multiple lists (one per query variant)         |
| **Output**      | A reordered single list            | A fused list (may also be reranked)           |
| **Optional in** | RAG Fusion                         | Not applicable                                 |

They are **complementary**: RAG Fusion improves retrieval breadth, while reranking enhances final precision.

<!-- ********************* -->
# References
<!-- ********************* -->

1. **LangChain Query Modification Tools**  
   https://docs.langchain.com/docs/components/chains/index_querying  
   Describes multi-query, reranking, and decomposition features in LangChain.

2. **LlamaIndex Query Transformations**  
   https://docs.llamaindex.ai/en/stable/examples/query_transformation/MultiQueryRetrieval/  
   Example notebooks for multi-query, HYDE, and decomposition techniques.

3. **RAG Fusion - Meta AI Blog**  
   https://ai.meta.com/blog/retrieval-augmented-generation/  
   Original discussion on RAG and fusion approaches.

4. **Step Back Prompting - Princeton NLP**  
   https://arxiv.org/abs/2302.11382  
   Research paper introducing the idea of abstraction-based prompting for better retrieval.

5. **HYDE (Hypothetical Document Embeddings)**  
   https://arxiv.org/abs/2212.10496  
   Original paper explaining the generation of hypothetical answers for dense retrieval.

6. **Haystack RAG Pipelines**  
   https://docs.haystack.deepset.ai/docs/rag  
   Implementation support for RAG Fusion and multi-query pipelines.

7. **DSPy by Stanford**  
   https://github.com/stanfordnlp/dspy  
   Library for structured programmatic prompting, supports decomposition and multi-query.

