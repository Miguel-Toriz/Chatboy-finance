Chatboy Finance — FinDocGPT (Challenge 02)

Web + Python prototype that turns natural‑language prompts into financial data retrieval, macro context, and ML forecasts.
Frontend is a single-page app (HTML/JS) with an analyst-style UI; backend (Python) fetches market data, builds features, and runs Prophet (forecast) and XGBoost (classification).
Designed during Hack‑Nation: Global AI Hackathon for Challenge 02 – FinDocGPT.

Project description

Goal: Make financial document analysis and investment insights accessible to non‑experts via a conversational workflow.

How it works
- Frontend (index.html): Chat-style interface with modes (Finance / Macro / ML). Uses Chart.js for visuals and mock data for a smooth demo.
- Backend (Python/Colab script):
  - Parses ticker & period from prompts
  - Downloads OHLCV from Yahoo Finance (yfinance/yahooquery)
  - Imputes missing values and normalizes OHLCV
  - Builds technical features; trains XGBoost (direction classification)
  - Aggregates to monthly and runs Prophet (+ regressors) for 12‑month forecast
  - Pulls World Bank indicators to compute a macro score
- LLM: Uses Google Gemini for prompt interpretation (OpenAI‑compatible architecture).

Current demo ships with a standalone UI (mocked responses) and a working backend you can run in Colab or locally. Wiring via HTTP can be added later.

Setup instructions

1) Clone repo
git clone https://github.com/<your-org>/chatboy-finance.git
cd chatboy-finance

2) Frontend (static)
Open frontend/index.html directly in your browser (no server required).  
The UI is in Spanish and uses mock data so you can demo instantly.

3) Backend (Python)

Option A — Google Colab (recommended for Prophet)
1. Upload backend/chatboy_backend.py and backend/codigo_mamalon.py to Colab, or open your exported notebook.
2. Set your Gemini API key in the environment:
import os
os.environ["GEMINI_API_KEY"] = "YOUR_KEY"
3. Install dependencies (see below) and run the script:
!pip install -q google-generativeai yfinance yahooquery python-dateutil pandas requests pycountry numpy                  xgboost imbalanced-learn scikit-learn prophet matplotlib
!python codigo_mamalon.py   # optional helper script
!python chatboy_backend.py  # main CLI (prompts in console)
4. Use examples:
Dame los datos financieros de Microsoft de los últimos 3 meses
Pronostica con prophet AMZN 12 meses
Entrena xgboost de AMZN de 2018-01-01 a 2024-01-02

Option B — Local (Python 3.10+)
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
export GEMINI_API_KEY="YOUR_KEY"  # Windows PowerShell: $env:GEMINI_API_KEY="YOUR_KEY"
python backend/chatboy_backend.py

Prophet note: On some systems Prophet compiles Stan models. If installation fails, install cmdstanpy and a C++ toolchain, or run in Colab where it’s preconfigured.

Dependencies / environment

Create a requirements.txt (or use this list directly):

google-generativeai
yfinance
yahooquery
python-dateutil
pandas
requests
pycountry
numpy
xgboost
imbalanced-learn
scikit-learn
prophet
matplotlib

Optional / sometimes needed:
cmdstanpy

Environment variables:
- GEMINI_API_KEY — your Google Generative AI key (don’t hardcode it in code).

Suggested repo structure

chatboy-finance/
├─ frontend/
│  └─ index.html              # Chat UI (Chart.js, mock data)
├─ backend/
│  ├─ chatboy_backend.py      # Main Python CLI (fetch, macro, XGBoost, Prophet)
│  └─ codigo_mamalon.py       # Auxiliary script (AMZN data prep & visuals)
├─ README.md
├─ requirements.txt
└─ LICENSE

Team (optional)

- Miguel Ramón Chávez Toriz — Backend & Modeling  
- María Fernanda Trejo Olvera — Frontend & UX  
- Jesús Abraham Salinas Sánchez — Data & Evaluation  
- Karla Paola Vieyra Camacho — Integrations & QA

Notes & next steps

- Frontend currently mocks data for snappy demos; connect to Python via a lightweight API (e.g., FastAPI/Flask) for live forecasts.
- Replace Gemini with OpenAI easily if preferred; architecture is model‑agnostic.
- Add persistence and auth if deploying beyond hackathon scope.
