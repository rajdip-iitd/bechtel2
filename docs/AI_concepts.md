# AI Concepts Worth Knowing

> A practical reference for anyone who uses, builds with, or makes decisions around AI systems.
> Concepts are grouped by what they help you *do*, not by how they were invented.

---

## How to use this page

Each concept below answers a real question. Start with the question, not the definition. If a concept solves a problem you have right now, that is the one to learn first.

---

## The Stack at a Glance

```
Tokens      →  how AI reads your input
Embeddings  →  how AI searches for meaning
Context     →  what the model can see right now
LLMs        →  the engine that generates text
RAG         →  connecting the model to your documents
Tools       →  letting the model take action
Agents      →  multi-step autonomous workflows
Memory      →  keeping state across sessions
Evals       →  testing whether any of this actually works
Guardrails  →  stopping the model from doing something harmful or expensive
```

---

## Part 1: How Models Read and Understand

### 1. Tokenization

AI does not read text the way you do. It breaks input into small pieces called **tokens**. A token can be a full word, a fragment of a word, punctuation, or a space.

This matters because:
- Token count controls cost and speed in API-based systems
- Long documents consume more of the model's available memory
- Rare words or technical terms may be split into many tokens, which can hurt accuracy

**What to do with this:** Before sending a long document to a model, clean and trim it. Ask it to read only the relevant section. Do not paste 90 pages when you need 3.

---

### 2. Embeddings

An embedding is a list of numbers that represents the *meaning* of a word, sentence, or document. Words and phrases that mean similar things end up as similar numbers, even if the exact words differ.

This is why searching *"payment hold"* can surface documents about *"refund delays"* — the meanings are close, so the numbers are close.

**What to do with this:** Give your documents clear, descriptive titles before indexing them. A file named `notes-final-v3.pdf` is harder for the system to use than `Refund Policy - Customers - 2026.pdf`.

---

### 3. Attention

**The question it answers:** How does the model know that "bank" means a river edge in one sentence and a financial institution in another?

Attention is a mechanism that lets the model look at surrounding words to understand what each word means in context. It does not read left to right like a person skimming. It weighs every word against every other word simultaneously.

**What to do with this:** Put your most important instruction at the top of your prompt. Give context in the middle. Repeat the critical rule near the end. The model pays more attention to what you make prominent.

---

### 4. Transformers

**The question it answers:** Why do GPT, Claude, Gemini, Llama, and Mistral all feel similar at a basic level?

Transformers are the underlying architecture that most modern language models share. They use attention (see above) to process language far more efficiently than earlier systems like recurrent neural networks.

**What to do with this:** Stop chasing model names. The architecture is similar across providers. Learn the system around the model — prompting, context, retrieval, and evaluation — because that is where most of the practical value lives.

---

### 5. Large Language Models (LLMs)

**The question it answers:** What is an LLM actually doing when it responds to me?

A large language model (LLM) is trained on a massive volume of text and code. Its core task is simple: predict the next token. At scale, that ability becomes writing, summarizing, translating, explaining, reasoning, and coding.

**What to do with this:** Treat an LLM as a capable assistant, not a verified database. It is very good at generating plausible-sounding text. That is also its main failure mode.

---

### 6. Context Window

**The question it answers:** Why did the model forget something I told it earlier in the conversation?

The context window is the total amount of text the model can hold in memory at one time: your prompt, the conversation history, any uploaded files, and the model's own response. When that window fills up, older content falls out.

**What to do with this:** Give the model one clean task at a time. If working with a long document, send one section per request and build up the answer incrementally.

---

## Part 2: Getting Better Outputs

### 7. Temperature

**The question it answers:** Why does the model give a different answer every time I ask the same question?

Temperature is a setting that controls how predictable or varied the model's outputs are. Low temperature produces more consistent, conservative answers. Higher temperature produces more creative, varied answers — and more chances of errors.

| Use case | Recommended temperature |
|---|---|
| Summarizing a document | Low |
| Writing code | Low |
| Extracting structured data | Low |
| Brainstorming titles | Medium to high |
| Creative writing | Medium to high |

**What to do with this:** For serious work, ask for the safest accurate answer first, then ask for creative variations separately.

---

### 8. Hallucination

**The question it answers:** Why did the model confidently cite a paper that does not exist?

Hallucination is when a model generates a wrong answer with high confidence. This happens because the model is optimized to produce *plausible* text, not *verified* text. It does not automatically check whether a claim is true.

This is most dangerous in domains involving law, health, money, technical specifications, and academic sources.

**What to do with this:** Force the model to show its work. Ask it to cite the source, quote the exact line, state its confidence level, and identify what remains uncertain. If it cannot do all four, treat the answer with caution.

---

### 9. Prompt Engineering

**The question it answers:** Why does a small change in how I phrase something produce a completely different result?

Prompt engineering is not magic wording. It is clear communication. The model has no context except what you give it, so the quality of your input directly shapes the quality of its output.

