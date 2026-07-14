   #########   Tokens and Embeddings  ############# 
1. Introduction to Tokenization

Large Language Models (LLMs) cannot understand human language directly. They only understand numbers. Therefore, before processing text, the text must be converted into a numerical form. The first step is tokenization, where the sentence is divided into smaller pieces called tokens. These tokens are then converted into numbers called token IDs. Finally, the model converts token IDs into embeddings so it can understand their meaning.

2. What is Tokenization?

Tokenization is the process of breaking text into smaller units called tokens. A token can be a whole word, part of a word, punctuation, number, or symbol. For example, the sentence "I love AI." can become ["I", "love", "AI", "."]. Tokenization makes text easier for the model to process. Every LLM performs tokenization before doing any prediction.

3. Why Tokenization is Important

Computers cannot understand words directly because they only process numbers. Tokenization converts text into manageable pieces that the model can understand. It also helps reduce the vocabulary size because many words share common subwords. Even if the model sees a new word, it can split it into known subwords and understand it. This makes LLMs more efficient and flexible.

4. Token IDs

After tokenization, every token is assigned a unique number called a token ID. These IDs are stored in the tokenizer's vocabulary. The language model receives only these numbers, not the original words. For example, "Hello" may have one token ID in GPT and a different token ID in BERT. Token IDs themselves have no meaning; they simply identify tokens.

5. Types of Tokenization

There are four main tokenization methods. Word Tokenization treats every word as one token but struggles with new words. Subword Tokenization splits words into meaningful parts like play + ing, making it the most popular approach. Character Tokenization treats every letter as a token but creates many more tokens. Byte Tokenization converts text into bytes and works well for multiple languages, emojis, and special symbols.

6. Subword Tokenization

Subword tokenization is the most commonly used method in modern LLMs. It stores both complete words and frequently occurring word parts. For example, "unhappiness" may become ["un", "happi", "ness"]. This allows the model to understand unseen words by combining known subwords. It reduces vocabulary size while maintaining good language understanding.

7. Different Tokenizers

Different language models use different tokenizers. BERT uses WordPiece, GPT models use Byte Pair Encoding (BPE), and Flan-T5 uses SentencePiece. Since every tokenizer has its own vocabulary, the same word may be split differently across models. Therefore, each pretrained model must use its own tokenizer. A GPT tokenizer cannot be directly used with BERT.

8. Factors Affecting Tokenizers

Three main factors determine how a tokenizer behaves. The first is the tokenization method (BPE, WordPiece, or SentencePiece). The second is the tokenizer parameters, such as vocabulary size, capitalization, and special tokens. The third is the training dataset, because a tokenizer trained on English text behaves differently from one trained on programming code or multilingual data.

9. Vocabulary Size

The vocabulary is the complete list of tokens known by the tokenizer. Common vocabulary sizes are around 30K, 50K, or even 100K tokens. A larger vocabulary reduces word splitting but requires more memory. A smaller vocabulary creates more subword tokens but saves memory. Choosing the right vocabulary size is an important tokenizer design decision.

10. Special Tokens

Special tokens have specific purposes instead of representing normal words. Examples include [CLS] for classification, [SEP] for separating sentences, [MASK] for masked word prediction, <pad> for padding shorter inputs, and <unk> for unknown words. Chat models also use <|user|>, <|assistant|>, and <|system|> to identify speakers. These tokens help organize the input for different tasks.

11. Comparison of Popular Tokenizers

BERT converts text to lowercase in its uncased version and replaces unknown characters with [UNK]. GPT-2 preserves capitalization and whitespace, making it better for text generation. GPT-4 uses a larger vocabulary and fewer tokens for many words, improving efficiency. StarCoder is optimized for programming code and handles whitespace and numbers better. Phi-3 uses Llama's tokenizer with additional chat tokens.

12. Importance of Whitespace

Whitespace is especially important in programming languages like Python because indentation defines the program structure. Code-focused models treat multiple spaces as a single token, making code easier to understand and generate. Better whitespace handling reduces the number of tokens and improves coding performance. This is why specialized code models outperform general language models on programming tasks.

13. What are Embeddings?

After tokenization, the model only has token IDs, which have no meaning. Embeddings convert each token ID into a vector of numbers that represents its meaning. Similar words have similar embedding vectors. For example, king and queen have embeddings that are close together. Embeddings allow the model to perform mathematical operations on language.

14. Token Embeddings

Every token in the tokenizer's vocabulary has its own embedding vector. These vectors are randomly initialized before training. During training, they are continuously updated so that words with similar meanings become closer together. Token embeddings are stored inside the language model. Whenever a token appears, its embedding is used as the model's input.

15. Contextualized Word Embeddings

Older methods gave each word one fixed embedding regardless of context. Modern language models create contextual embeddings, where the meaning of a word depends on the sentence. For example, bank has one meaning in "river bank" and another in "bank account." Because the surrounding words change, the embedding also changes. This makes LLMs much better at understanding language.

16. Text Embeddings

Sometimes we need one vector to represent an entire sentence or document instead of individual words. This is called a text embedding. It captures the overall meaning of the text. Text embeddings are widely used in semantic search, document similarity, recommendation systems, clustering, text classification, and Retrieval-Augmented Generation (RAG). Models like Sentence Transformers are specifically trained for this purpose.

17. Sentence Transformers

Sentence Transformers are pretrained models designed to generate high-quality text embeddings. A complete sentence is converted into one embedding vector, often containing hundreds of numerical values such as 768 dimensions. Sentences with similar meanings produce similar embeddings. These embeddings are used to compare documents and retrieve relevant information quickly. They are widely used in modern AI applications.

18. Word2Vec

Word2Vec is one of the earliest and most popular embedding algorithms. It learns embeddings by observing which words frequently appear together in sentences. Words with similar contexts receive similar vectors. For example, king, queen, prince, and emperor appear close together in the embedding space. Word2Vec introduced many concepts still used in modern AI.

19. Sliding Window

Word2Vec generates training examples using a sliding window. It selects one word as the center word and considers nearby words as its context. For every sentence, the window moves one word at a time, creating millions of training pairs. This helps the model learn which words commonly appear together. The larger the training data, the better the learned embeddings.

20. Skip-Gram

Skip-Gram is a Word2Vec training method that predicts surrounding words from a center word. The center word is the input, while neighboring words are the expected outputs. If two words frequently appear together, their embeddings become similar during training. This allows the model to capture semantic relationships between words. Skip-Gram works especially well for learning rare words.

21. Negative Sampling

If the model only learned from neighboring words, it could simply predict every word as related. To avoid this, Word2Vec introduces negative sampling. Random unrelated words are added as negative examples, and the model learns to distinguish between correct and incorrect word pairs. This makes embeddings much more accurate. Negative sampling also speeds up training significantly.

22. Contrastive Training

Contrastive training teaches a model by comparing positive pairs and negative pairs. Positive pairs should have similar embeddings, while negative pairs should be far apart. This learning technique is widely used in modern AI systems. It powers sentence embeddings, retrieval models, recommendation systems, multimodal AI, and many Retrieval-Augmented Generation (RAG) applications. It remains one of the most important ideas in machine learning.

23. Embeddings for Recommendation Systems

Embeddings are useful beyond language processing. In recommendation systems, songs can be treated as words and playlists as sentences. Using Word2Vec, songs that frequently appear together in playlists receive similar embeddings. When a user selects one song, the system recommends nearby songs in the embedding space. This same idea is used for recommending movies, products, videos, and other items