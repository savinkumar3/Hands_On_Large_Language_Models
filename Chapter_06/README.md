###############Chapter 6 Summary – Prompt Engineering and Reasoning with Large Language Models ###############
1. Introduction to Prompt Engineering

Chapter 6 focuses on how to effectively interact with Large Language Models (LLMs) using prompt engineering. In earlier chapters, the book introduced text representation models like BERT for classification tasks and generative models like GPT for text generation. This chapter explains that the quality of an LLM's response depends greatly on how the prompt is written. It introduces prompt engineering as the process of designing clear and effective prompts to guide the model toward producing accurate, relevant, and useful responses. The chapter also covers advanced prompting techniques, reasoning methods, output verification, and ways to control the model's responses.

2. Using Text Generation Models

Before working with prompts, it is important to understand how text generation models are used. The chapter explains that users can choose between proprietary models (such as ChatGPT) and open-source models (such as Phi-3, Llama, or Mistral). Although proprietary models often provide better performance, open-source models are free, customizable, and can run locally. The authors recommend starting with a small model like Phi-3-mini (3.8 billion parameters) because it requires less GPU memory while still providing good performance. The model is loaded using the Hugging Face transformers library along with its tokenizer, and a text-generation pipeline is created for generating responses.

3. Prompt Templates

The chapter explains that users usually write prompts as simple conversations, but internally, the tokenizer converts them into a chat template before sending them to the model. This template contains special tokens such as <|user|>, <|assistant|>, and <|end|> to identify the speaker and indicate where responses begin and end. These templates were used during the model's training, so following them helps the model understand the conversation correctly. The transformers.pipeline automatically applies the appropriate chat template, making it easier to interact with the model.

4. Controlling Model Output

LLMs do not always generate the same response because they predict the next word based on probabilities. The chapter explains how sampling parameters can control the randomness and creativity of generated text. Setting do_sample=False makes the model always choose the most probable token, producing deterministic outputs. When do_sample=True, parameters such as temperature, top_p, and top_k affect how creative the response becomes. A low temperature produces predictable responses, while a high temperature increases creativity. Similarly, top_p controls how many probable tokens are considered, and top_k limits the model to selecting from only the top k probable tokens. Different combinations of these parameters are suitable for tasks such as brainstorming, email writing, creative writing, and translation.

5. Introduction to Prompt Engineering

Prompt engineering is the process of carefully designing prompts to obtain better responses from an LLM. Since an LLM is essentially a next-word prediction machine, providing clear instructions greatly improves its performance. A prompt can simply be a few words, but well-designed prompts usually include an instruction and the data related to the task. Additional components such as output indicators, examples, and context help the model better understand the expected response. Prompt engineering is an iterative process where prompts are continuously modified and tested to improve output quality.

6. The Basic Ingredients of a Prompt

The chapter introduces the basic building blocks of a prompt. A simple prompt usually contains an instruction describing the task and the data the task refers to. More advanced prompts may include output indicators that specify how the answer should be formatted. For example, in sentiment classification, adding labels such as "Text:" and "Sentiment:" guides the model to output only "positive" or "negative" instead of generating a full sentence. Additional elements such as examples, context, audience, and formatting instructions can be added depending on the complexity of the task.

7. Instruction-Based Prompting

One of the most common prompting methods is instruction-based prompting, where the model is directly instructed to perform a specific task such as summarization, translation, classification, question answering, or code generation. The effectiveness of instruction-based prompting depends on several best practices. Prompts should be highly specific, clearly describing the required output. They should also minimize hallucinations by allowing the model to answer "I don't know" when uncertain. Furthermore, placing important instructions at the beginning or end of long prompts improves performance due to the primacy and recency effects.

8. Advanced Prompt Engineering

The chapter then introduces advanced prompt engineering techniques by expanding prompts into several modular components. These components include persona, instruction, context, output format, audience, tone, and data. For example, assigning the model the role of an expert researcher, specifying a professional tone, identifying the target audience, and requesting bullet-point summaries significantly improves the generated response. Since prompt components can be added, removed, or reordered, prompt engineering becomes an iterative experimentation process where users continuously refine prompts to obtain better results.

9. In-Context Learning (Zero-Shot, One-Shot, and Few-Shot Prompting)