A well-structured prompt includes:
- **Role:** who the model should act as
- **Context:** what the situation is
- **Task:** what you want it to do
- **Format:** how you want the answer structured
- **Examples:** what good looks like
- **Rules:** what to avoid or always include

---

### 10. Few-Shot Prompting

**The question it answers:** How do I get the model to match a specific tone or format without writing a long explanation?

Few-shot prompting means providing two or three examples of the output you want before asking for the real thing. Examples beat instructions. The model pattern-matches against what you show it.

```
Here are two examples of the style I want:

Example 1: [paste]
Example 2: [paste]

Now write a third one on: [your topic]
```

---

### 11. Context Engineering

**The question it answers:** My prompt looks fine but the model still gives bad answers. What am I missing?

Context engineering is the practice of deliberately shaping everything the model sees: the prompt, the examples, the retrieved documents, the rules, the output format, and any tool descriptions. A model is only as good as the context it operates in.

Before blaming the model, check the context. Most failures come from missing, noisy, or contradictory context rather than from the model itself.

---

## Part 3: Connecting Models to Your Data

### 12. Retrieval-Augmented Generation (RAG)

**The question it answers:** How do I get the model to answer from my own documents instead of making things up?

Retrieval-Augmented Generation (RAG) is a pattern where the system searches your document collection first, retrieves the most relevant chunks, and feeds them to the model before asking it to answer. The model then generates a response grounded in actual source material.

```
User question
    → Search documents
    → Retrieve best-matching chunks
    → Feed chunks to model
    → Model answers with sources
```

**What to do with this:** Build a RAG system before considering fine-tuning. In most real-world scenarios, giving the model access to the right documents produces better results than retraining.

---

### 13. Vector Databases

**The question it answers:** My knowledge base has thousands of documents. How does the system find the right one quickly?

A vector database stores embeddings and retrieves the most semantically similar entries to a query. Unlike keyword search, it finds documents based on *meaning*, not exact word matches.

For best results, use hybrid search: keyword search for exact matches (product codes, names, dates) and vector search for meaning (policy intent, topic similarity).

---

### 14. Chunking

**The question it answers:** I uploaded my entire policy manual but the answers are still wrong. Why?

Chunking is the process of splitting documents into smaller pieces before indexing them. A retrieval system can only return what it finds, and if a chunk mixes unrelated content, the model gets confused context.

**A good chunk** holds one clear, self-contained idea. It includes a title and source reference.

**A bad chunk** is a page boundary slice that cuts mid-sentence, mixes tables with body text, or includes headers and footers from the document layout.

---

### 15. Metadata

**The question it answers:** How does the system know whether a document is current or outdated?

Metadata is structured information attached to each chunk: source file, date, author, department, region, version, document type. This lets the retrieval system filter results before returning them to the model.

```json
{
  "source": "safety_procedures_2026",
  "department": "HSE",
  "region": "South Asia",
  "updated": "2026-01-15"
}
```

Add metadata before uploading documents, not after.

---

### 16. Structured Outputs

**The question it answers:** The model gives me a good answer in paragraph form, but I need it as a table or JSON for my workflow. What do I do?

Structured outputs are a way of constraining the model to respond in a fixed format: JSON, a table, a checklist, or a defined schema. This is essential when the output feeds into another system automatically.

If a downstream system will use the model's answer, never rely on free text. Define the schema you need and ask the model to follow it.

---

## Part 4: Making Models Act

### 17. Tools and Function Calling

**The question it answers:** Can the model actually look things up, run calculations, or update a database?

Tools (also called function calling) let a language model invoke external systems: a search engine, a calculator, a database query, a calendar API, or a code runner. The model decides when to use a tool, formats the request, receives the result, and incorporates it into its response.

**What to do with this:** Keep tools narrow. Do not give an agent full database access. Start with one read-only tool and expand scope only when you trust the outputs.

---

### 18. Model Context Protocol (MCP)

**The question it answers:** Every AI tool seems to have its own way of connecting to external data. Is there a standard?

The Model Context Protocol (MCP) is an open standard for connecting AI applications to external data sources, tools, and workflows in a consistent way. It allows AI systems to interact with files, databases, APIs, and local services without requiring custom integration code for each one.

Treat every MCP connection as a permission grant. Only connect what you trust, and review what each connection can access before enabling it.

---

### 19. AI Agents

**The question it answers:** Can AI complete a multi-step task on its own, or does it need a human to approve each step?

An AI agent is a system that can plan a sequence of actions, use tools to execute them, observe the results, and decide what to do next, all in pursuit of a defined goal. It is the difference between a model that answers a question and one that completes a task.

Agents are useful for research workflows, code review pipelines, inbox triage, data cleanup, and report generation.

**What to do with this:** Start small. Define the goal precisely, limit the tools available, and set explicit stop conditions. Without stop conditions, agents waste resources and repeat themselves.

---

### 20. Agent Loops

**The question it answers:** How does an agent actually keep going until the task is done?

An agent loop is the cycle an agent follows: **Think. Act. Observe. Repeat.** The model decides on an action, executes it via a tool, observes the result, and decides the next action. This continues until a stop condition is met.

