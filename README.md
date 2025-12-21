# 🛡️ Hybrid Phishing Email Classification: ML + LLM

A research project demonstrating how Large Language Models can strategically augment traditional Machine Learning for phishing detection.

**Author:** Farouk Aziz (BQ2AQM)  
**Contact:** azizfarouk85@gmail.com

## 🎯 Project Overview

This hybrid framework combines traditional ML with LLMs to create a more accurate, explainable, and multilingual phishing detection system. LLMs are applied strategically to enhance specific aspects rather than replacing ML entirely.

## 📦 Datasets

**Training**: Enron + Ling collections ([Kaggle](https://www.kaggle.com/datasets/naserabdullahalam/phishing-email-dataset)) - 26,097 emails  
**Testing**: Phishing Email Collection ([Kaggle](https://www.kaggle.com/datasets/subhajournal/phishingemails)) - 17,534 emails

## 🧩 Research Structure

### 1. ML Pipeline (`ML_pipeline.ipynb`)
- TF-IDF vectorization (10,000 features, n-grams 1-3)
- Trained models: Logistic Regression, Naive Bayes, SVM, Voting Classifier
- **Best model**: SVM with sigmoid kernel (ROC-AUC: 0.9984, F1: 0.9807)

### 2. Hybrid Classification (`hybrid_classification.ipynb`)
- Identify ML misclassifications on test set
- Apply LLMs to 25 most uncertain errors
- Compare error correction performance

### 3. Hybrid Explainability (`hybrid_explanation.ipynb`)
- Generate LIME explanations for 5 random emails
- Convert to natural language using LLMs
- Evaluate readability (Flesch Reading Ease scores)

### 4. Hybrid Translation (`hybrid_translation.ipynb`)
- Test 36 non-English emails across 18 languages
- Translate using LLMs with phishing-aware prompts
- Compare ML performance on raw vs. translated emails

## 📊 Key Results

### Traditional ML Baseline
- **ROC-AUC**: 0.9984 | **F1**: 0.9807 | **Accuracy**: 0.9828
- Training time: 3.13s | Prediction: 3.2ms per email

### Hybrid Classification (Error Correction on 25 Uncertain Cases)
| LLM | Accuracy | Errors Fixed | Time/Email |
|-----|----------|--------------|------------|
| **DeepSeek** | **88%** | 22/25 ✅ | 1.14s |
| Claude 3.5 | 80% | 20/25 | 1.41s |
| GPT-4 | 68% | 17/25 | 0.67s ⚡ |

### Hybrid Explainability (Readability)
| LLM | Flesch Score | Time/Email |
|-----|--------------|------------|
| **GPT-4** | **73.94** ✅ | 5.30s ⚡ |
| DeepSeek | 68.68 | 13.66s |
| Claude 3.5 | 43.46 | 6.49s |

*Higher Flesch scores = more readable for non-technical users*

### Hybrid Translation (36 Non-English Emails)
| Approach | Accuracy | Precision | Recall | F1 | Time/Email |
|----------|----------|-----------|--------|-----|------------|
| ML Only (Raw) | 58.3% | 56.0% | 77.8% | 0.651 | 6.75ms |
| **ML + DeepSeek** | **75.0%** ✅ | **80.0%** ✅ | 66.7% | 0.727 | 2.79s |
| **ML + GPT-4** | 72.2% | 70.0% | **77.8%** | **0.737** ✅ | 1.00s ⚡ |
| ML + Claude 3.5 | 69.4% | 68.4% | 72.2% | 0.703 | 2.01s |

**Translation improvement**: +28.7% accuracy over raw ML baseline

## 💡 Key Takeaways

1. **DeepSeek excels at error correction** (88% accuracy on hardest cases) and translation accuracy (75%)
2. **GPT-4 is fastest across all tasks** and produces most readable explanations (Flesch: 73.94)
3. **Selective LLM integration is cost-effective**: Process all emails with fast ML (3.2ms), apply LLM only to uncertain cases
4. **Translation dramatically improves multilingual detection**: 28.7% accuracy boost across 18 languages
5. **Hybrid approach outperforms ML alone** while maintaining efficiency

---

*This research proves that strategic LLM integration enhances ML phishing detection where it matters most: uncertain predictions, user explanations, and non-English content.*