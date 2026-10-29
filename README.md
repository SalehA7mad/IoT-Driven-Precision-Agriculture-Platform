# 🌾 IoT-Driven Precision Agriculture Platform

> **LSA** · **VRT Fertilizer** · **Center-Pivot VRI** – Cloud & IoT multi-field farm management

## Quick Start

### 1. Environment Setup
```bash
cp .env.example .env
pip install -r requirements.txt
```

### 2. Initialize Database
```bash
python -c "from database.db_client import init_db; init_db()"
```

### 3. Start FastAPI Backend
```bash
uvicorn api.main:app --reload --port 8000
```

### 4. Start Streamlit Portal
```bash
streamlit run portal/app.py --server.port 8501
```

### 5. Run Field Simulator (requires MQTT broker)
```bash
# Start local Mosquitto broker first, then:
python -m simulator.simulator
```

### 6. Run Ingestion Consumer
```bash
python -m ingestion.mqtt_consumer
```

## Docker Compose (Full Stack)
```bash
docker-compose up --build
```

## Architecture

| Layer | Component | Technology |
|-------|-----------|------------|
| **Simulation** | Field Simulator | paho-mqtt, Python |
| **Ingestion** | QA, Calibration, Dedup | Pydantic, Topp/van Genuchten |
| **Storage** | Star Schema | SQLAlchemy, SQLite/Azure SQL |
| **Service 1** | LSA Engine | AHP, GeoJSON overlay |
| **Service 2** | VRT Fertilizer | PyKrige, Scikit-learn K-Means |
| **Service 3** | VRI Irrigation | FAO-56, Kalman assimilation |
| **API** | REST Backend | FastAPI, state machine |
| **Portal** | Dashboard | Streamlit, Plotly |

## API Endpoints

- `GET /health` – Health check
- `POST /api/v1/lsa/evaluate/point` – LSA single-point evaluation
- `POST /api/v1/lsa/evaluate/field` – LSA field-wide analysis
- `POST /api/v1/vrt/interpolate` – Kriging interpolation
- `POST /api/v1/vrt/prescription/zone` – VRT zone prescription
- `POST /api/v1/vri/grid/generate` – Polar grid generation
- `POST /api/v1/vri/water-balance/step` – Daily water balance
- `POST /api/v1/vri/assimilate` – Sensor data assimilation
- `POST /api/v1/vri/prescription/generate` – VRI prescription
- `POST /api/v1/vri/feedback/evaluate` – As-applied feedback
- `POST /state/transition` – VRI state machine transition