Every agent loop needs explicit stop conditions:
- Task is complete
- Maximum tool calls reached
- Result is uncertain and needs human review
- Time or cost limit hit
- An action outside the permitted scope is required

Without these, the agent will loop indefinitely or take actions you did not intend.

---

## Part 5: Making It Work Over Time

### 21. Memory

**The question it answers:** Why does the model not remember what I told it last session?

By default, language models have no memory between conversations. Memory is a system built on top of the model that stores relevant information and injects it back into the context at the start of each session.

**Good memory stores** stable, reusable facts: preferences, project context, recurring decisions.

**Bad memory stores** transient states: emotions, one-off instructions, things that change frequently.

Memory should be editable. You should be able to view it, correct it, and delete entries that are no longer accurate.

---

### 22. Fine-Tuning, LoRA, and Quantization

**The question it answers:** When prompting is not enough, what are my other options?

These three techniques adapt or compress a model for a specific use case:

**Fine-tuning** retrains the model on domain-specific examples, teaching it a particular style, vocabulary, or task pattern.

**LoRA (Low-Rank Adaptation)** adds lightweight trainable layers to a frozen model instead of retraining everything. This dramatically reduces the compute and memory required.

**Quantization** reduces the numerical precision of model weights (for example, from 32-bit floats to 4-bit integers). This makes models smaller and faster to run on local or cheaper hardware, with some tradeoff in accuracy.

Before reaching for any of these, work through this sequence:

1. Better prompt
2. Better examples (few-shot)
3. Better context
4. RAG with high-quality documents
5. Fine-tuning
6. LoRA
7. Quantized local model

Most real-world problems are solved at step 1 through 4.

---

### 23. Evals (Evaluations)

**The question it answers:** How do I know whether my AI system is actually working correctly?

Evals are structured tests for AI outputs. You define what a correct answer looks like, run the model against a set of test cases, and measure how often it meets that standard.

A basic eval set for a document-answering system might check:

1. Does it answer from the source documents?
2. Does it say "I don't know" when the answer is not in the documents?
3. Does it follow the output format you specified?
4. Does it avoid leaking private or irrelevant information?
5. Does it give consistent answers across multiple runs?

Build evals before deploying any AI system to real users.

---

### 24. Guardrails / Hooks

**The question it answers:** How do I stop the model from doing something harmful, expensive, or embarrassing?

Guardrails (also called hooks) are rules, filters, and checks that constrain what a model can say or do. They can be applied at the prompt level (explicit instructions), the output level (filtering or classifying responses before they are returned), or the tool level (restricting which actions an agent is allowed to take).

Guardrails are especially important for:
- Systems that interact with customers or the public
- Agents that can take real-world actions (sending emails, querying databases, making purchases)
- Applications in regulated domains (legal, medical, financial)

---

## Quick Reference

| Concept | What it does | When to reach for it |
|---|---|---|
| Tokenization | Breaks input into pieces | Debugging cost or strange outputs |
| Embeddings | Converts meaning into numbers | Building search or recommendations |
| Attention | Weights context around each word | Understanding why phrasing matters |
| Transformers | Core architecture of modern LLMs | Understanding model families |
| LLMs | Generate text by predicting next token | Most language tasks |
| Context window | Working memory for a single session | Long documents or long conversations |
| Temperature | Controls output variability | Tuning creativity vs. accuracy |
| Hallucination | Confident wrong answers | Any high-stakes output |
| Prompt engineering | Structured model communication | Improving any output |
| Few-shot prompting | Teaching by example | Matching a style or format |
| Context engineering | Shaping everything the model sees | Complex workflows |
| RAG | Grounding answers in documents | Company knowledge bases |
| Vector databases | Semantic search at scale | Large document collections |
| Chunking | Splitting documents for retrieval | Building a knowledge base |
| Metadata | Labels that improve search | Document-heavy systems |
| Structured outputs | Fixed-format responses | Automation and workflows |
| Tools / Function calling | Model calls external systems | When answers need action |
| MCP | Standard for connecting AI to tools | Multi-system integrations |
| AI Agents | Multi-step autonomous workflows | Repetitive or research tasks |
| Agent loops | Think-act-observe cycle | Any autonomous system |
| Memory | Persistence across sessions | Long-running projects or assistants |
| Fine-tuning / LoRA | Adapting a model to a domain | When prompting plateaus |
| Quantization | Smaller, faster local models | Running models on local hardware |
| Evals | Testing AI outputs systematically | Before any real deployment |
| Guardrails | Constraining unsafe or wrong actions | Public-facing or agentic systems |

---

## The Underlying Idea

AI is not one thing. It is a stack of components, each doing a specific job.

Once you can name the parts, you can diagnose what is broken, ask better questions, make better decisions about what to build, and evaluate whether a vendor or tool is actually solving your problem or just using the right vocabulary.

The gap between *using AI* and *understanding AI* is smaller than it looks. This page is one way across it.

---

*Last updated: June 2026*
