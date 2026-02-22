# 🛡️ Hybrid Phishing Email Classification: ML + LLM

A research project demonstrating how Large Language Models can strategically augment traditional Machine Learning for phishing detection.

**Author:** Farouk Aziz (BQ2AQM)  
**Contact:** azizfarouk85@gmail.com

---

## 🎯 Project Overview

This hybrid framework combines traditional ML with LLMs to create a more accurate, explainable, and multilingual phishing detection system. LLMs are applied strategically to enhance specific aspects rather than replacing ML entirely.

---

## 📦 Datasets

### ML Training Dataset (Model Training & Internal Evaluation)
- **Source**: [Kaggle](https://www.kaggle.com/datasets/subhajournal/phishingemails)
- **Filename**: `Phishing_Email.csv`
- **Size**: 17,496 emails after cleaning
- **Distribution**: 10,972 Safe (0) | 6,524 Phishing (1)

### Hybrid Model Testing Dataset
- **Source**: [HuggingFace](https://huggingface.co/datasets/zionia/phishing-emails/tree/main/data)
- **Filename**: `test_dataset.csv` (converted from `test-00000-of-00001-8a6d39996ec0fb5b.parquet`)
- **Size**: 16,478 emails
- **Distribution**: 7,886 Safe (0) | 8,592 Phishing (1)

---

## 🧩 Research Structure

### 1. ML Pipeline (`ML_pipeline.ipynb`)

- **Text Preprocessing**: Advanced noise removal, normalization, duplicate removal, and filtering of very short emails  
- **Feature Engineering**: TF-IDF vectorization with 10,000 features and n-grams (1–3), including URL and email pattern handling  
- **Trained Model**: Linear Support Vector Machine (LinearSVC) with sigmoid probability calibration  
- **Probability Calibration**: `CalibratedClassifierCV` using 5-fold cross-validation  
- **Evaluation Strategy**: Stratified 80/20 train–test split with 10-fold stratified cross-validation.


### 2. Hybrid Classification (`hybrid_classification.ipynb`)
- Identify ML misclassifications on test set
- Apply LLMs to most uncertain errors
- Compare error correction performance across providers

### 3. Hybrid Explainability (`hybrid_explanation.ipynb`)
- Generate LIME explanations for random emails
- Convert technical outputs to natural language using LLMs
- Evaluate readability using Flesch Reading Ease scores

### 4. Hybrid Translation (`hybrid_translation.ipynb`)
- Test non-English emails across multiple languages
- Translate using LLMs with phishing-aware prompts
- Compare ML performance on raw vs. translated emails

---

## 📊 Key Results

### 1. Traditional ML Baseline (SVM with Sigmoid Calibration)

#### Training Performance (10-Fold Cross-Validation)
- **Cross-Validation F1**: 0.9809 ± 0.0046

#### Test Set Performance (3,500 emails)
| Metric | Score |
|--------|-------|
| **Accuracy** | 0.9837 |
| **Precision** | 0.9800 |
| **Recall** | 0.9762 |
| **F1 Score** | 0.9781 |
| **ROC-AUC** | 0.9986 |
| **Avg Precision** | 0.9977 |

#### Confusion Matrix
- True Negatives: 2,169
- False Positives: 26
- False Negatives: 31
- True Positives: 1,274

#### Additional Metrics
- **Specificity**: 0.9882
- **False Positive Rate**: 0.0118
- **False Negative Rate**: 0.0238

---

### 2. Hybrid Classification Results

#### ML-Only Performance on Full Test Dataset (16,478 emails)
| Metric | Score |
|--------|-------|
| **Accuracy** | 0.9030 |
| **Precision** | 0.9183 |
| **Recall** | 0.8935 |
| **F1 Score** | 0.9057 |

**Confusion Matrix:**
- True Negatives: 7,203
- False Positives: 683
- False Negatives: 915
- True Positives: 7,677

**Total ML Mistakes**: 1,598 (683 FP + 915 FN)

#### ML Prediction Speed
- **Total time**: 5.55 seconds for 16,478 emails
- **Average per email**: 0.34 ms

---

#### LLM Error Correction on Most Uncertain ML Mistakes

| LLM | Accuracy | Mistakes Fixed | Avg Time/Email |
|-----|----------|----------------|----------------|
| **GPT** | 68.00%  | 170/250 | **0.67s** ⚡ |
| **Claude** | **93.60%** ✅ | 234/250 | 2.06s |
| DeepSeek | 57.94% | 135/250 | 14.46s |

#### LLM Timing Summary
| Provider | Total Time | Avg per Email |
|----------|------------------------|---------------|
| **GPT** | **167.64s** ⚡ | 0.67s |
| Claude | 514.85s | 2.06s |
| DeepSeek | 3614.80s | 14.46s |

---

### 3. Hybrid Explainability Results

#### LLM Explanation Generation Time
| Provider | Total Time | Avg per Explanation |
|----------|------------|---------------------|
| **GPT** | **20.73s** ⚡ | 4.15s (4146ms) |
| DeepSeek | 33.62s | 6.72s (6725ms) |
| Claude | 35.22s | 7.04s (7045ms) |

#### Readability Scores (Flesch Reading Ease)
| Provider | Mean Score | Std Dev |
|----------|------------|---------|
| **DeepSeek** | **65.59** ✅ | 6.65 |
| GPT | 62.70 | 14.67 |
| Claude | 32.13 | 16.05 |

*Higher Flesch scores = more readable for non-technical users (0-100 scale)*

---

### 4. Hybrid Translation Results

#### Performance Comparison Across Translation Approaches

| Approach | Accuracy | Precision | Recall | F1 Score |
|----------|----------|-----------|--------|----------|
| ML Only (Raw) | 50.0% | 50.0% | 66.7% | 0.571 |
| **ML + DeepSeek** | **75.0%** ✅ | **68.0%** | **94.4%** ✅ | **0.791** ✅ |
| ML + GPT | 63.9% | 60.0% | 83.3% | 0.698 |
| ML + Claude | 58.3% | 55.2% | 88.9% | 0.681 |

**Translation Improvement**: +25.0% accuracy boost (DeepSeek) over raw ML baseline

#### Latency Analysis

| Approach | Avg ML Time | Avg Translation Time | Avg Total Time/Email |
|----------|-------------|----------------------|----------------------|
| **ML Only** | 6.83 ms | N/A | **6.83 ms** ⚡ |
| **ML + GPT** | 9.16 ms | 838 ms | **847 ms** ⚡ |
| ML + Claude | 8.57 ms | 2,025 ms | 2,033 ms |
| ML + DeepSeek | 9.66 ms | 2,602 ms | 2,611 ms |

---

## 💡 Key Takeaways

1. **Claude achieves the highest correction accuracy** (93.6% on the 250 most uncertain ML mistakes), outperforming GPT and DeepSeek.
2. **GPT is the fastest LLM** across tasks
3. **DeepSeek produces most readable explanations** (Flesch: 65.59) and achieves best translation accuracy (75%)
4. **Selective LLM integration is cost-effective**: Process all emails with fast ML (0.34ms), apply LLM only to uncertain cases
5. **Translation dramatically improves multilingual detection**: 25% accuracy boost with DeepSeek
6. **Hybrid approach outperforms ML alone** while maintaining efficiency for standard cases

---

*This research proves that strategic LLM integration enhances ML phishing detection where it matters most: uncertain predictions, user explanations, and non-English content.*