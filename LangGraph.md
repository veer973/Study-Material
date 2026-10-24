# Master Study Guide & Architectural Reference: Agentic AI Using LangGraph and RAG

---

## Table of Contents
1. [Agentic AI using LangGraph](#1-agentic-ai-using-langgraph)
   - 1.1 Conceptual Foundations: Traditional AI vs Generative AI vs Agentic AI
   - 1.2 Core Characteristics of Agentic Systems
   - 1.3 Why LangGraph? The Shift from Chains to Graphs
   - 1.4 Core LangGraph Building Blocks
   - 1.5 State Management and Reducers
   - 1.6 Persistence, Checkpoints, and Threads
   - 1.7 Tools and ToolNode Integration
   - 1.8 Human-in-the-Loop (HITL) Workflows
   - 1.9 Subgraphs: Shared State vs Separate State
   - 1.10 Model Context Protocol (MCP) Integration
2. [Memory Implementation](#2-memory-implementation)
   - 2.1 The Memory Hierarchy in LLM Applications
   - 2.2 Short-Term Memory (STM)
     - 2.2.1 In-Session State and Checkpointer Persistence
     - 2.2.2 Message Trimming (Context Window Budgeting)
     - 2.2.3 Message Deletion (Selective Memory Forgetting)
     - 2.2.4 Message Summarization (Context Compaction)
   - 2.3 Long-Term Memory (LTM)
     - 2.3.1 Semantic, Episodic, and Procedural Memory
     - 2.3.2 Cross-Thread Namespaces and Semantic Stores
     - 2.3.3 Enterprise Postgres LTM Integration
3. [Docker Integration](#3-docker-integration)
   - 3.1 Docker Fundamentals for AI Engineering
   - 3.2 Containers vs Virtual Machines
   - 3.3 Multi-Service Architecture in AI Systems
   - 3.4 Docker Setup and Configuration (`docker-compose.yml`)
   - 3.5 WSL 2 and Local Deployment Workflows
4. [RAG (Retrieval-Augmented Generation)](#4-rag-retrieval-augmented-generation)
   - 4.1 Foundations & Limitations of Traditional RAG
   - 4.2 Corrective RAG (CRAG)
     - 4.2.1 High-Level Architecture & Motivation
     - 4.2.2 Retrieval Evaluator (Correct, Ambiguous, Incorrect)
     - 4.2.3 Knowledge Refinement & Strip Deconstruction
     - 4.2.4 Web Search Fallback & Query Transformation
     - 4.2.5 Complete CRAG Implementation Workflow
   - 4.3 Self-Reflective RAG (Self-RAG)
     - 4.3.1 Reflection Critique Mechanism
     - 4.3.2 Evaluation Nodes: Retrieval Need, Relevance, Groundedness, Utility
     - 4.3.3 Multi-Step Self-RAG Graph Architecture
5. [Chatbot](#5-chatbot)
   - 5.1 Architecture & Multi-Tier Decoupling
   - 5.2 Backend Implementations
     - 5.2.1 Pure In-Memory Engine
     - 5.2.2 SQLite Persistent Checkpointing Engine
     - 5.2.3 Tool-Augmented Engine
     - 5.2.4 Asynchronous Multi-Server MCP Engine
     - 5.2.5 RAG-Enabled Conversational Engine
   - 5.3 Frontend Implementations (Streamlit)
     - 5.3.1 Conversation Thread Management & Session State
     - 5.3.2 Token Streaming & Real-Time Feedback
6. [Blog Writing Agent](#6-blog-writing-agent)
   - 6.1 System Purpose & Architecture
   - 6.2 The Orchestrator-Worker (Map-Reduce) Pattern with `Send`
   - 6.3 End-to-End Workflow & Node Specifications
     - 6.3.1 Router Node & Information Need Assessment
     - 6.3.2 Tavily Research & Evidence Collection
     - 6.3.3 Orchestrator / Planner Node
     - 6.3.4 Parallel Worker Section Drafting
     - 6.3.5 Reducer & Assembly Node
     - 6.3.6 Global Image Planner & DALL-E Generation
   - 6.4 Streamlit User Interface Integration
7. [Comprehensive Summary & Best Practices](#7-comprehensive-summary--best-practices)

---

# 1. Agentic AI using LangGraph

## 1.1 Conceptual Foundations: Traditional AI vs Generative AI vs Agentic AI

To understand modern agentic systems, we must analyze the paradigm shift across three generations of artificial intelligence:

| Dimension | Traditional AI | Generative AI | Agentic AI |
| :--- | :--- | :--- | :--- |
| **Core Objective** | Finding patterns in past data to predict labels or values. | Learning data distributions to create new, human-like content. | Achieving complex, multi-step goals autonomously with minimal oversight. |
| **Execution Nature**| Deterministic / Static. | Reactive: One prompt in $\rightarrow$ one response out. | Proactive: Formulates plans, uses tools, evaluates outcomes, and self-corrects. |
| **Memory Scope** | Model weights only. | Fixed context window (stateless across requests without external glue code). | Multi-tiered: Short-term conversational state + persistent long-term semantic stores. |
| **Control Flow** | Fixed procedural pipelines. | Single-turn invocation or simple linear prompt chains. | Dynamic cyclical graphs, branching conditions, loops, and parallel execution. |
| **Tool Usage** | Hard-coded API calls. | Function calling without autonomous multi-step reasoning. | Autonomous tool discovery, execution, reflection, and recovery. |

```text
┌───────────────────────────────────────────────────────────────┐
│                          Agentic AI                           │
│  [Goal-Oriented • Planning • Memory • Tools • Self-Correction]│
│                                                               │
│       ┌───────────────────────────────────────────────┐       │
│       │                 Generative AI                 │       │
│       │      [Content Creation • LLMs • Diffusion]    │       │
│       │                                               │       │
│       │       ┌───────────────────────────────┐       │       │
│       │       │        Traditional AI         │       │       │
│       │       │   [Patterns • Classification] │       │       │
│       │       └───────────────────────────────┘       │       │
│       └───────────────────────────────────────────────┘       │
└───────────────────────────────────────────────────────────────┘
```

> **Key Takeaway:** Generative AI is the creative engine; Agentic AI is the decision-making brain that steers that engine toward solving goals.

---

## 1.2 Core Characteristics of Agentic Systems

An agent is defined by five foundational capabilities:

### 1. Autonomy within Defined Boundaries
Autonomy is the system's ability to make decisions and execute actions independently without requiring step-by-step human intervention. Autonomy operates across three dimensions:
- **Decision Making:** Determining which step to execute next.
- **Execution:** Running tasks independently.
- **Tool Usage:** Interacting with external environments, databases, and APIs.

*Controlling Autonomy:* Unbounded autonomy creates severe risks (e.g., placing improper financial orders, hallucinating unauthorized actions). Effective architectures implement:
- **Permission Scopes:** Constraining tool permissions.
- **Guardrails & Policies:** Enforcing deterministic system rules.
- **Human-in-the-Loop (HITL) Checkpoints:** Requiring human confirmation before irreversible operations.

### 2. Goal Orientation
Agents operate with persistent objectives stored in state. While prompts provide immediate input, the **goal** serves as the compass for autonomy:
- Goals define *what* needs to be accomplished.
- Autonomy defines *how* the agent chooses to navigate toward that goal.
- Goals can incorporate hard constraints (e.g., maximum budget, word limit, tone requirements).

### 3. Deliberative Planning
Agents plan before acting:
1. **Candidate Plan Generation:** Generating one or more candidate action sequences.
2. **Evaluation:** Scoring plans against constraints and known facts.
3. **Execution & Dynamic Replanning:** As intermediate actions yield unexpected results, the agent updates its state and generates revised sub-plans.

### 4. Tool Usage & Environmental Grounding
LLMs are isolated models confined to their training data. Agents bridge this gap by querying live APIs, performing web searches (e.g., Tavily, DuckDuckGo), querying databases (SQL, Vector DBs), and invoking external compute protocols (e.g., MCP).

### 5. Reflection and Self-Correction
Rather than accepting first-pass outputs, agentic systems critique intermediate results. If an API call fails or retrieved documents are irrelevant, the agent detects the discrepancy and triggers corrective subroutines.

---

## 1.3 Why LangGraph? The Shift from Chains to Graphs

While basic LLM applications can be built with standard LangChain Expression Language (LCEL) chains, complex agentic systems require **LangGraph**.

```text
LangChain (Linear Chains)        LangGraph (Cyclic Multi-Agent Graphs)
┌──────┐   ┌──────┐   ┌──────┐        ┌─────────┐
│Step 1│──>│Step 2│──>│Step 3│        │  START  │
└──────┘   └──────┘   └──────┘        └────┬────┘
                                           ▼
                                    ┌──────────────┐◄────┐ (Self-Correction Loop)
                                    │  Agent Node  │     │
                                    └──────┬───────┘     │
                                           ├─────────────┤
                                     [Need Tools?]  [Evaluate Output]
                                           ▼             │
                                    ┌──────────────┐     │
                                    │  Tool Node   │─────┘
                                    └──────────────┘
```

### The Seven Architectural Challenges Solved by LangGraph:
1. **Control-Flow Complexity:** Linear chains cannot naturally represent conditional loops, retries, and multi-agent coordination. LangGraph models workflows as graphs ($V, E$) supporting cyclic edges.
2. **State Management:** Stateless chains require external glue code to pass context. LangGraph provides a centralized, typed, state machine where every node receives and updates a shared schema.
3. **Event-Driven & Multi-Step Execution:** LangGraph supports asynchronous node triggers, parallel fan-out (`Send`), and fan-in reduction.
4. **The Glue-Code Problem:** Custom `while` loops wrapping LLM calls lack standard abstractions. LangGraph standardizes graph compilation, conditional edges, and execution hooks.
5. **Fault Tolerance & Checkpointing:** In production, network blips or LLM rate limits cause crashes. LangGraph automatically snapshots state at every step using checkpointers.
6. **Human-in-the-Loop (HITL) for Long-Running Tasks:** The ability to pause graph execution (`interrupt()`), persist the graph to a database, and resume days later when a human approves the action.
7. **Observability & Subgraphs:** Native integration with tracing platforms (LangSmith) and the ability to nest complex workflows as modular subgraphs.

---

## 1.4 Core LangGraph Building Blocks

Every LangGraph system consists of four primary constructs:

```text
 ┌─────────────────────────────────────────────────────────────┐
 │                         StateGraph                          │
 │                                                             │
 │   START ───> [Node: Agent] ──(Conditional Edge)──> [Tools] │
 │                     ▲                                  │    │
 │                     └──────────(Normal Edge)───────────┘    │
 │                     │                                       │
 │                     └──(Done?)───> END                      │
 └─────────────────────────────────────────────────────────────┘
```

1. **State:** The centralized data structure (typically a Python `TypedDict` or Pydantic model) that holds the current snapshot of the application.
2. **Nodes:** Python functions (sync or async) that take the current `State` as input, perform computation (LLM call, database query, data transformation), and return a dictionary of state updates.
3. **Edges:** Rules controlling execution transfer:
   - **Normal / Sequential Edges:** Unconditionally transition execution from Node A to Node B: `graph.add_edge("node_a", "node_b")`.
   - **Conditional Edges:** Dynamic routing functions that inspect the state and return the name of the next destination node: `graph.add_conditional_edges("agent", router_function, {"tools": "tool_node", "end": END})`.
   - **Special Markers:** `START` (entry point of the graph) and `END` (terminal exit point).
4. **Compiled Graph:** The finalized, validated executable application created by calling `graph.compile(checkpointer=...)`.

---

## 1.5 State Management and Reducers

In LangGraph, node functions return updates to state attributes rather than replacing the entire state dictionary.

### Default Overwrite vs Custom Reducers
By default, if a node returns `{"topic": "New Topic"}`, LangGraph overwrites `state["topic"]`. However, for lists or conversation histories, overwriting destroys previous data. **Reducers** define how new updates merge with existing values.

```python
from typing import TypedDict, Annotated, List
import operator
from langchain_core.messages import BaseMessage
from langgraph.graph.message import add_messages

class AgentState(TypedDict):
    # Overwrite behavior: new value replaces old value
    current_topic: str
    
    # Custom reducer using operator.add: appends new items to existing list
    intermediate_steps: Annotated[List[str], operator.add]
    
    # LangGraph's specialized message reducer:
    # 1. Appends new messages
    # 2. Updates existing messages if an ID matches
    # 3. Removes messages marked with RemoveMessage
    messages: Annotated[List[BaseMessage], add_messages]
```

---

## 1.6 Persistence, Checkpoints, and Threads

### Checkpoints vs Checkpointers
- **Checkpoint:** An immutable snapshot of the graph's state taken at a specific point in time (e.g., after a node finishes execution). It contains the state dictionary, node version, and configuration.
- **Checkpointer:** The storage engine responsible for persisting and retrieving checkpoints.
  - `MemorySaver`: Stores checkpoints in RAM. Fast, but lost upon process shutdown.
  - `SqliteSaver` / `AsyncSqliteSaver`: Stores checkpoints inside a local SQLite database (`.db` file). Excellent for desktop applications and local testing.
  - `PostgresSaver`: Enterprise-grade storage in PostgreSQL tables, supporting high concurrency, multi-user deployments, and connection pooling.

### The Role of `thread_id`
A `thread_id` is a unique identifier assigned to an execution session. It acts as the partitioning key in the checkpointer database.
- Multiple independent conversations live in the same database table.
- When invoking the graph with `config={"configurable": {"thread_id": "session-123"}}`, LangGraph loads the latest checkpoint matching that `thread_id`, executes the graph, and saves the updated state back under the same thread.

```python
import sqlite3
from langgraph.checkpoint.sqlite import SqliteSaver

# Setup thread-safe SQLite connection
conn = sqlite3.connect("chatbot.db", check_same_thread=False)
checkpointer = SqliteSaver(conn=conn)

# Compile graph with persistence
app = workflow.compile(checkpointer=checkpointer)

# Execute on Thread 1
config_1 = {"configurable": {"thread_id": "user_alice"}}
app.invoke({"messages": [HumanMessage(content="Hello, my name is Alice")]}, config=config_1)

# Execute on Thread 2 (completely isolated state)
config_2 = {"configurable": {"thread_id": "user_bob"}}
app.invoke({"messages": [HumanMessage(content="Hello, my name is Bob")]}, config=config_2)
```

---

## 1.7 Tools and ToolNode Integration

Modern agents do not perform actions directly inside the LLM; they request external tool execution via structured output.

```text
┌──────────────┐
│  Agent Node  │◄────────────────────────────────┐
└──────┬───────┘                                 │
       │                                         │
       ▼ (LLM produces tool_calls)               │
[tools_condition]                                │ (ToolMessage)
       │ (Has tool calls?)                       │
       ├──────────────► [ToolNode] ──────────────┘
       │                 Executes external tool
       ▼ (No tool calls)
     [END]
```

### Implementation Pattern:
1. Define tools using the `@tool` decorator with explicit type annotations and descriptive docstrings.
2. Bind the tools to the LLM via `llm.bind_tools(tools)`.
3. Use LangGraph's prebuilt `ToolNode(tools)`.
4. Connect the agent node to the tool node using `tools_condition`.

```python
from langchain_core.tools import tool
from langgraph.prebuilt import ToolNode, tools_condition

@tool
def calculate_compound_interest(principal: float, rate: float, years: int) -> float:
    """Calculate compound interest given principal, annual rate (0-1), and years."""
    return principal * ((1 + rate) ** years)

tools = [calculate_compound_interest]
tool_node = ToolNode(tools)

# Bind tools to the model
model = ChatOpenAI(model="gpt-4o-mini").bind_tools(tools)

workflow = StateGraph(AgentState)
workflow.add_node("agent", call_model)
workflow.add_node("tools", tool_node)

workflow.add_edge(START, "agent")
workflow.add_conditional_edges("agent", tools_condition)
workflow.add_edge("tools", "agent")
```

---

## 1.8 Human-in-the-Loop (HITL) Workflows

Human-in-the-Loop is critical for high-stakes actions (financial transactions, data deletion, external communications). LangGraph provides native support via `interrupt()` and `Command`.

```text
[User asks to buy stock]
         ↓
    [Agent Node]
         ↓
  [purchase_stock Tool]
         ↓
  ┌────────────────────────────────────────────────────────┐
  │ interrupt("Approve buying 50 AAPL shares? (yes/no)")   │
  │ Graph execution halts. State snapshotted to database. │
  └────────────────────────────────────────────────────────┘
         │
         │ (Application exits or waits for human input)
         ▼
  [Human provides "yes" in UI]
         ↓
  [Graph resumed with Command(resume="yes")]
         ↓
  [Tool completes execution] ──> [Agent informs user]
```

### Code Implementation (`chatbot_with_hitl.py` pattern):
```python
from langgraph.types import interrupt, Command
from langchain_core.tools import tool

@tool
def execute_trade(symbol: str, quantity: int) -> dict:
    """Execute a stock trade with human approval."""
    # Interrupt halts graph execution and yields control to the caller
    approval = interrupt(f"Authorize trade: Buy {quantity} shares of {symbol}?")
    
    if isinstance(approval, str) and approval.strip().lower() == "yes":
        # Proceed with execution
        return {"status": "executed", "symbol": symbol, "quantity": quantity}
    else:
        return {"status": "rejected", "reason": "User denied approval"}
```

To resume the execution:
```python
# Resuming the graph after human review:
app.invoke(Command(resume="yes"), config=thread_config)
```

---

## 1.9 Subgraphs: Shared State vs Separate State

A **subgraph** is a graph embedded and executed as a node inside another parent graph. Subgraphs enable modularity, code reuse, and clean state boundaries.

```text
Separate-State Subgraph               Shared-State Subgraph
┌─────────────────────────────────┐   ┌─────────────────────────────────┐
│ Parent Graph (ParentState)      │   │ Parent Graph (GlobalState)      │
│   Node A ──> [Subgraph Node]    │   │   Node A                        │
│                   │             │   │     │                           │
│       ┌───────────▼───────────┐ │   │   ┌─▼─────────────────────────┐ │
│       │ Subgraph (SubState)   │ │   │   │ Subgraph (GlobalState)    │ │
│       │  SubNode 1 ──> SubNode│ │   │   │  Direct read/write access │ │
│       └───────────┬───────────┘ │   │   │  to parent fields         │ │
│                   │             │   │   └─┬─────────────────────────┘ │
│   Node B ◄────────┘             │   │     │                           │
│   Explicit input/output mapping │   │   Node B                        │
└─────────────────────────────────┘   └─────────────────────────────────┘
```

| Feature | Separate-State Subgraph | Shared-State Subgraph |
| :--- | :--- | :--- |
| **Schema Coupling** | Decoupled (Parent and child have independent schemas). | High (Both use the exact same state class or superset). |
| **Communication** | Explicit mapping in wrapper function. | Direct read/write into shared fields. |
| **Reusability** | **High:** Subgraph can be plugged into any project. | **Low:** Subgraph depends on parent field names. |
| **Best Used For** | Reusable libraries, independent agent modules. | Tight, specialized workflows within a single project. |

---

## 1.10 Model Context Protocol (MCP) Integration

The **Model Context Protocol (MCP)** is an open standard developed by Anthropic that standardizes how AI applications connect to external tools, data sources, and servers.

### Architecture in the Repository (`langgraph_mcp_backend.py`):
In production web apps (such as Streamlit), UI interactions run on synchronous request threads, while MCP servers communicate over asynchronous transports (`stdio` or `SSE`). The repository bridges this via a persistent background event loop:

```text
Streamlit Main Thread (Sync)            Background Event Loop (Async)
┌─────────────────────────┐             ┌───────────────────────────────┐
│ User message received   │             │ _ASYNC_LOOP                   │
│                         │             │                               │
│ run_async(coroutine)    │────────────►│ MultiServerMCPClient          │
│ Blocks sync thread and  │  Submit     │ Connects to local/remote MCPs │
│ awaits async result     │  coro       │ Retrieves tools & schemas     │
│                         │◄────────────│ Executes tool calls           │
│ Continues rendering UI  │  Return     │ Returns ToolMessage output    │
└─────────────────────────┘  data       └───────────────────────────────┘
```

```python
import asyncio
import threading
from langchain_mcp_adapters.client import MultiServerMCPClient

# 1. Start dedicated background async event loop
_ASYNC_LOOP = asyncio.new_event_loop()
_ASYNC_THREAD = threading.Thread(target=_ASYNC_LOOP.run_forever, daemon=True)
_ASYNC_THREAD.start()

def run_async(coro):
    """Bridge sync callers to async MCP operations."""
    future = asyncio.run_coroutine_threadsafe(coro, _ASYNC_LOOP)
    return future.result()

# 2. Multi-Server MCP Client initialization
async def init_mcp_client():
    client = MultiServerMCPClient({
        "filesystem": {
            "command": "npx",
            "args": ["-y", "@modelcontextprotocol/server-filesystem", "./data"]
        }
    })
    tools = await client.get_tools()
    return tools
```

---

# 2. Memory Implementation

## 2.1 The Memory Hierarchy in LLM Applications

Large Language Models are inherently stateless functions: $y = f(x)$. Every API invocation requires passing the full relevant context. Memory architectures in agentic systems emulate human cognition:

```text
┌─────────────────────────────────────────────────────────────────┐
│                     LLM Memory Architecture                     │
├────────────────────────────────┬────────────────────────────────┤
│    Short-Term Memory (STM)     │     Long-Term Memory (LTM)     │
│       (Within-Session)         │        (Cross-Session)         │
├────────────────────────────────┼────────────────────────────────┤
│ • In-Session Thread State      │ • Semantic Memory (User Facts) │
│ • Checkpoint Persistence       │ • Episodic Memory (Experiences)│
│ • Window Trimming (trim_msgs)  │ • Procedural Memory (Rules)    │
│ • Selective Deletion (Remove)  │ • Namespaces & Semantic Stores │
│ • Rolling Summarization        │ • PostgreSQL / Vector Backends │
└────────────────────────────────┴────────────────────────────────┘
```

---

## 2.2 Short-Term Memory (STM)

Short-Term Memory maintains conversational context within a single active conversation thread (`thread_id`).

### 2.2.1 In-Session State and Checkpointer Persistence
STM is stored inside `ChatState['messages']` using the `add_messages` reducer. The checkpointer (`SqliteSaver`) writes each state change to SQLite tables (`checkpoints`, `writes`). When the user rejoins the conversation, the checkpointer restores the exact history:

```python
class ChatState(TypedDict):
    messages: Annotated[list[BaseMessage], add_messages]
```

### 2.2.2 Message Trimming (Context Window Budgeting)
As conversations grow, passing thousands of tokens causes latency spikes, high API costs, and context window exhaustion. **Trimming** slices messages to fit within a strict token budget while protecting system instructions.

```python
from langchain_core.messages import trim_messages
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(model="gpt-4o-mini")

# Trim messages to retain at most 1000 tokens
trimmed_history = trim_messages(
    messages=state["messages"],
    max_tokens=1000,
    strategy="last",          # Keep most recent interactions
    token_counter=llm,        # Use model tokenizer for exact counting
    include_system=True,      # Always preserve initial system instructions
    allow_partial=False,      # Do not cut messages in half
    start_on="human"          # Ensure history begins on a user turn
)
```

### 2.2.3 Message Deletion (Selective Memory Forgetting)
LangGraph allows programmatic removal of sensitive data (passwords, credit cards) or obsolete intermediate reasoning steps using `RemoveMessage`.

```python
from langchain_core.messages import RemoveMessage

def purge_sensitive_info(state: ChatState):
    # Find message containing confidential information
    messages_to_delete = []
    for msg in state["messages"]:
        if "API_KEY" in msg.content or "SSN:" in msg.content:
            messages_to_delete.append(RemoveMessage(id=msg.id))
    
    # Returning RemoveMessage objects causes add_messages to delete them
    return {"messages": messages_to_delete}
```

### 2.2.4 Message Summarization (Context Compaction)
Rather than abruptly discarding older messages, **summarization** condenses past dialogue into an evolving summary block while retaining the most recent turns verbatim.

```text
Turn 1: User asks question    ──┐
Turn 2: AI answers              ├──> [Summarizer Node] ──> System: "Summary of earlier
Turn 3: User clarifies topic   ──┘                         conversation: User is an ML engineer
Turn 4: Recent user question  ─────────────────────────>   working on LangGraph..."
Turn 5: Recent AI answer      ─────────────────────────>   Turn 4 & 5 kept verbatim
```

```python
class SummarizedState(TypedDict):
    messages: Annotated[list[BaseMessage], add_messages]
    summary: str

def summarize_conversation(state: SummarizedState):
    summary = state.get("summary", "")
    if len(state["messages"]) > 6:
        # Prompt LLM to create/extend summary
        prompt = f"Extend the current summary ({summary}) using new messages: {state['messages'][:-2]}"
        response = llm.invoke(prompt)
        # Delete older messages, retain last 2 turns
        delete_messages = [RemoveMessage(id=m.id) for m in state["messages"][:-2]]
        return {"summary": response.content, "messages": delete_messages}
    return {}
```

---

## 2.3 Long-Term Memory (LTM)

While Short-Term Memory is tied to a specific `thread_id`, **Long-Term Memory** persists across conversations, application restarts, and separate user sessions.

### 2.3.1 Semantic, Episodic, and Procedural Memory
1. **Semantic Memory:** Generalized facts and user preferences (e.g., *"User prefers Python 3.12, uses dark mode, lives in New York"*).
2. **Episodic Memory:** Specific past interactions and episodes (e.g., *"On Tuesday, the agent successfully booked Flight AA102 for the user"*).
3. **Procedural Memory:** Rules and procedural workflows the agent has learned to follow (e.g., *"When answering finance questions, always display tabular comparisons"*).

### 2.3.2 Cross-Thread Namespaces and Semantic Stores
LangGraph provides a hierarchical **Store** abstraction (`BaseStore`) decoupled from `thread_id`. Keys are namespaced by user ID or domain:

```python
# Namespace structure: (user_id, memory_type)
namespace = ("user_101", "profile")

# Storing long-term user facts
store.put(
    namespace=namespace,
    key="coding_preferences",
    value={"language": "Python", "framework": "LangGraph", "style": "PEP8"}
)

# Retrieving long-term facts in any future thread
user_memory = store.get(namespace=namespace, key="coding_preferences")
```

### 2.3.3 Enterprise Postgres LTM Integration
In production, long-term memory is backed by PostgreSQL:
- Checkpoints live in relational schemas (`checkpoints` and `checkpoint_blobs`).
- Long-term semantic memories utilize `pgvector` for similarity search:

```text
┌─────────────────────────────────────────────────────────────┐
│                    PostgreSQL 16 Engine                     │
├──────────────────────────────┬──────────────────────────────┤
│      Checkpoints Schema      │       Vector Store LTM       │
│  (thread_id, checkpoint_id,  │   (embedding, namespace,     │
│   state_json, timestamp)     │    fact_payload, user_id)    │
└──────────────────────────────┴──────────────────────────────┘
```

---

# 3. Docker Integration

## 3.1 Docker Fundamentals for AI Engineering

AI applications are notoriously prone to the **"it works on my machine"** dilemma due to deep dependency trees (C++ build tools, specific CUDA/Python versions, vector libraries like FAISS, and relational drivers like `psycopg2`).

**Docker** encapsulates the entire operating system user space, Python runtime, libraries, environment variables, and services into portable, reproducible units.

```text
Without Docker:
Developer Laptop (Win 11)   ──> Works ✅
Teammate Laptop (macOS)     ──> Fails (Missing C++ compiler / dependencies) ❌
Production VM (Ubuntu 22)   ──> Fails (Different Python / SQLite version) ❌

With Docker:
                   ┌──────────────────────────────┐
                   │   Packaged Docker Container   │
                   │ [Ubuntu + Python + Libs + DB]│
                   └──────────────┬───────────────┘
                                  │
          ┌───────────────────────┼───────────────────────┐
          ▼                       ▼                       ▼
   Developer Laptop          CI/CD Runner           Cloud Production
       Works ✅                Works ✅                 Works ✅
```

---

## 3.2 Containers vs Virtual Machines

```text
      Virtual Machines (Heavy)                  Docker Containers (Lightweight)
┌───────────────────────────────────┐       ┌───────────────────────────────────┐
│ App 1 │ App 2 │ App 3             │       │ App 1   │ App 2   │ App 3     │
├───────┴───────┴───────────────────┤       ├─────────┴─────────┴───────────┤
│ Guest OS │ Guest OS │ Guest OS    │       │ Docker Engine (Containerd)    │
├───────────────────────────────────┤       ├───────────────────────────────┤
│ Hypervisor (Type 1 or 2)          │       │ Shared Host OS Linux Kernel   │
├───────────────────────────────────┤       ├───────────────────────────────┤
│ Physical Hardware                 │       │ Physical Hardware             │
└───────────────────────────────────┘       └───────────────────────────────┘
```

- **Virtual Machines:** Emulate entire hardware stacks and require a full guest operating system per instance. Slow startup times (minutes) and heavy resource consumption (gigabytes of RAM).
- **Containers:** Run as isolated processes directly on the host operating system's kernel. Near-instant startup (milliseconds) and minimal overhead.

---

## 3.3 Multi-Service Architecture in AI Systems

Modern agentic systems require multiple independent infrastructure components working concurrently:

```text
                                  ┌──────────────────────────────┐
                                  │       Docker Compose         │
                                  └──────────────┬───────────────┘
                                                 │
                   ┌─────────────────────────────┼─────────────────────────────┐
                   ▼                             ▼                             ▼
       ┌───────────────────────┐     ┌───────────────────────┐     ┌───────────────────────┐
       │   FastAPI / App       │     │   PostgreSQL 16       │     │   Redis / Qdrant      │
       │   AI Agent Backend    │────►│   State Checkpointer  │────►│   Semantic Caching    │
       │   Port 8000           │     │   Port 5442 (Host)    │     │   Port 6379           │
       └───────────────────────┘     └───────────────────────┘     └───────────────────────┘
```

---

## 3.4 Docker Setup and Configuration (`docker-compose.yml`)

The repository includes a dedicated Docker Compose configuration for local database persistence in `Memory Implementation and Docker/docker-compose.yml`:

```yaml
services:
  postgres:
    image: postgres:16
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: postgres
    ports:
      - "5442:5432"
```

### Detailed Field Breakdown:
- `services`: Defines the containerized applications managed by Docker Compose.
- `postgres`: The name of the service container.
- `image: postgres:16`: Pulls the official PostgreSQL version 16 image from Docker Hub.
- `environment`: Injects runtime environment variables configuring PostgreSQL authentication credentials and creating the default database named `postgres`.
- `ports: ["5442:5432"]`: **Port Forwarding**. Maps host port `5442` to container internal port `5432`. This avoids port collisions with any local PostgreSQL instance already running on the standard `5432` port.

### Essential Docker Commands:
```bash
# Start PostgreSQL in the background
docker compose up -d

# Check running container health and status
docker compose ps

# View live container logs
docker compose logs -f postgres

# Stop and remove containers without losing data
docker compose down
```

---

## 3.5 WSL 2 and Local Deployment Workflows

On Windows systems, Docker Desktop runs via **WSL 2 (Windows Subsystem for Linux)**:
1. Docker Desktop communicates with the Linux kernel inside WSL 2.
2. The Docker engine runs native Linux containers without virtualization translation overhead.
3. Python scripts executing in Windows can connect seamlessly to containerized databases via `localhost:5442`.

---

# 4. RAG (Retrieval-Augmented Generation)

## 4.1 Foundations & Limitations of Traditional RAG

Traditional (Naive) RAG operates as a static, linear pipeline:

```text
User Query ──> [Embedding Model] ──> [Vector Database] ──> Top-K Chunks ──> [LLM] ──> Response
```

### The Three Critical Failures of Traditional RAG:
1. **Blind Trust in Vector Search:** Vector databases compute semantic similarity, not factual relevance. Asking an out-of-domain question returns the closest mathematical vector, even if the content is completely irrelevant. Naive RAG passes these bad chunks directly to the LLM.
2. **Retrieval Redundancy:** Naive RAG retrieves chunks for simple queries (e.g., *"What is machine learning?"*) where the LLM already possesses world-class knowledge, introducing unnecessary latency and cost.
3. **Hallucination Amplification:** If retrieved context is noisy or conflicting, the LLM hallucinates explanations to bridge the logical gaps.

To fix these problems, the repository implements two advanced agentic RAG architectures: **Corrective RAG (CRAG)** and **Self-Reflective RAG (Self-RAG)**.

---

## 4.2 Corrective RAG (CRAG)

### 4.2.1 High-Level Architecture & Motivation
**Corrective RAG** introduces a dedicated **Retrieval Evaluator** between the retriever and generator. Instead of blindly trusting retrieved documents, CRAG grades retrieval quality and triggers corrective actions.

```text
                           ┌───────────────────────────────┐
                           │          User Query           │
                           └──────────────┬────────────────┘
                                          ▼
                           ┌───────────────────────────────┐
                           │       Vector Retrieval        │
                           └──────────────┬────────────────┘
                                          ▼
                           ┌───────────────────────────────┐
                           │      Retrieval Evaluator      │
                           └──────────────┬────────────────┘
                                          │
            ┌─────────────────────────────┼─────────────────────────────┐
            ▼ (High Confidence)           ▼ (Ambiguous Confidence)      ▼ (Low Confidence)
     [CORRECT CASE]               [AMBIGUOUS CASE]              [INCORRECT CASE]
            │                             │                             │
    Knowledge Refinement          Combine Vector Strips         Discard Vector Chunks
    (Decompose & Strip Noise)     + Web Search Results                  │
            │                             │                     Query Transformation
            │                             │                     + Web Search Fallback
            └─────────────────────────────┼─────────────────────────────┘
                                          ▼
                           ┌───────────────────────────────┐
                           │       Final Generation        │
                           └───────────────────────────────┘
```

---

### 4.2.2 Retrieval Evaluator (Correct, Ambiguous, Incorrect)
The Retrieval Evaluator grades document relevance across confidence thresholds:
- **Correct ($\text{Score} \ge \theta_{\text{high}}$):** The retrieved documents contain direct, sufficient answers.
- **Incorrect ($\text{Score} \le \theta_{\text{low}}$):** The retrieved documents are completely irrelevant.
- **Ambiguous ($\theta_{\text{low}} < \text{Score} < \theta_{\text{high}}$):** The documents are partially relevant but insufficient for a complete answer.

```python
from pydantic import BaseModel, Field
from typing import Literal

class GradeDocuments(BaseModel):
    """Binary score for relevance check on retrieved documents."""
    binary_score: Literal["yes", "no"] = Field(
        description="Documents are relevant to the question, 'yes' or 'no'"
    )

evaluator_llm = llm.with_structured_output(GradeDocuments)
```

---

### 4.2.3 Knowledge Refinement & Strip Deconstruction
When documents are graded *Correct*, passing full raw chunks still introduces noise. CRAG breaks documents into finer **knowledge strips** (sentences or short propositions), evaluates each strip individually, and discards extraneous sentences.

---

### 4.2.4 Web Search Fallback & Query Transformation
When documents are graded *Incorrect* or *Ambiguous*:
1. **Query Transformation:** The user query is rewritten into an optimized search engine syntax.
2. **Web Fallback:** The system calls external search APIs (Tavily Search) to retrieve fresh public knowledge.

```python
def transform_query_node(state: CRAGState) -> dict:
    question = state["question"]
    prompt = f"Reformulate this user question into an optimal web search query:\n{question}"
    rewritten_query = llm.invoke(prompt).content
    return {"question": rewritten_query}
```

---

### 4.2.5 Complete CRAG Implementation Workflow
The graph structure implemented in the `RAG/CRAG/` notebooks coordinates these components seamlessly:

```python
workflow = StateGraph(CRAGState)

workflow.add_node("retrieve", retrieve_node)
workflow.add_node("grade_documents", grade_documents_node)
workflow.add_node("generate", generate_node)
workflow.add_node("transform_query", transform_query_node)
workflow.add_node("web_search", web_search_node)

workflow.add_edge(START, "retrieve")
workflow.add_edge("retrieve", "grade_documents")

workflow.add_conditional_edges(
    "grade_documents",
    decide_to_generate,
    {
        "transform_query": "transform_query",
        "generate": "generate"
    }
)

workflow.add_edge("transform_query", "web_search")
workflow.add_edge("web_search", "generate")
workflow.add_edge("generate", END)
```

---

## 4.3 Self-Reflective RAG (Self-RAG)

### 4.3.1 Reflection Critique Mechanism
**Self-RAG** incorporates self-reflection at every critical junction. While CRAG focuses heavily on retrieval quality, Self-RAG also verifies **factual grounding (hallucination checks)** and **answer utility**.

```text
[User Query] ──> [Retrieve Needed?]
                        │
         ┌──────────────┴──────────────┐
         ▼ (Yes)                       ▼ (No)
   [Retrieve Chunks]            [Generate from Parametric Memory] ──> [END]
         │
   [Grade Relevance]
         │
   ┌─────┴─────────────────────┐
   ▼ (Relevant)                ▼ (Irrelevant)
[Generate Answer]        [Rewrite Query] ──> [Web Fallback]
   │
[Hallucination Check (IsGrounded?)]
   │
   ├───────────────────────────┐
   ▼ (Grounded)                ▼ (Hallucinated)
[Utility Check (IsUseful?)]  [Regenerate Output]
   │
   ├───────────────────────────┐
   ▼ (Useful)                  ▼ (Not Useful)
 [END]                   [Rewrite Query & Retry]
```

### 4.3.2 Evaluation Nodes
1. **`Retrieve?` Node:** Evaluates whether external knowledge is needed at all.
2. **`IsRelevant?` Node:** Confirms that retrieved documents match the prompt semantics.
3. **`IsGrounded?` Node:** Verifies that every claim in the generated output is strictly supported by the context tokens, eradicating hallucinations.
4. **`IsUseful?` Node:** Confirms the answer directly solves the user's inquiry.

---

# 5. Chatbot

## 5.1 Architecture & Multi-Tier Decoupling

The repository's chatbot implementation adheres to clean architecture principles by strictly separating the **User Interface Layer** (Streamlit) from the **Agentic Execution Engine** (LangGraph).

```text
┌─────────────────────────────────────────────────────────────┐
│                 Streamlit Frontend Layer                    │
│   • Multi-conversation sidebar selector                     │
│   • st.session_state management                             │
│   • Dynamic thread creation & deletion                      │
│   • Streaming response rendering via st.chat_message        │
└──────────────────────────────┬──────────────────────────────┘
                               │ Invokes graph with config
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                 LangGraph Backend Layer                     │
│   • ChatState schema with add_messages reducer              │
│   • Stateful SQLite Checkpointer (chatbot.db)               │
│   • ToolNode integration (search, calculators, finance)     │
│   • Async MultiServerMCPClient support                      │
│   • Thread retrieval via checkpointer.list()                │
└─────────────────────────────────────────────────────────────┘
```

---

## 5.2 Backend Implementations

### 5.2.1 Pure In-Memory Engine (`langgraph_backend.py`)
A fast, volatile engine utilizing `MemorySaver`. Ideal for testing graphs without writing database artifacts to disk.

### 5.2.2 SQLite Persistent Checkpointing Engine (`langgraph_database_backend.py`)
Provides complete persistence across browser refreshes and application restarts:
```python
from langgraph.graph import StateGraph, START, END
from typing import TypedDict, Annotated
from langchain_core.messages import BaseMessage
from langchain_openai import ChatOpenAI
from langgraph.checkpoint.sqlite import SqliteSaver
from langgraph.graph.message import add_messages
import sqlite3

class ChatState(TypedDict):
    messages: Annotated[list[BaseMessage], add_messages]

def chat_node(state: ChatState):
    response = llm.invoke(state['messages'])
    return {"messages": [response]}

# check_same_thread=False allows Streamlit's multi-threaded runners to share connection
conn = sqlite3.connect("chatbot.db", check_same_thread=False)
checkpointer = SqliteSaver(conn=conn)

graph = StateGraph(ChatState)
graph.add_node("chat_node", chat_node)
graph.add_edge(START, "chat_node")
graph.add_edge("chat_node", END)

chatbot = graph.compile(checkpointer=checkpointer)

def retrieve_all_threads():
    """Inspects SQLite checkpointer and extracts unique conversation thread IDs."""
    all_threads = set()
    for checkpoint in checkpointer.list(None):
        all_threads.add(checkpoint.config['configurable']['thread_id'])
    return list(all_threads)
```

### 5.2.3 Tool-Augmented Engine (`langgraph_tool_backend.py`)
Incorporates `ToolNode` and `tools_condition` to execute local Python functions.

### 5.2.4 Asynchronous Multi-Server MCP Engine (`langgraph_mcp_backend.py`)
Leverages `MultiServerMCPClient` to federate tools across multiple Model Context Protocol servers over a persistent background asyncio thread.

### 5.2.5 RAG-Enabled Conversational Engine (`langraph_rag_backend.py`)
Combines vector retrieval with conversational state, grounding chatbot answers in private corpora while retaining multi-turn context.

---

## 5.3 Frontend Implementations (Streamlit)

### 5.3.1 Conversation Thread Management & Session State
`streamlit_frontend_database.py` manages conversations using a sidebar UI:
- Users can create new chat sessions (`uuid.uuid4()`).
- All past conversation threads are dynamically fetched from the database using `retrieve_all_threads()`.
- Switching threads in the sidebar reloads the conversation history for that specific `thread_id` without resetting the application.

### 5.3.2 Token Streaming & Real-Time Feedback
`streamlit_frontend_streaming.py` streams tokens as they are generated by calling `app.stream(..., stream_mode="messages")`. This eliminates waiting for the complete LLM completion before displaying output.

---

# 6. Blog Writing Agent

## 6.1 System Purpose & Architecture

The **Blog Writing Agent** is an autonomous multi-agent content generation engine. Rather than prompting an LLM to generate a complete article in a single pass (which results in superficial, unstructured, and brief content), this system breaks production into specialized agentic roles using an **Orchestrator-Worker (Map-Reduce)** architecture.

```text
                                 ┌──────────────────────────────┐
                                 │       User Topic Input       │
                                 └──────────────┬───────────────┘
                                                ▼
                                 ┌──────────────────────────────┐
                                 │         Router Node          │
                                 │ (Closed-book / Hybrid / Open)│
                                 └──────────────┬───────────────┘
                                                │
                 ┌──────────────────────────────┴──────────────────────────────┐
                 ▼ (Needs Research)                                            ▼ (Closed-Book)
  ┌──────────────────────────────┐                                             │
  │     Tavily Web Research      │                                             │
  │   (Scoped Search Queries)    │                                             │
  └──────────────┬───────────────┘                                             │
                 │                                                             │
                 └──────────────────────────────┬──────────────────────────────┘
                                                ▼
                                 ┌──────────────────────────────┐
                                 │     Orchestrator / Plan      │
                                 │ (Tasks, Bullet Points, Words)│
                                 └──────────────┬───────────────┘
                                                ▼
                                   [Fan-Out via Send() API]
                   ┌────────────────────────────┼────────────────────────────┐
                   ▼                            ▼                            ▼
       ┌───────────────────────┐    ┌───────────────────────┐    ┌───────────────────────┐
       │     Section Worker    │    │     Section Worker    │    │     Section Worker    │
       │    (Section 1 Draft)  │    │    (Section 2 Draft)  │    │    (Section N Draft)  │
       └───────────┬───────────┘    └───────────┬───────────┘    └───────────┬───────────┘
                   │                            │                            │
                   └────────────────────────────┼────────────────────────────┘
                                                ▼
                                 ┌──────────────────────────────┐
                                 │         Reducer Node         │
                                 │  (Merge, Transitions, Check) │
                                 └──────────────┬───────────────┘
                                                ▼
                                 ┌──────────────────────────────┐
                                 │     Global Image Planner     │
                                 │ (Analyze Article & Prompts)  │
                                 └──────────────┬───────────────┘
                                                ▼
                                 ┌──────────────────────────────┐
                                 │     DALL-E Image Engine      │
                                 │  (Generate & Save to Disk)   │
                                 └──────────────┬───────────────┘
                                                ▼
                                 ┌──────────────────────────────┐
                                 │   Final Illustrated Article  │
                                 └──────────────────────────────┘
```

---

## 6.2 The Orchestrator-Worker (Map-Reduce) Pattern with `Send`

In LangGraph, distributing work across dynamically generated tasks is accomplished via the **`Send(node, arg)`** API.
- The **Orchestrator** inspects the research evidence and generates a list of section tasks: $[T_1, T_2, \dots, T_k]$.
- A conditional edge inspects this list and dispatches $k$ separate `Send("worker", task_spec)` requests.
- LangGraph runs these worker tasks concurrently.
- Each worker returns a tuple: `(section_id, markdown_content)`.
- The **Reducer** node collects all tuples, sorts them by `section_id`, and merges them into a coherent whole.

---

## 6.3 End-to-End Workflow & Node Specifications

### 6.3.1 Router Node & Information Need Assessment
The Router evaluates whether the topic is evergreen or requires fresh external facts:
- `closed_book`: Evergreen fundamentals (e.g., *"How QuickSort Works"*). No research needed.
- `hybrid`: Foundational concepts requiring modern libraries or models (e.g., *"Fine-tuning Llama 3 with LoRA"*).
- `open_book`: Volatile news, pricing, or regulatory events (e.g., *"Latest AI Regulation Bills 2026"*). Generates 3–10 targeted search queries.

```python
class RouterDecision(BaseModel):
    needs_research: bool
    mode: Literal["closed_book", "hybrid", "open_book"]
    queries: List[str] = Field(default_factory=list)
```

---

### 6.3.2 Tavily Research & Evidence Collection
If research is flagged, Tavily executes search queries in parallel, extracting title, URL, snippets, and publication dates into structured `EvidenceItem` models.

---

### 6.3.3 Orchestrator / Planner Node
The orchestrator synthesizes topic constraints, tone, audience, and gathered evidence into a comprehensive `Plan`:
```python
class Task(BaseModel):
    id: int
    title: str
    goal: str
    bullets: List[str]
    target_words: int
    requires_research: bool = False
    requires_code: bool = False

class Plan(BaseModel):
    blog_title: str
    audience: str
    tone: str
    blog_kind: Literal["explainer", "tutorial", "news_roundup", "comparison", "system_design"]
    tasks: List[Task]
```

---

### 6.3.4 Parallel Worker Section Drafting
Each worker receives an isolated prompt containing:
1. The global blog title, tone, and audience.
2. The specific task goal and bullet points to expand.
3. The exact target word count (enforcing depth without fluff).
4. Relevant evidence snippets from research.

```python
def fanout(state: State):
    """Dynamically spawn parallel section workers."""
    return [
        Send("worker", {"task": task, "evidence": state["evidence"], "plan": state["plan"]})
        for task in state["plan"].tasks
    ]
```

---

### 6.3.5 Reducer & Assembly Node
The Reducer gathers completed sections via `sections: Annotated[List[tuple[int, str]], operator.add]`. It:
1. Sorts sections by `id`.
2. Inserts smooth markdown headings and transitional paragraphs.
3. Formats code blocks and adds citation references.

---

### 6.3.6 Global Image Planner & DALL-E Generation
Rather than scattering ad-hoc images, the `GlobalImagePlan` node inspects the compiled article to identify 2–4 critical concepts that benefit from visual illustration (system design diagrams, flowcharts, conceptual models):
1. Replaces image positions in the text with unique placeholders (e.g., `[[IMAGE_1]]`).
2. Generates detailed DALL-E prompts.
3. Calls the OpenAI Image API, saves PNG files to `images/`, and swaps placeholders for markdown image tags `![caption](images/filename.png)`.

---

## 6.4 Streamlit User Interface Integration

The frontend (`streamlit_frontend.py`):
- Provides configuration inputs for Topic, Audience, Tone, and Constraints.
- Visualizes real-time progress as execution moves from Router $\rightarrow$ Research $\rightarrow$ Planner $\rightarrow$ Workers $\rightarrow$ Reducer $\rightarrow$ Image Generation.
- Renders the finalized, illustrated blog post directly in markdown with full image display and a one-click download button.

---

# 7. Comprehensive Summary & Best Practices

| Domain | Best Practice | Key Antipattern to Avoid |
| :--- | :--- | :--- |
| **Agent Design** | Use cyclic graphs (`StateGraph`) with conditional boundaries and explicit state schemas. | Using linear LCEL chains for complex loops or relying on brittle `while` loops. |
| **Memory Management**| Combine short-term checkpointing (`SqliteSaver`) with message trimming or rolling summarization. | Appending messages indefinitely until the LLM context window overflows. |
| **Persistence** | Use `thread_id` to partition independent user sessions inside centralized checkpointers. | Creating separate database files per user or mixing global states across conversations. |
| **Tool Execution** | Standardize tool protocols via MCP and decouple sync web servers from async execution loops. | Running long blocking tool operations on the main web application UI thread. |
| **RAG Systems** | Implement evaluation gates (CRAG/Self-RAG) to grade retrieval and verify factual grounding. | Blindly passing raw vector search chunks directly to generation without verification. |
| **Multi-Agent Systems**| Use the Orchestrator-Worker Map-Reduce pattern (`Send` API) for parallel execution. | Prompting a single model to produce an extensive, multi-section document in one pass. |
| **Infrastructure** | Containerize supporting services (PostgreSQL, Vector DBs) with Docker Compose for environment reproducibility. | Relying on locally installed native database servers with varying OS configurations. |

---
*Document compiled and verified against the complete codebase in `d:\Agentic AI Using LangGraph And RAG`.*
