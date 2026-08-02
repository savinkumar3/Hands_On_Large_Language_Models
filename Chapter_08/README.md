################### Chapter 8  – Semantic Search, Dense Retrieval, Reranking, RAG ################

1. Why Do We Need Better Search Systems?

Traditional search engines mainly search for matching words (keywords). If the user's query uses different words from the document, the search engine may fail to find the correct answer even though the document contains the required information.

Large Language Models (LLMs) make search much smarter by understanding the meaning (semantics) of text instead of only matching words. This is called semantic search. Chapter 8 explains how LLMs improve search using Dense Retrieval, Reranking, and Retrieval-Augmented Generation (RAG). These techniques are now used in systems like ChatGPT, Microsoft Bing AI, Google Gemini, and Perplexity AI.

2. What is Semantic Search?

Semantic Search is a search technique that understands the meaning of a sentence instead of only matching keywords. It uses embeddings to convert both documents and user queries into vectors (numerical representations). Documents that have similar meanings are placed close together in vector space.

For example, if the user searches "How accurate was the science?", a keyword search looks for the exact words "science" and "accurate." Semantic search instead understands that this question is asking about scientific accuracy, so it can retrieve a sentence saying "The film received praise for its scientific accuracy." Even though the wording is different, the meaning is the same.


3. Dense Retrieval

Dense Retrieval is a semantic search technique that retrieves documents based on the similarity of embeddings.

Instead of searching for matching keywords, the system converts the user's query into an embedding and compares it with the embeddings stored in a vector database. The documents whose vectors are closest to the query vector are returned as search results. This allows the system to retrieve relevant information even when different words are used.

4. Building a Dense Retrieval System

A dense retrieval system follows four main steps.

First, collect the documents you want to search.

Second, split long documents into smaller chunks such as sentences or paragraphs.

Third, convert every chunk into an embedding using an embedding model.

Finally, store all embeddings inside a vector index or vector database such as FAISS.

Later, when a user asks a question, the query is also converted into an embedding and compared against the stored embeddings to find the most similar chunks.

5. Example Using the Interstellar Wikipedia Article

The chapter demonstrates Dense Retrieval using the Wikipedia article of Interstellar.

The article is first divided into individual sentences. Each sentence becomes one searchable chunk. Every sentence is converted into a 4096-dimensional embedding using Cohere's embedding model. These embeddings are stored inside a FAISS index.

When the user asks "How precise was the science?", the query is converted into another embedding. FAISS compares this embedding with all stored sentence embeddings and returns the closest ones. The first retrieved sentence talks about the movie's scientific accuracy, showing how semantic search understands meaning instead of exact words.

6. Chunking Documents

Large Language Models cannot process extremely long documents at once because they have a limited context window. Therefore, long documents are divided into smaller pieces called chunks.

Chunking makes searching more accurate because each chunk focuses on a smaller idea. Instead of embedding an entire book into one vector, we embed many smaller chunks so the search system can retrieve only the relevant information.


7. Overlapping Chunks

Overlapping chunks repeat a few sentences between neighboring chunks.

For example,

Chunk 1:
Sentence 1
Sentence 2
Sentence 3

Chunk 2:
Sentence 3
Sentence 4
Sentence 5

Sentence 3 appears in both chunks.

This does not lose data. Instead, it intentionally duplicates some information so that if an important sentence lies near the boundary between two chunks, both chunks still contain enough context. The only disadvantage is slightly higher storage usage because some text is stored twice.

8. One Vector Per Document

One approach is to represent the whole document using one embedding vector.

This is simple and requires little storage. However, one vector must summarize the entire document, so many important details are lost. If the user searches for a specific fact buried deep inside the document, the single vector may not represent it well.

Therefore, one-vector-per-document is generally used only for small demonstrations or short documents.

9. Multiple Vectors Per Document

A much better approach is to split the document into chunks and create one embedding for each chunk.

This preserves much more information because each chunk focuses on one concept. When a query is searched, only the relevant chunk is retrieved instead of the entire document. This produces more accurate search results and is the approach used in almost all modern RAG systems.

10. Searching Embeddings

After embeddings are created, the user's query is also embedded.

The system compares the query embedding with every stored embedding using similarity measures such as cosine similarity or Euclidean distance.

The vectors with the smallest distance or highest similarity are returned as search results.

Smaller distance means higher semantic similarity.

11. Approximate Nearest Neighbor (ANN)

If the database contains only a few thousand vectors, we can compare the query with every vector.

However, when there are millions or billions of embeddings, this becomes too slow.

Approximate Nearest Neighbor (ANN) algorithms solve this problem by using smart indexing methods. Instead of checking every embedding, ANN quickly finds vectors that are very close to the query.

Although ANN is called "approximate," its results are usually almost identical to exact search while being thousands of times faster.

12. FAISS and Annoy

FAISS (Facebook AI Similarity Search) is a library developed by Meta.

It stores embeddings and searches through millions of vectors extremely quickly. It supports both CPUs and GPUs.

Annoy (Approximate Nearest Neighbors Oh Yeah) is another ANN library developed by Spotify. It is widely used for recommendation systems and semantic search.

Both libraries make semantic search practical for large datasets.

13. Vector Database

A vector database is a database specially designed for storing and searching embeddings.

Unlike FAISS, a vector database allows embeddings to be added, updated, or deleted easily. It also stores metadata and supports filtering.

Examples include Pinecone and Weaviate.

Vector databases are widely used in production RAG systems because they manage large collections of embeddings efficiently.

