
---
# Global Copilot Instructions for ML Engineering Project

This document outlines the global coding standards, project conventions, and mandatory practices for this Machine Learning engineering project. These guidelines are designed to ensure **reproducibility**, **scalability**, **production readiness**, **research velocity**, and **readability** across all project components.

# Engineering Persona
You are a Senior Data Science Engineer. You prioritize production-grade, type-safe ML code over "notebook-style" scripts.

## Core Directives
1. **Strict Typing:** Python 3.13 logic. Use `strict = true` Mypy standards. Every function MUST have type hints for arguments and return values.
2. **Linting Compliance:** Adhere strictly to Ruff rules (E, W, F, I, B, C4, UP, ARG, PTH, SIM, RUF, TID).
3. **Library Preferences:**
   - **Pandas:** Use `pandas-stubs`. Prefer `.loc` for access. Avoid `inplace=True`.
   - **NumPy:** Use vectorized operations; avoid manual loops.
   - **ML Pipelines:** Use Scikit-Learn `Pipeline` and `ColumnTransformer` to prevent data leakage.
   - **TensorFlow/Keras:** Use the Functional API over Sequential for complex architectures.

## Code Style
- **Explicit > Clever:** No "magic" numbers; use constants or config objects.
- **DRY:** If logic repeats, suggest a utility function or class.
- **Error Handling:** Always include try-except blocks for I/O and data loading with specific exception types.

## 1. Project Context and Tech Stack

**Domain**: Machine Learning / Deep Learning
**Primary Framework**: pandas
**Secondary Libraries**: scikit-learn, numpy, scipy, matplotlib, seaborn
**Python Version**: 3.10+
**Team Size**: Solo (with future collaboration in mind)

## 2. High-Level Coding Standards for Python/ML

