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

## KafkaInputAdapter

```python
class bentoml.adapters.KafkaInputAdapter(host:str, port:int, input_topic:str, output_topic:str)
```

`KafkaInputAdapter` inherits from `bentoml.adapters.BaseInputAdapter` and provides
support for doing input and output operations on Kafka streams.

The adapter houses a Kafka consumer(input stream) and producer(output stream). `KafkaInputAdapter`
automatically connects with the Kafka Broker to process data present on `input_topic`.

#### Example

```python
from bentoml import env, artifacts, api, BentoService
from bentoml.adapters import KafkaInputAdapter
from bentoml.frameworks.sklearn import SklearnModelArtifact

@env(infer_pip_packages=True)
@artifacts([SklearnModelArtifact('model')])
class IrisClassifier(BentoService):

    @api(
        input=KafkaInputAdapter(
            host='localhost',
            port=9093,
            input_topic='input_stream',
            output_topic='output_stream'
        )
    )
    def predict(self, df):
        # Optional pre-processing, post-processing code goes here
        return self.artifacts.model.predict(df)

```
