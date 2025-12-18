# 🛡️ Hybrid Phishing Email Classification: ML + LLM

A comprehensive research project exploring how Large Language Models (LLMs) can augment traditional Machine Learning for phishing email detection, explainability, and multilingual support.

**Author:** Farouk Aziz (BQ2AQM)  
**Contact:** azizfarouk85@gmail.com

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Datasets](#datasets)
- [Research Structure](#research-structure)
- [Results](#results)

## 🎯 Project Overview

This research demonstrates a **hybrid framework** that combines traditional Machine Learning with Large Language Models (LLMs) to create a more accurate, explainable, and multilingual phishing detection system. Rather than replacing ML models entirely, LLMs are strategically applied to enhance specific aspects of the detection pipeline.

### Research Objectives

This research investigates whether LLMs can:

1. **Improve classification performance** when integrated with ML models
2. **Correct the most uncertain ML misclassifications** efficiently without full-scale deployment
3. **Convert technical ML explanations** into human-readable language for non-technical users
4. **Enhance phishing detection for non-English emails** through intelligent translation

### Three-Component Hybrid Approach

#### 1. Hybrid Classification (Error Correction)
Apply LLMs only to the most uncertain ML misclassifications to improve accuracy with minimal computational overhead. This selective approach balances performance gains with cost-effectiveness.

#### 2. Hybrid Explainability (LIME + LLM)
Convert technical LIME feature attributions into natural language explanations, making ML predictions accessible and interpretable for end users.

#### 3. Hybrid Translation (Non-English Email Detection)
Use LLM-based translation as preprocessing for non-English phishing emails, preserving phishing semantics and suspicious URLs while handling informal language effectively.

## 📦 Datasets

### 1. Phishing Email Dataset (Training)
- **Source**: [Kaggle - Phishing Email Dataset](https://www.kaggle.com/datasets/naserabdullahalam/phishing-email-dataset)
- **Filename**: `phishing_email.csv`

### 2. Phishing Email Dataset (Testing)
- **Source**: [Kaggle - Phishing Emails](https://www.kaggle.com/datasets/subhajournal/phishingemails)
- **Filename**: `Dataset.csv`

## 🧩 Research Structure

### Traditional ML Pipeline

The baseline ML pipeline establishes high-performance phishing detection using classical techniques:

**Feature Engineering:**
- TF-IDF vectorization with unigrams, bigrams, and trigrams
- Captures single words, phrase patterns, and multi-word phishing indicators

**Models Evaluated:**
- Logistic Regression
- Complement Naive Bayes
- Linear SVM (calibrated with sigmoid kernel)
- Voting Classifier (ensemble)

**Selected Model:** SVM with sigmoid kernel achieved the highest ROC-AUC (0.9988)

**Evaluation Strategy:**
- 10-fold stratified cross-validation
- 80/20 train-test split
- Metrics: Accuracy, Precision, Recall, F1-score, ROC-AUC

**Output Artifacts:**
- `model.pkl` - Trained SVM classifier
- `tfidf_vectorizer.pkl` - Feature extractor

### Workflow Overview

**Step 1: ML Pipeline (`ML_pipeline.ipynb`)**
- Load and preprocess phishing email dataset
- Train multiple classifiers with TF-IDF features
- Evaluate and select best model (SVM sigmoid)
- Save trained model and vectorizer

**Step 2: Hybrid Classification (`hybrid_classification.ipynb`)**
- Load pretrained ML model
- Identify incorrectly classified emails
- Rank ML mistakes by prediction uncertainty
- Apply LLMs (DeepSeek, GPT-4, Claude 3.5) to top-25 uncertain cases
- Compare LLM error correction performance

**Step 3: Hybrid Explainability (`hybrid_explenation.ipynb`)**
- Generate LIME explanations for ML predictions
- Convert feature attributions to natural language using LLMs
- Evaluate explanation readability (Flesch Reading Ease scores)
- Compare readability across different LLMs

**Step 4: Hybrid Translation (`hybrid_translation.ipynb`)**
- Test ML model on raw non-English emails (baseline)
- Translate non-English emails using each LLM
- Classify translated emails with ML model
- Compare translation-enhanced performance

## 📊 Results

### 1. Traditional ML Performance (ROC-AUC)

| Model | ROC-AUC | Training Time | Prediction Time |
|-------|---------|---------------|-----------------|
| Logistic Regression | 0.9984 | 0.62s | 5.3ms |
| Complement Naive Bayes | 0.9954 | 0.06s | 13.0ms |
| **SVM (Sigmoid Kernel)** | **0.9988** ✅ | 9.36s | 35.8ms |
| Voting Classifier | 0.9987 | 9.96s | 81.3ms |

**Key Finding:** SVM with sigmoid kernel achieved the best performance with near-perfect classification (ROC-AUC: 0.9988).

### 2. Hybrid Classification Results (25 Uncertain ML Mistakes)

| LLM | Accuracy | ML Mistakes Fixed | Avg Time per Email |
|-----|----------|-------------------|--------------------|
| **DeepSeek** | **0.60** | **15/25** ✅ | 16.23s |
| Claude 3.5 | 0.32 | 8/25 | 5.33s |
| GPT-4 | 0.16 | 4/25 | 4.80s |

**Key Finding:** DeepSeek corrected 60% of the most uncertain ML errors, demonstrating that selective LLM integration can significantly improve classification without processing all emails.

**Cost-Efficiency Analysis:**
- ML-only prediction: 0.62ms per email (10.87s total for dataset)
- Hybrid approach: Apply LLM only to uncertain cases (25 emails out of thousands)
- Result: Substantial accuracy gain with minimal additional computation

### 3. Hybrid Explainability Results (Readability Comparison)

| LLM | Flesch Reading Ease Score | Avg Generation Time | Interpretation |
|-----|---------------------------|---------------------|----------------|
| **GPT-4** | **76** ✅ | 10.73s | Most readable (fairly easy) |
| DeepSeek | 68 | 10.62s | Moderately readable |
| Claude 3.5 | 54 | 7.76s | More technical (standard) |

**Key Finding:** GPT-4 produced the most accessible explanations for non-technical users, successfully converting LIME feature attributions into human-readable language.

**Explanation Quality:** Higher Flesch Reading Ease scores indicate simpler language that's easier for general audiences to understand.

### 4. Hybrid Translation Results (Non-English Detection)

| Approach | Accuracy | F1-Score | Precision | Avg Time per Email |
|----------|----------|----------|-----------|---------------------|
| ML Only (Raw) | 0.52 | 0.67 | - | 8.68ms |
| ML + DeepSeek | 0.75 | 0.79 | - | 2.08s |
| **ML + GPT-4** | **0.83** ✅ | **0.85** ✅ | **Highest** | 1.37s |
| ML + Claude 3.5 | 0.80 | 0.82 | - | 3.81s |

**Key Finding:** LLM-based translation dramatically improved non-English phishing detection. GPT-4 translation achieved the best performance with 59% accuracy improvement over raw ML (0.83 vs 0.52).

**Translation Advantages:**
- Preserves phishing-related semantics and context
- Maintains URLs and suspicious links unchanged
- Handles informal language, slang, and cultural nuances better than standard translation APIs

### Performance Summary

**LLM Strengths by Task:**
- **Best Error Correction:** DeepSeek (60% accuracy on uncertain cases)
- **Best Explainability:** GPT-4 (Flesch score: 76)
- **Best Translation:** GPT-4 (83% accuracy, 85% F1-score)
- **Fastest Explanations:** Claude 3.5 (7.76s per explanation)

### Key Contributions

1. **Selective LLM Integration:** Demonstrated that targeted LLM usage on uncertain cases is more cost-effective than full-scale deployment
2. **Explainability Enhancement:** Proved that LLMs can bridge the gap between technical ML outputs and user understanding
3. **Multilingual Robustness:** Showed that LLM translation enables effective phishing detection across languages (59% accuracy improvement)
4. **Comparative Analysis:** Provided comprehensive benchmarking of three major LLMs (DeepSeek, GPT-4, Claude 3.5) across multiple dimensions

---

**💡 This research demonstrates that hybrid ML+LLM approaches offer superior performance, explainability, and multilingual capabilities compared to either approach alone.**