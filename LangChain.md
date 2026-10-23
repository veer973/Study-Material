# Complete Master Notes: Generative AI, LangChain, RAG & AI Agents

A comprehensive, production-grade guide covering the foundational principles, architectural patterns, and code implementations across **Generative AI with LangChain**, **Retrieval-Augmented Generation (RAG)**, and **Autonomous AI Agents**.

---

## Table of Contents

- [Section 1: Generative AI & LangChain Foundations](#section-1-generative-ai--langchain-foundations)
  - [1.1 Fundamentals of GenAI & Large Language Models](#11-fundamentals-of-genai--large-language-models)
  - [1.2 Introduction to LangChain & Core Ecosystem](#12-introduction-to-langchain--core-ecosystem)
  - [1.3 Component 1: Models (LLMs, Chat Models & Embeddings)](#13-component-1-models-llms-chat-models--embeddings)
  - [1.4 Mathematical Foundations: Vector Spaces & Cosine Similarity](#14-mathematical-foundations-vector-spaces--cosine-similarity)
  - [1.5 Component 2: Prompts, Messages & Memory](#15-component-2-prompts-messages--memory)
  - [1.6 Component 3: Chains & LangChain Expression Language (LCEL)](#16-component-3-chains--langchain-expression-language-lcel)
  - [1.7 Deep Dive into Runnables & Execution Primitives](#17-deep-dive-into-runnables--execution-primitives)
  - [1.8 Structured Output: Parsers vs. Native Schema Enforcement](#18-structured-output-parsers-vs-native-schema-enforcement)
- [Section 2: Retrieval-Augmented Generation (RAG)](#section-2-retrieval-augmented-generation-rag)
  - [2.1 Why RAG? (Parametric Knowledge vs. External Context)](#21-why-rag-parametric-knowledge-vs-external-context)
  - [2.2 Fine-Tuning vs. In-Context Learning vs. RAG](#22-fine-tuning-vs-in-context-learning-vs-rag)
  - [2.3 The 4-Stage Complete RAG Pipeline](#23-the-4-stage-complete-rag-pipeline)
  - [2.4 Component 1: Document Loaders (Eager vs. Lazy Loading)](#24-component-1-document-loaders-eager-vs-lazy-loading)
  - [2.5 Component 2: Text Chunking & Splitting Strategies](#25-component-2-text-chunking--splitting-strategies)
  - [2.6 Component 3: Vector Stores vs. Vector Databases (ChromaDB & FAISS)](#26-component-3-vector-stores-vs-vector-databases-chromadb--faiss)
  - [2.7 Component 4: Retrievers & Advanced Retrieval Strategies](#27-component-4-retrievers--advanced-retrieval-strategies)
  - [2.8 End-to-End Application: YouTube Transcript RAG Chatbot](#28-end-to-end-application-youtube-transcript-rag-chatbot)
  - [2.9 Production-Ready Enterprise RAG Architecture (Multimodal PDFs)](#29-production-ready-enterprise-rag-architecture-multimodal-pdfs)
- [Section 3: Building Autonomous AI Agents Using LangChain](#section-3-building-autonomous-ai-agents-using-langchain)
  - [3.1 What is an AI Agent? Core Characteristics](#31-what-is-an-ai-agent-core-characteristics)
  - [3.2 The ReAct Framework (Reasoning + Acting)](#32-the-react-framework-reasoning--acting)
  - [3.3 Built-in Tools, Toolkits & Custom Tool Development](#33-built-in-tools-toolkits--custom-tool-development)
  - [3.4 Tool Binding, Calling, Execution & Injected Arguments](#34-tool-binding-calling-execution--injected-arguments)
  - [3.5 Agent Execution Loop: Internal Mechanics](#35-agent-execution-loop-internal-mechanics)
  - [3.6 Architectural Evolution: AgentExecutor vs. LangGraph](#36-architectural-evolution-agentexecutor-vs-langgraph)

---

# Section 1: Generative AI & LangChain Foundations

```
                            LANGCHAIN ECOSYSTEM
 ┌───────────────────────────────────────────────────────────────────────┐
 │                                                                       │
 │    ┌─────────────┐       ┌──────────────┐       ┌──────────────┐      │
 │    │   Models    │ ----> │   Prompts    │ ----> │    Chains    │      │
 │    │ (LLM / Chat)│       │ (Static/Dyn) │       │ (LCEL Pipes) │      │
 │    └─────────────┘       └──────────────┘       └──────────────┘      │
 │           │                     │                       │             │
 │           ▼                     ▼                       ▼             │
 │    ┌─────────────┐       ┌──────────────┐       ┌──────────────┐      │
 │    │   Memory    │       │   Indexes    │       │    Agents    │      │
 │    │ (State/Hist)│       │(Load/Split/DB)       │(Tools/ReAct) │      │
 │    └─────────────┘       └──────────────┘       └──────────────┘      │
 │                                                                       │
 └───────────────────────────────────────────────────────────────────────┘
```

---

## 1.1 Fundamentals of GenAI & Large Language Models

### What is Generative AI?
**Generative AI (GenAI)** refers to deep learning systems capable of generating new, synthetic content—such as text, images, audio, synthetic data, or code—by identifying, modeling, and reproducing complex statistical patterns from vast training corpora, effectively mimicking human creativity.

### What is a Large Language Model (LLM)?
A **Large Language Model (LLM)** is a neural network architecture (predominantly based on the **Transformer**) trained on web-scale textual datasets comprising hundreds of billions to trillions of tokens. It reads, interprets, reasons over, and generates natural language text.

### How an LLM Processes Information Under the Hood
When a user transmits a text prompt to an LLM:

1. **Tokenization**:
   The input string is segmented into discrete subword units called **tokens** using algorithms such as Byte-Pair Encoding (BPE) or WordPiece. Each unique token corresponds to an integer ID in the model's vocabulary matrix.
2. **Embedding Lookup**:
   Token IDs are mapped to dense, high-dimensional vector representations ($d \in [768, 4096+]$) via a learned embedding matrix. These vectors capture initial semantic and positional values (positional encodings).
3. **Transformer Layers & Self-Attention**:
   The embedded vectors propagate through consecutive Transformer blocks composed of **Multi-Head Self-Attention** and **Feed-Forward Networks (FFN)**. 
   - **Self-attention** dynamically computes weight matrices between every token and every other token in the sequence.
   - *Example*: Consider the homonym **"bank"**:
     - *"The bank is on the river."* $\rightarrow$ The self-attention mechanism correlates *"bank"* strongly with *"river"*, attending to the meaning of a geographical riverbank.
     - *"I deposited money in the bank."* $\rightarrow$ The attention weights link *"bank"* to *"deposited"* and *"money"*, resolving the meaning to a financial institution.
4. **Parametric Knowledge vs. Database Search**:
   An LLM does **not** query a database at inference time. All facts, syntactic rules, reasoning behaviors, and conceptual associations are encoded directly into billions of numerical parameters (weights) optimized during pretraining.
5. **Next-Token Prediction & Decoding**:
   The final layer computes a logit score across the entire vocabulary, converting it into a normalized probability distribution via **Softmax**:
   $$P(w_i) = \frac{e^{z_i}}{\sum_j e^{z_j}}$$
   The decoding strategy selects the next token sequentially (autoregressively) until a special stop token (`<|endoftext|>`) is produced.

### Controlling Creativity: Temperature
- **Low Temperature ($0.0 - 0.3$)**: Sharpens the probability distribution. The model deterministically selects the top token. Ideal for mathematical calculation, factual extraction, and code generation.
- **Moderate Temperature ($0.5 - 0.7$)**: Balanced distribution. Ideal for conversational assistants, summarization, and analysis.
- **High Temperature ($0.8 - 1.5+$)**: Flattens the distribution, giving lower-probability tokens higher selection chances. Produces creative writing. At extreme values ($>1.5$), it induces hallucinations and disjointed output.

---

## 1.2 Introduction to LangChain & Core Ecosystem

### What is LangChain?
**LangChain** is an open-source framework designed to simplify the construction of end-to-end applications powered by Large Language Models. Rather than managing raw HTTP payloads, socket streams, and string parsing manually, LangChain provides a standardized interface for chaining models, prompts, data loaders, vector stores, memory engines, and tools.

### Core Advantages
1. **Chains & Composition**: Create modular execution pipelines where the output of one step seamlessly feeds into the next.
2. **Model-Agnostic Interfaces**: Switch between model providers (OpenAI, Anthropic, Google Gemini, local HuggingFace) with minimal code alterations.
3. **Memory & State Handling**: Carry dialogue context across multi-turn interactions.
4. **Extensive Ecosystem**: Over 700+ native integrations with vector databases, tools, APIs, and document loaders.

### The 6 Core Modules of LangChain
1. **Models**: Unified interfaces for language models (LLMs, Chat Models) and embedding generators.
2. **Prompts**: Parameterized templating, few-shot prompt construction, and message serialization.
3. **Chains**: Sequential and parallel workflows that orchestrate multiple primitives.
4. **Memory**: Persistence abstractions for recording conversational turns.
5. **Indexes**: Ingestion, chunking, and retrieval of external unstructured knowledge (Document Loaders, Text Splitters, Vector Stores, Retrievers).
6. **Agents**: Autonomous decision-making engines utilizing LLMs to dynamically select and invoke external tools.

---

## 1.3 Component 1: Models (LLMs, Chat Models & Embeddings)

```
                            LANGUAGE MODELS
                                   │
                  ┌────────────────┴────────────────┐
                  ▼                                 ▼
             Base LLMs                         Chat Models
       (Text-In / Text-Out)            (Message-In / Message-Out)
       e.g., davinci-002,              e.g., gpt-4o, claude-3-5-sonnet,
       text-davinci-003                gemini-1.5-pro
```

### Base LLMs vs. Chat Models

| Feature | Base LLM (`llm = OpenAI()`) | Chat Model (`model = ChatOpenAI()`) |
| :--- | :--- | :--- |
| **Input / Output Format** | Raw string in $\rightarrow$ Raw string out | Sequence of structured Message objects in $\rightarrow$ `AIMessage` out |
| **Role Awareness** | No native concept of speakers; acts as a raw autocomplete engine | Explicitly understands roles: `SystemMessage`, `HumanMessage`, `AIMessage` |
| **Instruction Tuning** | Prone to running off topic or completing prompts rather than answering | Fine-tuned with RLHF to follow instructions, maintain persona, and obey guardrails |
| **Multi-Turn Context** | Requires manual string concatenation of past transcripts | Native list structure representing conversational history |

### Open Source vs. Closed API Models
1. **Closed API Models** (OpenAI GPT-4o, Anthropic Claude, Google Gemini): Hosted remotely; accessed via authenticated REST APIs. Zero local hardware requirements, high reasoning ability, but recurring operational costs and third-party data transmission.
2. **Open Source Models** (Llama 3, Mistral, TinyLlama, Qwen): Weights are publicly accessible. Can be deployed on private infrastructure for total data privacy, zero API rate limits, and custom domain fine-tuning.

#### Local Execution with Hugging Face & PyTorch
To run an open-source model completely offline:
- **`AutoTokenizer`**: Maps text into token IDs based on the model's exact tokenizer configuration.
- **`AutoModelForCausalLM`**: Loads the pre-trained weights into PyTorch tensors. Parameter `device_map="auto"` offloads layers across available GPUs and RAM.
- **`pipeline`**: High-level Hugging Face inference pipeline combining tokenization, tensor forward-pass, and text decoding.
- **`HuggingFacePipeline`** & **`ChatHuggingFace`**: LangChain wrappers adapting local Hugging Face pipelines to standard LangChain Runnable interfaces.

```python
from transformers import AutoTokenizer, AutoModelForCausalLM, pipeline
from langchain_huggingface import HuggingFacePipeline, ChatHuggingFace

model_path = r"D:\Hugging Face Model\Tiny Lama_Text Generation"
tokenizer = AutoTokenizer.from_pretrained(model_path)
model = AutoModelForCausalLM.from_pretrained(model_path, device_map="auto")

pipe = pipeline(
    task="text-generation",
    model=model,
    tokenizer=tokenizer,
    temperature=0.7,
    max_new_tokens=256
)

llm = HuggingFacePipeline(pipeline=pipe)
chat_model = ChatHuggingFace(llm=llm)
```

---

## 1.4 Mathematical Foundations: Vector Spaces & Cosine Similarity

### What is an Embedding?
An embedding model maps discrete text chunks into a continuous, dense vector space $\mathbb{R}^d$:
$$f: \text{Text} \longrightarrow [x_1, x_2, \dots, x_d]$$
Unlike sparse lexical representations (such as One-Hot encoding or TF-IDF), dense embeddings project words with shared semantic context into proximate geometric locations.

### Mathematical Definition of Cosine Similarity
Given two dense embedding vectors $\vec{u}$ and $\vec{v}$:

$$\text{Cosine Similarity}(\vec{u}, \vec{v}) = \frac{\vec{u} \cdot \vec{v}}{\|\vec{u}\| \|\vec{v}\|} = \frac{\sum_{i=1}^d u_i v_i}{\sqrt{\sum_{i=1}^d u_i^2} \sqrt{\sum_{i=1}^d v_i^2}}$$

- **Value Range**: $[-1, 1]$ (or $[0, 1]$ for non-negative embeddings).
- **Geometric Meaning**: Measures the cosine of the angle between two vectors, reflecting orientation similarity rather than magnitude differences.

### Manual Implementation: Semantic Document Search
```python
from langchain_openai import OpenAIEmbeddings
from sklearn.metrics.pairwise import cosine_similarity
import numpy as np

embedding = OpenAIEmbeddings(model="text-embedding-3-large", dimensions=300)

documents = [
    "Virat Kohli is an Indian cricketer known for his aggressive batting and leadership.",
    "MS Dhoni is a former Indian captain famous for his calm demeanor and finishing skills.",
    "Sachin Tendulkar, also known as the 'God of Cricket', holds many batting records.",
    "Rohit Sharma is known for his elegant batting and record-breaking double centuries."
]

query = "Tell me about Virat Kohli"

# 1. Vectorize documents and query
doc_embeddings = embedding.embed_documents(documents)
query_embedding = embedding.embed_query(query)

# 2. Compute pairwise cosine similarity
scores = cosine_similarity([query_embedding], doc_embeddings)[0]

# 3. Retrieve highest scoring document index
best_index, highest_score = sorted(list(enumerate(scores)), key=lambda x: x[1])[-1]

print(f"Match: {documents[best_index]} (Score: {highest_score:.4f})")
```

---

## 1.5 Component 2: Prompts, Messages & Memory

```
                           PROMPT EXECUTION
                                  │
                               invoke()
                                  │
             ┌────────────────────┴────────────────────┐
             ▼                                         ▼
       Single Message                           List of Messages
     (Single-turn query)                       (Multi-turn chat)
             │                                         │
      ┌──────┴──────┐                           ┌──────┴──────┐
      ▼             ▼                           ▼             ▼
   Static        Dynamic                     Static        Dynamic
   String    PromptTemplate                 Messages  ChatPromptTemplate
                                            - System
                                            - Human
                                            - AI
```

### Prompt Templating
Hardcoding strings creates fragile code. LangChain offers dynamic parameterization:

1. **`PromptTemplate` (Single-String Variables)**:
   ```python
   from langchain_core.prompts import PromptTemplate
   template = PromptTemplate(
       template="Explain {concept} to a {audience} in {tone} tone.",
       input_variables=["concept", "audience", "tone"]
   )
   formatted = template.format(concept="Quantum Computing", audience="5-year-old", tone="playful")
   ```

2. **`ChatPromptTemplate` (Role-Based Structural Templates)**:
   ```python
   from langchain_core.prompts import ChatPromptTemplate
   chat_template = ChatPromptTemplate.from_messages([
       ("system", "You are an expert diagnostic assistant specializing in {domain}."),
       ("human", "My symptoms are: {symptoms}")
   ])
   messages = chat_template.format_messages(domain="Cardiology", symptoms="Chest pain, dizziness")
   ```

3. **`FewShotPromptTemplate` (In-Context Learning Demonstration)**:
   Feeds demonstration exemplars directly into the prompt to guide reasoning or output taxonomy without updating model weights.

### The Message Hierarchy
- **`SystemMessage`**: Informs the model about its role, behavior, output format, and ethical boundaries.
- **`HumanMessage`**: Represents user prompts or inputs.
- **`AIMessage`**: Contains responses generated by the language model.
- **`ToolMessage`**: Transmits raw outputs from executed external functions back to the LLM.

### Dynamic History with `MessagesPlaceholder`
A `MessagesPlaceholder` acts as a dynamic structural anchor within a `ChatPromptTemplate`. It does not store state; rather, it reserves a spot in the prompt template where an arbitrary list of past `HumanMessage` and `AIMessage` objects can be injected at runtime.

```python
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder

chat_template = ChatPromptTemplate.from_messages([
    ("system", "You are a customer service representative."),
    MessagesPlaceholder(variable_name="chat_history"),
    ("human", "{query}")
])

# At runtime, inject conversation history dynamically
final_prompt = chat_template.invoke({
    "chat_history": [
        HumanMessage(content="My order number is #4912"),
        AIMessage(content="Thank you. How can I help with order #4912?")
    ],
    "query": "Where is it right now?"
})
```

---

## 1.6 Component 3: Chains & LangChain Expression Language (LCEL)

### From Legacy Chains to LCEL
Historically, LangChain used specialized classes such as `LLMChain`, `SequentialChain`, and `RetrievalQA`. These suffered from rigid interfaces, hidden execution loops, and poor composability.

Modern LangChain uses **LangChain Expression Language (LCEL)**, standardizing all components around the Unix-inspired **pipe operator (`|`)**:

$$\text{Input} \longrightarrow \text{Prompt} \mid \text{Model} \mid \text{OutputParser} \longrightarrow \text{Output}$$

```python
from langchain_openai import ChatOpenAI
from langchain_core.prompts import PromptTemplate
from langchain_core.output_parsers import StrOutputParser

prompt = PromptTemplate.from_template("Translate the following into French: {text}")
model = ChatOpenAI(model="gpt-4o-mini")
parser = StrOutputParser()

# Declarative, production-ready chain
chain = prompt | model | parser
response = chain.invoke({"text": "Hello, how are you?"})
```

---

## 1.7 Deep Dive into Runnables & Execution Primitives

A **Runnable** is the atomic unit of execution in LangChain. Any object implementing the Runnable protocol guarantees three core execution methods:
- **`.invoke(input)`**: Synchronous execution on a single input.
- **`.batch([inputs])`**: Concurrent, multi-threaded execution across an iterable of inputs.
- **`.stream(input)`**: Incremental output streaming chunk-by-chunk.

### Runnable Primitives Breakdown

```
                            RUNNABLE PRIMITIVES
 ┌───────────────────────────────────────────────────────────────────────┐
 │                                                                       │
 │  RunnableSequence  ──> r1 | r2 | r3 (Linear pipelining)              │
 │                                                                       │
 │  RunnableParallel  ──> { "branch1": r1, "branch2": r2 } (Concurrent) │
 │                                                                       │
 │  RunnablePassthrough -> Passes input data downstream untouched        │
 │                                                                       │
 │  RunnableLambda    ──> Wraps any custom Python function as a Runnable │
 │                                                                       │
 │  RunnableBranch    ──> Conditional if-else branching logic           │
 │                                                                       │
 └───────────────────────────────────────────────────────────────────────┘
```

1. **`RunnableSequence`**:
   Chains components sequentially. Equivalent to `r1 | r2 | r3`. The output of $r_n$ becomes the input to $r_{n+1}$.
2. **`RunnableParallel` / `RunnableMap`**:
   Accepts a single input dictionary, replicates it across multiple independent branches simultaneously, and aggregates their results into a unified output dictionary.
3. **`RunnablePassthrough`**:
   Allows original input keys to flow untouched alongside processed branches. Vital in RAG pipelines where the user's raw question must be preserved alongside retrieved context chunks.
4. **`RunnableLambda`**:
   Transforms standard Python functions into first-class LCEL Runnables:
   ```python
   from langchain_core.runnables import RunnableLambda
   clean_text = RunnableLambda(lambda text: text.strip().lower())
   ```
5. **`RunnableBranch`**:
   Evaluates a sequence of `(condition_callable, runnable)` pairs, executing the branch corresponding to the first truthy condition, or falling back to a default runnable.

---

## 1.8 Structured Output: Parsers vs. Native Schema Enforcement

Software applications require deterministic, typed, machine-readable structures (e.g., JSON), not conversational prose.

```
                          STRUCTURED OUTPUT
                                  │
          ┌───────────────────────┴───────────────────────┐
          ▼                                               ▼
  Native Schema Binding                          Output Parsers
(LLMs with Tool Calling / JSON Mode)            (Universal Text Parsers)
  - Pydantic BaseModel                           - StrOutputParser
  - TypedDict                                    - JsonOutputParser
  - JSON Schema                                  - PydanticOutputParser
```

### Approach 1: Native `.with_structured_output()` (Modern Standard)
Modern instruction-tuned models support Function Calling / Tool Calling schemas directly at the API level:

```python
from pydantic import BaseModel, Field

class StudentProfile(BaseModel):
    name: str = Field(description="Student's legal name")
    cgpa: float = Field(gt=0, lt=10, description="Cumulative Grade Point Average")
    active: bool = Field(default=True)

llm = ChatOpenAI(model="gpt-4o")
structured_llm = llm.with_structured_output(StudentProfile)

# Returns a validated StudentProfile Pydantic object
student = structured_llm.invoke("Veeraj scored an 8.9 CGPA in Computer Science.")
```

#### Comparison: Pydantic vs. TypedDict vs. JSON Schema

| Schema Method | Type Enforcement | Runtime Data Validation | Default Values & Coercion | Primary Use Case |
| :--- | :--- | :--- | :--- | :--- |
| **`Pydantic`** | High | Yes (strict validation) | Supported | Production backend Python applications requiring strict schema guarantees. |
| **`TypedDict`** | Medium (type hints only)| No runtime validation | Not supported | Lightweight projects trusting the LLM where minimal overhead is required. |
| **`JSON Schema`** | High | Yes (via validator) | Dependent on engine | Polyglot or cross-platform APIs communicating outside Python. |

### Approach 2: Output Parsers (For Legacy / Open-Source LLMs)
For models lacking native function-calling interfaces, Output Parsers inject formatting instructions directly into the system prompt and parse the resulting text response via regex or AST parsing.

---

# Section 2: Retrieval-Augmented Generation (RAG)

```
                            THE COMPLETE RAG ARCHITECTURE
 ┌─────────────────────────────────────────────────────────────────────────────────┐
 │                                                                                 │
 │   [INDEXING - Offline]                                                          │
 │   Raw Documents (PDF, CSV, Web) ──> Text Splitters ──> Embeddings ──> Vector DB  │
 │                                                                                 │
 │   [RETRIEVAL - Online]                                                          │
 │   User Query ──> Query Embedding ──> Similarity / MMR Search ──> Top-K Chunks   │
 │                                                                                 │
 │   [AUGMENTATION]                                                                │
 │   Top-K Chunks + System Prompt + User Query ──> Grounded Augmented Prompt       │
 │                                                                                 │
 │   [GENERATION]                                                                  │
 │   Augmented Prompt ──> LLM ──> Factually Grounded Response with Citations      │
 │                                                                                 │
 └─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2.1 Why RAG? (Parametric Knowledge vs. External Context)

While LLMs demonstrate broad general reasoning, standalone LLMs suffer from fundamental limitations:
1. **Knowledge Cutoff Dates**: Inability to access events occurring after training completion.
2. **Ignorance of Private Enterprise Data**: Models lack access to internal corporate wikis, customer logs, or private documents.
3. **Hallucination**: Generating plausible-sounding yet factually invented citations and assertions.

**Retrieval-Augmented Generation (RAG)** overcomes these limitations by combining **information retrieval** with **text generation**. The LLM is transformed from an encyclopedic storehouse into a reasoning engine that operates over dynamically retrieved reference documents.

---

## 2.2 Fine-Tuning vs. In-Context Learning vs. RAG

```
                 COMPARING ADAPTATION PARADIGMS
 ┌─────────────────┬──────────────────────┬───────────────────────────────┐
 │ Paradigm        │ How Knowledge Enters │ Primary Trade-offs            │
 ├─────────────────┼──────────────────────┼───────────────────────────────┤
 │ Fine-Tuning     │ Updates model weights│ High compute cost; static;    │
 │ (SFT, RLHF)     │ via backpropagation  │ difficult to delete facts.    │
 ├─────────────────┼──────────────────────┼───────────────────────────────┤
 │ In-Context      │ Injected manually    │ Limited by context window;    │
 │ Learning (ICL)  │ inside prompt string │ manual document curation.     │
 ├─────────────────┼──────────────────────┼───────────────────────────────┤
 │ RAG             │ Dynamically retrieved│ Low compute cost; real-time;  │
 │                 │ from vector database │ strict citations & audit trail│
 └─────────────────┴──────────────────────┴───────────────────────────────┘
```

---

## 2.3 The 4-Stage Complete RAG Pipeline

1. **Stage 1: Indexing (Offline Preparation)**:
   Documents are loaded, cleaned, decomposed into semantic chunks, vectorized via embedding models, and written to persistent vector stores.
2. **Stage 2: Retrieval (Online Querying)**:
   The user query is embedded into the vector space. The system calculates distance metrics (cosine, dot product, L2) to retrieve the top-$k$ most semantically relevant chunks.
3. **Stage 3: Augmentation (Prompt Grounding)**:
   The retrieved text chunks are formatted into a context payload and injected into a strict prompt template instructing the model to answer *only* from the provided facts.
4. **Stage 4: Generation (Synthesis)**:
   The LLM ingests the grounded prompt and synthesizes a concise, attributable answer containing citations.

---

## 2.4 Component 1: Document Loaders (Eager vs. Lazy Loading)

Document loaders extract unstructured data from heterogeneous sources into standardized LangChain `Document` objects:
```python
Document(
    page_content="Textual content extracted from source...",
    metadata={"source": "annual_report.pdf", "page": 12, "author": "Finance"}
)
```

### Supported Loaders
- **`TextLoader`**: Plaintext `.txt` files.
- **`PyPDFLoader`**: Extracts text page-by-page from `.pdf` documents.
- **`CSVLoader`**: Loads tabular row data as discrete documents.
- **`DirectoryLoader`**: Iterates through entire filesystem trees using glob matching (`glob="**/*.pdf"`).
- **`WebBaseLoader`**: Uses BeautifulSoup to scrape live HTML pages.

### `load()` vs. `lazy_load()`

| Metric | `loader.load()` (Eager) | `loader.lazy_load()` (Lazy) |
| :--- | :--- | :--- |
| **Return Type** | `List[Document]` | `Iterator[Document]` (Generator) |
| **Memory Allocation**| Allocates all documents into RAM at once | Allocates one document at a time on demand |
| **Direct Indexing** | Supported (`docs[0]`, `len(docs)`) | Not supported (requires iterative traversal) |
| **Recommended Scale**| Small to medium collections ($< 100$ files)| Massive datasets, high-throughput streaming pipelines |

---

## 2.5 Component 2: Text Chunking & Splitting Strategies

LLMs have finite context limits, and embedding long texts degrades semantic specificity. Text splitting segments documents into coherent fragments.

### `split_text()` vs. `create_documents()`
- **`split_text(["string"])`**: Returns a raw `List[str]`. Loses all metadata attachments.
- **`create_documents(["string"])`**: Returns `List[Document]`, wrapping each chunk with `page_content` and metadata dicts. Essential for vector store ingestion.

### Chunking Strategies Breakdown
1. **Length-Based Splitting (`CharacterTextSplitter`)**: Splits strictly at fixed character boundaries. Often breaks words or sentences unnaturally.
2. **Recursive Character Splitting (`RecursiveCharacterTextSplitter`)**:
   Iteratively attempts splits along a hierarchy of natural separators:
   `["\n\n", "\n", " ", ""]`
   Preserves paragraph and sentence integrity before falling back to word or character splits.
3. **Document-Structure Splitting (`MarkdownHeaderTextSplitter`)**:
   Splits text along markdown headings (`#`, `##`, `###`), preserving document hierarchy in the metadata.
4. **Semantic Chunking (`SemanticChunker`)**:
   Calculates embedding vectors for consecutive sentences. When the cosine distance between neighboring sentences spikes past a statistical breakpoint threshold (e.g., standard deviation or percentile), a chunk boundary is placed. Chunks are defined by conceptual topic shifts rather than character counts.

```python
from langchain_experimental.text_splitter import SemanticChunker
from langchain_openai.embeddings import OpenAIEmbeddings

splitter = SemanticChunker(
    OpenAIEmbeddings(),
    breakpoint_threshold_type="standard_deviation",
    breakpoint_threshold_amount=0.5
)
chunks = splitter.create_documents([raw_text])
```

---

## 2.6 Component 3: Vector Stores vs. Vector Databases (ChromaDB & FAISS)

```
                       VECTOR STORE vs. VECTOR DATABASE
 ┌────────────────────────────────────┬────────────────────────────────────┐
 │ Vector Store (e.g., FAISS)         │ Vector Database (e.g., ChromaDB)   │
 ├────────────────────────────────────┼────────────────────────────────────┤
 │ In-memory index library            │ Full database engine               │
 │ Fast similarity search             │ Persistent CRUD operations         │
 │ No native persistence or metadata  │ Native metadata filtering & storage│
 │ Requires index + docstore + ID map │ Complete Tenant/DB/Collection tier │
 └────────────────────────────────────┴────────────────────────────────────┘
```

### ChromaDB Architecture & Lifecycle
ChromaDB organizes data in a 4-tier hierarchy:
$$\text{Tenant} \longrightarrow \text{Database} \longrightarrow \text{Collection} \longrightarrow \text{Documents}$$

- **Insertion**: Documents are embedded and written to persistent SQLite and HNSW binary files (`add_documents()`).
- **Metadata Filtering**: Exact key-value matching combined with vector search:
  ```python
  results = vectorstore.similarity_search(
      query="Who is the fast bowler?",
      filter={"team": "Mumbai Indians"},
      k=2
  )
  ```
- **Updates & Deletions**: Supports mutating document content and deleting by unique UUIDs (`vectorstore.delete(ids=[...])`).

---

## 2.7 Component 4: Retrievers & Advanced Retrieval Strategies

A **Retriever** is a LangChain Runnable that accepts a string query and returns a list of relevant `Document` objects. It abstracts the underlying storage mechanism.

```
                           ADVANCED RETRIEVERS
 ┌────────────────────────────────────────────────────────────────────────┐
 │ 1. Maximum Marginal Relevance (MMR)                                    │
 │    Balances relevance with diversity (eliminates duplicate chunks)     │
 │                                                                        │
 │ 2. Contextual Compression Retriever                                    │
 │    LLMChainExtractor strips irrelevant sentences from chunks           │
 │                                                                        │
 │ 3. Multi-Query Retriever                                               │
 │    LLM expands single query into 3-5 variations, returning union       │
 └────────────────────────────────────────────────────────────────────────┘
```

### 1. Maximum Marginal Relevance (MMR)
Standard cosine search often returns duplicate or near-identical text chunks. MMR balances query relevance with inter-document diversity:

$$\text{MMR} = \arg\max_{d_i \in R \setminus S} \left[ \lambda \cdot \text{Sim}(d_i, q) - (1 - \lambda) \max_{d_j \in S} \text{Sim}(d_i, d_j) \right]$$

- $\lambda = 1$: Standard similarity search.
- $\lambda = 0$: Maximal diversity.
- $\lambda = 0.5$: Balanced mix of relevance and diversity.

```python
retriever = vectorstore.as_retriever(
    search_type="mmr",
    search_kwargs={"k": 3, "lambda_mult": 0.5}
)
```

### 2. Contextual Compression Retriever
Retrieved chunks often contain only one or two relevant sentences surrounded by irrelevant filler. A **Contextual Compression Retriever** runs an `LLMChainExtractor` over each retrieved chunk, extracting only the relevant sentences before passing them to the final prompt. This reduces token consumption and context window clutter.

```python
from langchain_classic.retrievers import ContextualCompressionRetriever
from langchain_classic.retrievers.document_compressors import LLMChainExtractor

compressor = LLMChainExtractor.from_llm(llm=ChatOpenAI())
compression_retriever = ContextualCompressionRetriever(
    base_retriever=vectorstore.as_retriever(search_kwargs={"k": 5}),
    base_compressor=compressor
)
compressed_docs = compression_retriever.invoke("What is photosynthesis?")
```

### 3. Multi-Query Retriever
A user query may be ambiguously phrased. The **Multi-Query Retriever** uses an LLM to generate 3 to 5 semantically distinct variations of the original query, searches the vector store for each variation independently, and returns the deduplicated union of all matching documents.

```python
from langchain_classic.retrievers import MultiQueryRetriever

multi_retriever = MultiQueryRetriever.from_llm(
    retriever=vectorstore.as_retriever(search_kwargs={"k": 3}),
    llm=ChatOpenAI(model="gpt-4o-mini")
)
unique_docs = multi_retriever.invoke("How to maintain work-life balance?")
```

---

## 2.8 End-to-End Application: YouTube Transcript RAG Chatbot

An end-to-end implementation of an interactive video QA pipeline:

```python
from youtube_transcript_api import YouTubeTranscriptApi
from langchain_text_splitters import RecursiveCharacterTextSplitter
from langchain_openai import OpenAIEmbeddings, ChatOpenAI
from langchain_community.vectorstores import FAISS
from langchain_core.prompts import PromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnableParallel, RunnablePassthrough, RunnableLambda

# 1. Ingest YouTube Transcript
video_id = "Gfr50f6ZBvo"
transcript_data = YouTubeTranscriptApi().fetch(video_id, languages=["en"])
raw_text = "".join(chunk.text for chunk in transcript_data)

# 2. Text Chunking
splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
documents = splitter.create_documents([raw_text])

# 3. Vector Indexing
embeddings = OpenAIEmbeddings(model="text-embedding-3-small")
vectorstore = FAISS.from_documents(documents=documents, embedding=embeddings)
retriever = vectorstore.as_retriever(search_kwargs={"k": 4})

# 4. Context Formatter
def format_docs(docs):
    return "\n\n".join(doc.page_content for doc in docs)

# 5. LCEL RAG Assembly
prompt = PromptTemplate.from_template("""
You are a helpful assistant. Answer strictly based on the provided video context.
If the answer is not contained within the context, respond: "I cannot find that in the video."

Context:
{context}

Question: {question}
""")

chain = (
    RunnableParallel({
        "context": retriever | RunnableLambda(format_docs),
        "question": RunnablePassthrough()
    })
    | prompt
    | ChatOpenAI(model="gpt-4o-mini", temperature=0.2)
    | StrOutputParser()
)

print(chain.invoke("Summarize the main argument made in the video."))
```

---

## 2.9 Production-Ready Enterprise RAG Architecture (Multimodal PDFs)

```
                       ENTERPRISE MULTIMODAL RAG PIPELINE
 ┌────────────────────────────────────────────────────────────────────────┐
 │ 1. INGESTION & PARSING                                                 │
 │    - Native text: Direct extraction                                    │
 │    - Scanned text: OCR (AWS Textract / Tesseract / Document AI)        │
 │    - Tables: Camelot / Tabula -> Structured JSON / Markdown Tables     │
 │    - Charts / Images: Multimodal Vision (GPT-4o / Claude Vision)       │
 │                                                                        │
 │ 2. SMART CHUNKING & EMBEDDINGS                                         │
 │    - Table-aware & section-aware chunking                              │
 │    - Embedding with BGE-large, E5, or OpenAI text-embedding-3          │
 │    - Stored in enterprise vector DB (Pinecone, Qdrant, Milvus)         │
 │                                                                        │
 │ 3. HYBRID RETRIEVAL & RERANKING                                        │
 │    - Dense vector search (semantic) + Sparse BM25 (exact keyword/ID)   │
 │    - Cross-Encoder Reranker (Cohere Rerank / BGE-Reranker)             │
 │                                                                        │
 │ 4. GROUNDED GENERATION                                                 │
 │    - Augmented prompt with strict guardrails                           │
 │    - Generation with page-level citations and confidence scoring       │
 └────────────────────────────────────────────────────────────────────────┘
```

---

# Section 3: Building Autonomous AI Agents Using LangChain

```
                           THE REACT AGENT LOOP
 ┌───────────────────────────────────────────────────────────────────────┐
 │                                                                       │
 │   User Query                                                          │
 │       │                                                               │
 │       ▼                                                               │
 │   ┌───────────────┐     Thought: "What do I need to do?"              │
 │   │               │ ──> Action: Select tool (e.g., Search, Calculator)│
 │   │  LLM Engine   │     Action Input: Arguments for tool              │
 │   │  (Reasoning)  │                                                   │
 │   │               │ <── Observation: Tool execution output            │
 │   └───────────────┘                                                   │
 │       │                                                               │
 │   [Loop repeats until goal is achieved]                               │
 │       │                                                               │
 │       ▼                                                               │
 │   Final Answer                                                        │
 │                                                                       │
 └───────────────────────────────────────────────────────────────────────┘
```

---

## 3.1 What is an AI Agent? Core Characteristics

An **AI Agent** is an autonomous software system powered by a reasoning model (LLM) that receives a high-level goal, dynamically creates execution plans, selects and executes external tools, analyzes real-world observations, and iterates until the objective is accomplished.

### Key Characteristics
1. **Goal-Driven**: The user specifies *what* to achieve, not *how* to achieve it.
2. **Autonomous Planning**: Dynamically decomposes complex challenges into sequential subtasks.
3. **Tool Utilization**: Interfaces with external systems via APIs, terminal shells, databases, and calculators.
4. **Context-Aware Memory**: Retains an active record (scratchpad) of previous actions and observations.
5. **Adaptive Error Recovery**: Self-corrects when a tool call fails or returns empty data.

---

## 3.2 The ReAct Framework (Reasoning + Acting)

Introduced by Yao et al. (2022), **ReAct** synergizes reasoning traces with execution actions:

```
User Query: "What is the age difference between the current presidents of France and the US?"

Thought 1: I need to find the current president of France and their birthdate.
Action 1: SearchTool
Action Input 1: "current president of France birthdate"
Observation 1: "Emmanuel Macron was born on December 21, 1977."

Thought 2: Now I need to find the current president of the US and their birthdate.
Action 2: SearchTool
Action Input 2: "current president of the United States birthdate"
Observation 2: "Joe Biden was born on November 20, 1942."

Thought 3: Now I must calculate the difference between 1977 and 1942.
Action 3: CalculatorTool
Action Input 3: "1977 - 1942"
Observation 3: "35"

Thought 4: I have collected all necessary information.
Final Answer: The age difference between the two presidents is approximately 35 years.
```

---

## 3.3 Built-in Tools, Toolkits & Custom Tool Development

### What is a Tool?
A **Tool** is a Python callable wrapped with a schema that exposes its name, description, and input arguments to an LLM.

### The 3 Ways to Create Custom Tools

```python
# =========================================================================
# Approach 1: The @tool Decorator (Idiomatic & Fast)
# =========================================================================
from langchain_core.tools import tool

@tool
def multiply(a: int, b: int) -> int:
    """Multiplies two integers and returns the product."""
    return a * b

# =========================================================================
# Approach 2: StructuredTool.from_function() (Explicit Pydantic Schema)
# =========================================================================
from langchain_core.tools import StructuredTool
from pydantic import BaseModel, Field

class DivisionSchema(BaseModel):
    numerator: float = Field(description="The number to divide")
    denominator: float = Field(description="The number to divide by (non-zero)")

def divide_func(numerator: float, denominator: float) -> float:
    return numerator / denominator

divide_tool = StructuredTool.from_function(
    func=divide_func,
    name="divide_numbers",
    description="Divides two floating-point numbers",
    args_schema=DivisionSchema
)

# =========================================================================
# Approach 3: Subclassing BaseTool (Full Control & Async Support)
# =========================================================================
from langchain_core.tools import BaseTool
from typing import Type

class CustomAPITool(BaseTool):
    name: str = "custom_data_fetcher"
    description: str = "Fetches secure remote records"
    args_schema: Type[BaseModel] = DivisionSchema

    def _run(self, numerator: float, denominator: float) -> str:
        # Synchronous implementation
        return f"Result: {numerator / denominator}"

    async def _arun(self, numerator: float, denominator: float) -> str:
        # Asynchronous non-blocking implementation
        return f"Async Result: {numerator / denominator}"
```

---

## 3.4 Tool Binding, Calling, Execution & Injected Arguments

```
                          THE TOOL CALLING LIFECYCLE
 ┌────────────────────────────────────────────────────────────────────────┐
 │                                                                        │
 │ 1. TOOL BINDING                                                        │
 │    llm_with_tools = llm.bind_tools([multiply_tool])                    │
 │    Registers tool schemas with the LLM API                             │
 │                                                                        │
 │ 2. TOOL CALLING                                                        │
 │    ai_message = llm_with_tools.invoke("Multiply 6 by 7")               │
 │    LLM outputs tool call request: {name: 'multiply', args: {a: 6, b: 7}}│
 │                                                                        │
 │ 3. TOOL EXECUTION                                                      │
 │    tool_output = multiply_tool.invoke(ai_message.tool_calls[0])        │
 │    The local Python function executes with the provided arguments       │
 │                                                                        │
 │ 4. TOOL MESSAGE RETURN                                                 │
 │    messages.append(tool_output)                                        │
 │    llm_with_tools.invoke(messages)                                     │
 │    Passes the result back to the LLM to formulate the final answer     │
 │                                                                        │
 └────────────────────────────────────────────────────────────────────────┘
```

### Injected Tool Arguments (`InjectedToolArg`)
In advanced agent architectures, certain parameters should **not** be chosen or generated by the LLM (e.g., user IDs, active database connections, API secrets, or dynamic exchange rates). Using `Annotated[T, InjectedToolArg]` hides these parameters from the LLM's schema, allowing the runtime engine to inject them directly before tool execution.

```python
from typing import Annotated
from langchain_core.tools import tool, InjectedToolArg

@tool
def convert_currency(
    amount: float,
    conversion_rate: Annotated[float, InjectedToolArg]
) -> float:
    """Calculates target currency value from an amount."""
    return amount * conversion_rate
```

---

## 3.5 Agent Execution Loop: Internal Mechanics

The execution engine maintains state through discrete internal data structures:

- **User Query**: The initial instruction triggering the planning cycle.
- **`AgentScratchpad`**: A chronological record storing all previous `Thought`, `Action`, `Action Input`, and `Observation` entries. Injected into every iterative prompt to maintain context.
- **`AgentAction`**: Dataclass emitted by the agent specifying which tool to execute and the input arguments to pass.
- **`Observation`**: The raw string or structured payload returned by the executed tool. Appended to the scratchpad.
- **`AgentFinish`**: Emitted when the agent determines that the goal has been achieved, containing the final answer string returned to the user.

---

## 3.6 Architectural Evolution: AgentExecutor vs. LangGraph

```
                 AGENT ARCHITECTURAL GENERATIONS
 ┌──────────────────────────────────┬─────────────────────────────────────┐
 │ Legacy: AgentExecutor            │ Modern: LangGraph                   │
 ├──────────────────────────────────┼─────────────────────────────────────┤
 │ Fixed, hardcoded ReAct loop      │ Explicit state graph (nodes + edges)│
 │ Black-box execution              │ Deterministic conditional routing   │
 │ Difficult to pause or inspect    │ Built-in checkpointing & persistence│
 │ Single-agent design              │ Native multi-agent collaboration    │
 │ In-memory only                   │ Human-in-the-loop approval gates    │
 └──────────────────────────────────┴─────────────────────────────────────┘
```

### LangGraph Architecture
Starting with LangChain v1.x, **LangGraph** is the standard framework for production agents. Instead of running inside an opaque `AgentExecutor` while-loop, agent workflows are defined as explicit graphs:
- **State**: A centralized, type-safe data schema passed between nodes.
- **Nodes**: Python functions representing LLM reasoning steps, tool executions, or database operations.
- **Edges**: Directed connections between nodes. **Conditional edges** dynamically inspect state to determine whether to route to a tool execution node or terminate at the end node.
- **Checkpoints**: Native persistence engines enabling multi-agent coordination, time-travel debugging, and human approval steps.

---

## Summary Comparison Matrix

| Domain | Core Abstraction | Primary Methods | Modern LangChain Best Practice |
| :--- | :--- | :--- | :--- |
| **Foundations** | `Runnable` | `.invoke()`, `.batch()`, `.stream()` | Use LCEL pipes (`\|`) rather than legacy Chains |
| **Prompts** | `ChatPromptTemplate` | `.format_messages()` | Use `MessagesPlaceholder` for dynamic history injection |
| **Output** | Schema Enforcement | `.with_structured_output()` | Use `Pydantic` models for runtime validation |
| **RAG Ingestion**| `DocumentLoader` | `.load()`, `.lazy_load()` | Use `lazy_load()` for memory-efficient streaming |
| **RAG Splitting**| `TextSplitter` | `.create_documents()` | Use `SemanticChunker` or recursive splitting with metadata |
| **Vector Storage**| `VectorStore` | `.similarity_search()`, `.add_documents()` | Combine vector similarity with metadata filters |
| **Retrieval** | `Retriever` | `.invoke()` | Use Hybrid Search (BM25 + Dense) with Cross-Encoder Reranking |
| **Tools** | `BaseTool` | `.invoke()`, `bind_tools()` | Use `@tool` with Pydantic type annotations & `InjectedToolArg` |
| **Agents** | State Graph | Graph execution loop | Migrate from `AgentExecutor` to **LangGraph** |
