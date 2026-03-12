# Google Cloud PCA Renewal Exam

An interactive practice exam for the **Google Cloud Professional Cloud Architect Renewal** certification, administered by a Senior Google Cloud Certification Examiner.

## Overview

This repository contains 25 scenario-based exam questions aligned with:

- [PCA Renewal Exam Guide](https://services.google.com/fh/files/misc/professional_cloud_architect_renewal_exam_guide_eng.pdf)
- [Cymbal Retail Case Study](https://services.google.com/fh/files/misc/cymbal_retail_case_study_english.pdf)
- [Altostrat Media Case Study](https://services.google.com/fh/files/misc/altostrat_media_case_study_english.pdf)

Questions assess architectural judgment, technical depth, and operational reasoning across all six exam sections:

| # | Section |
|---|---------|
| 1 | Designing and Planning a Cloud Solution Architecture |
| 2 | Managing and Provisioning a Solution Infrastructure |
| 3 | Designing for Security and Compliance |
| 4 | Analyzing and Optimizing Technical and Business Processes |
| 5 | Managing Implementation |
| 6 | Ensuring Solution and Operations Reliability |

## Files

| File | Description |
|------|-------------|
| `index.html` | Interactive single-page exam application (open in browser) |
| `questions.json` | Structured question data: scenarios, options, correct answers, explanations |

## Running the Exam

**Option 1 – Python HTTP server (recommended):**
```bash
python3 -m http.server 8080
# Open http://localhost:8080 in your browser
```

**Option 2 – Node.js:**
```bash
npx serve .
```

**Option 3 – VS Code Live Server extension:** right-click `index.html` → _Open with Live Server_.

> **Note:** The exam must be served over HTTP (not opened as a `file://` URL) because `index.html` fetches `questions.json` via the Fetch API.

## Exam Format

- **25 questions** — multiple choice, one correct answer each
- **~50 minutes** estimated completion time
- **70% passing score** (18 / 25 correct)
- Instant feedback with detailed explanations after each answer
- Per-section score breakdown on the results screen
- Full answer review with explanations

## Question Distribution

| Case Study | Questions |
|-----------|-----------|
| Cymbal Retail | 11 |
| Altostrat Media | 7 |
| General / Architecture | 7 |
