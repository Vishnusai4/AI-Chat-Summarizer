# AI-Chat-Summarizer

This repo contains the implementation of an LLM based chat summarizer. The open-source Google FLAN-T5 LLM which is a Seq2Seq model was picked for fine-tuning and inference. The model was fine-tuned using a single T4 GPU instance.

**Dataset:** The LLM model specifically fine-tunes on the DialogSum dataset which contains 10k+ conversations with the ground-truth summary annotated by a human linguist.

**Tokenizer:** Converts natural language to tokens. The pre-trained tokenizer of the FLAN-T5 model is used here.

**GenAI configs:** 
1. Precision: [BFLOAT 16](https://cloud.google.com/blog/products/ai-machine-learning/bfloat16-the-secret-to-high-performance-on-cloud-tpus). Known for better stability than the half preicision fp16.
2. Max new tokens: 50. This is the max number of tokens in the completion of the LLM.
3. Input token limits: 512
4. Rank for PEFT LORA: 32.


**Input to the model:** In-context prompts giving clear instructions to the model to summarize a given conversation. Strategies tried:
1. Zero-shot inferencing: The LLM is asked to summarize a given chat conversation directly.
2. One-shot inferencing: The LLM is given one example of chat conversation-summary to learn before asking it to summarize the test conversation.
3. Few-shot inferencing: The LLM is given around 5-6 examples.

The inutition with the various prompt is that the LLM is expected to infer a better completion after learning patterns from the given examples. However, no noticeable improvements were found for the DialogSum dataset.

**Fine-tuning with LORA:**

[LORA](https://arxiv.org/abs/2106.09685) decomposes the trainable parameters into low rank matrices determined by the rank factor r. This reduced the total number of trainable parameters from 251116800 to 3538944 (1.41\%) improving the ROUGE-1 metric by 3\%.
