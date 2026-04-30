# Sales Analysis Platform

A full-stack sales analysis dashboard with:

- Frontend: HTML, CSS, JavaScript
- Backend: Node.js + Express
- Analysis engine: Python + Pandas + Matplotlib + Seaborn + python-pptx

## Project Structure

- `frontend/` - dashboard UI
- `backend/` - Express server and upload handling
- `python/` - data cleaning, analysis, and chart generation
- `sample-data/` - example dataset for testing

## Setup

### 1. Install Node dependencies

```bash
npm install
```

### 2. Install Python dependencies

```bash
python -m pip install -r python/requirements.txt
```

If your machine uses `py` or `python3`, use that command instead.
If `python` is not on PATH when you start the app, set `PYTHON_BIN` before running:

```bash
set PYTHON_BIN=py
```

If the launcher still does not find Python, you can point `PYTHON_BIN` to the full interpreter path, such as `C:\Users\<you>\AppData\Local\Programs\Python\Python312\python.exe`.

To enable AI-generated report copy for PowerPoint exports, set `OPENAI_API_KEY` before starting the app. You can also override the model with `OPENAI_REPORT_MODEL` if needed.

To enable the optional LLM planner for the new Ask Your Data assistant, the same `OPENAI_API_KEY` is used. You can override that planner model with `OPENAI_DATA_QA_MODEL`. The final numbers still come from computed CSV results, not from the LLM directly.

### 3. Optional: enable Postgres persistence

If you want saved datasets and chat history to survive server restarts, configure Postgres before starting the app.

Set `DATABASE_URL`, for example:

```bash
export DATABASE_URL=postgresql://postgres:postgres@localhost:5432/sales_analysis_platform
```

The backend will auto-create the `datasets` and `chat_messages` tables at startup. A reference schema is also available in `backend/schema.sql`.

For local development, the backend also auto-loads `.env.local` if it exists.

If `DATABASE_URL` is not set, the app still works, but dataset recall falls back to in-memory sessions only.

### 4. Start the app

```bash
npm start
```

Open:

```text
http://localhost:3000
```

## How It Works

1. Upload a CSV or Excel file.
2. The app inspects the file and suggests useful analysis columns and chart types.
3. Choose cleaning and analysis options.
4. Run analysis to get:
   - KPIs
   - charts
   - cleaned data preview
   - business insights
   - problem statements
5. Use the Mock AI Graph Builder to create an interactive chart from any X/Y combination.
6. Download a PowerPoint report with insights, charts, recommendations, and optional AI-generated executive copy.
7. Use Ask Your Data to ask plain-English questions such as:
   - top 10 salesmen this year
   - which month had the highest sales
   - total profit in California
   - forecast next month sales
8. With Postgres enabled, you can later ask about a saved dataset without uploading it again, for example:
   - top 10 salesman from superstore sample dataset
   - use my latest dataset and forecast next month sales

## Example Dataset

Use `sample-data/sales_sample.csv` to test the platform.

## Notes

- Excel support includes `.xlsx` and `.xls`.
- Generated charts and cleaned CSV files are stored in `backend/outputs/`.
- Uploaded temp files are stored in `backend/uploads/` and cleaned up after processing.
- If the backend is not running, CSV files still get a local browser-side inspection and analysis fallback.
- Ask Your Data currently accepts CSV uploads for question answering.
- When Postgres is enabled, uploaded datasets and chat messages are stored persistently.
- Saved dataset recall is scoped by a browser-specific client key in local storage.
- Ranking, totals, and month/year questions are answered deterministically from the CSV.
- Forecast answers now use a trained lag-feature forecasting model when enough history exists and the backtest is reasonable, with a trend fallback when the dataset is too small or noisy.
- Forecast responses include the method and a simple confidence signal so you can judge how reliable the prediction is.
