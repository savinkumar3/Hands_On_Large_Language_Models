   #########    Looking Inside Large Language Models   ############ 
1. Forward Pass

The forward pass is the complete process that an LLM follows to generate text. It starts when the user enters a prompt, which is converted into token IDs by the tokenizer. These token IDs are transformed into embeddings and passed through multiple Transformer blocks, where the model understands the context and meaning of the sentence. Finally, the LM Head predicts the probability of every token in the vocabulary, and one token is selected as the output. This process repeats until the model generates the complete response.

2. Tokenizer

A tokenizer converts input text into smaller units called tokens and assigns each token a unique numerical ID. Transformers cannot understand words directly, so token IDs are used instead. The tokenizer contains a vocabulary of all the tokens known to the model. Different models use different tokenization methods, such as Byte Pair Encoding (BPE), WordPiece, and SentencePiece. The tokenizer is always the first component in the forward pass.

3. Token Embeddings

After tokenization, each token ID is converted into a token embedding, which is a vector of numbers representing the meaning of the token. Words with similar meanings usually have similar embeddings. These embeddings are the actual input to the Transformer blocks. They allow the model to understand semantic relationships between words instead of simply treating them as numbers. Token embeddings are learned during model training.

4. Neural Network

The neural network is the main part of the Transformer model and is responsible for understanding language. It consists of many Transformer blocks connected one after another. As token embeddings pass through these blocks, the model learns grammar, facts, reasoning, and relationships between words. Each block improves the representation of every token before passing it to the next block. Most of the computation in an LLM happens inside this neural network.

5. Transformer Block

A Transformer block is the core processing unit of a Transformer model. Each block contains a self-attention layer and a feedforward neural network (FFN). The self-attention layer helps each token gather useful information from other tokens in the sentence, while the FFN performs deeper language processing and stores learned knowledge. Modern Transformer blocks also use RMSNorm and residual connections to improve training stability. Multiple Transformer blocks work together to understand the input completely.

6. Self-Attention

Self-attention allows each token to look at other tokens in the sentence and identify which ones are most important. This helps the model understand context and relationships between words. For example, in the sentence "The dog chased the squirrel because it was fast," self-attention helps determine whether "it" refers to the dog or the squirrel. This mechanism enables the Transformer to generate context-aware and meaningful text. Self-attention is one of the most important innovations in Transformer models.

7. Query, Key, and Value (QKV)

The self-attention mechanism works using three matrices called Query (Q), Key (K), and Value (V). The Query represents what the current token is searching for, the Key represents the information available in each token, and the Value contains the actual information to be shared. The Query is compared with all the Keys to calculate relevance scores. These scores are then used to combine the Value vectors into a new representation. This process allows the model to understand context efficiently.

8. Relevance Scores

A relevance score tells the model how important one token is when processing another token. It is calculated by comparing the Query vector of the current token with the Key vectors of all previous tokens. Tokens with higher relevance scores receive more attention during processing. These scores are used to combine information from the Value vectors. As a result, the model focuses more on important words and less on irrelevant ones.

9. Feedforward Neural Network (FFN)

The Feedforward Neural Network (FFN) is the second major component of every Transformer block. After self-attention gathers contextual information, the FFN performs deeper computations on each token independently. It learns language patterns, grammar, facts, and reasoning from the training data. The FFN is responsible for much of the model's knowledge and generalization ability. Together with self-attention, it enables the Transformer to generate meaningful text.

10. Language Modeling (LM) Head

The Language Modeling Head (LM Head) is the final layer of the Transformer model. It converts the output vectors from the last Transformer block into probability scores for every token in the vocabulary. The token with the highest probability or one selected through sampling becomes the next generated token. The LM Head itself does not understand language; it simply converts learned features into predictions. It is responsible for producing the final output.

11. Decoding (Sampling)

Once the LM Head produces probability scores, the model must choose one token to generate. This process is called decoding. Greedy decoding always selects the highest-probability token, while sampling introduces randomness by selecting tokens based on their probabilities. Sampling usually produces more natural and creative text than greedy decoding. The selected token is then added to the input, and the forward pass repeats.

12. Parallel Token Processing

One of the biggest advantages of Transformers is that they process all input tokens simultaneously instead of one after another. Each token has its own computation path while still interacting with other tokens through attention. This parallel processing makes Transformers much faster than older sequential models such as RNNs and LSTMs. Only the last token's output is used to predict the next token during text generation. Parallel processing greatly improves efficiency.

13. Context Length

