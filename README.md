# Prescription NLP Extraction Pipeline

A hybrid NLP pipeline for extracting structured prescription information from noisy raw prescription text.

---

# Problem Statement

Given raw prescription text like:

```json
{
  "raw_text": "Tab. Amitriptyline 25 mg 1 tablet OD for 5 days at bedtime"
}
```

extract structured fields:

- medicine_name
- form
- strength
- dosage
- frequency
- duration
- purpose
- notes

---

# Project Structure

```text
.
├── prescription_raw_text_only.json
├── structured_output.json
├── prescription_pipeline.py
├── requirements.txt
└── README.md
```

---

# Approach

This project uses a **Hybrid NLP + Rule-Based Pipeline**.

Instead of relying purely on regex or purely on LLMs, the system combines:

- spaCy NLP tokenization
- Regex-based medical entity extraction
- Rule-based normalization
- Token boundary detection
- Postprocessing cleanup

This improves robustness on noisy clinical prescription text.

---

# NLP Pipeline Flow

```text
                    +----------------------+
                    | Raw Prescription Text|
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | Text Preprocessing   |
                    | Lowercasing/Cleanup  |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | spaCy Tokenization   |
                    +----------+-----------+
                               |
                               v
         +---------------------------------------------+
         | Hybrid Entity Extraction                    |
         |---------------------------------------------|
         | Regex: strength, dosage, duration           |
         | Rules: form, frequency, notes, purpose      |
         | NLP: medicine name boundary detection       |
         +----------------+----------------------------+
                          |
                          v
               +----------------------+
               | Normalization Layer  |
               +----------+-----------+
                          |
                          v
               +----------------------+
               | Structured JSON      |
               +----------------------+
```

---

# Features

- Handles noisy prescription text
- Supports medical abbreviations
- Supports compact durations:
  - x5d
  - 10d
  - 2w
  - 1m
- Handles decimal strengths:
  - 2.5 mg
- Extracts:
  - medicine name
  - form
  - strength
  - dosage
  - frequency
  - duration
  - purpose
  - notes
- Generalized token-based extraction
- Hybrid NLP + regex architecture

---

# Technologies Used

- Python
- spaCy
- Regex (`re`)
- JSON
- pandas

---

# Installation

## Clone Repository

```bash
git clone https://github.com/Advika0909/prescription-nlp-pipeline.git

cd prescription-nlp-pipeline
```

---

## Install Dependencies

```bash
pip install -r requirements.txt
```

---

## Download spaCy Model

```bash
python -m spacy download en_core_web_sm
```

---

# Usage

## Run Pipeline

```bash
python prescription_pipeline.py
```

---

# Input Format

```json
[
  {
    "raw_text": "Tab. Ciprofloxacin 250 mg 1 tablet BD for 10d empty stomach"
  }
]
```

---

# Output Format

```json
[
  {
    "raw_text": "Tab. Ciprofloxacin 250 mg 1 tablet BD for 10d empty stomach",
    "medicine_name": "Ciprofloxacin",
    "form": "tablet",
    "strength": "250 mg",
    "dosage": "1 tablet",
    "frequency": "BD",
    "duration": "10 days",
    "purpose": "",
    "notes": "empty stomach"
  }
]
```

---

# Extraction Strategy

The extraction pipeline uses a hybrid approach:

| Field | Method |
|---|---|
| medicine_name | NLP token boundary detection |
| form | Dictionary + regex |
| strength | Regex |
| dosage | Regex |
| frequency | Rule matching |
| duration | Regex normalization |
| purpose | Keyword extraction |
| notes | Rule matching |

---

# Evaluation

The project includes heuristic extraction accuracy for:

- Medicine extraction
- Strength extraction
- Frequency extraction
- Duration extraction

Example:

```text
Medicine Extraction Accuracy : 99.85%
Strength Extraction Accuracy : 90.39%
Frequency Extraction Accuracy: 74.65%
Duration Extraction Accuracy : 74.32%
Overall Heuristic Accuracy: 84.80%
```

---

# Challenges Faced

Prescription text is highly inconsistent and noisy.

Examples:

- x5d
- 10d
- missing punctuation
- shorthand notation
- OCR-style formatting
- merged tokens

Pure regex approaches were insufficient.

A hybrid NLP + rule-based system significantly improved robustness.

---

# Future Improvements

- Train custom medical NER model
- Add OCR support
- Use biomedical ontologies
- Add fuzzy drug matching
- Add evaluation against annotated ground truth
- Deploy as FastAPI service

---

# Example Results

## Input

```text
Tab Vitamin D3 1000 IU 1 tablet OD x 3 days after food
```

## Output

```json
{
  "medicine_name": "Vitamin D3",
  "form": "tablet",
  "strength": "1000 IU",
  "dosage": "1 tablet",
  "frequency": "OD",
  "duration": "3 days",
  "purpose": "",
  "notes": "after food"
}
```

---

# Requirements


```txt
spacy
pandas
```

---

# Run in Google Colab

```python
!pip install spacy pandas
!python -m spacy download en_core_web_sm
```

---

# Repository Structure

```text
prescription-nlp-pipeline/
│
├── README.md
├── requirements.txt
├── prescription_pipeline.py
├── prescription_raw_text_only.json
├── structured_output.json

```

---

# Author

Advika Sawant
