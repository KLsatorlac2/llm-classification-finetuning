# LLM Classification Finetuning

A QLoRA-based solution for the Kaggle **LLM Classification Finetuning** competition.

---

## Overview

Given a user prompt and two anonymous LLM responses, the model predicts which response is preferred by humans:

- `winner_model_a`
- `winner_model_b`
- `winner_tie`

The task is formulated as a 3-class sequence classification problem.

---

## Method

- **Base Model:** Qwen2.5-1.5B-Instruct
- **Fine-tuning:** QLoRA
- **Quantization:** 4-bit
- **PEFT:** LoRA
- **Task:** 3-class sequence classification
- **Data Augmentation:** A/B response swapping
- **Test-Time Augmentation:** Original + swapped A/B predictions
- **Evaluation Metric:** Log Loss

---

## Training Configuration

| Parameter | Config |
|---|---|
| Base Model | Qwen2.5-1.5B-Instruct |
| Quantization | 4-bit |
| LoRA Rank | 16 |
| LoRA Alpha | 32 |
| Max Length | 1024 |
| Batch Size | 2 |
| Gradient Accumulation | 4 |
| Epochs | 1 |
| Learning Rate | 2e-5 |
| Precision | FP16 |

**Actual Effective Batch Size:** `2 × 4 = 8`

---

## Input Format

The prompt and two responses are converted into a single classification sequence.

Conceptually:
```text
Prompt:
{prompt}

Response A:
{response_a}

Response B:
{response_b}
```

The model then predicts one of three classes:
```text
A wins
B wins
Tie
```

---

## Submission

The final submission contains:
```text
id
winner_model_a
winner_model_b
winner_tie
```

Example:
```text
id,winner_model_a,winner_model_b,winner_tie
0,0.42,0.37,0.21
1,0.15,0.71,0.14
2,0.33,0.29,0.38
```
The three probability columns should sum to approximately: 1.0

---

## Project Structure

```text
llm-classification-finetuning/
│
├── README.md
├── .gitignore
├── .gitattributes
├── requirements.txt
│
└── notebooks/
    └── llm_classification_qllora.ipynb
```

The main implementation is contained in the Kaggle notebook.

---

## Simplified Prediction Pipeline

```text
Prompt
  +
Response A
  +
Response B
      │
      ▼
Qwen2.5-1.5B-Instruct
      │
      ▼
4-bit QLoRA
      │
      ▼
3-Class Classification(A,B,Tie)
```

---

## Future Improvements

Possible directions for future experiments include:

* Larger Qwen models
* Longer sequence lengths
* More training epochs
* Learning-rate tuning
* LoRA rank/alpha tuning
* Better prompt formatting
* More sophisticated data augmentation
* Ensemble of multiple checkpoints
* Multiple-model ensemble
* Improved probability calibration

These improvements are not required for the current baseline pipeline.

---

## Summary

This project implements a lightweight LLM preference classifier using:

```text
Qwen2.5-1.5B-Instruct
          +
4-bit Quantization
          +
QLoRA
          +
A/B Swap Augmentation
          +
3-Class Classification
          +
Test-Time Augmentation
```

The goal is to build a memory-efficient and reproducible Kaggle solution for predicting human preferences between two anonymous LLM responses.

---
