# LAW-XAI: Legal Question Answering and Judgment Explanation System for the Indian Judiciary

##  Overview

**LAW-XAI** is a comprehensive AI-powered legal reasoning system designed specifically for the Indian judicial context. This repository builds upon the publicly released *IndicLegalQA* dataset and introduces significant enhancements, including judgment outcome prediction, structured legal explanation generation, and model fine-tuning for QA tasks. The central goal is to build an explainable, accurate, and interpretable legal assistant capable of operating on Indian Supreme Court judgments.

---

## Motivation

Understanding legal decisions in India is often hindered by the complexity and verbosity of legal judgments. Although previous systems focused on legal QA, they rarely supported downstream tasks like judgment prediction or fine-grained explanation generation. To address this, LAW-XAI introduces a structured pipeline:

* Fine-tuned QA system on Indian court data
* Predictive reasoning on case outcomes
* Natural language generation of multi-faceted legal explanations
* Support for explainability via LLMs and rule-based fallback systems

---

## 🔗 Key Features

*  **QA System Fine-Tuned on Indian Law**: Derived from Supreme Court judgments
*  **LLM-Driven Judgment Prediction**: Structured QA pairs used to predict case outcome
*  **Explanation Generation**:

  * Primary Method: **Mixtral LLM**
  * Fallback Method: **Rule-Based Explanation Ranking**
*  **Three-Tiered Explanation Decomposition**:

  * **Law Article Explanation**
  * **Charge Explanation**
  * **Term of Penalty Explanation**
*  **Model Fine-Tuning**: Falcon model fine-tuned on IndicLegalQA dataset
*  **BERTScore-based Evaluation**: Semantic similarity measurement between generated and true answers

---

##  Dataset: IndicLegalQA

* **Source**: Indian Supreme Court Judgments (via Kaggle)
* **Size**: 10,000 QA pairs from 1,256 judgments
* **Domains**: Criminal, Civil, Constitutional, Property, Procedural, Family, Employment, Taxation, etc.
* **Metadata**: Includes case name and judgment date
* **Format**: Structured JSON files

---

##  Research Extensions

This repository extends the IndicLegalQA framework with several innovative components:

### 1. **Model**

*  Replaced with: **Mixtral** — enhanced multi-expert open LLM

### 2. **Judgment Prediction Module**

* Input: Structured QA pairs from judgment
* Output: Case outcome label (e.g., allowed/dismissed)

### 3. **Legal Explanation Generation**

* Input: QA pairs + prediction
* Output: Human-readable, structured justification

#### Explanation Types:

* **Law Article**: Legal provisions or statutes relied on
* **Charge**: Nature of the accusation
* **Term of Penalty**: Penalties or sentences imposed

### 4. **Fallback Ranking System**

When the LLM fails to generate complete or reliable explanations, a rule-based system ranks and selects the best explanation from templates or retrieved rationale.

### 5. **Fine-tuning on Falcon Model**

* Model: `falcon-rw-1b`
* Method: Supervised fine-tuning (SFT)
* Task: QA generation
* Dataset: Subset of IndicLegalQA

---

##  Architecture Overview

```
PDF Judgments --> Structured QA Pairs --> Judgment Label Prediction --> Explanation Generation
                                           |                              |
                                           |                              +--> Rule-Based Explanation (Fallback)
                                           |
                                   Fine-tuned Falcon Model
```

---

##  Evaluation

| Component           | Metric             | Result                |
| ------------------- | ------------------ | --------------------- |
| QA Accuracy         | BERTScore (SBERT)  | \~0.89                |
| Explanation Quality | Manual + Heuristic | High interpretability |
| Judgment Prediction | Accuracy           | \~82% (on test split) |

Evaluation performed using:

* **SBERT**: `paraphrase-MiniLM-L6-v2`
* **Cosine similarity** for QA quality
* Manual review for legal correctness

---

##  Installation

```bash
# Clone the repo
https://github.com/<your-username>/LAW-XAI.git
cd LAW-XAI

# Create virtual env
python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows

# Install dependencies
pip install -r requirements.txt
```

---

##  Run Instructions

### 1. Generate Structured QA

```bash
python qa_structuring/build_structured_qa.py
```

### 2. Predict Case Label

```bash
python prediction/label_predictor.py
```

### 3. Generate Explanations

```bash
python explanations/explanation_pipeline.py
```

### 4. Evaluate

```bash
python evaluation/score_with_bertscore.py
```
---

##  Acknowledgements

* Built using the **IndicLegalQA** dataset
* Powered by **Mixtral**, **Falcon**, **BERTScore**, and open-source legal AI

---

##  Future Work

* Incorporate **High Court and District Court judgments**
* Add support for **multilingual legal documents**
* Extend to **legal contract review** and **policy compliance**
* Integrate into an **interactive legal chatbot** with RAG pipeline

---

