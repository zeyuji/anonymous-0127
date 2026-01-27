# LLM-PeerReview

This repository contains the official implementation of the paper **"Scoring, Reasoning, and Selecting the Best! Ensembling Large Language Models via a Peer-Review Process"**.

LLM-PeerReview is an unsupervised framework designed to select the most ideal response from multiple LLM-generated candidates by mimicking the academic peer-review process.

## Repository Overview

This project includes:

* **LLM-PeerReview Variants**: `Average` and `Weighted` (incorporating truth inference).
* **Baselines**: Reproductions of several ensemble methods including Random, GaC, Agent-Forest, and Smoothie.
* **Benchmarks**: Support for evaluation on GSM8K, MATH, TriviaQA, and AlpacaEval.

---

## 1. Environment & Setup

### 1.1 System Requirements

* **OS**: Linux (Ubuntu 20.04+ recommended)
* **Python**: 3.10.16 or higher
* **GPU**: NVIDIA V100 32GB or equivalent (required for model inference)
* **Storage**: 100GB+ SSD for models and datasets

### 1.2 Installation

We recommend using a virtual environment:

```bash
# Using conda
conda create -n llm-peerreview python=3.10.16
conda activate llm-peerreview

# Install dependencies
pip install -r requirements.txt
```

To download the required pre-trained models, run:

```bash
bash ./Script/LLM_Download.sh
```

---

## 2. Usage Pipeline

The workflow consists of four main steps:

### 2.1 Response Generation

Generate candidate responses from individual models (e.g., Llama-3.1-8B, Mistral-7B, Qwen series):

```bash
bash ./Script/Response_Generate/New_7B_Response_Generate.sh
```

### 2.2 Response Scoring

Apply the PeerReview scoring mechanism where models act as reviewers to evaluate each other:

```bash
# Example for GSM8K
bash ./Script/Response_Scoring/judge/judge_gsm8k400.sh
```

### 2.3 Ensemble Generation

Run the ensemble strategies to select the final output.

* **Baselines (Random, GaC, Agent Forest, Smoothie)**:
```bash
bash ./Script/Ensemble_Generate/Random_Generate.sh
bash ./Script/Ensemble_Generate/Smoothie-Global_Generate.sh
```


* **PeerReview (Ours)**:
```bash
# Standard Average
bash ./Script/Ensemble_Generate/PeerReview_Average_Generate.sh
# Enhanced with Truth Inference
bash ./Script/Ensemble_Generate/PeerReview_Average_Ti_Generate.sh
```

### 2.4 Evaluation

Evaluate the performance of individual models and ensemble methods:

```bash
# Evaluate PeerReview outputs
bash ./Script/Response_Evaluation/PeerReview_Average_Ensemble_Response_Evaluate.sh
```

---

## 3. Method Description

The LLM-PeerReview framework operates in three distinct phases:

1. **Scoring**: Each model in the ensemble generates a response and subsequently acts as a judge to score responses from other models.
2. **Reasoning**: A truth inference algorithm aggregates these scores, accounting for potential biases and the varying reliability of different judges.
3. **Selecting**: The response with the highest aggregated score is selected as the final output for the given query.
