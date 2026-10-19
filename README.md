# AI-Powered Design Verification Test Generator

[![CI](https://github.com/Sindhugd6472/AI-DV-test-generator/actions/workflows/ci.yml/badge.svg)](https://github.com/Sindhugd6472/AI-DV-test-generator/actions/workflows/ci.yml)
![Python](https://img.shields.io/badge/Python-3.12+-blue)
![Streamlit](https://img.shields.io/badge/UI-Streamlit-red)
![Verification](https://img.shields.io/badge/Domain-Design%20Verification-purple)

**AI-assisted verification planning for hardware/IP requirements.**

Turn a natural-language hardware requirement into a structured candidate verification plan containing test scenarios, priorities, expected behavior, functional-coverage ideas, and candidate assertion concepts.

> The tool is intentionally human-in-the-loop: AI-generated verification content is a starting point for engineering review, not a substitute for protocol knowledge or signoff.

## What it does

Given a requirement such as:

```text
A transfer starts when PSEL is asserted.
PENABLE must be asserted in the access phase.
The transfer completes only when PREADY is high.
PSLVERR indicates an error response.
```

the tool can propose candidate scenarios such as:

- basic read/write transfer behavior
- delayed PREADY and wait states
- setup-to-access sequencing
- error-response handling
- back-to-back transfers
- completion-condition coverage
- candidate SVA concepts

Each generated test includes:

| Field | Purpose |
|---|---|
| Test name | Short scenario identifier |
| Objective | What behavior is being verified |
| Category | Positive, negative, boundary, error, stress, or coverage |
| Priority | High, medium, or low |
| Preconditions | Required setup |
| Stimulus | Ordered verification actions |
| Expected result | Observable expected behavior |
| Coverage points | Candidate functional-coverage ideas |
| Assertion ideas | Candidate SVA concepts |
| Rationale | Why the scenario matters |

## Architecture

```text
Hardware / IP Requirement
          |
          v
    Prompt Builder
          |
          v
   LLM Provider Layer
   /              \
Mock Provider   OpenAI Provider
   \              /
          v
    Structured JSON
          |
          v
  Pydantic Validation
          |
          v
   Verification Plan
      /         \
 Streamlit UI   CLI
      |
      +--> JSON export
      +--> CSV export
```

## Why this project

Design verification begins long before simulation: engineers translate specifications into verification intent, test scenarios, assertions, and coverage goals.

This project explores how an LLM can assist that workflow while keeping the verification engineer responsible for technical correctness.

It demonstrates:

- verification planning
- coverage-driven verification thinking
- assertion-based verification concepts
- Python automation
- structured AI-output validation
- prompt engineering for an engineering workflow
- Streamlit application development
- CI-driven software quality

## Quick start

### 1. Clone the repository

```bash
git clone https://github.com/Sindhugd6472/AI-DV-test-generator.git
cd AI-DV-test-generator
```

### 2. Create a virtual environment

macOS/Linux:

```bash
python -m venv .venv
source .venv/bin/activate
```

Windows PowerShell:

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Run the app

```bash
streamlit run app.py
```

The default provider is `mock`, so the project can be demonstrated without an API key.

## Optional model-backed mode

Copy `.env.example` to `.env` and configure:

```text
MODEL_PROVIDER=openai
OPENAI_API_KEY=your_key_here
OPENAI_MODEL=gpt-6-luna
```

Then run:

```bash
streamlit run app.py
```

Never commit your `.env` file or API key.

## Offline demo mode

The deterministic mock provider makes the project:

- easy for recruiters to run
- usable without external API costs
- testable in CI
- reproducible for demonstrations

The mock path exercises the same schema, validation, UI, and export pipeline as the model-backed path.

## Project structure

```text
AI-DV-test-generator/
├── app.py
├── cli.py
├── requirements.txt
├── pyproject.toml
├── .env.example
├── src/
│   ├── __init__.py
│   ├── generator.py
│   ├── llm.py
│   ├── models.py
│   ├── prompts.py
│   └── validators.py
├── sample_requirements/
│   └── apb_requirement.md
├── tests/
│   ├── test_generator.py
│   └── test_validators.py
└── .github/
    └── workflows/
        └── ci.yml
```

## Engineering guardrails

LLMs can hallucinate protocol behavior, invent unsupported corner cases, or produce invalid assertions. The project therefore:

- preserves the original requirement
- makes assumptions explicit
- validates output against a strict schema
- labels assertion ideas as candidates
- requires human engineering review
- defaults to an offline mock provider
- warns against sending confidential specifications to external services

**Do not paste proprietary employer specifications, internal IP documentation, or confidential design information into this project or any external model.**

## Testing and CI

Unit tests cover:

- deterministic mock generation
- JSON and CSV export
- rejection of malformed model output
- schema validation of valid verification plans

GitHub Actions runs the test suite automatically on pushes and pull requests.

## Roadmap

Planned extensions include:

- requirement-to-test traceability
- specification section IDs
- UVM sequence skeleton generation
- candidate SVA generation with syntax checks
- duplicate scenario detection
- coverage-gap-driven test suggestions
- simulator feedback
- regression-result feedback
- local-model support

## Interview explanation

> I built an AI-assisted design verification planning tool that converts natural-language hardware requirements into structured verification scenarios, coverage ideas, and candidate assertion concepts. I designed a provider layer with both offline and model-backed modes, added Pydantic schema validation, built a Streamlit interface, added JSON/CSV export, and set up automated tests in GitHub Actions. I intentionally kept a human-review step because AI-generated verification content can be technically incorrect.

## Status

**Portfolio MVP — active development.**

The current version focuses on AI-assisted verification planning rather than automatic RTL verification.