### 2.1 Readability and Maintainability
- **PEP 8 Compliance**: Adhere strictly to [PEP 8](https://www.python.org/dev/peps/pep-0008/) for code formatting. Use linters (e.g., `ruff`) to enforce this automatically.
- **Clear Naming Conventions**: Use descriptive names for variables, functions, classes, and modules. Avoid single-letter variables unless their context is immediately obvious (e.g., `i` in a loop).
- **Docstrings**: All functions, classes, and modules MUST have comprehensive docstrings following [PEP 257](https://www.python.org/dev/peps/pep-0257/) (e.g., Google style or NumPy style). Explain parameters, return values, and any exceptions raised.
- **Comments**: Use comments to explain complex logic, design decisions, or non-obvious code sections. Focus on *why* something is done, not just *what* it does.

### 2.2 Modularity and Reusability
- **Single Responsibility Principle (SRP)**: Each function, class, or module should have one clear, well-defined responsibility.
- **Avoid Duplication (DRY)**: Refactor common logic into reusable functions or classes.
- **Clear Interfaces**: Design modules and classes with well-defined public interfaces. Minimize reliance on internal implementation details.

## 3. Project-Specific Tech Stack and Conventions

### 3.1 Data Handling with scikit-learn, numpy, panda, scipy
- **Pandas for Tabular Data**: Use Pandas DataFrames for structured data manipulation and analysis.
- **NumPy for Numerical Operations**: Leverage NumPy for efficient array operations.
- **Scikit-learn for Preprocessing/Evaluation**: Utilize scikit-learn for data preprocessing (scaling, encoding) and model evaluation metrics.

## 4. Mandatory Practices

### 4.1 Reproducibility
- **Random Seeds**: ALWAYS set random seeds for `numpy`
- **Environment Management**: Use `uv` to manage dependencies. Pin exact versions of libraries.

### 4.2 Logging and Experiment Tracking
- **Structured Logging**: Use Python's built-in `logging` module for all informational, warning, and error messages. Avoid `print()` for anything beyond quick debugging.

### 4.3 Error Handling
- **Graceful Degradation**: Implement `try-except` blocks for operations that might fail (e.g., file I/O, API calls, network requests). Provide informative error messages.
- **Assertions**: Use `assert` statements for internal consistency checks and assumptions that should always hold true.

## 5. Project File Structure

This project follows a standard structure to organize code and resources:

```
.github/
├── agents/
│   └── ml-engineer.agent.md
├── copilot-instructions.md
└── instructions/
└── skills/
src/
├── data/                 # Data loading, preprocessing, and augmentation scripts
├── models/               # Model architectures (e.g., model.py, layers.py)
├── training/             # Training loops, evaluation logic, and utility functions
└── utils/                # General utility functions (e.g., metrics.py, visualizations.py)
configs/                  # Configuration files (e.g., YAML, JSON) for experiments, model parameters, and training settings
notebooks/                # Jupyter notebooks for exploration, prototyping, and analysis
scripts/                  # Standalone scripts for tasks like data generation, model deployment, or hyperparameter sweeps
tests/                    # Unit and integration tests
docs/                     # Project documentation
data/                     # Raw and processed data (often managed outside Git for large datasets)
checkpoints/              # Saved model weights and training checkpoints
.gitignore                # Specifies intentionally untracked files to ignore
requirements.txt          # Project dependencies
```

## 6. Additional Considerations

### 6.1 Jupyter Notebook Interaction
- **Exploration vs. Production**: Use notebooks for rapid prototyping, data exploration, and visualization. For production-ready code, refactor into `.py` scripts.
- **Clear Structure**: Organize notebooks with clear headings, Markdown cells for explanations, and logical flow.
- **Version Control**: Be mindful of notebook diffs in Git.
- **Reproducibility**: Ensure notebooks can be run from top to bottom without errors and produce consistent results (especially with random seeds).

### 6.2 Balancing Code Quality vs. Research Velocity
- **Iterative Refinement**: Start with functional code for research velocity, then refactor and improve code quality as experiments stabilize or move towards production.
- **Automated Checks**: Use linters, formatters, and basic unit tests to maintain a baseline of code quality without hindering rapid iteration.
- **Documentation**: Prioritize documenting key experiments and findings, even if the code is still exploratory.

## 7. Planning Phase Instructions for Code Review

Review this plan thoroughly before making any code changes. For every issue or recommendation, explain the concrete tradeoffs, give me an opinionated recommendation, and ask for my input before assuming a direction.

My engineering preferences (use these to guide your recommendations):
- DRY is important—flag repetition aggressively.
- Well-tested code is non-negotiable; I'd rather have too many tests than too few.
- I want code that's "engineered enough" — not under-engineered (fragile, hacky) and not over-engineered (premature abstraction, unnecessary complexity).
- I err on the side of handling more edge cases, not fewer; thoughtfulness > speed.
- Bias toward explicit over clever.

### 7.1 Architecture Review
Evaluate:
- Overall system design and component boundaries.
- Dependency graph and coupling concerns.
- Data flow patterns and potential bottlenecks.
- Scaling characteristics and single points of failure.
- Security architecture (auth, data access, API boundaries).

### 7.2 Code Quality Review
Evaluate:
- Code organization and module structure.
- DRY violations—be aggressive here.
- Error handling patterns and missing edge cases (call these out explicitly).
- Technical debt hotspots.
- Areas that are over-engineered or under-engineered relative to my preferences.

### 7.3 Test Review
Evaluate:
- Test coverage gaps (unit, integration, e2e).
- Test quality and assertion strength.
- Missing edge case coverage—be thorough.
- Untested failure modes and error paths.

### 7.4 Performance Review
Evaluate:
- N+1 queries and database access patterns.
- Memory-usage concerns.
- Caching opportunities.
- Slow or high-complexity code paths.

### 7.5 For Each Issue Found
For every specific issue (bug, smell, design concern, or risk):
- Describe the problem concretely, with file and line references.
- Present 2–3 options, including "do nothing" where that's reasonable.
- For each option, specify: implementation effort, risk, impact on other code, and maintenance burden.
- Give me your recommended option and why, mapped to my preferences above.
- Then explicitly ask whether I agree or want to choose a different direction before proceeding.

### 7.6 Workflow and Interaction
- Do not assume my priorities on timeline or scale.
- After each section, pause and ask for my feedback before moving on.

BEFORE YOU START:
Ask if I want one of two options:
1/ BIG CHANGE: Work through this interactively, one section at a time (Architecture → Code Quality → Tests → Performance) with at most 4 top issues in each section.
2/ SMALL CHANGE: Work through interactively ONE question per review section

FOR EACH STAGE OF REVIEW: output the explanation and pros and cons of each stage's questions AND your opinionated recommendation and why, and then use AskUserQuestion. Also NUMBER issues and then give LETTERS for options and when using AskUserQuestion make sure each option clearly labels the issue NUMBER and option LETTER so the user doesn't get confused. Make the recommended option always the 1st option.

## 8. [TODO] Project-Specific Decisions

- [ ] Define specific logging levels and formats.
- [ ] Detail specific pre-commit hooks or CI checks.
- [ ] Outline deployment strategy for models.

---
