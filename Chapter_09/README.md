                  ################Chapter 9: Multimodal Large Language Models #################

1. Introduction to Multimodal Large Language Models

Large Language Models (LLMs) were originally designed to understand and generate only text. However, many real-world applications require understanding different types of data such as images, audio, video, and text. These different types of data are called modalities, and models that can process more than one modality are called multimodal models. Multimodal models combine information from multiple sources, allowing them to answer questions about images, describe pictures, understand videos, and perform more intelligent reasoning. By combining vision and language, these models become much more useful than text-only models.

2. Transformer, Structured Data, and Unstructured Data

A Transformer is a deep learning architecture that learns relationships between different parts of the input using the attention mechanism. It is the foundation of modern language models like GPT, BERT, and Llama. Structured data is organized in rows and columns, such as Excel sheets or databases, making it easy to search and analyze. Unstructured data has no fixed format and includes images, text, audio, and videos. Since images and text are unstructured, Transformers are designed to convert them into numerical representations (embeddings) that computers can understand and process.

3. Transformers for Vision (Vision Transformer - ViT)

The Vision Transformer (ViT) applies the Transformer architecture to images. Since images do not contain words, they cannot be tokenized like text. Instead, ViT divides an image into small patches (usually 16 × 16 pixels), and each patch acts like a word token. These patches are converted into numerical embeddings using linear projection. The Transformer encoder then processes these embeddings to learn relationships between different parts of the image. Finally, the learned image embeddings can be used for image classification, object detection, and other computer vision tasks.

4. Multimodal Embedding Models

Embedding models convert data into numerical vectors while preserving their meaning. Earlier models generated embeddings only for text, but multimodal embedding models generate embeddings for both text and images in the same vector space. Since both embeddings share the same space, similar images and text are located close together. This allows users to compare images with text directly. Such models make applications like image search using text, text retrieval using images, clustering, and recommendation systems much more efficient.

5. CLIP (Contrastive Language-Image Pre-training)

CLIP is one of the most popular multimodal embedding models developed by OpenAI. It is trained using millions of image-caption pairs. An image encoder converts images into image embeddings, while a text encoder converts captions into text embeddings. The similarity between these embeddings is calculated using cosine similarity. During training, the model learns to increase similarity for matching image-caption pairs and decrease similarity for incorrect pairs. This training method is called contrastive learning, which helps CLIP understand the relationship between images and text.

6. OpenCLIP

OpenCLIP is the open-source version of CLIP and works similarly. It uses three main components: a tokenizer for text, a processor for images, and the CLIP model itself. The tokenizer converts text into tokens and token IDs, while the processor resizes images and converts them into pixel values. The model generates 512-dimensional embeddings for both text and images. Since both embeddings have the same dimensions, their similarity can be calculated using cosine similarity. A higher similarity score means the image and text describe similar content.

7. Making Text Generation Models Multimodal

Traditional text generation models like GPT and Llama understand only text. To make them understand images, researchers combine pretrained vision models with pretrained language models. Instead of training an entirely new model, they build a bridge between these two models. This approach saves huge amounts of training time, computational power, and data. As a result, language models become capable of answering questions about images, describing scenes, and reasoning using both visual and textual information.

8. BLIP-2 (Bootstrapping Language-Image Pre-training)

BLIP-2 is a multimodal framework that connects a pretrained Vision Transformer (ViT) with a pretrained Large Language Model (LLM). The connection is made through a trainable module called the Q-Former (Querying Transformer). The Vision Transformer extracts image features, while the Q-Former converts these visual features into language-friendly embeddings. A projection layer adjusts their dimensions before sending them to the LLM. Since only the Q-Former is trained while the ViT and LLM remain frozen, BLIP-2 is much faster and cheaper to train than building a multimodal model from scratch.

9. BLIP-2 Workflow

The BLIP-2 workflow begins by passing an image to the Vision Transformer, which extracts important visual features and converts them into image embeddings. These embeddings are then given to the Q-Former, which translates visual information into language-friendly embeddings. A projection layer changes the embedding size so that it matches the format expected by the LLM. Finally, the LLM combines the image information with the user's prompt and generates a natural language response. This process enables BLIP-2 to understand images and answer questions without retraining the entire language model.

10. Preprocessing Multimodal Inputs

Before BLIP-2 can process images and text, both inputs must be preprocessed. The AutoProcessor performs this task by resizing images to 224 × 224 pixels, converting them into pixel values, normalizing them, and transforming them into tensors. Images are converted into pixel values because deep learning models understand only numerical data. For text, the processor uses the GPT2TokenizerFast, which converts sentences into token IDs. Long words may be split into subword tokens, and the symbol Ġ represents a space before a word. After preprocessing, both image and text are ready for the BLIP-2 model.

11. Use Case 1: Image Captioning

Image captioning is the process of automatically generating a text description for an image. The input image is first preprocessed into pixel values and then passed through the Vision Transformer, Q-Former, and Large Language Model. The model generates token IDs representing the caption, which are converted into readable English using batch_decode(). For example, an image of a sports car may produce the caption "An orange supercar driving on the road at sunset." Image captioning is widely used in photo management, accessibility tools, e-commerce product descriptions, and automatic image labeling systems.

12. Use Case 2: Multimodal Chat-Based Prompting (Visual Question Answering)

Multimodal chat-based prompting allows BLIP-2 to understand both an image and a text question at the same time. The processor prepares the image and question before passing them to the Vision Transformer and the Large Language Model. The model combines visual information with the user's question to generate an appropriate answer. Unlike image captioning, users can continue asking follow-up questions because previous questions and answers are stored in memory. This creates a chatbot-like experience where BLIP-2 can have a conversation about an image while remembering the context.

13. Interactive Visual Chatbot

The chapter demonstrates how to build an interactive visual chatbot using ipywidgets in Jupyter Notebook. A text box is created where users type questions about an image. Every question is combined with previous conversation history stored in a memory list, allowing BLIP-2 to remember earlier interactions. The processor prepares both the image and the prompt, and the BLIP-2 model generates an answer, which is displayed in the notebook. This memory-based approach makes the conversation feel natural and enables users to ask multiple related questions about the same image