The context length is the maximum number of tokens that a Transformer can process at one time. For example, a model with a context length of 4K can process up to 4,096 tokens. Longer context lengths allow the model to understand longer documents and conversations. If the input exceeds the context limit, some information may need to be removed or truncated. Context length is an important limitation of Transformer models.

14. Multi-Head Attention

Instead of using only one attention mechanism, Transformers perform multiple attention operations simultaneously, called Multi-Head Attention. Each attention head learns different types of relationships, such as grammar, meaning, or long-distance dependencies. The outputs from all heads are combined to form a richer representation of the input. This improves the model's ability to understand complex language. Multi-Head Attention is one of the key reasons Transformers perform so well.

15. Multi-Query Attention (MQA)

Multi-Query Attention (MQA) is an optimized version of Multi-Head Attention. In the original method, every attention head has separate Query, Key, and Value matrices. MQA allows each head to keep its own Query matrix while sharing the Key and Value matrices across all heads. This reduces memory usage and speeds up inference. It is especially useful for very large language models.

16. Grouped-Query Attention (GQA)

Grouped-Query Attention (GQA) improves upon MQA by dividing attention heads into groups. Each group shares its own Key and Value matrices instead of all heads sharing a single pair. This balances speed and model quality better than MQA. Modern models such as Llama 2, Llama 3, and Phi-3 use GQA because it provides efficient attention while maintaining high accuracy.

17. Flash Attention

Flash Attention is an optimized implementation of the attention mechanism designed for GPUs. Instead of changing the attention algorithm, it improves how data is moved between different types of GPU memory. This significantly reduces memory usage and speeds up both training and inference. Flash Attention produces exactly the same results as standard attention but much faster. Many modern LLMs use Flash Attention for better performance.

18. Local and Sparse Attention

Local Attention allows a token to attend only to nearby tokens, while Sparse Attention allows it to attend only to selected tokens instead of every previous token. Both methods reduce the amount of computation needed for long sequences. GPT-3 combines full attention with sparse attention to improve efficiency without losing too much quality. These techniques make processing long documents faster and more memory-efficient.

19. Residual Connections

Residual connections add the original input of a layer to its output before passing it to the next layer. This helps preserve important information and prevents it from being lost during deep processing. Residual connections also make it easier to train very deep Transformer models by improving gradient flow. Almost all modern neural networks use this technique. It improves both stability and accuracy.

20. RMS Normalization (RMSNorm)

RMSNorm is a normalization technique used in modern Transformer models to keep the values of token embeddings balanced. It scales the input values using their Root Mean Square (RMS), making training more stable and efficient. RMSNorm performs fewer calculations than LayerNorm while achieving similar or better performance. Models such as Llama, Mistral, and Phi-3 use RMSNorm because it is faster and uses less memory.

21. Rotary Positional Embeddings (RoPE)

Transformers process all tokens in parallel, so they need a way to understand word order. Rotary Positional Embeddings (RoPE) add positional information directly inside the attention mechanism by modifying the Query and Key vectors. Unlike older absolute positional embeddings, RoPE captures both the position of words and the distance between them. This helps the model understand long sequences more effectively. RoPE is widely used in modern LLMs.

22. Padding

Padding is the process of adding special PAD tokens to shorter input sequences so that all sequences in a batch have the same length. This allows the GPU to process multiple sentences together efficiently. PAD tokens do not contain meaningful information and are ignored by the model. Although padding simplifies batch processing, it can waste memory when many PAD tokens are added. Therefore, efficient training methods try to reduce unnecessary padding.

23. Packing

Packing is a technique used during training to reduce the wasted space caused by padding. Instead of filling unused positions with PAD tokens, multiple short documents are combined into one context using separator tokens. This increases GPU utilization and reduces unnecessary computation. Packing allows the model to train faster without changing its learning process. It is commonly used when training large language models.

24. KV Cache

KV Cache (Key-Value Cache) is an optimization used during text generation. It stores the previously computed Key and Value matrices from the attention mechanism so that they do not need to be recalculated for every new token. This significantly speeds up inference and reduces computation. Without KV Cache, the model would repeatedly process all previous tokens whenever a new token is generated. Modern LLMs always use KV Cache during inference.

25. Overall Working of a Transformer-based LLM

A Transformer-based LLM starts by converting the user's input into token IDs and then into embeddings. These embeddings pass through multiple Transformer blocks, where self-attention gathers contextual information and feedforward networks perform deeper reasoning. The LM Head converts the final representations into probability scores for all vocabulary tokens, and a decoding strategy selects the next token. This process repeats until the complete response is generated. Modern techniques such as GQA, Flash Attention, RoPE, RMSNorm, packing, and KV Cache make the model faster, more memory-efficient, and capable of producing accurate and coherent text.