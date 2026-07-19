1. Introduction to Text Classification

Text classification is the process of automatically assigning a label or category to a piece of text. Examples include classifying movie reviews as positive or negative, emails as spam or not spam, and news articles into different topics. In this chapter, sentiment analysis is used as the main classification task. The Rotten Tomatoes dataset, containing positive and negative movie reviews, is used to demonstrate different classification techniques. The chapter compares several approaches, ranging from pretrained representation models to modern generative models.

2. The Rotten Tomatoes Dataset

The chapter uses the Rotten Tomatoes dataset available on the Hugging Face Hub. It contains 10,662 movie reviews, equally divided into positive and negative sentiments. The dataset is split into training (8530 reviews), validation (1066 reviews), and test (1066 reviews) datasets. The training data is used to train models, the validation data helps tune parameters, and the test data measures the model's final performance. Each review contains two fields: text (movie review) and label (0 = negative, 1 = positive).

3. Text Classification with Task-Specific Representation Models

Task-specific representation models are pretrained models that have already been fine-tuned for a particular task, such as sentiment analysis. The chapter uses RoBERTa (cardiffnlp/twitter-roberta-base-sentiment-latest) as an example. The tokenizer first converts each review into tokens, and the model predicts probability scores for different sentiment classes. The class with the highest probability is selected as the final prediction. Without any additional training, this model achieved an F1-score of 0.80.

4. Tokenization

Before a language model can understand text, it must first convert words into tokens. A tokenizer breaks a sentence into smaller pieces that the model can process. These tokens are converted into numerical IDs and then into embeddings before being passed to the neural network. Tokenization also allows the model to understand unknown words by breaking them into smaller subwords. Every representation and generative model begins with this tokenization step.

5. Running Inference with the Pipeline

The Hugging Face pipeline() function simplifies using pretrained models. It automatically loads both the tokenizer and the pretrained model. Every movie review from the test dataset is passed through the pipeline to obtain sentiment scores. The score for negative and positive sentiment is compared, and the class with the highest score becomes the prediction. All predictions are stored for later evaluation.

6. Model Evaluation

After making predictions, the model is evaluated using a classification report. The report includes Precision, Recall, Accuracy, and F1-score. Precision measures how many predicted positive reviews were actually positive. Recall measures how many actual positive reviews were correctly identified. Accuracy measures the overall percentage of correct predictions, while the F1-score balances precision and recall. Throughout the chapter, the weighted F1-score is used as the main performance metric.

7. Confusion Matrix

The confusion matrix explains how predictions are classified into four categories: True Positive (TP), True Negative (TN), False Positive (FP), and False Negative (FN). These values help calculate precision, recall, accuracy, and F1-score. A good model has high TP and TN values while keeping FP and FN as low as possible. Understanding the confusion matrix helps explain why a model performs well or poorly.

8. Classification Using Embedding Models

Instead of directly predicting labels, embedding models convert text into dense numerical vectors called embeddings. These embeddings capture the semantic meaning of each sentence. The chapter uses Sentence Transformers (all-mpnet-base-v2) to generate embeddings for every movie review. These embeddings become features for a machine learning classifier. Since the embedding model is kept frozen, only the classifier is trained.

9. Sentence Transformers

Sentence Transformers are pretrained models that convert entire sentences into fixed-length embeddings. Every movie review is represented as a 768-dimensional vector. Reviews with similar meanings produce similar embeddings. Unlike task-specific models, these embeddings can be reused for many tasks, including semantic search, clustering, recommendation systems, and classification. Sentence Transformers are highly flexible and computationally efficient.

10. Logistic Regression Classifier

After generating embeddings, a Logistic Regression classifier is trained using the embedding vectors as input features. Logistic Regression learns how to separate positive and negative reviews based on these embeddings. Only the classifier is trained, while the embedding model remains unchanged. This lightweight approach achieved an F1-score of 0.85, outperforming the task-specific RoBERTa model.

11. Zero-Shot Classification

