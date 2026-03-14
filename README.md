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

---

## Google Cloud Managed Databases: Comparison

| Service | Type | Data Model | Scale / Consistency | Best For | Not Ideal For |
|---------|------|------------|---------------------|----------|---------------|
| **Cloud SQL** | Relational (OLTP) | Tables / SQL (MySQL, PostgreSQL, SQL Server) | Regional; vertical + read-replicas | Lift-and-shift of existing relational workloads, web apps, CMS | Globally distributed writes, horizontal sharding at planet scale |
| **Cloud Spanner** | Relational (OLTP) | Tables / SQL (ANSI 2011 + extensions) | Global; strong consistency; unlimited horizontal scale | Mission-critical apps needing global ACID transactions (banking, inventory, gaming leaderboards) | Small/simple workloads where cost is a concern; analytics |
| **AlloyDB for PostgreSQL** | Relational (OLTP + analytics) | Tables / PostgreSQL-compatible | Regional; columnar engine for analytics | PostgreSQL workloads requiring higher performance and built-in ML inference | Apps that need multi-region active-active writes |
| **Firestore** | NoSQL – Document | JSON-like documents / collections | Multi-region; strong or eventual consistency | Mobile/web apps, real-time sync, hierarchical flexible schemas | Complex relational queries, heavy analytics, large flat tables |
| **Firebase Realtime Database** | NoSQL – Document (JSON tree) | Single JSON tree | Global (single region); eventual consistency | Low-latency real-time sync for mobile apps at moderate scale | Complex querying, very large datasets |
| **Bigtable** | NoSQL – Wide-column | Key/value rows with column families | Single-region or multi-cluster; eventual | High-throughput time-series, IoT telemetry, AdTech, ML feature store | Ad-hoc SQL queries, transactions, small datasets |
| **BigQuery** | Analytical (OLAP/DWH) | Columnar tables / SQL | Serverless; petabyte scale; eventual | Data warehousing, ad-hoc analytics, ML with BigQuery ML | Transactional workloads, low-latency point lookups |
| **Memorystore (Redis / Valkey)** | In-memory cache | Key/value, lists, sets, sorted sets | Single-region; in-memory only | Caching, session storage, pub/sub, leaderboards | Durable primary storage, large datasets |
| **Memorystore (Memcached)** | In-memory cache | Key/value | Single-region; in-memory only | Simple distributed cache for read-heavy applications | Persistence, complex data structures |

### Managed Database Decision Guide

```
Need SQL + global scale + strong consistency?  → Cloud Spanner
Need SQL + best PostgreSQL performance?        → AlloyDB for PostgreSQL
Need SQL + simple regional relational DB?      → Cloud SQL
Need real-time sync for mobile/web?           → Firestore / Firebase Realtime DB
Need massive throughput, time-series, IoT?    → Bigtable
Need petabyte analytics / data warehouse?     → BigQuery
Need a low-latency cache?                     → Memorystore (Redis/Valkey or Memcached)
```

---

## Compute & App Services: Comparison

The table below covers **App Engine**, **Managed Instance Groups (MIGs)**, **Cloud Run**, **Cloud Functions**, and **Google Kubernetes Engine (GKE)**.

| Service | Abstraction Level | Deployment Unit | Auto-scaling | Cold Starts | Use When | Avoid When |
|---------|------------------|-----------------|-------------|-------------|----------|------------|
| **App Engine Standard** | PaaS | Web app (source code) | Yes – scales to zero | Yes (fast, sandboxed runtimes) | Simple web/API apps, rapid iteration, zero ops overhead | Need OS-level control, custom runtimes, persistent background threads |
| **App Engine Flexible** | PaaS (container-based) | Docker container | Yes (no scale-to-zero) | Slower | Apps that need custom runtimes or libraries not in Standard | Cost-sensitive workloads that can tolerate idle instances |
| **Managed Instance Groups (MIGs)** | IaaS – VM fleet | VM instance template | Yes (autoscaler) | None (VMs always running) | Stateful workloads, Windows workloads, legacy apps needing full VM control | Serverless-first architectures; when container abstraction suffices |
| **Cloud Run** | Serverless containers | OCI container image | Yes – scales to zero | Yes (container startup) | Stateless HTTP/event-driven containerized services, microservices | Long-running stateful jobs > 60 min, workloads needing GPUs (use Cloud Run GPUs if available), heavy persistent storage |
| **Cloud Functions (1st gen / 2nd gen)** | FaaS – Functions | Single function (source or container) | Yes – scales to zero | Yes | Event-driven glue code, webhooks, Pub/Sub triggers, lightweight transformations | Large monolithic logic, long-running processes, high startup-latency sensitivity |
| **GKE Autopilot** | Managed Kubernetes (serverless nodes) | Kubernetes workload | Yes – node-level auto-provisioning | Near-zero (pods) | Kubernetes-native workloads without managing node infrastructure | Simple apps that don't need Kubernetes primitives |
| **GKE Standard** | Managed Kubernetes | Kubernetes workload | Yes (node pools + HPA/VPA) | None | Complex microservices, mixed workloads, GPU/TPU jobs, custom node tuning | Teams without Kubernetes expertise; simple stateless services |

