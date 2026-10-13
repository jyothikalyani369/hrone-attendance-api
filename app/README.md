# Employee Attendance & Analytics API

A FastAPI backend implementing core business rules for employee punch tracking, manual overrides with full history logs, and advanced analytics via MongoDB aggregation pipelines.

## Local Operations
1. Populate local environment parameters within `.env`
2. Run installation configurations: `pip install -r requirements.txt`
3. Seed baseline datasets: `python sample_seed.py`
4. Spin up server instance: `uvicorn app.main:app --port 8000 --reload`
5. Explore routes live at: `http://127.0.0`
