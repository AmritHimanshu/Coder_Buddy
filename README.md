# 🤖 Coder Buddy

**Coder Buddy** is an AI-powered coding agent built while learning and experimenting with **LangGraph, LangChain, LLM orchestration, structured outputs, and tool calling**.

The project aims to transform a user's natural-language software requirement into a structured development workflow — from understanding the requirement to planning, architecture, and implementation.

> **Learning by building:** Instead of only studying LangGraph concepts theoretically, this project was created to understand how agentic workflows can be designed and orchestrated in a real application.

---

## 🚀 Overview

Traditional LLM-based coding assistants often follow a simple pattern:

```text
User Prompt
     ↓
    LLM
     ↓
Generated Code
```

Coder Buddy explores a more structured, agent-based approach:

```text
                   User Requirement
                          │
                          ▼
                  ┌───────────────┐
                  │ Planner Agent │
                  └───────┬───────┘
                          │
                          ▼
                ┌───────────────────┐
                │ Architect Agent   │
                └─────────┬─────────┘
                          │
                          ▼
                 ┌─────────────────┐
                 │ Coding Agent    │
                 └────────┬────────┘
                          │
                          ▼
                    Project Files
```

Each stage has a specific responsibility, while **LangGraph manages the workflow and state between the agents**.

---

## ✨ Key Features

- 🧠 **Requirement Planning** — Converts natural-language requirements into a structured project plan.
- 🏗️ **Architecture & Task Decomposition** — Breaks the project plan into file-level implementation tasks.
- 💻 **AI-Assisted Coding** — Uses LLMs to implement tasks within the generated project.
- 🔗 **Multi-Agent Workflow** — Separates planning, architecture, and implementation responsibilities.
- 🗂️ **Project File Management** — Provides tools for agents to interact with project files.
- 📦 **Structured LLM Outputs** — Uses Pydantic models to enforce structured responses.
- 🔄 **State-Based Execution** — Uses LangGraph's `StateGraph` to pass information between workflow stages.
- 🛠️ **Tool Calling** — Enables the agent to interact with its development environment through defined tools.

---

## 🧠 Architecture

Coder Buddy follows a sequential agent workflow.

### 1. Planner Agent

The Planner receives the user's software requirement and creates a high-level project plan.

For example:

```text
User:
"Build a simple calculator website"
```

The Planner may produce:

```text
Project:
Calculator Website

Tech Stack:
HTML, CSS, JavaScript

Features:
- Number input
- Addition
- Subtraction
- Multiplication
- Division
- Clear button
```

This information is represented using structured **Pydantic models**.

---

### 2. Architect Agent

The Architect receives the project plan and converts it into concrete implementation tasks.

For example:

```text
Project Plan
     ↓
Architect
     ↓
Implementation Tasks
```

Example:

```text
Task 1:
File: index.html
Create the calculator interface and button layout.

Task 2:
File: style.css
Implement the calculator layout and styling.

Task 3:
File: script.js
Implement calculator operations and button interactions.
```

The Architect focuses on **what needs to be implemented**, rather than directly writing the code.

---

### 3. Coding Agent

The Coding Agent receives the implementation tasks and performs the actual development work.

It can interact with the generated project through tools such as file creation, file reading, and file modification.

The overall workflow becomes:

```text
Requirement
     ↓
Planner
     ↓
Project Plan
     ↓
Architect
     ↓
Implementation Tasks
     ↓
Coding Agent
     ↓
Generated / Modified Files
```

---

## 🔗 LangGraph Workflow

The workflow is implemented using LangGraph's `StateGraph`.

A simplified version of the graph looks like:

```python
from langgraph.graph import StateGraph, START

graph = StateGraph(dict)

graph.add_node("planner", planner_agent)
graph.add_node("architect", architect_agent)

graph.add_edge(START, "planner")
graph.add_edge("planner", "architect")
```

This creates the following execution flow:

```text
START
  │
  ▼
Planner
  │
  ▼
Architect
```

As the project evolves, additional nodes can be added for coding, testing, debugging, and validation.

---

## 📦 Structured Outputs

Coder Buddy uses **Pydantic** to define the structure expected from the LLM.

For example:

```python
class ImplementationTask(BaseModel):

    filepath: str

    task_description: str


class TaskPlan(BaseModel):

    implementation_steps: list[ImplementationTask]
```

Instead of relying on free-form text, the LLM is expected to return information following a predefined structure.

This makes the output easier for subsequent agents to consume programmatically.

---

## 🛠️ Tools

