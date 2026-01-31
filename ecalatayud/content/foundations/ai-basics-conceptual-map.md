---
title: "AI Basics: A Conceptual Map"
date: 2025-12-06
tags: ["foundations", "ai-basics", "deep-learning", "transformers", "llms"]
draft: false
---

When people say “AI” today, they often mean systems that:

- recognise objects in images  
- understand and generate text  
- answer questions, write code, or summarise documents  
- combine text, images, audio, video…  

Most of these systems are based on **modern machine learning**, especially **deep learning**, and more recently **large language models (LLMs)** and **multimodal models**.

This post builds a **conceptual map of modern AI**: how we got here, what the key building blocks are, and where backpropagation, transformers, LLMs and multimodal models fit in.

---

## 1. From rules to learning systems

Historically, AI started with **rule-based systems**:

- Humans wrote explicit rules:  
  > IF symptoms match pattern X THEN suggest diagnosis Y  
- Good when experts can describe the logic.
- Hard to scale and maintain as complexity grows.

Modern AI largely moved to **learning from data** instead of hand-crafted rules:

1. **Classic machine learning** – simpler models trained on features (regression, trees, SVMs, etc.)  
2. **Deep learning** – neural networks with many layers that learn their own internal representations  
3. **Transformers, LLMs and foundation models** – scalable architectures trained on massive datasets, often used as general-purpose components

Rule-based ideas are still used (e.g. business constraints around a model), but the core intelligence in modern systems comes from **learned models**.

---

## 2. Machine learning in one picture

At a high level, most practical ML still fits in three families:

- **Supervised learning** – learn from labelled examples  
- **Unsupervised learning** – find structure without labels  
- **Reinforcement learning** – learn by trial and error with rewards  

We won’t go deep into classic algorithms here, but it’s useful to keep this mental map:

- supervised → prediction (classification, regression)  
- unsupervised → structure (clustering, embeddings, anomalies)  
- RL → behaviour over time (agents in environments)

Deep learning mostly **extends** supervised and unsupervised learning with more powerful models.

---

## 3. Deep learning and backpropagation: the core engine

Modern AI is dominated by **neural networks**, trained using **backpropagation**.

### 3.1 What a neural network actually does

Conceptually, a neural network is just:

- a chain (or graph) of simple computations  
- with parameters (weights and biases)  
- that transforms inputs into outputs

Example:

- Input: pixels of an image  
- Output: probabilities for each class (cat, dog, etc.)  

Each layer computes some transformation, and the last layer produces a prediction.

### 3.2 Backpropagation in simple terms

To make the network useful, we need to adjust the weights during training so that its predictions match the desired outputs.

Training loop idea:

1. Take an input (e.g. an image) and its label (e.g. “cat”).  
2. Run the network forward → get a prediction.  
3. Compare prediction with the label using a **loss function** (e.g. cross-entropy, MSE, etc.).  
4. Use **backpropagation** to compute how much each weight contributed to the error.  
5. Update the weights in the direction that **reduces** the loss (gradient descent or a variant).

Backpropagation is an efficient way of applying the **chain rule** of calculus through all layers:

- we compute gradients layer by layer, from the output back to the input  
- this lets us update millions (or billions) of parameters in a reasonable time

Almost all deep learning today – CNNs, RNNs, transformers, LLMs – is trained using **backprop + some optimiser** (SGD, Adam, etc.).

---

## 4. Transformers and large language models (LLMs)

Transformers have become the **default architecture** for many tasks, especially in natural language processing and, more recently, vision and multimodal models.  
On top of transformers, we build **large language models (LLMs)**.

### 4.1 Why transformers?

Traditional sequence models, like RNNs and LSTMs, process sequences step by step.  
Transformers take a different approach:

- They use **self-attention** to let each position in a sequence look at all other positions.  
- This makes it easier to capture long-range dependencies (e.g. relationships between words far apart in a sentence).  
- They are highly parallelisable, which fits GPU/TPU hardware extremely well.

As a result, transformers scale very well in **model size**, **data size** and **compute**.

### 4.2 Large language models (LLMs)

LLMs are **transformer-based models** trained on huge corpora of text (and sometimes code):

- Input: a sequence of tokens (words, subwords, characters)  
- Task during training: predict the next token at each step  

By training on large and diverse datasets, LLMs learn:

- syntax and semantics of language  
- world knowledge encoded in the data  
- patterns of reasoning and problem solving to some extent  

At inference time, we can:

- prompt them with instructions and context  
- ask them to generate answers, code, summaries, translations, explanations, etc.

Conceptually, an LLM is:

- a transformer architecture  
- with very large capacity (many parameters)  
- trained with backpropagation on the next-token prediction objective  

Many modern AI applications are built by **wrapping an LLM** with:

- tools (search, code execution, external APIs)  
- retrieval systems (RAG)  
- safety and policy layers  
- application-specific logic and UI

---

## 5. Multimodal models: beyond text

The next step after “just text” is **multimodal AI**: models that can handle more than one type of input and/or output.

Examples:

- Text + images → image captioning, visual question answering, “describe this picture”  
- Text + images + code → technical assistants that read diagrams and code  
- Text + audio → speech recognition and generation  
- Text + video → video understanding or generation

### 5.1 How multimodal models fit in the map

Most multimodal models still follow the same high-level pattern:

1. **Encoders** turn raw inputs (pixels, audio waveforms, tokens) into internal representations (embeddings).  
2. A core model (often transformer-based) processes and combines these representations.  
3. **Decoders** turn internal representations back into outputs (text, images, audio, etc.).

In many current systems:

- the **text side** is handled by an LLM,  
- additional encoders map images / audio / video into the same “language space” or a compatible embedding space,  
- the model is trained (again with backprop) to perform multimodal tasks: answer questions about images, follow instructions involving text and images, and so on.

In practice, much of the “multimodal AI” you see today is really **multimodal LLMs**: LLMs extended with extra input channels and training objectives.

---

## 6. Where do classic algorithms and rule-based systems fit now?

Even in a world dominated by transformers, LLMs and multimodal models, older ideas have their place:

- **Classic ML models** (logistic regression, trees, gradient boosting) are still:
  - strong baselines  
  - easier to train and deploy  
  - very effective on structured/tabular data  

- **Rule-based systems and expert logic**:
  - wrap models with business constraints  
  - enforce safety and compliance  
  - handle edge cases and overrides

Real AI systems often combine:

- deep learning / transformers / LLMs for perception and language  
- classic ML for specific prediction tasks  
- rules and domain logic for control, safety and integration

---

## 7. A modern learning path (very high level)

If you want to understand modern AI conceptually, a reasonable order is:

1. **Supervised learning basics**  
   - loss functions, overfitting, train/val/test splits  

2. **Neural networks and backpropagation**  
   - forward pass, loss, gradient descent, regularisation  

3. **Transformers**  
   - attention, self-attention, encoder/decoder architectures  

4. **Large language models (LLMs)**  
   - next-token prediction  
   - prompting and instruction-following  
   - adaptation techniques (fine-tuning, LoRA, RAG at a high level)  

5. **Multimodal models**  
   - how different modalities are encoded and combined  
   - how LLMs act as the “core” of many multimodal systems  

The important thing is to keep the **conceptual map** in mind:

> backpropagation is the training engine,  
> transformers are the dominant modern architecture,  
> LLMs are large transformer models trained on huge text (and code) corpora,  
> multimodal models extend them to multiple data types,  
> and classic methods + rules still play supporting roles in real systems.