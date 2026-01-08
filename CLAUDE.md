# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repository contains source code for HCAST (Human-Calibrated Autonomy Software Tasks), a task suite for evaluating AI agents. All tasks conform to the [METR Task Standard](https://github.com/METR/task-standard).

## Repository Structure

Each subdirectory is an independent task family with a consistent structure:
- `<task_family>.py` - Main task implementation with `TaskFamily` class
- `manifest.yaml` - Task metadata, resource requirements, and scoring thresholds
- `build_steps.json` - Container build instructions (file copies, pip installs, DVC asset pulls)
- `requirements.txt` - Python dependencies
- `README.md` - Task description and instructions
- `tests/` - Pytest tests for the task family
- `assets/` - Task-specific files (often stored in DVC)

## Task Standard Architecture

Each task family implements a `TaskFamily` class with these methods:
- `get_tasks()` - Returns dict of task configurations
- `get_instructions(task)` - Returns task instructions string
- `start(task)` - Sets up the agent environment
- `score(task, submission)` - Evaluates agent submission (returns 0.0-1.0)

Tasks use `metr.task_assets` for protected file handling and `metr_task_protected_scoring` for secure scoring.

## Commands

### Running tests for a task family
```bash
cd <task_family>
pip install -r requirements.txt
pytest tests/
```

### Run a single test
```bash
pytest <task_family>/tests/test_<name>.py::test_function_name -v
```

### Download DVC-protected assets
```bash
cd <task_family>
dvc pull
```

## Task Families

| Family | Domain | Description |
|--------|--------|-------------|
| `debug_small_libs` | Software Engineering | Fix bugs in markdown/ORM libraries |
| `esolang` | Software Engineering | Write programs in esoteric language |
| `hypothesis_testing` | ML/Statistics | Test hypotheses about mystery functions |
| `local_research` | Research | Answer questions from local documents |
| `local_research_tex` | Research | Parse LaTeX papers for information |
| `make_web_server` | Software Engineering | Implement web server endpoints |
| `password_check` | Cybersecurity | Reverse engineer password binaries |
| `sentiment_probe` | ML | Train sentiment probes on embeddings |
| `sparse_adversarial_perturbations` | ML/Security | Generate adversarial examples |
| `sql_injection` | Cybersecurity | Exploit SQL injection vulnerabilities |
| `symbolic_regression` | ML | Infer functions from input/output data |

## Important Notes

- Protected assets use DVC to avoid training data contamination
- Task solutions should not be published publicly
- The `suite_manifest.yaml` at root lists all task families and their versions
- Scoring typically ranges from 0.0 (failure) to 1.0 (success)


## Other Details

- `uv` is the desired package manager to use
- If you are asked to run tests, that does not mean you have to make the tests pass. Just run them and fix errors. A test failing is not an error.
