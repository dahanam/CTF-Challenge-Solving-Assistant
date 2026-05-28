# CTF Challenge-Solving Assistant — LLM Agent

An AI-powered agent that autonomously solves beginner and intermediate 
text-based Capture The Flag (CTF) cybersecurity challenges using GPT-4o, 
chain-of-thought prompting, custom CTF tools, and Retrieval-Augmented 
Generation (RAG).

**NOTE:** Need to add docker!

---

## Overview

This project implements and evaluates three progressively advanced LLM 
agent strategies for solving CTF challenges without a Docker environment:

| Strategy | Description |
|----------|-------------|
| **Zero-Shot Baseline** | GPT-4o with chain-of-thought prompting only |
| **Few-Shot Prompt Tuning** | GPT-4o with in-context examples |
| **RAG Agent** | GPT-4o + retrieval from a holdout challenge corpus |

---

## Key Results

| Dataset | Zero-Shot | Few-Shot | RAG |
|---------|-----------|----------|-----|
| InterCode-CTF | 25% | — | **75%** (+50%) |
| NYU CTF Bench | 0% | — | 0% |

RAG was invoked on all 26 challenges and produced a **+50% task success 
rate improvement** on InterCode-CTF by retrieving from a closely matched 
holdout corpus. NYU CTF Bench results confirm that RAG is most effective 
when the retrieval corpus matches the test distribution.

---

## Agent Tools

The agent has access to four custom CTF-solving tools:

- **Base64 Decode** — Decodes Base64-encoded strings
- **Hex Decode** — Converts hexadecimal to plaintext
- **Caesar Brute-Force** — Tries all 25 Caesar cipher rotations
- **Python Execution** — Runs arbitrary Python code for complex challenges

---

## Datasets

- **InterCode-CTF** (`ic_ctf.json`) — 100 text-based picoCTF challenges, 
  no Docker required. Covers the same challenges as the PicoCTF Writeups 
  dataset referenced in the original proposal.
- **NYU CTF Bench** — Development and test splits filtered to text-solvable 
  categories (`cry`, `misc`) using the `nyuctf` library

---

## Evaluation

Task 4 includes a full ablation study comparing all three strategies across 
both datasets on:
- **Exact Match Accuracy**
- **F1 / Task Success Rate**
- **Average Partial Score**

Ablation conditions:
- Removing retrieval → RAG vs Zero-Shot (retrieval contribution)
- Removing tools → Zero-Shot vs No-Tools (tool contribution)
- Removing CoT prompt → Few-Shot partial score drop (prompt impact)

---

## Tech Stack

- **Python** — Core language
- **OpenAI GPT-4o** — Core reasoning model
- **LangChain / OpenAI API** — Agent framework and API calls
- **nyuctf** — NYU CTF Bench dataset loader
- **Google Colab** — Development environment
- **Datasets:** InterCode-CTF (picoCTF), NYU CTF Bench

---

## Setup

### Requirements
```bash
pip install openai nyuctf pandas numpy tqdm
```

### API Key
This notebook uses the OpenAI API. Add your key to Colab Secrets:
1. Click the 🔑 icon in the left sidebar
2. Add a secret named `OPENAI_API_KEY`
3. The notebook loads it automatically via `userdata.get("OPENAI_API_KEY")`

### Datasets
- **InterCode-CTF:** Place `ic_ctf.json` in your Google Drive and update 
  the path in the notebook
- **NYU CTF Bench:** Place the dataset folder in 
  `MyDrive/CTF_Dataset` and mount your Drive when prompted

---

## Project Structure

| Section | Description |
|---------|-------------|
| Task 1 | Dataset loading and preprocessing |
| Task 2 | Baseline LLM agent (Zero-Shot + Chain-of-Thought) |
| Task 3 | Few-Shot prompt tuning + RAG agent with tool use |
| Task 4 | Evaluation and ablation study |

---

## Authors

Dahana Moz Ruiz & Maria Santos — Kean University, Spring 2026
