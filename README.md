# AI-Chat-Summarizer

This repo contains the implementation of an LLM based chat summarizer. The open-source Google FLAN-T5 LLM which is a Seq2Seq model was picked for fine-tuning and inference.

**Dataset:** The LLM model specifically fine-tunes on the DialogSum dataset which contains 10k+ conversations with the ground-truth summary annotated by a human linguist.

**Tokenizer:** 

**Input to the model:** In-context prompts giving clear instructions to the model to summarize a given conversation. Strategies tried:
1. Zero-shot inferencing: The LLM is asked to summarize a given chat conversation directly.
2. One-shot inferencing: The LLM is given one example of chat conversation-summary to learn before asking it to summarize the test conversation.
3. Few-shot inferencing: The LLM is given around 5-6 examples.

The inutition with the various prompt is that the LLM is expected to infer a better completion after learning patterns from the given examples. However, no noticeable improvements were found for the DialogSum dataset.

**Fine-tuning with LORA:**

[LORA](https://arxiv.org/abs/2106.09685) decomposes the trainable parameters into low rank matrices determined by the rank factor r. This reduced the total number of trainable parameters from 251116800 to 3538944 (1.41\%) improving the ROUGE-1 metric by 3\%.
