# Coder using crewai

A demonstration of using the [CrewAI](https://github.com/crewai/crewai) framework to automatically plan, write, execute, and collect output from Python coding assignments using an LLM-based “Coder” agent.

---

## Table of Contents

- [Overview](#overview)  
- [Features](#features)  
- [Getting Started](#getting-started)  
  - [Prerequisites](#prerequisites)  
  - [Installation](#installation)  
- [Configuration](#configuration)  
- [Usage](#usage)  
- [Code Structure](#code-structure)  
- [Customization](#customization)  
- [Contributing](#contributing)  
- [License](#license)  

---

## Overview

This repository showcases how to orchestrate an LLM-powered coding agent within a Python project using **CrewAI**. The “Coder” agent will:

1. **Plan** how to solve a given assignment.  
2. **Write** the corresponding Python code.  
3. **Execute** the code in a sandboxed Docker environment.  
4. **Collect** both code and runtime output into a text file.

It’s ideal for automated coding exercises, educational demos, or rapid prototyping of algorithmic assignments.

---

## Features

- **LLM-driven**: Uses `openai/gpt-4o-mini` to plan and generate code.  
- **Safe execution**: Runs user-generated code inside Docker containers (via CrewAI’s “safe” mode).  
- **Sequential workflow**: Planning → coding → execution → output collection.  
- **Configurable**: Easily add new agents, tasks, or change outputs via YAML.  
- **Extensible**: Integrate additional agents or workflow steps.

---

## Getting Started

### Prerequisites

- **Python 3.9+**  
- **Docker Desktop** (for sandboxed code execution)  
- A valid **OpenAI API key** (set as `OPENAI_API_KEY` in environment)

### Installation

1. **Clone the repo**  
   ```bash
   git clone https://github.com/Rishi-Kora/coder-using-crewai.git
   cd coder-using-crewai


2. **Create & activate a virtual environment**

   ```bash
   python -m venv .venv
   source .venv/bin/activate    # on Unix/macOS
   .venv\Scripts\activate       # on Windows
   ```

3. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   ```

   > *If a `requirements.txt` is not present, add:*
   >
   > ```
   > crewai
   > openai
   > pysbd
   > ```

4. **Ensure Docker is running**.

---

## Configuration

Two main YAML files control the system:

* **`config/agents.yaml`**
  Defines each LLM-based agent.

  ```yaml
  coder:
    role: "Python Developer"
    backstory: "You're a seasoned python developer with a knack for writing clean, efficient code."
    goal: "You write python code to achieve this assignment: {assignment}..."
    llm: "openai/gpt-4o-mini"
  ```
* **`config/tasks.yaml`**
  Declares the tasks the crew should perform.

  ```yaml
  coding_task:
    description: "Write python code to achieve this: {assignment}"
    expected_output: "A text file including code plus its output."
    agent: "coder"
    output_file: "output/code_and_output.txt"
  ```

To add a new task or agent, simply extend these YAML files and reference them in `crew.py`.

---

## Usage

By default, the repository includes an example assignment (Leibniz series for π):

```python
assignment = (
  "Write a python program to calculate the first 10,000 terms "
  "of this series, multiplying the total by 4: "
  "1 - 1/3 + 1/5 - 1/7 + ..."
)
```

Run the workflow with:

```bash
python main.py
```

* **Output**:

  * Generated Python code
  * Execution logs & final result
  * Combined into `output/code_and_output.txt`

---

## Code Structure

```
.
├── config/
│   ├── agents.yaml        # Agent definitions
│   └── tasks.yaml         # Task definitions
├── crew.py                # Assembles Agent, Task, Crew
├── main.py                # Entry point: runs Coder().crew().kickoff()
├── output/                # Generated code & outputs
│   └── code_and_output.txt
└── requirements.txt       # Python dependencies
```

* **`crew.py`**
  Uses CrewAI decorators to wire up:

  * `@agent` → the LLM-backed “coder”
  * `@task`  → the code-generation task
  * `@crew`  → sequential process runner
* **`main.py`**
  Populates the assignment prompt, invokes the crew, and prints raw output.

---

## Customization

* **Change the assignment**: Edit `main.py`’s `assignment` variable.
* **Use a different LLM**: Update `llm:` in `config/agents.yaml`.
* **Alter execution mode**: In `crew.py`, modify `code_execution_mode` (`"safe"` vs `"insecure"`).
* **Add agents/tasks**:

  1. Define in respective YAML.
  2. Decorate new methods in `crew.py`.
  3. Include them in the `Crew(...)` call.

---

## Contributing

1. Fork the repo
2. Create a feature branch (`git checkout -b feature/foo`)
3. Commit your changes (`git commit -am 'Add foo'`)
4. Push to your branch (`git push origin feature/foo`)
5. Open a Pull Request

Please adhere to existing code style and include tests/examples where applicable.

---

## License

This project is released under the **MIT License**. See [LICENSE](LICENSE) for details.


