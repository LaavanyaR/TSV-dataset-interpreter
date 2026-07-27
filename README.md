TSV Dataset Interpreter & Report Publisher

A lightweight Python proof-of-concept (POC) that loads a TSV dataset, interprets its structure, converts it to CSV/JSON/XML, and publishes a downloadable tabular report of key fields — all through a single-page Streamlit app.

Status: Proof of concept. Built to validate the workflow end-to-end, not hardened for production (see Limitations below).

What it does
Load a TSV file from a fixed local/drive path, a GitHub URL, or a manual upload — via a simple form.
Interpret the dataset: preview it, and see a schema summary (non-null counts, unique values, sample values per column).
Convert the full dataset to CSV, JSON, or XML with one click.
Publish a report: pick the "key fields" that matter (or accept smart defaults), see them rendered as a clean table with row/column/duplicate counters, and download the report in your chosen format.
Demo flow
TSV file  →  pandas DataFrame  →  [convert: CSV/JSON/XML]  →  download
                    ↓
            [pick key fields]
                    ↓
            tabular report  →  download
Tech stack
Component	Choice	Why
Language	Python 3.9+	wide library support, minimal boilerplate
UI	Streamlit	form, tables, and download buttons built in — no separate frontend needed
Parsing	pandas	purpose-built TSV/CSV parser, native to_csv/to_json
XML export	xml.etree.ElementTree (stdlib)	no extra dependency, full control over tags
Remote fetch	requests	fetch TSVs from GitHub raw URLs
Testing	pytest	unit tests for loader/converter/report logic
Project structure
tsv-report-poc/
├── app.py                  # Streamlit UI (entry point)
├── requirements.txt
├── src/
│   ├── data_loader.py      # load_from_local / _github / _upload
│   ├── converter.py        # convert() -> csv / json / xml bytes
│   └── report.py           # infer_key_columns, build_key_field_report
├── sample_data/
│   └── sample_employees.tsv
└── tests/
    └── test_converter.py
Getting started
bash
git clone https://github.com/<your-username>/tsv-report-poc.git
cd tsv-report-poc

python3 -m venv venv
source venv/bin/activate       # Windows: venv\Scripts\activate

pip install -r requirements.txt
streamlit run app.py

The app opens at http://localhost:8501. On first load, the sample dataset at sample_data/sample_employees.tsv is pre-filled — just click Load dataset to try it immediately.

Run tests
bash
pytest tests/ -v
Limitations / what's not included

This is a POC, so the following are intentionally out of scope for now:

No authentication — don't deploy this publicly as-is
No large-file handling — the whole file is loaded into memory
No private GitHub repo support — only public raw URLs work out of the box (adding a token is a small change in data_loader.py)
No persistence — nothing is saved between sessions; each report is generated fresh
Possible next steps
Point the "fixed path" loader at a real network share, S3, or GCS bucket
Add authentication before exposing beyond localhost
Support private GitHub repos via a personal access token
Persist report runs to a database for history/audit
Containerize with Docker for consistent deployment
