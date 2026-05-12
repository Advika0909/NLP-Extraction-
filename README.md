# Prescription NLP Extraction Pipeline

A custom NLP pipeline for extracting structured prescription information from noisy raw prescription text using spaCy-based Named Entity Recognition (NER) and rule-based normalization.

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
├── prescription_pipeline_ner.py
├── requirements.txt
└── README.md
```

---

# Approach

This project uses a custom NLP pipeline combining:

- spaCy Named Entity Recognition (NER)
- Regex-based entity extraction
- Token boundary alignment
- Rule-based normalization
- Postprocessing cleanup

The system is designed to handle noisy and inconsistent prescription text commonly found in real-world clinical data.

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
                    | Cleanup & Normalize  |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | Entity Detection     |
                    | Regex + spaCy NER   |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | Custom NER Training  |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | Entity Extraction    |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | Postprocessing       |
                    | Normalization        |
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
- Supports shorthand medical notation
- Handles compact durations:
  - x5d
  - 10d
  - 2w
  - 1m
- Handles decimal strengths:
  - 2.5 mg
- Extracts:
  - medicine name
  - strength
  - dosage
  - frequency
  - duration
  - purpose
  - notes
- Custom NER training pipeline
- Token boundary alignment validation
- Structured JSON output generation

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
git clone https://github.com/Advika0909/NLP-Extraction-

cd NLP-Extraction-
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
python prescription_pipeline_ner.py
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

The pipeline combines NLP and rule-based extraction methods.

| Field | Method |
|---|---|
| medicine_name | Custom spaCy NER |
| form | Rule-based normalization |
| strength | Regex extraction |
| dosage | Regex extraction |
| frequency | Regex + rules |
| duration | Regex normalization |
| purpose | Keyword extraction |
| notes | Rule matching |

---

# Challenges Faced

Prescription text is highly inconsistent and noisy.

Examples include:

- x5d
- 10d
- missing punctuation
- shorthand notation
- OCR-style formatting
- merged tokens
- inconsistent spacing

To improve robustness, a custom NER pipeline was developed with preprocessing and normalization techniques.

---

# Future Improvements

- Fine-tune biomedical transformer models
- Add OCR support
- Add fuzzy medicine matching
- Use biomedical ontologies
- Build annotated evaluation dataset
- Deploy as FastAPI service
- Improve contextual entity recognition

---

# Example Result

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
├── prescription_pipeline_ner.py
├── prescription_raw_text_only.json
├── structured_output.json
```

---

# Author

**Advika Sawant**
