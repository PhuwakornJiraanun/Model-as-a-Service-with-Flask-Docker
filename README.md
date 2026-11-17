# Model as a Service with Flask & Docker

Flask web service that predicts California median house value with a scikit-learn `RandomForestRegressor` trained on the California Housing dataset.

> `model/model.pkl` (~145MB) is not committed because it exceeds GitHub's 100MB file limit.
> The Docker build trains the model automatically (`python train_model.py`).

## Build and run with Docker

```bash
git clone https://github.com/PhuwakornJiraanun/Model-as-a-Service-with-Flask-Docker.git
cd Model-as-a-Service-with-Flask-Docker

docker build -t ml-service .
docker run -p 5000:5000 ml-service
```

Open http://localhost:5000 in a browser.

The build needs an internet connection to download the dataset.

## Run without Docker

```bash
pip install -r requirements.txt
python train_model.py   # creates model/model.pkl
python app.py           # http://localhost:5000
```

## API

| Method | Path | Description |
|---|---|---|
| GET | `/` | Web form for prediction |
| GET | `/health` | Health check, lists feature names |
| POST | `/predict` | JSON prediction |

Features (in order): `MedInc, HouseAge, AveRooms, AveBedrms, Population, AveOccup, Latitude, Longitude`

```bash
curl -X POST http://localhost:5000/predict \
  -H "Content-Type: application/json" \
  -d '{"features":[8.3252,41,6.98,1.02,322,2.55,37.88,-122.23]}'
# {"prediction": 4.28}   (unit: $100,000)
```

## Files

- `app.py` – Flask app
- `train_model.py` – trains and saves `model/model.pkl`
- `templates/main.html` – web UI
- `Dockerfile`, `requirements.txt`