The Coding Agent can use tools to interact with the generated project.

Examples include:

```text
Read File
   ↓
Understand existing code

Write File
   ↓
Create new source files

Modify File
   ↓
Update existing implementation
```

This allows the LLM to move beyond simply **generating text** and interact with an actual project workspace.

---

## 📁 Project Structure

The project is organized around the agent workflow and supporting utilities.

```text
Coder_Buddy/
│
├── ...
│
├── generated_project/
│   └── ...
│
├── ...
│
├── requirements.txt
├── README.md
└── ...
```

> The exact project structure may evolve as additional agents and tools are implemented.

---

## 🔄 Current Workflow

The current workflow focuses on:

```text
User Requirement
       ↓
   🧠 Planner
       ↓
  📋 Project Plan
       ↓
 🏗️ Architect
       ↓
📝 Implementation Tasks
       ↓
 💻 Coding Agent
       ↓
📁 Generated Project
```

---

## 🧰 Tech Stack

| Technology | Purpose |
|---|---|
| **Python** | Core programming language |
| **LangGraph** | Agent workflow orchestration |
| **LangChain** | LLM application framework |
| **LLMs** | Reasoning and code generation |
| **Pydantic** | Structured data validation |
| **Tool Calling** | Agent-to-tool interaction |
| **Pathlib** | Project and file management |

---

## 🎯 Why I Built This

I built Coder Buddy primarily as a **hands-on learning project while learning LangGraph**.

Rather than learning individual concepts in isolation, I wanted to understand how they work together in an actual AI application.

Through this project, I explored:

- LangGraph `StateGraph`
- Nodes and edges
- Graph entry points
- State management
- Multi-agent workflows
- Agent orchestration
- Pydantic structured outputs
- LLM tool calling
- Prompt design
- File-system tools
- LLM token limitations
- Debugging structured-output failures

One of the biggest lessons from the project has been that building agentic systems involves much more than simply calling an LLM API.

---

## 📚 Learning Journey

Some of the challenges encountered while developing Coder Buddy included:

### Structured Output Failures

LLMs can sometimes generate incomplete or invalid structured responses, particularly when the expected output is large.

This required understanding how:

```text
LLM
 ↓
Structured Output
 ↓
Tool / Function Call
 ↓
Pydantic Validation
```

works internally.

### Token Limits

Large implementation plans can generate substantial output.

Managing token limits therefore became an important part of designing the prompts and agent workflow.

### State Management

Since multiple agents operate sequentially, information must be passed correctly between nodes.

For example:

```text
Planner State
     ↓
Architect State
     ↓
Coder State
```

Understanding this state flow was one of the key reasons for building the project with LangGraph.

---

## 🔮 Future Improvements

The project is still under development.

Planned improvements include:

- [ ] Complete Coding Agent implementation
- [ ] Add file creation and modification tools
- [ ] Add code execution tools
- [ ] Add automated testing
- [ ] Add debugging agent
- [ ] Add error recovery loops
- [ ] Add human-in-the-loop approval
- [ ] Add project validation
- [ ] Add persistent project state
- [ ] Improve prompt and token efficiency
- [ ] Add a web-based interface
- [ ] Add support for larger and more complex projects

The long-term goal is to evolve Coder Buddy into a more capable **AI software engineering assistant**.

---

## ⚙️ Installation

### 1. Clone the repository

```bash
git clone <your-repository-url>

cd Coder_Buddy
```

### 2. Create a virtual environment

```bash
python -m venv .venv
```

Activate it on Linux/macOS:

```bash
source .venv/bin/activate
```

On Windows:

```bash
.venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure environment variables

Create a `.env` file:

```env
GROQ_API_KEY=your_api_key_here
```

> Never commit your API keys or `.env` file to GitHub.

### 5. Run the project

Use the appropriate entry point for the current implementation:

```bash
python main.py
```

---

## 🤝 Contributing

This project is primarily a learning and experimentation project, but suggestions, improvements, and discussions are welcome.

If you find an issue or have an idea for improving the agent architecture, feel free to open an issue or submit a pull request.

---

## 📌 Project Status

**🚧 Work in Progress**

Coder Buddy is actively being developed and used as a practical project for exploring **LangGraph, agentic AI, LLM orchestration, and AI-assisted software engineering**.

---

## 👨‍💻 Author

**Himanshu Singh**

Building and learning in the field of:

- Generative AI
- Agentic AI
- AI Engineering
- Full-Stack Development

---

⭐ If you find this project interesting, consider giving the repository a star!