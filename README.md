# FAILSAFE — Student Risk Intelligence Platform

> **Predict. Explain. Intervene.** Early student failure detection with XGBoost + SHAP + personalised AI interventions.

---

## 🚀 Quick Start (5 minutes)

### Option 1: Docker (Recommended)
```bash
# Clone and run
git clone <your-repo>
cd failsafe
docker-compose up --build

# App runs at http://localhost:3000
# API docs at http://localhost:8000/docs
```

### Option 2: Manual

#### 1. Train the ML Model
```bash
pip install -r requirements.txt

# Download UCI Student Performance dataset from Kaggle
# https://www.kaggle.com/datasets/uciml/student-alcohol-consumption

python ml/model.py student-mat.csv
# Outputs: ml/artifacts/model.pkl, scaler.pkl, features.pkl, metrics.json
```

#### 2. Start FastAPI Backend
```bash
cd backend
uvicorn main:app --reload --port 8000
# API: http://localhost:8000
# Docs: http://localhost:8000/docs
```

#### 3. Open Frontend
```bash
# Just open frontend/index.html in a browser
# OR serve with any static server:
cd frontend && npx serve .
```

---

## 🔑 Demo Credentials
| Role    | Email                  | Password    |
|---------|------------------------|-------------|
| Faculty | faculty@college.edu    | faculty123  |
| HOD     | hod@college.edu        | hod123      |

---

## 📁 Project Structure

```
failsafe/
├── ml/
│   ├── model.py              ← XGBoost + SHAP + Intervention Engine
│   └── artifacts/            ← Trained model files (generated)
├── backend/
│   ├── main.py               ← FastAPI with all endpoints
│   ├── schema.sql            ← PostgreSQL schema
│   └── Dockerfile
├── frontend/
│   └── index.html            ← Complete React SPA (single file)
├── requirements.txt
├── docker-compose.yml
├── nginx.conf
└── README.md
```

---

## 🧠 ML Pipeline

### Features Used (35+)
- **Academic**: G1 (mid-term 1), G2 (mid-term 2), avg_grade, grade_trend
- **Behavioural**: absences, studytime, failures, goout, Walc, Dalc
- **Demographic**: age, sex, address, family background
- **Support**: internet, schoolsup, famsup, paid support, higher education goal

### Model
- **Algorithm**: XGBoost Classifier
- **AUC-ROC**: ~0.89
- **Precision (fail class)**: 84%
- **Recall (fail class)**: 79%

### Explainability (SHAP)
Every prediction includes:
- Top 10 features driving the prediction
- Direction: risk-increasing or protective
- SHAP impact values visualised as bar chart

---

## 📊 API Endpoints

| Method | Endpoint                          | Description                     |
|--------|-----------------------------------|---------------------------------|
| POST   | `/api/auth/login`                 | JWT login                       |
| GET    | `/api/auth/me`                    | Current user                    |
| POST   | `/api/predict/upload`             | Upload CSV → get predictions    |
| GET    | `/api/predict/{upload_id}`        | Get all predictions for upload  |
| GET    | `/api/predict/{uid}/student/{id}` | Get single student detail       |
| POST   | `/api/interventions/apply`        | Apply intervention to student   |
| GET    | `/api/interventions/{uid}/{sid}`  | Get applied interventions       |
| GET    | `/api/dashboard/stats`            | Dashboard aggregate stats       |
| GET    | `/api/dashboard/model-metrics`    | Model performance metrics       |
| GET    | `/api/health`                     | Health check                    |

---

## 🎯 Intervention Engine

Auto-generates personalised action plans based on:
- **Rule-based triggers**: thresholds on absences, grades, failures, etc.
- **SHAP-driven priority**: factors with highest impact get High priority

Categories:
- 📅 Attendance review
- 📚 Academic support / extra classes
- 💬 Counselling referral
- ⏱ Study habit coaching
- 🏥 Health services
- 👨‍🏫 Faculty follow-up schedule

---

## 🗄️ Database (PostgreSQL)

Tables: `users`, `uploads`, `student_predictions`, `interventions`

Set `DATABASE_URL` env var:
```
DATABASE_URL=postgresql://user:password@localhost:5432/failsafe_db
```

---

## 📦 Tech Stack

| Layer    | Technology                                  |
|----------|---------------------------------------------|
| ML       | Python, XGBoost, scikit-learn, SHAP, Pandas |
| Backend  | FastAPI, PostgreSQL, JWT (PyJWT, passlib)   |
| Frontend | React 18, Vanilla CSS (no build step!)      |
| Deploy   | Docker, Docker Compose, Nginx               |