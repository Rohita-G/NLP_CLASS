
# CS5760 Natural Language Processing — Fall 2026
## Homework 2

**Student Name:** Rohita Gangishetty

---

## Overview

This assignment has two parts:

1. **Confusion Matrix Evaluation** — computing precision, recall, and their macro- and micro-averages from a multi-class confusion matrix.
2. **Bigram Language Model** — building a simple bigram language model from a small text corpus, calculating unigram/bigram counts and probabilities, and using the model to score and compare sentences.

---

## Part I: Confusion Matrix Evaluation

### What this does

Given a 3-class confusion matrix (Cat, Dog, Rabbit), the code computes:
- Per-class **True Positives (TP)**, **False Positives (FP)**, and **False Negatives (FN)**
- Per-class **Precision** and **Recall**
- **Macro-averaged** Precision and Recall (simple average across classes — treats every class equally)
- **Micro-averaged** Precision and Recall (pools TP/FP/FN across all classes first — treats every sample equally)

### Confusion Matrix Used

| Actual \ Predicted | Cat | Dog | Rabbit |
|---|---|---|---|
| **Cat** | 5 | 10 | 5 |
| **Dog** | 15 | 20 | 10 |
| **Rabbit** | 0 | 15 | 10 |

### Results

| Class | TP | FP | FN | Precision | Recall |
|---|---|---|---|---|---|
| Cat | 5 | 15 | 15 | 0.250 | 0.250 |
| Dog | 20 | 25 | 25 | 0.444 | 0.444 |
| Rabbit | 10 | 15 | 15 | 0.400 | 0.400 |

- **Macro Precision** = 0.365
- **Macro Recall** = 0.365
- **Micro Precision** = 0.389
- **Micro Recall** = 0.389

### Interpretation

Macro-averaging treats every class equally regardless of how many samples it has, which is useful when class fairness matters. Micro-averaging pools all predictions together first, so larger classes (like Dog, with more total samples) have a bigger influence on the final score. In this dataset, the two averages come out close to each other, but they would diverge more with a more imbalanced matrix.

---

## Part II: Bigram Language Model

### What this does

Using a small 3-sentence training corpus, the code:
1. Counts how often each individual word appears (**unigram counts**)
2. Counts how often each pair of consecutive words appears (**bigram counts**)
3. Computes bigram probabilities using **Maximum Likelihood Estimation (MLE)**:

   ```
   P(word2 | word1) = count(word1, word2) / count(word1)
   ```

4. Uses these bigram probabilities to compute the overall probability of a full sentence (by multiplying the bigram probabilities along the sentence)
5. Compares two candidate sentences to see which one the model considers more probable

### Training Corpus

```python
corpus = [
    ["<s>", "I", "love", "NLP", "</s>"],
    ["<s>", "I", "love", "deep", "learning", "</s>"],
    ["<s>", "deep", "learning", "is", "fun", "</s>"]
]
```

`<s>` marks the start of a sentence and `</s>` marks the end.

### Results

**Sentence probabilities:**
- P(`<s> I love NLP </s>`) = **0.333**
- P(`<s> I love deep learning </s>`) = **0.167**

**Model preference:** The model prefers **Sentence 1** (`"I love NLP"`), since it has the higher overall probability.

### Interpretation

Sentence 1 is shorter, and since every bigram probability is a fraction ≤ 1, each additional word in a sentence adds one more multiplication step that can only lower (or at best maintain) the overall probability. This is why Sentence 2 — being one word longer — ends up with a lower total probability, even though both sentences are grammatically valid and appear in the training data.

---

## How to Run

1. Open `NLP_Homework_2.ipynb` in Jupyter Notebook, JupyterLab, or Google Colab.
2. Run all cells in order from top to bottom.
3. Part I (Confusion Matrix) and Part II (Bigram Language Model) are self-contained — no external files or datasets are required, since the confusion matrix and training corpus are both defined directly in the notebook.

### Requirements

- Python 3
- No external libraries required (uses only built-in Python — no numpy/pandas/sklearn dependency)

---

## Files in this Repository

| File | Description |
|---|---|
| `NLP_Homework_2.ipynb` | Main notebook containing both Part I and Part II |
| `README.md` | This file — explains the assignment, approach, and results |

---

## Submission Notes

- Source code pushed to this GitHub repository as required.
- Code is commented throughout to explain each step (TP/FP/FN calculation, MLE formula, sentence scoring logic, etc.).
- GitHub link submitted on BrightSpace per course instructions.
