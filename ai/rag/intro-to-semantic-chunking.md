<!-- ********************* -->
# Semantic Chunking
<!-- ********************* -->

Note: Created from conversations with ChatGPT

## Table of Contents

1. Introduction
2. Why Chunking is Needed
3. What is Semantic Chunking
4. Semantic vs Fixed-Size Chunking
5. Techniques and Approaches
6. Tools and Libraries for Semantic Chunking
7. Working with Large Documents
8. Sample Code: LangChain SemanticTextSplitter
9. LLMs vs Embedding Models
10. Challenges and Considerations
11. References

<!-- ********************* -->
# Introduction
<!-- ********************* -->

Semantic chunking is a technique used to split large documents into smaller, meaningful segments based on the content's semantics rather than arbitrary lengths. This is particularly important in retrieval-based systems such as **Retrieval-Augmented Generation (RAG)** and semantic search, where the goal is to extract and retrieve the most contextually relevant parts of a document.

<!-- ********************* -->
# Why Chunking is Needed
<!-- ********************* -->

Large documents often exceed token limits for models or are too broad to index efficiently. Chunking solves this by:

- Making documents manageable in size
- Enabling fine-grained retrieval
- Improving accuracy in information retrieval and summarization

Without chunking, important context may be lost or diluted, especially when feeding documents into LLM pipelines.

<!-- ********************* -->
# What is Semantic Chunking
<!-- ********************* -->

Semantic chunking is the process of dividing text into chunks that align with natural semantic boundaries—such as paragraphs, sections, or coherent idea units. The aim is to preserve meaning and contextual relevance within each chunk.

Instead of blindly splitting every 500 tokens, semantic chunking looks at:

- Sentence boundaries
- Thematic continuity
- Logical structure (e.g., introduction, examples, conclusion)

It tries to answer: *"If I pulled this chunk on its own, would it still make sense?"*

<!-- ********************* -->
# Semantic vs Fixed-Size Chunking
<!-- ********************* -->

| Feature                  | Fixed-size Chunking            | Semantic Chunking                     |
|--------------------------|--------------------------------|----------------------------------------|
| Splitting logic          | Character or token count       | Semantic boundaries (ideas, topics)   |
| Context preservation     | Often broken                   | Maintained                            |
| Overlap needed           | Yes (often)                    | Sometimes optional                    |
| Use case suitability     | Simpler pipelines              | Higher-quality retrieval               |

Fixed-size chunking is easier to implement but often results in chunks that cut off mid-sentence or lose context. Semantic chunking creates higher-value chunks for downstream tasks.

<!-- ********************* -->
# Techniques and Approaches
<!-- ********************* -->

Popular methods for semantic chunking include:

- **Sentence boundary detection**: Using NLP libraries like spaCy or NLTK to split by sentence or paragraph.
- **TextTiling**: An older algorithm that detects topic shifts to identify natural segment boundaries.
- **Sliding window with scoring**: Moving window methods that score chunks based on semantic coherence.
- **ML-based segmentation**: Using transformers to classify or detect chunk boundaries.
- **Embedding-based methods**: Measuring vector similarity between segments to detect where ideas change.

These methods can be combined or tuned based on the document structure (e.g., markdown headings, HTML tags).

<!-- ********************* -->
# Tools and Libraries for Semantic Chunking
<!-- ********************* -->

Several open-source libraries and LLM-based tools can be used for semantic chunking:

### NLP Libraries:

- **spaCy**: Offers sentence boundary detection and syntactic parsing to help break documents naturally.
- **NLTK**: Provides sentence tokenization and older techniques like TextTiling.
- **LangChain**: Includes `RecursiveCharacterTextSplitter` and `SemanticTextSplitter` that work with embeddings.
- **Haystack**: A framework for building RAG pipelines, with customizable document chunking components.

### LLMs for Semantic Chunking:

Large language models can also assist in chunking by interpreting document structure or summarizing in segments. Useful LLM-based approaches include:

- Prompting models like GPT-4 or Claude to detect topic shifts and mark boundaries.
- Using OpenAI function calling or tools in LangChain to break documents into semantically coherent chunks.
- Fine-tuning or instructing models to output structured chunked segments.

LLMs are particularly helpful when traditional NLP signals (like punctuation or headers) are missing or unreliable.

<!-- ********************* -->
# Working with Large Documents
<!-- ********************* -->

When working with large documents, the chunking process typically follows this pattern:

1. **Preprocessing**: Load the full document and normalize text (e.g., remove extra whitespace, correct encoding).
2. **Segmenting**: Use libraries like spaCy, LangChain, or NLTK to break the document into candidate chunks.
3. **Semantic grouping**: Optionally use an embedding model (e.g., SentenceTransformers or OpenAI embeddings) to measure similarity and refine the boundaries.
4. **Chunk filtering**: Discard irrelevant or low-content chunks based on length or keyword presence.
5. **Storage**: Store chunks with metadata (e.g., source, position in document) in a vector database or search index.

