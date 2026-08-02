 ######################### Chapter 10 Summary – Text Embedding Models  ####################

This chapter explains how modern text embedding models are created, trained, evaluated, and improved. It starts with the concept of contrastive learning, introduces SBERT (Sentence-BERT), explains different loss functions, shows how to train and fine-tune embedding models, discusses Augmented SBERT, unsupervised learning with TSDAE, and finally explains domain adaptation. Below is a beginner-friendly summary of every topic.

1. What is Contrastive Learning?

Contrastive learning is one of the most important techniques used to train text embedding models. The main idea is to teach the model that similar sentences should have embeddings that are close together, while different sentences should have embeddings that are far apart in vector space. Instead of simply learning the meaning of one sentence, the model always compares two or more sentences to understand their relationship. For example, "The dog is sleeping" and "A puppy is resting" should be close because they have similar meanings, whereas "The dog is sleeping" and "The airplane is flying" should be far apart. By repeatedly comparing similar and dissimilar sentence pairs, the model gradually learns semantic meaning. This approach is similar to how Word2Vec learns word embeddings by comparing neighboring and random words.

2. SBERT (Sentence-BERT)

SBERT (Sentence-BERT) is an improved version of BERT designed specifically for creating sentence embeddings efficiently. The original BERT uses a Cross-Encoder, where two sentences are processed together to calculate similarity. Although this gives very accurate similarity scores, it is extremely slow because every sentence must be compared with every other sentence. SBERT solves this problem by using a Bi-Encoder (Siamese Network). Each sentence is passed through the same BERT model separately, producing one fixed-size embedding for each sentence. These embeddings can then be compared using cosine similarity without running BERT again. This makes SBERT much faster and suitable for applications like semantic search, clustering, recommendation systems, retrieval, and duplicate detection.

3. Creating an Embedding Model

To build an embedding model, we first prepare a dataset containing similar and dissimilar sentence pairs. These sentence pairs are commonly taken from Natural Language Inference (NLI) datasets such as MNLI from the GLUE benchmark. Each pair has a label indicating whether the sentences are entailment (similar), neutral, or contradiction (different). These labeled pairs become the training examples for contrastive learning. The better the quality and quantity of the training data, the better the embedding model becomes. Large datasets help the model learn more stable and accurate semantic representations.

4. Training an SBERT Model

Training begins by loading a pretrained BERT model and converting it into an SBERT model using the SentenceTransformer framework. During training, every sentence pair passes through SBERT to generate sentence embeddings. These embeddings are then compared using a chosen loss function, which measures how correct the embeddings are based on the labels. If the embeddings are not correct, the loss function calculates the error. Using backpropagation and an optimizer, the model updates its weights to reduce this error. This process repeats for every batch and every epoch until the model learns to produce high-quality sentence embeddings. Finally, the trained model is evaluated and saved for future use.

5. Softmax Loss

Softmax Loss was one of the earliest loss functions used in SBERT training. After generating embeddings for two sentences, the embeddings are combined and passed through a softmax classifier, which predicts whether the sentence pair belongs to classes like entailment, neutral, or contradiction. The predicted class is compared with the true label, and the difference becomes the training error. Backpropagation then updates the model weights to reduce this error. Although Softmax Loss works correctly, it is mainly designed for classification tasks and does not directly optimize sentence similarity. Therefore, newer loss functions generally produce much better embedding quality.

6. Cosine Similarity Loss

Cosine Similarity Loss directly teaches the model how similar two sentences should be. Instead of predicting classes, each sentence pair is assigned a similarity score between 0 and 1, where 1 means highly similar and 0 means different. The model generates embeddings, calculates their cosine similarity, and compares this value with the true similarity score. The difference becomes the loss, and backpropagation updates the weights accordingly. Since cosine similarity directly measures semantic closeness, this loss function usually performs much better than Softmax Loss. In the chapter, the Pearson cosine score improved significantly from about 0.59 to 0.72.

7. Multiple Negatives Ranking (MNR) Loss

