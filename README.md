# bentoML-kafka-integration
Integrating Apache Kafka with BentoML

## Development instructions

1. Create and activate a Python virtual environment

```python
python -m venv venv/
source venv/bin/activate
```

2. Change working directory to `BentoML`

```
cd BentoML
```

3. Run the `main.py` to create BentoML service

```
python main.py

```

4. Test the model APIs

```
bentoml serve IrisClassifier:latest
```

5. Containerize

```
bentoml containerize IrisClassifier:latest -t iris-classifier
```

6. Start container using docker

```
docker run -p 5000:5000 iris-classifier:latest --workers=1 --enable-microbatch
```