Rather than only describing a task, users can also provide examples of the expected behavior. This technique is called in-context learning. If no examples are given, it is called zero-shot prompting. Providing one example is known as one-shot prompting, while providing multiple examples is called few-shot prompting. These examples teach the model the desired format and style of the output. The chapter demonstrates this using made-up words such as "Gigamuru" and "screeg," where showing one example helps the model correctly generate a sentence using a new invented word. Few-shot prompting greatly improves both the content and structure of generated responses.

10. Chain Prompting

Instead of solving a complex problem with a single prompt, chain prompting breaks the task into multiple smaller prompts. The output from one prompt becomes the input for the next prompt. For example, an LLM first generates a product name, then creates a slogan using that name, and finally generates a sales pitch using both the name and slogan. This sequential workflow improves response quality because the model focuses on one subtask at a time. Chain prompting is also useful for response validation, writing stories, creating long documents, and combining outputs from multiple models.

11. Reasoning with Generative Models

The chapter introduces reasoning as an important capability of modern LLMs. Although current LLMs do not truly reason like humans, they can mimic reasoning through pattern recognition and carefully designed prompts. Human reasoning is compared to System 1 thinking, which is fast and intuitive, and System 2 thinking, which is slow, logical, and reflective. Prompt engineering techniques attempt to encourage the model to imitate System 2 thinking by making it reason step by step before producing an answer.

12. Chain-of-Thought Prompting

Chain-of-Thought (CoT) prompting encourages the model to explain its reasoning before giving the final answer. Instead of directly solving a problem, the model generates intermediate reasoning steps. This significantly improves performance on complex tasks such as mathematics and logical reasoning. The chapter also introduces Zero-Shot Chain-of-Thought, where simply adding phrases like "Let's think step by step" encourages the model to perform reasoning without providing any examples. Generating intermediate reasoning helps the model use previously generated information to arrive at more accurate final answers.

13. Self-Consistency

Even with chain-of-thought prompting, different responses may be generated because of sampling randomness. Self-consistency addresses this problem by asking the model the same question multiple times using different random sampling settings. Each response independently reasons through the problem, and the final answer is selected using majority voting. This approach increases reliability but requires multiple model executions, making it slower than a single inference.

14. Tree-of-Thought Prompting

Tree-of-Thought (ToT) extends chain-of-thought by allowing the model to explore multiple reasoning paths simultaneously. Instead of following a single reasoning process, the model generates several possible intermediate solutions, evaluates them, removes weak paths, and continues only with the best ones. Since this requires many model calls, the chapter presents a simplified prompting approach where multiple virtual experts discuss the problem together until they reach a consensus. This technique is particularly useful for creative writing, brainstorming, planning, and solving complex reasoning tasks.

15. Output Verification

The chapter emphasizes that real-world AI applications require reliable outputs. Generated responses should be structured, valid, ethical, and factually accurate. Output verification ensures that responses conform to expected formats such as JSON, avoid invalid values, reduce hallucinations, and satisfy ethical guidelines. The chapter identifies three approaches for controlling outputs: providing examples, using constrained grammars during generation, and fine-tuning the model. Fine-tuning is covered in a later chapter, while this chapter focuses on the first two methods.

16. Providing Examples for Structured Output

One way to improve output consistency is to provide examples of the expected format. For instance, instead of simply asking the model to generate an RPG character in JSON, users can provide a JSON template showing the exact structure they expect. The model then follows this structure much more consistently. This demonstrates that few-shot learning not only improves content quality but also helps maintain consistent output formatting. However, the model may still occasionally ignore the provided example because it remains a probabilistic generator.

17. Grammar-Based Constrained Sampling

The chapter concludes by introducing Grammar: Constrained Sampling, which provides stronger control than few-shot learning. Instead of merely suggesting an output format, constrained sampling forces the model to generate tokens that satisfy predefined grammar rules. Libraries such as llama-cpp-python, Guidance, Guardrails, and LMQL support this functionality. Using llama-cpp-python, the GGUF version of the Phi-3 model is loaded, and the parameter response_format={"type":"json_object"} activates JSON grammar constraints. As a result, every generated response is guaranteed to be valid JSON, making LLM outputs much more reliable for production systems that require structured data.