Zero-shot classification is used when no labeled training data is available. Instead of training a classifier, the labels themselves are converted into embeddings by writing descriptions like "A positive review" and "A negative review." Each movie review embedding is compared with these label embeddings using cosine similarity. The label with the highest similarity is selected as the prediction. This method achieved an F1-score of 0.78 without any labeled training data.

12. Cosine Similarity

Cosine similarity measures how similar two embeddings are by calculating the angle between them. A similarity score closer to 1 indicates that the embeddings are very similar, while a score closer to 0 indicates they are unrelated. During zero-shot classification, cosine similarity compares every review embedding with every label embedding. The label having the highest similarity score becomes the predicted class. It is one of the most commonly used similarity measures in NLP.

13. Text Classification with Generative Models

Unlike representation models that directly predict labels, generative models produce text as output. Therefore, they require a natural language instruction called a prompt. Instead of predicting "positive" or "negative" automatically, the model reads the instruction and generates the appropriate answer. Prompt engineering plays an important role in improving the quality of generated responses. This approach makes generative models flexible enough to perform many different NLP tasks.

14. Flan-T5 Model

Flan-T5 is an encoder-decoder Transformer trained on thousands of instruction-following tasks. It accepts text as input and generates text as output. Before classification, every movie review is prefixed with the instruction "Is the following sentence positive or negative?" The model then generates either "positive" or "negative". These textual outputs are converted into numerical labels (1 and 0) for evaluation. Flan-T5 achieved an F1-score of 0.84.

15. Prompt Engineering

Prompt engineering is the process of writing clear instructions that help generative models understand what task to perform. Since generative models can perform many different tasks, they need explicit guidance. Better prompts usually produce better results. For sentiment analysis, prompts clearly instruct the model to classify reviews as positive or negative. Good prompt design significantly improves model performance.

16. ChatGPT for Classification

ChatGPT is a closed-source generative model developed by OpenAI. Unlike open-source models, it is accessed through the OpenAI API instead of being downloaded locally. ChatGPT was trained using instruction tuning and preference tuning, allowing it to follow instructions and generate human-like responses. By giving it a prompt asking it to return 1 for positive reviews and 0 for negative reviews, it successfully classified movie reviews. ChatGPT achieved the highest performance in the chapter with an F1-score of 0.91.

17. OpenAI API

The OpenAI API allows developers to communicate with ChatGPT through the internet. A user creates an API key, which acts as a secure password for accessing the model. Every request is sent to OpenAI's servers, processed there, and the generated response is returned. Since the model runs on OpenAI's servers, no powerful local hardware is required. However, API usage may involve costs and rate limits.

18. Instruction Tuning vs Preference Tuning

Instruction tuning trains the model using examples of prompts paired with correct answers so it learns to follow instructions. Preference tuning improves the model further by showing multiple generated responses and asking humans to rank them from best to worst. The model learns which responses people prefer and adjusts its behavior accordingly. This produces responses that are more natural, helpful, and conversational. Preference tuning is one of the reasons ChatGPT performs so well.

19. Comparison of Classification Methods

The chapter compares five different approaches to text classification. RoBERTa (task-specific model) achieved an F1-score of 0.80. Sentence Transformers + Logistic Regression achieved 0.85. Zero-shot classification achieved 0.78 without any labeled data. Flan-T5 achieved 0.84 using prompt-based generation. ChatGPT produced the best performance with an F1-score of 0.91, demonstrating the power of large generative language models.

20. Overall Chapter Conclusion

Chapter 4 demonstrates that text classification can be solved using several different approaches depending on the available resources and data. Task-specific models provide simple and reliable classification, embedding models offer flexibility, zero-shot classification removes the need for labeled data, and generative models use natural language instructions to perform classification. ChatGPT achieved the best results, while embedding-based methods provided an excellent balance between accuracy and computational cost. The chapter highlights the growing importance of embeddings, prompt engineering, and generative AI in modern Natural Language Processing