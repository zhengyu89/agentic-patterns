# My Agentic Patterns Journey

> **Attribution:** This project is my personal fork of the excellent [agentic-patterns]([https://github.com/neural-maze/agentic-patterns](https://github.com/neural-maze/agentic-patterns-course)) repository by **neural-maze**. Proper attribution is a core practice in the open-source community, and I want to acknowledge the original author for their foundational work.

<p align="center">
  <img alt="logo" src="img/agentic_patterns.png" width=600 />
</p>

<h1 align="center">My Agentic Patterns Journey</h1>
<h3 align="center">Implementing and Exploring Agentic Patterns with Groq</h3>

<p align="center">
  <img alt="logo" src="img/groq.png" width=200 />
</p>

This is my personal fork where I'm learning and experimenting with agentic patterns. I'm using pure and simple API calls to **Groq**, without frameworks like LangChain, LangGraph, etc., to understand the core mechanics.

---

## Table of Contents

1. [Introduction](#introduction)
2. [The 4 Agentic Patterns](#the-4-agentic-patterns)
    - [Reflection Pattern 🤔](#reflection-pattern-)
    - [Tool Pattern 🛠](#tool-pattern-)
    - [Planning Pattern 🧠](#planning-pattern-)
    - [Multiagent Pattern 🧑🏽‍🤝‍🧑🏻](#multiagent-pattern-)
3. [Setup](#setup)
4. [Groq API Key](#groq-api-key)
5. [Usage Examples](#usage-examples)
    - [Reflection Agent](#using-a-reflection-agent---reflection-pattern)
    - [Tool Use - REST API](#creating-and-using-tools---my-rest-api-project)
    - [Planning - ReAct](#reasoning-with-a-react-agent---planning-pattern)
    - [MultiAgent Crew](#defining-and-running-a-crew-of-agents---multiagent-pattern)
6. [My Learning Workflow](#my-learning-workflow)

---

## Introduction

This repository is my personal exploration of the **4 agentic patterns** defined by Andrew Ng. My goal is to gain a deep, practical understanding of how these patterns work by building and experimenting with them using the **Groq API**.

---

## The 4 Agentic Patterns

### Reflection Pattern 🤔

<p align="center">
  <img alt="logo" src="img/reflection_pattern.png" width=500 />
</p>

A very basic pattern but, despite its simplicity, it provides surprising performance gains for the LLM response. It allows the LLM to reflect on its results, suggesting modifications, additions, or improvements.

- 📘 Check the notebook for a step-by-step explanation.
- 🧠 See [`ReflectionAgent`](src/agentic_patterns/reflection_pattern/reflection_agent.py) for implementation.

---

### Tool Pattern 🛠

<p align="center">
  <img alt="logo" src="img/tool_pattern.png" width=600 />
</p>

Tools allow LLMs to access external knowledge or perform actions outside the model. With tools, the agent can search the web, access files, query APIs, and more.

- 📘 Check the notebook for a step-by-step explanation.
- 🔧 See [`ToolAgent`](src/agentic_patterns/tool_pattern/tool_agent.py) and [`tool`](src/agentic_patterns/tool_pattern/tool.py).

---

### Planning Pattern 🧠

<p align="center">
  <img alt="logo" src="img/planning_pattern.png" width=500 />
</p>

The Planning Pattern helps break down complex tasks into smaller steps and execute them in sequence. The most known example is the **ReAct** technique.

- 📘 Check the notebook for a step-by-step explanation.
- 🧩 See [`ReactAgent`](src/agentic_patterns/planning_pattern/react_agent.py).

---

### Multiagent Pattern 🧑🏽‍🤝‍🧑🏻

<p align="center">
  <img alt="logo" src="img/multiagent_pattern.png" width=500 />
</p>

In the Multiagent Pattern, multiple agents collaborate as a "crew" to solve complex problems by splitting subtasks among specialized roles.

- 📘 Check the notebook for a step-by-step explanation.
- 🧑‍🚀 See [`Agent`](src/agentic_patterns/multiagent_pattern/agent.py) and [`Crew`](src/agentic_patterns/multiagent_pattern/crew.py).

---

## Setup

Clone the repository:

```bash
git clone https://github.com/YourUsername/agentic-patterns.git
cd agentic-patterns
```

Install dependencies:

```bash
poetry install
```

Or use pip (if you're managing manually):

```bash
pip install -r requirements.txt
```

Set up your `.env` file as described below.

---

## Groq API Key

Create an `.env` file at the project root:

```
GROQ_API_KEY="gsk_YourActualGroqApiKeyHere"
```

Refer to `.env.example` for the correct format.

---

## Usage Examples

### Using a Reflection Agent - Reflection Pattern

```python
from agentic_patterns import ReflectionAgent

agent = ReflectionAgent()

final_response = agent.run(
    user_msg="Generate a Python implementation of the Merge Sort algorithm",
    generation_system_prompt="You are a Python programmer tasked with generating high quality Python code",
    reflection_system_prompt="You are Andrej Karpathy, an experienced computer scientist who provides clear, concise feedback."
)

print(final_response)
```

---

### Creating and Using Tools - My REST API Project

```python
from agentic_patterns.tool_pattern.tool import tool
from agentic_patterns.tool_pattern.tool_agent import ToolAgent
import os

@tool
def write_code_to_file(file_path: str, code: str) -> str:
    try:
        os.makedirs(os.path.dirname(file_path), exist_ok=True)
        with open(file_path, 'w') as f:
            f.write(code)
        return f"Successfully wrote code to {file_path}"
    except Exception as e:
        return f"An error occurred: {e}"

api_agent = ToolAgent(tools=[write_code_to_file])

output = api_agent.run(
    user_msg="Please write a simple Flask API in a file named 'app.py'. The API should have one endpoint, '/', that returns a JSON object with the message 'Hello, World!'"
)

print(output)
```

---

### Reasoning with a ReAct Agent - Planning Pattern

```python
from agentic_patterns.planning_pattern.react_agent import ReactAgent
# Assume tools are defined: sum_two_elements, multiply_two_elements, compute_log

agent = ReactAgent(tools=[sum_two_elements, multiply_two_elements, compute_log])

agent.run(user_msg="I want to calculate the sum of 1234 and 5678, multiply the result by 5, and then take the logarithm of the final result.")
```

---

### Defining and Running a Crew of Agents - MultiAgent Pattern

```python
from agentic_patterns.multiagent_pattern.crew import Crew
from agentic_patterns.multiagent_pattern.agent import Agent

with Crew() as crew:
    agent_1 = Agent(
        name="Poet Agent",
        backstory="You are a well-known poet.",
        task_description="Write a short poem about the meaning of life."
    )

    agent_2 = Agent(
        name="Poem Translator Agent",
        backstory="You are an expert translator skilled in Spanish.",
        task_description="Translate the poem you receive into Spanish."
    )

    agent_1 >> agent_2  # Define workflow

    crew.run()
```

---

## My Learning Workflow

1. **Review the Original Notebooks**: Go through `notebooks/` to understand each pattern.
2. **Experiment and Modify**: Play with prompts and tools to understand behaviors.
3. **Apply to New Problems**: Use patterns on my own projects (like the REST API one).
4. **(Optional) Read the Source Code**: Explore the `src/` folder to deepen understanding of abstractions.

---

Happy experimenting! 🚀
