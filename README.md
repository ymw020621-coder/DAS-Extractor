# DAS-Extractor

**Automated Detection and Classification of Data Availability Statements in Scholarly Articles**

## Overview

This project extracts and classifies Data Availability Statements (DAS) from ~7,301 medical and engineering research papers (2015-2024) using DeepSeek-V3 API and rule-based text processing.

## Dataset

- **Medicine & Health Sciences**: 3,988 articles (2020-2024)
- **Engineering**: 3,313 articles (2015-2024)
- **Total**: ~7,301 full-text PDFs

## Method

### 1. PDF Processing
- Extract text using PyMuPDF
- Clean headers, footers, and normalize spacing

### 2. Candidate Section Identification (3-phase)
1. **Keyword matching**: Extract ±3,000 characters around DAS-related keywords
2. **Conclusion-References fallback**: Extract text between these sections
3. **References-only fallback**: Extract 3,000 characters before References

### 3. LLM Classification
- **Model**: DeepSeek-V3-0324
- **Temperature**: 0 (deterministic)
- **Output**: DAS content, confidence level (High/Mid/Low), reasoning

### 4. Sharing Type Classification
10 categories adapted from Federer et al. (2018):
- `repository`, `upon_request`, `in_paper`, `in_paper_and_SI`, `in_SI`
- `access_restricted`, `combination`, `location_not_stated`, `na_value`, `other`

## Validation Results (500-paper sample)

**Engineering (n=250)**:
- Boolean Accuracy: 99.6%
- F1 Score: 0.998
- Exact Matches: 249/250

**Medical (n=250)**:
- Boolean Accuracy: 100%
- F1 Score: 0.973
- Exact Matches: 231/244

## Installation

```bash
pip install -r requirements.txt
```

## Usage

### Basic Workflow
1. Set your DeepSeek API key in the notebook
2. Configure PDF folder paths
3. Run cells sequentially

### Optional: Handle "Maybe" Cases
After detection, process uncertain cases,this categorizes 226 "Maybe" cases into:
- **No DAS Header** (177): Auto-classified as false positives
- **Need Review** (49): Requires manual inspection

## Key Features

- Cost-efficient: ~370-740 tokens per paper vs. 4,000-8,000 for full-text
- High accuracy: >99% detection rate
- Smart "Maybe" handling: Auto-filters 177 "No Header" false positives, only 49 require manual review
- Scalable: Processed 7,301 papers


## Citation

```bibtex
@software{das_extractor_2025,
  author = {Joshua Wong},
  title = {DAS-Extractor: Automated Detection and Classification of Data Availability Statements},
  year = {2025},
  url = {https://github.com/ymw020621-coder/DAS-Extractor}
}
```


## References

- Federer et al. (2018) - DAS classification taxonomy
- DeepSeek-V3-0324 - LLM classification model
- PyMuPDF - PDF text extraction
