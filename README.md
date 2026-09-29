# Credora_Finance-
# Credora Finance — Credit Risk & Loan Approval Analytics Platform

A Django-based credit risk management platform that loads customer, credit, loan, and transaction data, trains ML models to predict loan defaults, scores customers on a 0–100 risk scale, and presents insights through a comprehensive dashboard.

## Quick Start

```powershell
# 1. Create virtual environment
python -m venv .venv
.\.venv\Scripts\Activate.ps1

# 2. Install dependencies
pip install -r requirements.txt

# 3. Create database
python manage.py makemigrations
python manage.py migrate

# 4. Load data, train model, score customers, and seed demo accounts
python manage.py load_data
python manage.py train_model
python manage.py score_customers
python manage.py setup_demo_users

# 5. Generate AI report (requires OPENAI_API_KEY in .env)
python manage.py generate_ai_report

# 6. Run the server
python manage.py runserver
```

### User Registration & Sign-In Workflow

1. **Register**: Navigate to `http://127.0.0.1:8000/register/` to create a new user account with your name, email, password, and assigned role (`Underwriter`, `Risk Manager`, `Admin`, or `Analyst`).
2. **Sign In**: Once registered, sign in at `http://127.0.0.1:8000/login/` with your credentials to access the credit risk platform.

If PowerShell blocks activation:
```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

## Environment Variables

Copy `.env.example` to `.env` and fill in your OpenAI API key:

```env
OPENAI_API_KEY=sk-your-key-here
OPENAI_MODEL=gpt-5.5
DEBUG=True
SECRET_KEY=change-me-in-production
```

## Project Structure

```
├── credora/              # Django project settings
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── risk_engine/          # Main application
│   ├── models.py         # Customer, CreditHistory, LoanApplication, Transaction, ScoredCustomer
│   ├── views.py          # Dashboard, customer list, application list, report views
│   ├── admin.py          # Admin site registrations
│   ├── urls.py           # URL routing
│   ├── templates/        # HTML templates (dashboard, customers, applications, report)
│   └── management/commands/
│       ├── load_data.py          # Load CSV → DB
│       ├── train_model.py        # Train 4 ML models, select champion
│       ├── score_customers.py    # Score all customers 0–100
│       └── generate_ai_report.py # AI-powered risk report
├── data/                 # Source CSV datasets
├── docs/                 # PRD and SQL schema
├── models/               # Trained model artifacts (generated)
├── reports/              # AI report output (generated)
├── requirements.txt
├── manage.py
├── .env / .env.example
└── README.md
```

## Dashboard Components (8 PRD Requirements)

1. **Loan Approval Rate** — KPI card + monthly trend chart
2. **Default Rate** — KPI card + breakdown by loan type
3. **Customer Risk Distribution** — Low / Medium / High doughnut chart
4. **Credit Score Distribution** — Band-wise bar chart
5. **Geographic Loan Distribution** — Horizontal bar chart by state
6. **Monthly Loan Applications** — Time-series bar chart
7. **Approval vs Rejection Trend** — Multi-line area chart
8. **High-Risk Customer List** — Filterable table by risk category and state

## ML Models Trained

| Model | Evaluated Metrics |
|-------|-------------------|
| Logistic Regression | Accuracy, Precision, Recall, F1, ROC-AUC, Confusion Matrix |
| Decision Tree | Same |
| Random Forest | Same |
| XGBoost | Same |

The champion model is selected by highest ROC-AUC and saved to `models/default_model.pkl`.

## Data Privacy

- PII (names, emails, phones) is **never** sent to OpenAI
- Only aggregate portfolio statistics are included in the AI report payload
- The OpenAI API key is stored in `.env` (not committed)

## URLs

| URL | Description |
|-----|-------------|
| `/` | Main dashboard with all 8 PRD components |
| `/customers/` | Customer directory with risk ratings & predict links |
| `/applications/` | Loan application listing with assess risk buttons |
| `/predict/` | Real-time loan default prediction & EMI economics engine |
| `/report/` | AI-generated credit risk report |
| `/login/` | Staff sign-in page with 1-click demo accounts |
| `/register/` | Staff registration with role assignment |
| `/profile/` | Current user profile, role badge & authorized access |
| `/admin/` | Django admin interface |

## Real-Time Prediction Engine Features

- **Champion ML Pipeline**: Automatically applies feature transformations, encoders, and scaler to predict default probability (0–100%).
- **Underwriting Recommendation**: Instant decisioning: *Approve (Fast Track)*, *Manual Review Required*, or *Decline / Reject*.
- **Loan Debt Economics**: Calculates estimated monthly EMI, total interest payable, total repayment, and post-loan Debt-to-Income (DTI).
- **Attribution & Drivers**: Ranks key features and displays proportional visual impact bars indicating whether factors increase or decrease risk.
- **Instant Pre-fill**: Autocomplete search to pull existing customer bureau records, plus 1-click preset personas (*Prime*, *Near-Prime*, *Subprime*).


## Tech Stack

- **Backend**: Django 4.2, Postgresql
- **ML**: scikit-learn, XGBoost, pandas, numpy
- **Frontend**: Vanilla HTML/CSS/JS, Chart.js
- **AI**: OpenAI Python SDK (Responses API)
