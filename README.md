# AI Sentiment Analysis

A BERT-based sentiment analysis system that classifies product reviews as **Negative**, **Neutral** or **Positive**, served through a small FastAPI application with a web dashboard.

This is a team project. This repository is a fork of the team repository: [manar321R/AI-sentiment-analysis](https://github.com/manar321R/AI-sentiment-analysis).

## Features

- Fine-tuned `bert-base-uncased` model for 3-class sentiment classification
- Text preprocessing pipeline (HTML and emoji removal, lowercasing, whitespace normalization)
- REST API for single reviews and for batch prediction from a CSV file
- Web dashboard that shows the sentiment distribution with Chart.js
- CI/CD pipeline with GitHub Actions (install, unit tests, validation, deploy gate)
- Docker image ready for Hugging Face Spaces

## Project structure

```
.
├── MANAR_data_loading.py        # Step 1: load and prepare the dataset (Google Colab)
├── Lamar_data_preprocessing.py  # Step 1: text preprocessing and data splitting
├── samiyah_model_setup.py       # Step 2: training setup and data loaders
├── Mawadda_training.py          # Step 2: BERT fine-tuning
├── WareefSaad_inference.py      # Step 3: inference and evaluation
├── Toleen_evaluation.py         # Validation script used by the CI/CD pipeline
├── Deployment/                  # FastAPI app, dashboard (index.html) and Dockerfile
├── tests/                       # Unit tests and load test
├── .github/workflows/           # CI/CD pipeline
└── requirements.txt
```

The numbered steps (data loading, preprocessing, training, inference) were written as Google Colab notebooks exported to `.py` files, so they expect to run in Colab with Google Drive mounted.

## Getting started

```bash
git clone https://github.com/IMawaddah/AI-sentiment-analysis.git
cd AI-sentiment-analysis
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Run the API

The trained model is not stored in this repository because of its size. Place the fine-tuned model folder (`sentiment_bert_model_best`) next to `Deployment/main.py`, then run:

```bash
cd Deployment
uvicorn main:app --host 0.0.0.0 --port 7860
```

Open `http://localhost:7860` for the dashboard.

### API endpoints

| Method | Path | Description |
|---|---|---|
| `GET` | `/` | Web dashboard |
| `POST` | `/predict` | Upload a CSV file (the first column is used as the review text) and get the sentiment distribution plus the first 20 predictions |
| `POST` | `/predict_text` | Send `{"text": "..."}` and get `{"sentiment": "Positive"}` |

Example:

```bash
curl -X POST http://localhost:7860/predict_text \
  -H "Content-Type: application/json" \
  -d '{"text": "Great phone case, fits perfectly!"}'
```

### Run the tests

```bash
python -m pytest tests/test_pipeline.py -v
```

### Docker

```bash
cd Deployment
docker build -t sentiment-api .
docker run -p 7860:7860 sentiment-api
```

## Tech stack

Python, PyTorch, Hugging Face Transformers, FastAPI, scikit-learn, pandas, Chart.js, Docker, GitHub Actions.

## Team

Developed by a team of students; see the [contributors page](https://github.com/manar321R/AI-sentiment-analysis/graphs/contributors) of the team repository for everyone who took part.