If using an LLM, you can split the document into manageable blocks (e.g., paragraphs or pages), then prompt the LLM with instructions like:

> "Split this text into logically coherent sections that can be understood independently."

The LLM can then return JSON or markdown representing the structure. For very large documents, you may need to stream or window the input into the model in parts.

<!-- ********************* -->
# Sample Code: LangChain SemanticTextSplitter
<!-- ********************* -->

Below is a basic example of using `SemanticTextSplitter` from LangChain:

```python
from langchain.document_loaders import TextLoader
from langchain.text_splitter import SemanticTextSplitter
from langchain.embeddings import OpenAIEmbeddings

# Load document
loader = TextLoader("example.txt")
documents = loader.load()

# Initialize embedding model
embedding_model = OpenAIEmbeddings()

# Initialize semantic splitter
splitter = SemanticTextSplitter(
    embeddings=embedding_model,
    chunk_size=500,
    chunk_overlap=50
)

# Split document into semantic chunks
chunks = splitter.split_documents(documents)

for chunk in chunks:
    print(chunk.page_content)
```

### What it uses under the hood:
- It leverages **embedding models** (like OpenAI, Cohere, or HuggingFace models) to compute semantic similarity between parts of the document.
- It does **not use LLMs directly** in the splitting process, but rather embedding models that help detect where semantic shifts occur.

However, you can pair this with LLMs in other stages—such as post-processing, labeling, or summarizing the chunks.

<!-- ********************* -->
# LLMs vs Embedding Models
<!-- ********************* -->

Understanding the difference between **embedding models** and **LLMs (large language models)** is essential for choosing the right tool for semantic chunking and other NLP tasks.

| Feature                 | Embedding Models                       | Large Language Models (LLMs)             |
|-------------------------|----------------------------------------|------------------------------------------|
| Primary Purpose         | Vector representation of text         | Text generation, reasoning, Q&A          |
| Output                  | Fixed-size dense vectors               | Natural language text                    |
| Typical Use             | Similarity search, clustering, chunking | Chatbots, summarization, code gen        |
| Computational Cost      | Low to moderate                        | High                                     |
| Token Limit Issues      | Rarely                                 | Frequently (limited input size)          |
| Examples                | `text-embedding-ada-002`, `all-MiniLM` | GPT-4, Claude, LLaMA, Gemini             |

### Key Points:
- **Embedding models** are faster and cheaper to run, and ideal for tasks like semantic chunking, reranking, and retrieval.
- **LLMs** are more powerful for understanding and generating text, but come with higher latency and cost.
- In practice, embedding models are often used in the early stages (e.g., chunking, retrieval), while LLMs are used for higher-level synthesis and generation.

### Notable Embedding Models:
- [OpenAI: text-embedding-ada-002](https://platform.openai.com/docs/guides/embeddings): Fast, scalable model for semantic search and chunking.
- [Hugging Face: all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2): Lightweight transformer for sentence embeddings.
- [Cohere Embeddings](https://docs.cohere.com/docs/embeddings): Optimized for multilingual and long-document embeddings.

<!-- ********************* -->
# Challenges and Considerations
<!-- ********************* -->

Semantic chunking introduces new complexities:

- **Computational cost**: More logic or model passes compared to naïve chunking.
- **Heuristics tuning**: Deciding when a shift in topic or meaning is significant enough to trigger a split.
- **Overlapping windows**: May still be needed to avoid losing cross-boundary context.
- **Evaluation difficulty**: No one-size-fits-all ground truth for a "perfect" chunk.

Despite this, semantic chunking is gaining traction due to its ability to retain high-fidelity meaning across chunked text segments.

<!-- ********************* -->
# References
<!-- ********************* -->

1. [TextTiling: Segmenting Text into Multi-paragraph Subtopic Passages](https://www.aclweb.org/anthology/J97-1003.pdf): An early algorithm for topic-based segmentation.
2. [Semantic Text Splitting (LangChain)](https://python.langchain.com/docs/modules/data_connection/document_transformers/semantic_text_splitter): LangChain's implementation of semantic chunking using embeddings.
3. [SpaCy Sentence Segmentation](https://spacy.io/usage/linguistic-features#sbd): SpaCy’s guide to breaking text into sentences.
4. [NLTK Tokenize](https://www.nltk.org/api/nltk.tokenize.html): Tools for sentence and word tokenization.
5. [Sentence Transformers](https://www.sbert.net/): Often used in embedding-based methods for semantic chunking.
6. [OpenAI Embeddings](https://platform.openai.com/docs/guides/embeddings): Official docs for `text-embedding-ada-002`.
7. [MiniLM at Hugging Face](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2): Model card for a fast embedding model.
8. [Cohere Embeddings](https://docs.cohere.com/docs/embeddings): API guide and examples for Cohere's embedding services.