### Compute / App Service Decision Guide

```
Lift-and-shift VM workload?                          → Managed Instance Groups (MIGs)
Simple web/API, no container, no ops?                → App Engine Standard
Containerized stateless HTTP service, scale to zero? → Cloud Run
Event-driven function, lightweight trigger handler?  → Cloud Functions
Kubernetes-native, complex microservices?            → GKE Standard or Autopilot
Kubernetes without managing nodes?                   → GKE Autopilot
```

### Key Differentiators

| Factor | Cloud Functions | Cloud Run | App Engine | GKE |
|--------|----------------|-----------|------------|-----|
| Max request timeout | 9 min (HTTP) / 10 min (event) | 60 min | 10 min (Standard) / 60 min (Flexible) | Unlimited |
| Container control | No | Full | Partial | Full |
| Scale to zero | Yes | Yes | Yes (Standard) | No (unless Autopilot + scale-to-zero) |
| Stateful workloads | No | Limited | No | Yes (StatefulSets) |
| GPU/TPU support | No | Limited (GPUs via Cloud Run GPU) | No | Yes |
| Pricing model | Per invocation + duration | Per CPU/memory + requests | Per instance-hour | Per node/pod resource |

---

## AI and Machine Learning Tools: Comparison

### Vertex AI Platform (Unified ML Platform)

| Tool / Service | Category | Description | Best For |
|----------------|----------|-------------|----------|
| **Vertex AI Workbench** | Development | Managed Jupyter notebooks (JupyterLab) with enterprise security | Data scientists building and experimenting with ML models |
| **Vertex AI Training** | Training | Managed custom training jobs (pre-built containers or custom) | Training TensorFlow, PyTorch, scikit-learn models at scale |
| **Vertex AI Experiments** | MLOps | Track, compare, and manage training runs and hyperparameters | Experiment tracking and reproducibility |
| **Vertex AI Pipelines** | MLOps / Orchestration | Managed ML pipelines (Kubeflow Pipelines / TFX) | Automating end-to-end ML workflows (data → train → evaluate → deploy) |
| **Vertex AI Feature Store** | Feature Engineering | Centralized managed repository for ML features | Sharing, storing, and serving features consistently across models |
| **Vertex AI Model Registry** | MLOps | Versioned model catalog for governance | Tracking model versions, comparing metrics, and managing lifecycle |
| **Vertex AI Prediction** | Serving | Online (real-time) and batch prediction endpoints | Serving trained models with auto-scaling infrastructure |
| **Vertex AI Model Monitoring** | MLOps / Monitoring | Detects data drift and skew in production models | Monitoring model performance post-deployment |
| **Vertex AI AutoML** | No-code ML | Automated model training for tabular, image, text, video | Teams without deep ML expertise needing production-quality models |
| **Vertex AI Matching Engine** | Similarity Search | Managed vector similarity search (ANN) | Recommendation engines, semantic search, embedding lookup at scale |
| **Vertex AI Neural Architecture Search** | AutoML Advanced | Finds optimal neural network architectures | Maximizing model accuracy on custom datasets |
| **Vertex AI Vizier** | Hyperparameter Tuning | Black-box optimization service | Tuning hyperparameters of any ML or non-ML system |

### Generative AI & Foundation Models

