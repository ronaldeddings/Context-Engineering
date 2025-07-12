# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is the **Context Engineering** repository - a comprehensive framework for advanced context management in language models, implementing cutting-edge research from IBM Zurich, Princeton ICML, Singapore-MIT, and others. The repository provides practical implementations of context engineering concepts that go beyond simple prompt engineering.

## Key Concepts

### Core Architecture: Progressive Complexity Model
The codebase follows a biologically-inspired progression:
```
atoms → molecules → cells → organs → neural systems → neural fields
  │        │         │        │            │                │
single   few-shot  memory   multi-    cognitive      context as fields
prompts  examples  states   agents    tools         with emergence
```

### Protocol Shells (Pareto-lang)
The repository uses a domain-specific language for defining cognitive operations:
```
/protocol.name {
    intent: "purpose",
    input: { structured_inputs },
    process: [ operations ],
    output: { expected_outputs }
}
```

## Development Commands

Since this is primarily a documentation and template repository, there are no traditional build commands. Instead:

### Running Python Examples
```bash
# Run tutorial scripts
python 10_guides_zero_to_hero/01_min_prompt.py
python 10_guides_zero_to_hero/02_expand_context.py
python 10_guides_zero_to_hero/03_control_loops.py

# Run template examples
python 20_templates/control_loop.py
python 20_templates/shell_runner.py

# Run cognitive tool examples
python cognitive-tools/cognitive-programs/program-examples.py
```

### Working with Jupyter Notebooks
```bash
# Launch Jupyter to work with notebooks
jupyter notebook 10_guides_zero_to_hero/

# Or use VS Code's notebook interface for .ipynb files
```

### Python Development Tools
```bash
# Check syntax
python -m py_compile <file>.py

# Run with Python's unittest (if tests are added)
python -m unittest discover

# Format code (install black/isort first)
pip install black isort
python -m black .
python -m isort .

# Lint code (install pylint/flake8 first)
pip install pylint flake8
python -m pylint <file>.py
python -m flake8 <file>.py
```

## High-Level Architecture

### 1. **Foundation Layers** (`00_foundations/`)
- **Atoms to Neural Fields**: Progressive complexity from single prompts to continuous semantic fields
- **Symbolic Mechanisms**: Implementation of Princeton's three-stage abstraction-induction-retrieval
- **Quantum Semantics**: Multiple meaning superposition based on observer context
- **Field Theory**: Context treated as dynamic neural fields with attractors and resonance

### 2. **Protocol System** (`60_protocols/`)
The repository implements field protocols that enable:
- **Attractor Co-emergence**: Multiple semantic patterns emerging and stabilizing
- **Recursive Memory**: Self-reinforcing memory patterns through field dynamics
- **Field Resonance**: Amplification of aligned meanings
- **Self-Repair**: Autonomous error correction through field mechanisms

### 3. **Cognitive Tools Framework** (`cognitive-tools/`)
Based on IBM Zurich research, implements:
- **Structured Reasoning Templates**: Modular cognitive operations
- **Verification and Validation**: Built-in correctness checking
- **Tool Composition**: Combining tools for complex reasoning
- **Meta-cognitive Reflection**: Self-improvement mechanisms

### 4. **Integration Patterns**
The system provides multiple integration strategies:
- **Vertical Integration**: Scaling from simple to complex (atoms → fields)
- **Horizontal Integration**: Composing protocols (e.g., `attractor.co.emerge` + `recursive.emergence`)
- **Recursive Self-Improvement**: Protocols that evolve themselves

## Key Implementation Areas

### Templates (`20_templates/`)
- `control_loop.py`: Neural field control loop implementation
- `field_protocol_shells.py`: Protocol shell templates
- `symbolic_residue_tracker.py`: Tracking persistent information
- `attractor_detection.py`: Identifying emergent patterns
- `emergence_metrics.py`: Measuring emergent behaviors

### Examples (`30_examples/`)
- Progressive implementations from simple chatbots to field laboratories
- Each example demonstrates specific context engineering concepts
- Focus on practical application of theoretical concepts

### Protocols (`60_protocols/shells/`)
- `attractor.co.emerge.shell`: Creating co-emergent semantic patterns
- `recursive.emergence.shell`: Self-evolving field dynamics
- `field.resonance.scaffold.shell`: Amplifying aligned meanings
- `context.memory.persistence.attractor.shell`: Persistent memory fields

## Working with This Repository

### For Research/Study:
1. Start with `00_foundations/` to understand theory
2. Progress through `10_guides_zero_to_hero/` for hands-on learning
3. Examine `60_protocols/` for advanced field theory implementations

### For Implementation:
1. Copy templates from `20_templates/` as starting points
2. Study examples in `30_examples/` for complete implementations
3. Use protocol shells from `60_protocols/shells/` for field operations

### For Contributing:
1. Follow patterns established in existing code
2. Document new protocols using Pareto-lang format
3. Include both theoretical foundation and practical examples
4. Add visual representations (ASCII diagrams) where helpful

## Important Patterns

### Field Protocol Pattern
```python
# Fields are continuous semantic spaces
field = NeuralField(
    attractors=[...],     # Stable patterns
    resonance=0.8,        # Alignment strength
    boundary_dynamics=... # Edge behavior
)
```

### Cognitive Tool Pattern
```python
# Tools are structured reasoning operations
tool = CognitiveTool(
    intent="clear purpose",
    process=["step1", "step2", ...],
    verification=lambda x: validate(x)
)
```

### Protocol Shell Usage
```python
# Protocols define reusable operations
protocol = load_protocol("attractor.co.emerge")
result = protocol.execute(context)
```

## Research Foundations

This repository operationalizes research from:
- **IBM Zurich**: Cognitive tools as structured prompt templates
- **Princeton ICML**: Emergent symbolic mechanisms in LLMs
- **Singapore-MIT MEM1**: Memory-reasoning synergy
- **Quantum Semantics**: Observer-dependent meaning actualization
- **Shanghai AI**: LLM attractors and field dynamics

Understanding these papers (linked in README.md and CITATIONS.md) provides deeper insight into the implementations.