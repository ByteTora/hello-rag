<div align='center'>
    <img src="./images/hello-rag.png" alt="alt text" width="100%">
    <h1>Hello-RAG</h1>
</div>

<p align='center'>
    <b>English</b> | <a href="./README.zh.md">简体中文</a>
</p>

<div align='center'>

[![Powered by OrcaRouter](https://img.shields.io/badge/Powered_by-OrcaRouter-2563eb)](https://www.orcarouter.ai/ref/ref_007610f54c1e5ca8be45)

</div>

This repository takes a clear, hands-on approach to Retrieval-Augmented Generation (RAG), breaking advanced techniques down into simple, easy-to-understand implementations. Instead of relying on frameworks like `LangChain` or `FAISS`, it uses familiar Python libraries — `openai`, `numpy`, `fitz (PyMuPDF)`, and a few others — to build everything from scratch.

The goal is simple: provide code that is easy to read, modify, and learn from. By focusing on the fundamentals, this project demystifies RAG and makes it easier to understand how it really works.

| Chapter | Key Content | Status |
| --- | --- | --- |
| [01 Simple RAG](./content/01_simple_rag.md) | A basic RAG implementation | ✅ |
| [02 Semantic Chunking](./content/02_semantic_chunking.md) | Split text by semantic similarity into more meaningful chunks | ✅ |
| [03 Context Enriched Retrieval](./content/03_context_enriched_rag.md) | Retrieve neighboring chunks for more context | ✅ |
| [04 Contextual Chunk Headers](./content/04_contextual_chunk_headers_rag.md) | Add descriptive headers to chunks before embedding | ✅ |
| [05 Document Augmentation](./content/05_doc_augmentation_rag.md) | Generate questions from text chunks to improve retrieval | ✅ |
| [06 Query Transformation](./content/06_query_transform.md) | Rewrite, expand, or decompose queries to improve retrieval, including step-back prompting and sub-query decomposition | ✅ |
| [07 Reranker](./content/07_reranker.md) | Use an LLM to re-rank initially retrieved results for better relevance | ✅ |
| [08 Relevant Segment Extraction](./content/08_rse.md) | Identify and reconstruct continuous text segments while preserving context | ✅ |
| [09 Contextual Compression](./content/09_contextual_compression.md) | Filter and compress retrieved chunks to maximize relevant information | ✅ |
| [10 Feedback Loop](./content/10_feedback_loop_rag.md) | Learn and improve a RAG system over time from user feedback | ✅ |
| [11 Adaptive RAG](./content/11_adaptive_rag.md) | Dynamically pick the best retrieval strategy based on query type | ✅ |
| [12 Self-RAG](./content/12_self_rag.md) | Dynamically decide when and how to retrieve, and evaluate relevance, support, and utility | ✅ |
| [13 Fusion RAG](./content/13_fusion_rag.md) | Combine vector search and keyword-based (BM25) retrieval for better results | ✅ |
| [14 Graph RAG](./content/14_graph_rag.md) | Organize knowledge as a graph so related concepts can be traversed | ✅ |
| [15 Hierarchical Indices](./content/15_hierarchy_rag.md) | Build hierarchical indices (summaries + detailed chunks) for efficient retrieval | ✅ |
| [16 HyDE RAG](./content/16_HyDE_rag.md) | Use hypothetical document embeddings to improve semantic matching | ✅ |
| [17 CRAG](./content/17_crag.md) | Dynamically evaluate retrieval quality and fall back to web search | ✅ |
| **Chunking Techniques** | | |
| [Chunk Size Selector](./content/001_chunk_size_selector.md) | Explore how different chunk sizes affect retrieval performance | ✅ |
| [Proposition Chunking](./content/002_proposition_chunking.md) | Decompose documents into atomic factual statements for precise retrieval | ✅ |

## Simple RAG

Retrieval-Augmented Generation (RAG) is a hybrid approach that combines information retrieval with generative models. By incorporating external knowledge, it enhances the performance of language models, improving accuracy and factual correctness.

-----
Implementation steps:

- **Data Ingestion**: Load and preprocess text data.
- **Chunking**: Split the data into smaller chunks to improve retrieval performance.
- **Embedding Creation**: Use an embedding model to convert text chunks into numerical representations.
- **Semantic Search**: Retrieve relevant chunks based on the user's query.
- **Response Generation**: Use a language model to generate a response from the retrieved text.

![](./images/1_VL2fP0HuSoqc66GC1vbA7w.webp)

----

- For the full implementation and analysis, see: [01_simple_rag.ipynb](https://github.com/ByteTora/hello-rag/blob/main/src/full/01_simple_rag.ipynb)

## Semantic Chunking

Text chunking is a key step in Retrieval-Augmented Generation (RAG): large bodies of text are split into meaningful segments to improve retrieval accuracy. Unlike fixed-length chunking, semantic chunking splits text based on content similarity between sentences.

------
Implementation steps:

- Extract text from PDF files: split it into sentences
- Build semantic chunks from the extracted text:
    - Compute similarity between adjacent sentences
    - Determine chunk breakpoints from similarity drops, using one of three methods: percentile, standard deviation, or interquartile range
    - Split the text at the breakpoints to get semantic chunks
- Create embeddings
- Retrieve documents based on the query
- Use the retrieved text for model-generated answers

![](./images/1_7b5LkCrQ7XhUN9Ktu3Q1sQ.webp)

----

- For the full implementation and analysis, see: [02_semantic_chunking.ipynb](https://github.com/ByteTora/hello-rag/blob/main/src/full/02_semantic_chunking.ipynb)
- For the core functions, see: [02_semantic_chunking_core.py](https://github.com/ByteTora/hello-rag/blob/main/src/core/02_semantic_chunking_core.py)

## Context Enriched Retrieval

Retrieval-Augmented Generation (RAG) enhances AI responses by retrieving relevant knowledge from external sources. Traditional retrieval returns isolated text snippets, which can lead to incomplete answers.
To solve this, we introduce context-enriched retrieval, ensuring the retrieved information includes neighboring chunks for better coherence.

------
Implementation steps:

- Data ingestion: extract text from PDFs
- Overlapping context chunking: split text into overlapping chunks to preserve context
- Embedding creation: convert text chunks into numerical representations
- Context-aware retrieval: retrieve relevant chunks along with their neighbors for better completeness
- Answer generation: use a language model to generate answers from the retrieved context
- Evaluation: evaluate model performance using an evaluation dataset

![](./images/1_RtkAa3uP2XlaRs8uUAduwA.webp)

----

- For the full implementation and analysis, see: [04_context_enriched_rag.ipynb](https://github.com/ByteTora/hello-rag/blob/main/src/full/04_context_enriched_rag.ipynb)
- For the core functions, see: [04_context_enriched_rag_core.py](https://github.com/ByteTora/hello-rag/blob/main/src/core/04_context_enriched_rag_core.py)

## Contextual Chunk Headers

Retrieval-Augmented Generation (RAG) improves the factual accuracy of language models by retrieving relevant external knowledge before generating a response. However, standard chunking often loses important context, hurting retrieval quality.
Contextual Chunk Headers (CCH) enhance RAG by adding high-level context (such as a document title or section heading) to each chunk before embedding. This improves retrieval quality and prevents irrelevant responses.

------
Implementation steps:

- Data ingestion: extract text from PDFs
- **Chunking with contextual headers: extract section headings (or have the model generate titles for chunks) and prepend them to the chunks**
- Embedding creation: convert text chunks into numerical representations
- Semantic search: retrieve relevant chunks based on the user's query
- Answer generation: use a language model to generate answers from the retrieved context
- Evaluation: evaluate model performance using an evaluation dataset

![](./images/Contextual_Chunk_Headers.webp)

----

- For the full implementation and analysis, see: [05_contextual_chunk_headers_rag.ipynb](https://github.com/ByteTora/hello-rag/blob/main/src/full/05_contextual_chunk_headers_rag.ipynb)
- For the core functions, see: [05_contextual_chunk_headers_rag_core.py](https://github.com/ByteTora/hello-rag/blob/main/src/core/05_contextual_chunk_headers_rag_core.py)

## Document Augmentation

An enhanced RAG approach that improves results through document augmentation and question generation. By generating relevant questions for each text chunk, we improve the retrieval process, enabling the language model to produce better responses.

------
Implementation steps:

- Data ingestion: extract text from PDF files
- Chunking: split text into manageable chunks
- **Question generation: generate relevant questions for each chunk**
- Embedding creation: create embeddings for both chunks and generated questions
- Vector store creation: build a simple vector store using NumPy
- Semantic search: retrieve chunks and questions relevant to the user's query
- Response generation: generate answers from the retrieved content
- Evaluation: evaluate the quality of generated responses

![](./images/Document_Augmentation.webp)

----

- For the full implementation and analysis, see: [06_doc_augmentation_rag.ipynb](https://github.com/ByteTora/hello-rag/blob/main/src/full/06_doc_augmentation_rag.ipynb)
- For the core functions, see: [06_doc_augmentation_rag_core.py](https://github.com/ByteTora/hello-rag/blob/main/src/core/06_doc_augmentation_rag_core.py)

## Query Transformation

By modifying the user's query, we can significantly improve the relevance and completeness of retrieved information.

Three query transformation techniques are implemented to enhance retrieval performance in RAG systems without relying on dedicated libraries like LangChain.

------
Core concepts

1. Query rewriting: make the query more specific and detailed to improve search precision.

2. Step-back prompting: generate a broader query to retrieve useful contextual information.

3. Sub-query decomposition: split complex queries into simpler components for comprehensive retrieval.

------
Implementation steps:

- Process documents to create a vector store: extract text from PDFs, split it into chunks, and build the vector store
- Apply query transformation techniques:
    - Query rewriting: make the query more specific and detailed to improve retrieval accuracy
    - Step-back prompting: generate a broader query to retrieve background context
    - Sub-query decomposition: split complex queries into simpler components for comprehensive retrieval
- Embed the transformed queries and retrieve documents
- Generate answers from the retrieved content

![](./images/Query_Transformation.webp)

----

- For the full implementation and analysis, see: [07_query_transform.ipynb](https://github.com/ByteTora/hello-rag/blob/main/src/full/07_query_transform.ipynb)
- For the core functions, see: [07_query_transform_core.py](https://github.com/ByteTora/hello-rag/blob/main/src/core/07_query_transform_core.py)

## Reranker

Implements reranking techniques to improve retrieval quality in RAG systems. Reranking acts as a second-stage filter after initial retrieval, ensuring the most relevant content is used for response generation.

------
Core concepts

1. **Initial retrieval**: a first pass using basic similarity search (faster but less accurate)
2. **Document scoring**: evaluate each retrieved document's relevance to the query
3. **Reranking**: sort documents by relevance score
4. **Selection**: use only the most relevant documents for response generation

------
Implementation steps:

- Process documents to create a vector store: extract text from PDFs, split it into chunks, and build the vector store
- Create query embeddings and retrieve documents: an initial query that recalls a larger set of documents
- Apply reranking:
    - LLM-based reranking: rerank search results using LLM relevance scores
    - Keyword-based reranking: a simple method based on keyword match count and position
- Generate answers using the reranked context

![](./images/Reranker.webp)

-----

- For the full implementation and analysis, see: [08_reranker.ipynb](https://github.com/ByteTora/hello-rag/blob/main/src/full/08_reranker.ipynb)
- For the core functions, see: [08_reranker_core.py](https://github.com/ByteTora/hello-rag/blob/main/src/core/08_reranker_core.py)

## RSE

Relevant Segment Extraction (RSE) improves the context quality of RAG systems. Instead of retrieving a set of isolated chunks, RSE identifies and reconstructs continuous text segments that provide better context, giving the language model stronger support.

-----
Core concepts:

Relevant segments tend to cluster within a document. By identifying these clusters and preserving their continuity, RSE provides large language models with more coherent context.

-----
Implementation steps:

- Process documents to create a vector store: extract text from PDFs, split it into chunks (0 overlap), and build the vector store
- Compute relevance scores and chunk values for the query:
    - First, get all chunks with similarity scores
    - Derive relevance scores, defaulting to 0 when not in the results, and apply a penalty to turn irrelevant chunks into negative values
- Find the best text chunk based on chunk values: use a variant of the maximum subarray sum algorithm
- Reconstruct the text segment from the best chunk: rebuild the passage based on chunk indices
- Format the passage as context and generate answers

![](./images/RSE.webp)

----

- For the full implementation and analysis, see: [09_rse.ipynb](https://github.com/ByteTora/hello-rag/blob/main/src/full/09_rse.ipynb)
- For the core functions, see: [09_rse_core.py](https://github.com/ByteTora/hello-rag/blob/main/src/core/09_rse_core.py)

## Contextual Compression

Contextual Compression improves the efficiency of RAG systems by filtering and compressing retrieved text chunks, keeping only the most relevant content. This reduces noise and improves response quality.

When retrieving documents for RAG, you often get chunks containing both relevant and irrelevant information. Contextual compression helps by:

- Removing irrelevant sentences and paragraphs
- Focusing only on query-relevant information
- Maximizing useful signal within the context window

This article provides three methods:

1. Selective filtering: analyze document chunks and extract only the sentences or paragraphs directly relevant to the user's query, removing everything else.
2. Summarization: create concise summaries of chunks, focusing only on query-relevant information.
3. Extraction: extract complete sentences from chunks that precisely match the user's query.

-----
Implementation steps:

- Process documents to create a vector store: extract text from PDFs, split it into chunks, and build the vector store
- Create query embeddings and retrieve the top-k most similar chunks
- Apply compression to the retrieved chunks:
    - Selective filtering: analyze chunks and extract only sentences/paragraphs directly relevant to the query
    - Summarization: create concise summaries focused on query-relevant information
    - Extraction: extract complete sentences that precisely match the query
- Drop any empty compressed chunks
- Build the context from compressed chunks and generate answers

![](./images/Contextual_Compression.webp)

----

- For the full implementation and analysis, see: [10_contextual_compression.ipynb](https://github.com/ByteTora/hello-rag/blob/main/src/full/10_contextual_compression.ipynb)
- For the core functions, see: [10_contextual_compression_core.py](https://github.com/ByteTora/hello-rag/blob/main/src/core/10_contextual_compression_core.py)

## Feedback Loop

Implements a RAG system with a feedback loop mechanism that iteratively improves performance through continuous learning. The system collects and integrates user feedback so every interaction improves the relevance and quality of responses.

-----
Traditional RAG systems use a static retrieval mode that relies solely on embedding similarity. This system instead builds a dynamic optimization framework through the feedback loop:

- Remembering effective/ineffective interaction patterns
- Dynamically adjusting document relevance weights
- Incorporating high-quality Q&A pairs into the knowledge base
- Continuously enhancing intelligence through user interaction

-----
Implementation steps:

- Load historical feedback datasets
- Preprocess and chunk documents
- Optionally fine-tune the vector index based on historical feedback
- Run retrieval and generation with feedback-corrected relevance scoring
- Collect new user feedback for future optimization
- Persist feedback data to support continuous learning

![](./images/Feedback_Loop.webp)

----

- For the full implementation and analysis, see: [11_feedback_loop_rag.ipynb](https://github.com/ByteTora/hello-rag/blob/main/src/full/11_feedback_loop_rag.ipynb)
- For the core functions, see: [11_feedback_loop_rag_core.py](https://github.com/ByteTora/hello-rag/blob/main/src/core/11_feedback_loop_rag_core.py)

## Adaptive RAG

An Adaptive Retrieval system that dynamically picks the optimal retrieval strategy based on query type, significantly improving response accuracy and relevance across diverse question scenarios.

-----
Different query types require different retrieval strategies. This system implements a four-stage optimization pipeline:

1. Query-type classification (factual / analytical / opinion / contextual)
2. Adaptive retrieval-strategy selection
3. Executing the specialized retrieval technique
4. Generating a tailored response

-----
Implementation steps:

- Process documents to extract text, chunk it, and create embeddings
- Classify the query to determine its type: Factual, Analytical, Opinion, or Contextual
- Retrieve documents using an adaptive strategy based on the query type
- Generate answers based on the query, retrieved documents, and query type

![](./images/Adaptive_RAG.webp)

----

- For the full implementation and analysis, see: [12_adaptive_rag.ipynb](https://github.com/ByteTora/hello-rag/blob/main/src/full/12_adaptive_rag.ipynb)
- For the core functions, see: [12_adaptive_rag_core.py](https://github.com/ByteTora/hello-rag/blob/main/src/core/12_adaptive_rag_core.py)

## Self RAG

Self-RAG is an advanced RAG system that dynamically decides when and how to use retrieved information. Unlike traditional RAG approaches, Self-RAG introduces reflection points throughout the retrieval and generation process, producing higher-quality and more reliable answers.

-----
Self-RAG core components

1. **Retrieval decision**: determine whether a specific query requires retrieval
2. **Document retrieval**: fetch potentially relevant documents when needed
3. **Relevance evaluation**: assess how relevant each retrieved document is
4. **Response generation**: build answers from relevant context
5. **Support evaluation**: verify that the answer is correctly grounded in the context
6. **Utility evaluation**: score the overall usefulness of the generated answer

-----
Implementation steps:

- Determine whether retrieval is needed: the LLM decides if retrieval is required — factual queries need retrieval, opinion queries do not
- Retrieve documents: create query embeddings and retrieve documents
- Evaluate document relevance: use the model to score each chunk; if relevant, add it to the context list
- Process each relevant context with support and utility evaluation
- Pick the best answer: run utility evaluation on candidate answers and select the best one

![](./images/Self_RAG.webp)

----

- For the full implementation and analysis, see: [13_self_rag.ipynb](https://github.com/ByteTora/hello-rag/blob/main/src/full/13_self_rag.ipynb)
- For the core functions, see: [13_self_rag_core.py](https://github.com/ByteTora/hello-rag/blob/main/src/core/13_self_rag_core.py)

## Fusion RAG

Implements a hybrid retrieval system that combines the strengths of semantic vector search and keyword-based BM25 retrieval. This approach improves retrieval quality by capturing both conceptual similarity and exact keyword matches.

-----
Why is hybrid retrieval important?

Traditional RAG systems usually rely on vector search alone, which has limitations:

- Vector search captures semantic similarity but can miss exact keyword matches
- Keyword search is good for specific terms but lacks semantic understanding
- Different query types perform very differently across retrieval methods

-----
Hybrid retrieval achieves complementary strengths by:

- Running vector-based and keyword-based retrieval in parallel
- Normalizing scores from both methods
- Combining results via a weighted formula
- Reranking documents based on the combined score

-----
Implementation steps:

- Extract text from PDF files
- Tokenize text with the jieba tokenizer and create a vector store
- Perform keyword matching on the query using the BM25 algorithm
- Perform semantic matching on the query using vector search
- Combine and rerank the results from both methods with weights
- Return the final search results

![](./images/Fusion.webp)

----

- For the full implementation and analysis, see: [16_fusion_rag.ipynb](https://github.com/ByteTora/hello-rag/blob/main/src/full/16_fusion_rag.ipynb)
- For the core functions, see: [16_fusion_rag_core.py](https://github.com/ByteTora/hello-rag/blob/main/src/core/16_fusion_rag_core.py)

## Graph RAG

Graph RAG is a technique that enhances traditional RAG systems by organizing knowledge as a connected graph rather than a flat collection of documents. This lets the system navigate related concepts and find more contextually relevant information than standard vector similarity methods.

------
Key advantages of Graph RAG

- Preserves relationships between information
- Enables contextual traversal through connected concepts
- Improves handling of complex, multi-part queries
- Provides better explainability through visualized knowledge paths

------
Implementation steps:

- Extract text from PDFs
- Split the extracted text into overlapping chunks
- Build a knowledge graph from the chunks: use a model to summarize key concepts from each chunk, build knowledge-graph nodes from those concepts, compute edge weights from concept overlap and semantic similarity, and create edges — producing the knowledge graph
- Traverse the knowledge graph to find query-relevant information: compute similarity between the query and all nodes, sort by similarity (descending), take the top-k most similar nodes as starting points, and use a priority-queue-based breadth-first traversal to find the most relevant information
- Generate responses from the query and relevant chunks

![](./images/Knowledge_Graph.webp)

----

- For the full implementation and analysis, see: [17_graph_rag.ipynb](https://github.com/ByteTora/hello-rag/blob/main/src/full/17_graph_rag.ipynb)
- For the core functions, see: [17_graph_rag_core.py](https://github.com/ByteTora/hello-rag/blob/main/src/core/17_graph_rag_core.py)

## Hierarchical Indices

Implements a hierarchical indexing method (Hierarchical Indices) for RAG systems. This technique improves retrieval with a two-level search: first identify relevant document sections via summaries, then retrieve specific details from those sections.

-----
Traditional RAG methods treat all text chunks equally, which can lead to:

- Loss of context when chunks are too small
- Irrelevant retrieval results when the document collection is large
- Inefficient search across the whole corpus

-----
Hierarchical retrieval solves these problems by:

- Creating concise summaries of larger document sections
- Searching those summaries first to identify relevant sections
- Then retrieving detailed information only from those sections
- Preserving context while keeping specific details

-----
Implementation steps:

- Extract pages from PDFs
- Create a summary for each page, adding summary text and metadata to a summary list
- Create detailed chunks for each page by splitting the page text into chunks
- Create embeddings for both and store them in a vector store
- Hierarchically retrieve relevant chunks using the query: first retrieve relevant summaries, collect the pages from those summaries, then filter out chunks not from relevant pages and retrieve detailed chunks from the relevant pages
- Generate answers from the retrieved chunks

![](./images/Hierarchical_Indices.webp)

----

- For the full implementation and analysis, see: [18_hierarchy_rag.ipynb](https://github.com/ByteTora/hello-rag/blob/main/src/full/18_hierarchy_rag.ipynb)
- For the core functions, see: [18_hierarchy_rag_core.py](https://github.com/ByteTora/hello-rag/blob/main/src/core/18_hierarchy_rag_core.py)

## HyDE

Implements HyDE (Hypothetical Document Embedding) — an innovative retrieval technique that converts a user query into a hypothetical answer document before retrieval. This method bridges the semantic gap between short queries and long documents.

------
Traditional RAG systems embed the user's short query directly, but this often fails to capture the rich semantic information needed for optimal retrieval. HyDE solves this by:

- **Generating a hypothetical document that answers the query**
- **Embedding that expanded document instead of the raw query**
- **Retrieving documents similar to the hypothetical document**
- **Producing more contextually relevant answers as a result**

------
Implementation steps:

- Extract text content from PDF files
- Split text into chunks
- Create a vector store storing text chunks and metadata as vectors
- Use the model to answer the user's query (generating a hypothetical document)
- Create embeddings for the hypothetical document
- Retrieve similar chunks based on the hypothetical document
- Use the retrieved chunks to build context and generate answers

![](./images/HyDE.webp)

----

- For the full implementation and analysis, see: [19_HyDE_rag.ipynb](https://github.com/ByteTora/hello-rag/blob/main/src/full/19_HyDE_rag.ipynb)
- For the core functions, see: [19_HyDE_rag_core.py](https://github.com/ByteTora/hello-rag/blob/main/src/core/19_HyDE_rag_core.py)

## CRAG

Implements **Corrective RAG (CRAG)** — an advanced approach that dynamically evaluates retrieved information and, when necessary, corrects the retrieval process using web search as a fallback.

-----
CRAG improves on traditional RAG in the following ways:

- Evaluating retrieved content before use
- Dynamically switching between different knowledge sources based on relevance
- Correcting retrieval results with web search when local knowledge is insufficient
- Merging information from multiple sources when appropriate

-----
Implementation steps:

- Process documents and create a vector database
- Create query embeddings and retrieve documents
- Evaluate document relevance: assess the retrieved content
- Execute the appropriate knowledge-acquisition strategy: high relevance (score > 0.7) uses the document content directly; low relevance (score < 0.3) uses web search; medium relevance (0.3–0.7) combines document and web-search results, merging them — in hybrid search, the model distills the retrieved content to avoid redundancy
- Generate the final answer

![](./images/Crag.webp)

----

- For the full implementation and analysis, see: [20_crag.ipynb](https://github.com/ByteTora/hello-rag/blob/main/src/full/20_crag.ipynb)
- For the core functions, see: [20_crag_core.py](https://github.com/ByteTora/hello-rag/blob/main/src/core/20_crag_core.py)

## Chunking Techniques

### Chunk Size Selector

Choosing the right chunk size is critical for retrieval accuracy in a RAG pipeline. The goal is to balance retrieval performance against response quality.

-----
The following approach evaluates different chunk sizes:

- Extract text from PDFs
- Split text into chunks of different sizes
- Create embeddings for each chunk
- Retrieve relevant chunks for a query
- Generate responses using the retrieved chunks
- Evaluate response quality
- Compare results across chunk sizes

-----
Implementation steps:

- Extract text from PDFs: get page text per page
- Split text into chunks of different sizes and create embeddings for each chunk
- Retrieve relevant chunks based on the query
- Generate answers using the retrieved text chunks
- Evaluate retrieval answer quality across chunk sizes

----

- For the full implementation and analysis, see: [03_chunk_size_selector.ipynb](https://github.com/ByteTora/hello-rag/blob/main/src/full/03_chunk_size_selector.ipynb)
- For the core functions, see: [03_chunk_size_selector_core.py](https://github.com/ByteTora/hello-rag/blob/main/src/core/03_chunk_size_selector_core.py)

### Proposition Chunking

Proposition Chunking — an advanced method that achieves more precise retrieval by decomposing documents into atomic factual statements. Unlike traditional character-count-based chunking, proposition chunking preserves the semantic integrity of individual facts.

-----
Proposition chunking enables more precise retrieval by:

1. Decomposing content into atomic, self-contained factual statements
2. Creating smaller, more granular retrieval units
3. Enabling more precise matching between queries and relevant content
4. Filtering out low-quality or incomplete propositions

-----
Implementation steps:

- Extract text from PDF files
- Create chunks from the extracted text
- Generate propositions from the chunks
- Evaluate each proposition

----

- For the full implementation and analysis, see: [14_proposition_chunking.ipynb](https://github.com/ByteTora/hello-rag/blob/main/src/full/14_proposition_chunking.ipynb)
- For the core functions, see: [14_propostion_chunking_core.py](https://github.com/ByteTora/hello-rag/blob/main/src/core/14_propostion_chunking_core.py)

## Optional: Run with OrcaRouter

This project uses an OpenAI-compatible interface, so you can point the gateway at **OrcaRouter** — a zero-markup LLM gateway exposing 200+ models behind one endpoint. Configure three environment variables and you're set:

```bash
export LLM_BASE_URL=https://api.orcarouter.ai/v1
export LLM_API_KEY=sk-orca-...
export LLM_MODEL_ID=orcarouter/auto
```

- **Get an API key**: [orcarouter.ai/ref/ref_007610f54c1e5ca8be45](https://www.orcarouter.ai/ref/ref_007610f54c1e5ca8be45) | **Models**: [orcarouter.ai/models](https://www.orcarouter.ai/models) | **Docs**: [docs.orcarouter.ai](https://docs.orcarouter.ai)
- **Provider config**: [`providers/orcarouter.json`](https://github.com/ByteTora/hello-rag/blob/main/providers/orcarouter.json) · [`.env.example`](https://github.com/ByteTora/hello-rag/blob/main/.env.example)

*Listed on [Built with OrcaRouter](https://www.orcarouter.ai/built-with) — attribution, not endorsement.*

## RESOURCES:
- https://levelup.gitconnected.com/testing-18-rag-techniques-to-find-the-best-094d166af27f
- https://github.com/FareedKhan-dev/all-rag-techniques