| Tool / Service | Category | Description | Best For |
|----------------|----------|-------------|----------|
| **Vertex AI Gemini API** | LLM / Multimodal | Access to Gemini 1.5/2.0 models (text, image, audio, video) | Conversational AI, summarization, code generation, multimodal reasoning |
| **Vertex AI Model Garden** | Model Hub | Pre-trained and open-source models (Gemini, Llama, Mistral, etc.) | Discovering, deploying, and fine-tuning foundation models |
| **Vertex AI Agent Builder** | AI Agents / RAG | Build LLM-powered agents and search/RAG applications | Grounded search, customer service bots, enterprise knowledge bases |
| **Vertex AI Extensions** | LLM Tool Use | Connects LLMs to real-time APIs and Google services | Giving models access to live data (Search, Maps, custom APIs) |
| **Vertex AI Prompt Management** | Prompt Engineering | Version and manage prompts | Systematic prompt experimentation and production prompt governance |
| **Vertex AI Evaluation Service** | Gen AI Evaluation | Evaluate LLM and Gen AI outputs at scale | Benchmarking model quality, safety, and groundedness |
| **Duet AI / Gemini for Google Cloud** | AI Assistance | AI code completion and chat in Cloud Console, Cloud Shell, IDEs | Developer productivity, infrastructure troubleshooting |

### Pre-built AI APIs (No ML Expertise Required)

| API | Domain | Key Capabilities |
|-----|--------|-----------------|
| **Cloud Vision API** | Computer Vision | Object detection, OCR, face/landmark/logo detection, Safe Search |
| **Cloud Video Intelligence API** | Video AI | Shot detection, label detection, speech transcription in video |
| **Cloud Natural Language API** | NLP | Sentiment analysis, entity recognition, syntax parsing, content classification |
| **Cloud Translation API** | Language | Neural machine translation (100+ languages), auto-detect |
| **Cloud Speech-to-Text API** | Speech | Real-time and batch transcription, speaker diarization, custom models |
| **Cloud Text-to-Speech API** | Speech | Neural TTS with WaveNet and Studio voices |
| **Cloud Document AI** | Document Processing | Form parsing, invoice/receipt extraction, document classification |
| **Recommendations AI** | Personalization | Product recommendations trained on e-commerce event data |
| **Contact Center AI (CCAI)** | Conversational AI | Virtual agents (Dialogflow), agent assist, conversation insights |
| **Dialogflow CX / ES** | Conversational AI | Enterprise (CX) and standard (ES) chatbot / virtual agent builder |
| **Cloud Retail API** | Retail AI | Search and recommendations for retail (search ranking, product discovery) |
| **Timeseries Insights API** | Anomaly Detection | Real-time anomaly detection in large-scale time-series data |

### BigQuery ML (ML Inside the Data Warehouse)

| Feature | Description | Best For |
|---------|-------------|----------|
| **BigQuery ML** | Train and run ML models using SQL directly in BigQuery | Analysts who know SQL but not Python/ML frameworks |
| Supported models | Linear/logistic regression, k-means, matrix factorization, DNN, boosted trees, AutoML, imported TF/ONNX models | Wide range of use cases without leaving BigQuery |
| **BQML + Vertex AI** | Export BQML models to Vertex AI for serving | When you need scalable online prediction beyond BigQuery |

### AI/ML Tool Decision Guide

```
No ML expertise, need quick model?                         → Vertex AI AutoML or Pre-built AI APIs
Standard NLP/Vision/Speech task?                           → Pre-built AI APIs (Vision, NLP, Speech, etc.)
Custom ML model, full control?                             → Vertex AI Training + Prediction
SQL-only workflow, ML on structured data?                  → BigQuery ML
Generative AI / LLM application?                          → Vertex AI Gemini API + Agent Builder
RAG / grounded search application?                        → Vertex AI Agent Builder (Search)
Fine-tune open-source foundation model?                    → Vertex AI Model Garden
End-to-end ML pipeline automation?                         → Vertex AI Pipelines
Feature consistency across models?                         → Vertex AI Feature Store
Vector/semantic similarity search?                         → Vertex AI Matching Engine
Monitor production model drift?                            → Vertex AI Model Monitoring
```
