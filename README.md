# LLM-PeerReview

This repository contains the official implementation of the paper **"Scoring, Reasoning, and Selecting the Best! Ensembling Large Language Models via a Peer-Review Process"**.

LLM-PeerReview is an unsupervised framework designed to select the most ideal response from multiple LLM-generated candidates by mimicking the academic peer-review process.

## Repository Overview

This project includes:

- **LLM-PeerReview Variants**: `Average` and `Weighted` (incorporating truth inference).
- **Baselines**: Reproductions of several ensemble methods including Random, GaC, Agent-Forest, and Smoothie.
- **Benchmarks**: Support for evaluation on GSM8K, MATH, TriviaQA, and AlpacaEval.

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

Generate responses from various LLMs on benchmark datasets:

* **Standard 7B Models**: Generate responses using four different 7B models (Llama-3.1-8B, Mistral-7B, Qwen2-7B, Qwen2.5-7B) on the target datasets.

```bash
bash ./Script/Response_Generate/New_7B_Response_Generate.sh
```

**Output**: Generated responses are saved in the LLM_Response/ directory with organized subfolders for each model and dataset.

### 2.2 Response Scoring (PeerReview Method)

Score the generated responses using our PeerReview methodology. We provide scoring scripts for different datasets:

* **GSM8K Dataset**:

```bash
bash ./Script/Response_Scoring/judge/judge_gsm8k400.sh
```

**Note**: The scoring process leverages the LLM-as-a-Judge paradigm, where each available LLM acts as a reviewer to evaluate and assign scores to all candidate responses, forming the foundation for subsequent ensemble selection.

### 2.3 Ensemble Methods

Combine multiple model responses using different ensemble strategies. We compare our proposed method against several established baselines:

1. **Random**: A random-selection baseline that returns a response from a randomly chosen LLM in the ensemble.

```bash
bash ./Script/Ensemble_Generate/Random_Generate.sh
```

2. **GaC**: A recent token-level ensemble-during-inference method.

```bash
bash ./Script/Response_Generate/GaC_7B_Response_Generate.sh
```

3. **Agent Forest**: A recently proposed similarity-based ensemble method.

```bash
bash ./Script/Ensemble_Generate/Agent_forest_Generate.sh
```

4. **Smoothie-Global & Smoothie-Local**: Strong similarity-based ensemble methods that operate at the global level and the local level, respectively.

```bash
# Global variant
bash ./Script/Ensemble_Generate/Smoothie-Global_Generate.sh

# Local variant
bash ./Script/Ensemble_Generate/Smoothie-Local_Generate.sh
```

5. **PeerReview Average (Ours)**: Our primary ensemble method which averages scores from multiple LLM judges.

```bash
bash ./Script/Ensemble_Generate/PeerReview_Average_Generate.sh
```

6. **PeerReview Average with Truth Inference (Ours)**: An enhanced variant that employs a graphical-model-based truth inference algorithm for reliability-aware score aggregation.

```bash
bash ./Script/Ensemble_Generate/PeerReview_Average_Ti_Generate.sh
```

### 2.4 Evaluation

Evaluate the quality of the generated responses and the performance of different ensemble methods:

1. **Single LLM Evaluation**: Evaluate responses from the standard 7B models.

```bash
bash ./Script/Response_Evaluation/New_7B_Response_Evaluate.sh
```

2. **GaC Baseline Evaluation**: Evaluate responses from the GaC baseline model.

```bash
bash ./Script/Response_Evaluation/GaC_7B_Response_Evaluate.sh
```

3. **Scored Response Evaluation**: Evaluate the outcomes after applying the PeerReview scoring process.

```bash
bash ./Script/Response_Evaluation/New_7B_Judge_Response_Evaluate.sh
```

4. **Baseline Ensemble Evaluation**: Evaluate the results produced by baseline ensemble methods (Random, Agent Forest, Smoothie).

```bash
bash ./Script/Response_Evaluation/Baseline_Ensemble_Response_Evaluate.sh
```

5. **PeerReview Ensemble Evaluation**: Evaluate the final outputs of our proposed PeerReview ensemble methods.

```bash
bash ./Script/Response_Evaluation/PeerReview_Average_Ensemble_Response_Evaluate.sh
```