14. Fine-Tuning Embedding Models

Embedding models can be fine-tuned to improve retrieval accuracy.

Training data contains positive and negative examples.

Positive pairs contain a query and its correct document.

Negative pairs contain a query and an unrelated document.

During training, the model learns to move positive embeddings closer together while pushing negative embeddings farther apart. This makes future searches much more accurate.

15. Keyword Search (BM25)

BM25 is one of the most popular keyword search algorithms.

It searches by matching exact words between the query and the document.

It does not understand synonyms or sentence meaning.

For example,

Query:
"How accurate was the science?"

BM25 mainly searches for the words "science" and "accurate." If those exact words are missing, BM25 may miss the correct answer.

16. Dense Retrieval vs BM25

BM25 searches using exact words.

Dense Retrieval searches using meaning.

Dense Retrieval can find relevant information even when different words are used.

BM25 is better for exact names, codes, product IDs, and phrases.

Dense Retrieval is better for natural-language questions.

Modern search systems often combine both methods.

17. Reranking

Reranking is the second stage of a search pipeline.

The first-stage retriever retrieves many candidate documents.

The reranker then examines each candidate carefully and changes their order according to relevance.

Instead of simply returning the documents retrieved by BM25 or Dense Retrieval, the reranker identifies which documents actually answer the user's question best.

18. How a Reranker Works

A reranker receives the user's query together with one candidate document.

Both are given to the language model at the same time.

The model understands both texts together and predicts a relevance score between 0 and 1.

Documents with higher relevance scores move to the top of the results.

This produces much better rankings than keyword search alone.

19. Cross-Encoder (monoBERT)

Most rerankers use a Cross-Encoder such as monoBERT.

Unlike Dense Retrieval, which embeds the query and document separately, a Cross-Encoder processes both together.

Because the model compares every word in the query with every word in the document, it understands their relationship much better.

This produces very accurate relevance scores, although it is slower than Dense Retrieval.

20. Retrieve and Re-Rank Pipeline

Modern search systems usually work in two stages.

Stage 1:
Retrieve 100 or 1000 possible documents quickly using BM25 or Dense Retrieval.

Stage 2:
Use a Cross-Encoder reranker to carefully score these candidates.

Finally, only the highest-ranked documents are shown to the user.

This combines speed with high accuracy.

21. Evaluating Search Systems

To evaluate a search system, we need:

A document collection.
A set of search queries.
Relevance judgments showing which documents are correct for each query.

Using this test data, different search systems can be compared fairly.

22. Precision at K

Precision at K measures how many of the top K retrieved documents are relevant.

If the top three results contain two correct documents,

Precision@3 = 2/3 = 0.67

Higher precision means better search quality.

23. Average Precision (AP)

Average Precision measures one query.

It rewards systems that place relevant documents earlier in the ranking.

A search system that returns the correct answer in the first position receives a higher AP than one returning it in the third position.

Therefore, AP evaluates both correctness and ranking quality.

24. Mean Average Precision (MAP)

MAP evaluates the entire search system.

First, calculate Average Precision for every query.

Then calculate the average of all those AP scores.

This single number represents the overall quality of the search engine.

A higher MAP indicates a better search system.

25. Retrieval-Augmented Generation (RAG)

Retrieval-Augmented Generation (RAG) combines search with text generation.

Instead of answering using only its training knowledge, the LLM first searches for relevant documents.

These retrieved documents are then given to the LLM as context.

The LLM generates an answer using this retrieved information.

In simple words:

RAG = Search + LLM

26. Why RAG is Better

Normal LLMs sometimes hallucinate because they rely only on their training knowledge.

RAG greatly reduces hallucinations by retrieving real documents before answering.

Since the answer is based on retrieved information, it becomes more factual, current, and reliable.

27. From Search to RAG

Converting a search system into RAG is simple.

First, retrieve the most relevant documents.

Next, send both the user's question and those retrieved documents to the LLM.

Finally, the LLM reads the documents and writes a complete natural-language answer.

Instead of showing raw search results, users receive a summarized answer.

28. Local RAG

Instead of using cloud APIs, RAG can also run locally.

The chapter uses:

Phi-3 Mini for text generation.
GTE-Small for creating embeddings.
FAISS for storing embeddings.
LangChain to connect everything.

This creates a fully local RAG system without sending data to external servers.

29. Prompt Template in RAG

The prompt template combines:

Retrieved context
User question

The LLM is instructed to answer only using the provided context.

This grounding helps reduce hallucinations and improves factual accuracy.

30. Advanced RAG Techniques

More advanced RAG systems improve retrieval in several ways.

Query Rewriting rewrites long or confusing questions into short search-friendly queries.

Multi-Query RAG creates multiple search queries for questions that need information from different topics.

Multi-Hop RAG performs multiple sequential searches where one search depends on the previous search results.

Query Routing chooses different databases depending on the question, such as HR documents or customer records.

Agentic RAG allows the LLM to make decisions, use multiple tools, search different databases, and perform actions like an intelligent agent.

31. Evaluating RAG Systems

RAG systems are evaluated using additional metrics beyond search accuracy.

These include:

Fluency – Is the answer natural and easy to read?
Perceived Utility – Is the answer useful?
Citation Recall – Are the statements supported by retrieved documents?
Citation Precision – Do the citations correctly support the answer?
Faithfulness – Is the answer consistent with the retrieved context?
Answer Relevance – Does the answer actually answer the user's question?

Libraries like Ragas and methods like LLM-as-a-Judge help automate these evaluations.