Multiple Negatives Ranking (MNR) Loss is one of the most powerful loss functions used for training embedding models. Instead of using only positive and negative labels, it uses an anchor sentence, its correct positive sentence, and several negative sentences. The model tries to make the anchor close to its correct positive while pushing it away from all negatives. Many negatives are automatically created from other examples within the same training batch, making training very efficient. Even better performance can be achieved using hard negatives, which are incorrect answers that are still very similar to the anchor. MNR Loss usually produces stronger embeddings than Softmax or Cosine Similarity Loss and achieved around 0.80 Pearson score in the chapter.

8. Evaluating Embedding Models

After training, the embedding model must be evaluated to measure its quality. One common benchmark is STSB (Semantic Textual Similarity Benchmark), where human-labeled sentence pairs are assigned similarity scores between 0 and 5. The model generates embeddings, calculates cosine similarity, and compares it with the human scores. Performance is measured using metrics such as Pearson Correlation and Spearman Correlation, where values closer to 1 indicate better performance. Although STSB is simple and widely used, it evaluates only one semantic similarity task.

9. MTEB (Massive Text Embedding Benchmark)

MTEB is a much larger benchmark designed to evaluate embedding models across many different tasks. Instead of testing only sentence similarity like STSB, MTEB evaluates models on classification, retrieval, reranking, clustering, pair classification, semantic similarity, summarization, and question answering. It contains 58 datasets, supports 112 languages, and measures both accuracy and inference speed. Because it covers many real-world applications, MTEB gives a much more reliable evaluation of an embedding model. However, it takes much longer to run, so STSB is often used during development.

10. Fine-Tuning an Embedding Model (Supervised)

Instead of training an embedding model completely from scratch, we can start with an already trained SBERT model and fine-tune it on our own labeled dataset. This approach is faster, cheaper, and usually produces better results because the pretrained model already understands general language. During fine-tuning, we simply replace the base BERT model with a pretrained SBERT model such as all-MiniLM-L6-v2 and continue training using our own data and an appropriate loss function like MNR Loss. Since the model already has strong embeddings, only small weight updates are needed. In the chapter, fine-tuning improved the Pearson score to about 0.85, showing the advantage of transfer learning.

11. Augmented SBERT

Augmented SBERT is used when only a small labeled dataset is available. First, a Cross-Encoder is trained on the small labeled dataset, called the Gold Dataset, which contains manually verified labels. The trained Cross-Encoder is then used to automatically label thousands of additional unlabeled sentence pairs, creating a Silver Dataset. Although the silver labels are machine-generated and may contain small errors, combining the Gold and Silver datasets creates a much larger training dataset. Finally, SBERT is trained using both datasets. This approach greatly reduces manual labeling while still achieving performance close to training on a fully labeled large dataset.

12. Unsupervised Learning using TSDAE

Sometimes no labeled data is available. In such situations, TSDAE (Transformer-based Sequential Denoising Auto-Encoder) can train an embedding model without labels. The idea is simple: random words are removed from a sentence to create a damaged version. This damaged sentence passes through an encoder to generate an embedding, and then a decoder tries to reconstruct the original sentence from that embedding. If reconstruction is poor, the model updates its weights. By repeatedly reconstructing original sentences, the encoder gradually learns meaningful sentence embeddings. After training, only the encoder is kept for generating embeddings, while the decoder is discarded. Even without labels, TSDAE achieved a strong Pearson score of around 0.70.

13. Domain Adaptation

Domain adaptation is used when a model must work well in a specialized field such as medicine, banking, law, or finance. A general embedding model may not fully understand domain-specific vocabulary. To solve this, the model is first trained using an unsupervised method like TSDAE or Masked Language Modeling (MLM) on a large collection of domain-specific text. This teaches the model the vocabulary and writing style of the new domain. Afterward, the model is fine-tuned using supervised SBERT training with labeled sentence pairs. This two-step process is called Adaptive Pretraining followed by Adaptive Fine-Tuning. It produces highly accurate embedding models even when labeled data is limited.