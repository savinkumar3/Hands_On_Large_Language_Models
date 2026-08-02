                      #############Chapter 5##########
1. Introduction to Text Clustering 

Text clustering is an unsupervised learning technique used to group similar documents based on their meaning (semantic similarity) instead of predefined labels. Unlike text classification, clustering does not require labeled data. It automatically discovers hidden patterns and groups in a collection of text documents. Text clustering is useful for organizing large amounts of unstructured text, detecting outliers, finding incorrectly labeled data, and speeding up manual labeling. Modern Transformer-based embedding models have greatly improved text clustering because they understand the context and meaning of words rather than treating text as just a collection of words.

2. Embedding Documents

The first step in text clustering is converting every document into a numerical vector called an embedding. An embedding captures the semantic meaning of a document so that documents with similar meanings have similar vectors. In this chapter, the thenlper/gte-small Sentence Transformer model is used because it performs well on semantic similarity tasks and is computationally efficient. The dataset contains 44,949 ArXiv research paper abstracts, and each document is converted into a 384-dimensional embedding. These embeddings become the features used for clustering similar documents.

3. Dimensionality Reduction Using UMAP

Document embeddings have many dimensions, making clustering difficult due to the curse of dimensionality. Therefore, the embeddings are compressed into fewer dimensions using UMAP (Uniform Manifold Approximation and Projection). UMAP reduces the 384-dimensional embeddings to 5 dimensions while preserving most of the semantic relationships between documents. Parameters like n_components=5, min_dist=0.0, and metric="cosine" help produce tighter and more meaningful clusters. Although some information is lost during compression, UMAP significantly improves clustering efficiency.

4. Clustering with HDBSCAN

After dimensionality reduction, the documents are grouped using HDBSCAN (Hierarchical Density-Based Spatial Clustering of Applications with Noise). Unlike k-means, HDBSCAN does not require the number of clusters to be specified beforehand. It automatically discovers clusters based on the density of the data and identifies outliers, which are documents that do not belong to any cluster. In this chapter, HDBSCAN creates 156 clusters from the ArXiv dataset. The min_cluster_size parameter controls the minimum number of documents required to form a cluster.

5. Inspecting and Visualizing Clusters

Once clusters are created, they are manually inspected by reading a few documents from each cluster to understand their content. For example, one cluster contains papers related to sign language translation, showing that the clustering process grouped semantically similar documents together. To visualize the clusters, UMAP reduces the embeddings to 2 dimensions, allowing them to be plotted on an x–y graph. Each color represents a different cluster, while gray points represent outliers. Since dimensionality reduction is only an approximation, manual inspection is still important for validating the quality of the clusters.

6. From Text Clustering to Topic Modeling 

Text clustering groups similar documents, but it does not explain what each group is about. Topic modeling extends clustering by discovering the main themes (topics) present in each cluster. Instead of only grouping documents, topic modeling represents each topic using important keywords or labels. Traditional methods like Latent Dirichlet Allocation (LDA) use a Bag-of-Words approach that ignores word order and context. Modern methods such as BERTopic use Transformer embeddings, which capture semantic meaning and produce more accurate topics.

7. BERTopic – A Modular Topic Modeling Framework 

BERTopic is a topic modeling framework that combines modern embedding models with traditional statistical techniques. It first creates document embeddings, reduces their dimensions with UMAP, clusters them using HDBSCAN, and finally generates meaningful topic representations using c-TF-IDF. A major advantage of BERTopic is its modular design, where each component acts like a Lego block and can be replaced independently. For example, the embedding model, clustering algorithm, or topic representation method can be changed without affecting the rest of the pipeline. This flexibility allows BERTopic to support supervised, hierarchical, dynamic, multimodal, online, and zero-shot topic modeling.

8. Bag-of-Words and c-TF-IDF

After clusters are created, BERTopic represents each topic using c-TF-IDF (class-based Term Frequency–Inverse Document Frequency). It first builds a Bag-of-Words, which counts how many times each word appears in a cluster. Unlike regular TF-IDF, c-TF-IDF treats an entire cluster as one document. Words that appear frequently in one cluster but rarely in other clusters receive higher weights, making them representative keywords for that topic. Common words like "the", "is", and "of" receive lower weights because they appear in almost every cluster.

9. BERTopic Functions 

BERTopic provides several useful functions for exploring topics. The get_topic_info() function displays all discovered topics, the number of documents in each topic, and their representative keywords. The get_topic() function returns the keywords and weights for a specific topic. The find_topics() function searches for topics similar to a user-provided phrase using semantic similarity. BERTopic also provides visualization functions such as visualize_documents(), visualize_barchart(), visualize_heatmap(), and visualize_hierarchy() to explore relationships between topics interactively.

10. Representation Models (Lego Blocks) 

The initial keywords generated by c-TF-IDF are meaningful but sometimes contain redundant or less informative words. BERTopic improves these keywords using representation models, which act as additional Lego blocks placed after c-TF-IDF. These blocks rerank or refine the keywords without changing the clusters themselves. Since representation models operate only once per topic instead of once per document, they are computationally efficient. BERTopic supports several representation models, including KeyBERTInspired, Maximal Marginal Relevance (MMR), and Large Language Models (LLMs) like GPT.

11. KeyBERTInspired Representation Model 

KeyBERTInspired improves topic representations by selecting more meaningful keywords using document embeddings and cosine similarity. It first identifies representative documents for each topic using c-TF-IDF and computes an average embedding for the topic. Candidate keyword embeddings are then compared with the topic embedding using cosine similarity. Keywords that are semantically closest to the topic receive higher rankings. This method often removes stop words and produces clearer keywords, although some domain-specific abbreviations such as NMT (Neural Machine Translation) may sometimes disappear because embedding models do not always represent abbreviations well.

12. Maximal Marginal Relevance (MMR) 

Maximal Marginal Relevance (MMR) improves topic representations by increasing keyword diversity. Sometimes c-TF-IDF produces redundant keywords such as summary, summaries, and summarization, which all express the same idea. MMR removes similar keywords and replaces them with different but still relevant ones, making the topic representation more informative. The diversity parameter controls how different the selected keywords should be. MMR therefore balances relevance and diversity, producing concise and meaningful topic descriptions.

13. Text Generation Lego Block

Instead of showing only keywords, BERTopic can use a Large Language Model (LLM) such as Flan-T5 or GPT-3.5 to generate short topic labels. BERTopic provides the LLM with a few representative documents and the topic keywords through a carefully designed prompt. The model then generates a concise label, such as "Neural Machine Translation", instead of displaying only keywords like translation, NMT, and BLEU. Since the LLM is called only once per topic rather than once per document, the process remains efficient even for very large datasets. GPT-3.5 generally produces more accurate and human-readable labels than smaller models like Flan-T5.

14. Overall Chapter Summary

Chapter 5 explains how modern language models can automatically discover hidden themes in large collections of text. Documents are first converted into embeddings, compressed using UMAP, and clustered using HDBSCAN. BERTopic then represents each cluster with meaningful keywords using c-TF-IDF and further improves them using KeyBERTInspired, MMR, or Large Language Models. Its modular "Lego block" architecture allows each component to be replaced independently, making the framework highly flexible. Overall, BERTopic combines semantic embeddings, clustering, keyword extraction, and generative AI to create accurate, interpretable, and customizable topic models for real-world text analysis.