# LLM Classification Finetuning

A QLoRA-based solution for the Kaggle **LLM Classification Finetuning** competition.

## Overview

Given a user prompt and two anonymous LLM responses, the model predicts which response is preferred by humans:

- `winner_model_a`
- `winner_model_b`
- `winner_tie`

The task is formulated as a 3-class sequence classification problem.

## Method

- **Base Model:** Qwen2.5-1.5B-Instruct
- **Fine-tuning:** QLoRA
- **Quantization:** 4-bit
- **PEFT:** LoRA
- **Task:** 3-class sequence classification
- **Data Augmentation:** A/B response swapping
- **Test-Time Augmentation:** Original + swapped A/B predictions
- **Evaluation Metric:** Log Loss

## Pipeline

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
