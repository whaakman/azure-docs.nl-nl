---
title: Het gebruik van een bestaand model
titleSuffix: Azure Machine Learning service
description: Leer hoe u Azure Machine Learning-service kunt gebruiken met modellen die zijn getraind buiten de service. U kunt registreren modellen die buiten Azure Machine Learning-service zijn gemaakt en ze vervolgens te implementeren als een webservice of de Azure IoT Edge-module.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 06/19/2019
ms.openlocfilehash: 332129c9847c317369d5631c3af584da9430e9dd
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67453682"
---
# <a name="how-to-use-an-existing-model-with-azure-machine-learning-service"></a>Over het gebruik van een bestaand model met Azure Machine Learning-service

Informatie over het gebruik van een bestaande virtuele machine learning-model met de Azure Machine Learning-service.

Als u een machine learning-model dat is getraind buiten de Azure Machine Learning-service hebt, kunt u de service voor het model als een webservice of naar een IoT Edge-apparaat implementeren. 

> [!TIP]
> Dit artikel bevat basisinformatie over het registreren en implementeren van een bestaand model. Zodra geïmplementeerd, wordt in Azure Machine Learning-service biedt bewaking voor uw model. U kunt er ook voor het opslaan van de ingevoerde gegevens verzonden naar de implementatie, die kan worden gebruikt voor gegevens drift analyse of training nieuwe versies van het model.
>
> Zie voor meer informatie over de concepten en termen die hier worden gebruikt, [beheren, implementeren en controleren van machine learning-modellen](concept-model-management-and-deployment.md).
>
> Raadpleeg voor algemene informatie over het implementatieproces [Implementeer modellen met Azure Machine Learning-service](how-to-deploy-and-where.md).

## <a name="prerequisites"></a>Vereisten

* Een werkruimte van Azure Machine Learning-service. Zie voor meer informatie, [maken van een werkruimte](setup-create-workspace.md).

    > [!TIP]
    > De Python-voorbeelden in dit artikel wordt ervan uitgegaan dat de `ws` variabele is ingesteld op de werkruimte van uw Azure Machine Learning-service.
    >
    > De CLI-voorbeelden gebruiken een tijdelijke aanduiding van `myworkspace` en `myresourcegroup`. Vervang deze door de naam van uw werkruimte en de resourcegroep waarvan deze deel uitmaakt.

* The Azure Machine Learning SDK. Zie voor meer informatie de sectie Python-SDK van [maken van een werkruimte](setup-create-workspace.md#sdk).

* De [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) en [Machine Learning CLI-extensie](reference-azure-machine-learning-cli.md).

* Een getraind model. Het model moet worden doorgegeven aan een of meer bestanden op uw ontwikkelomgeving.

    > [!NOTE]
    > Om te demonstreren registreren van een model getraind buiten Azure Machine Learning-service, gebruikt u de voorbeelden van codefragmenten in dit artikel de modellen die zijn gemaakt door de Paolo Ripamonti Twitter sentiment analysis-project: [ https://www.kaggle.com/paoloripamonti/twitter-sentiment-analysis ](https://www.kaggle.com/paoloripamonti/twitter-sentiment-analysis).

## <a name="register-the-models"></a>De modellen registreren

Registreren van een model, kunt u opslaat, versie, en het bijhouden van metagegevens over modellen in uw werkruimte. In de volgende Python en CLI-voorbeelden de `models` map bevat de `model.h5`, `model.w2v`, `encoder.pkl`, en `tokenizer.pkl` bestanden. In dit voorbeeld worden de bestanden die zijn opgenomen in geüpload de `models` Active directory als een nieuwe registratie-model met de naam `sentiment`:

```python
from azureml.core.model import Model
# Tip: When model_path is set to a directory, you can use the child_paths parameter to include
#      only some of the files from the directory
model = Model.register(model_path = "./models",
                       model_name = "sentiment",
                       description = "Sentiment analysis model trained outside Azure Machine Learning service",
                       workspace = ws)
```

Zie voor meer informatie de [Model.register()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model(class)?view=azure-ml-py#register-workspace--model-path--model-name--tags-none--properties-none--description-none--datasets-none--model-framework-none--model-framework-version-none--child-paths-none-) verwijzing.

```azurecli
az ml model register -p ./models -n sentiment -w myworkspace -g myresourcegroup
```

Zie voor meer informatie de [az ml-model registreren](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-register) verwijzing.


Zie voor meer informatie in de registratie van het model in het algemeen [beheren, implementeren en controleren van machine learning-modellen](concept-model-management-and-deployment.md).


## <a name="define-inference-configuration"></a>Deductie configuratie definiëren

De configuratie van de Deductie definieert de omgeving die wordt gebruikt voor het uitvoeren van het geïmplementeerde model. De configuratie van de Deductie verwijst naar de volgende bestanden die worden gebruikt voor het model dat wordt uitgevoerd wanneer deze geïmplementeerd:

* De runtime. De enige geldige waarde voor de runtime is momenteel Python.
* Een post-script. Dit bestand (met de naam `score.py`) wordt het model geladen wanneer de geïmplementeerde service wordt gestart. Het is ook verantwoordelijk voor het ontvangen van gegevens, deze wordt doorgegeven aan het model en vervolgens een antwoord wordt geretourneerd.
* Een bestand met conda-omgeving. Dit bestand definieert de Python-pakketten die nodig zijn voor het model en de post-script uitvoeren. 

Het volgende voorbeeld ziet u een eenvoudige Deductie-configuratie met behulp van de Python-SDK:

```python
from azureml.core.model import InferenceConfig

inference_config = InferenceConfig(runtime= "python", 
                                   entry_script="score.py",
                                   conda_file="myenv.yml")
```

Zie voor meer informatie de [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) verwijzing.

De CLI wordt de configuratie van de Deductie geladen uit een YAML-bestand:

```yaml
{
   "entryScript": "score.py",
   "runtime": "python",
   "condaFile": "myenv.yml"
}
```

Zie voor meer informatie over de configuratie van Deductie [Implementeer modellen met Azure Machine Learning-service](how-to-deploy-and-where.md).

### <a name="entry-script"></a>Post-script

Het script invoer heeft slechts twee vereiste functies, `init()` en `run(data)`. Deze functies worden gebruikt voor het initialiseren van de service bij het opstarten en het model met behulp van de gegevens van aanvragen door een client doorgegeven uitvoeren. De rest van het script wordt gebruikt voor het laden van en uitvoeren van de modellen.

> [!IMPORTANT]
> Er is een algemene post-script die geschikt is voor alle modellen niet. Het is altijd specifiek voor het model dat wordt gebruikt. Deze moet weten hoe het model laadt de gegevensindeling die het model verwacht, en hoe u gegevens met behulp van het model te beoordelen.

De volgende Python-code is een voorbeeld van de post-script (`score.py`):

```python
import pickle
import json
import time
from keras.models import load_model
from keras.preprocessing.sequence import pad_sequences
from gensim.models.word2vec import Word2Vec
from azureml.core.model import Model

# SENTIMENT
POSITIVE = "POSITIVE"
NEGATIVE = "NEGATIVE"
NEUTRAL = "NEUTRAL"
SENTIMENT_THRESHOLDS = (0.4, 0.7)
SEQUENCE_LENGTH = 300

# Called when the deployed service starts
def init():
    global model
    global tokenizer
    global encoder
    global w2v_model

    # Get the path where the model(s) registered as the name 'sentiment' can be found.
    model_path = Model.get_model_path('sentiment')
    # load models
    model = load_model(model_path + '/model.h5')
    w2v_model = Word2Vec.load(model_path + '/model.w2v')

    with open(model_path + '/tokenizer.pkl','rb') as handle:
        tokenizer = pickle.load(handle)

    with open(model_path + '/encoder.pkl','rb') as handle:
        encoder = pickle.load(handle)

# Handle requests to the service
def run(data):
    try:
        # Pick out the text property of the JSON request.
        # This expects a request in the form of {"text": "some text to score for sentiment"}
        data = json.loads(data)
        prediction = predict(data['text'])
        #Return prediction
        return prediction
    except Exception as e:
        error = str(e)
        return error

# Determine sentiment from score
def decode_sentiment(score, include_neutral=True):
    if include_neutral:
        label = NEUTRAL
        if score <= SENTIMENT_THRESHOLDS[0]:
            label = NEGATIVE
        elif score >= SENTIMENT_THRESHOLDS[1]:
            label = POSITIVE
        return label
    else:
        return NEGATIVE if score < 0.5 else POSITIVE

# Predict sentiment using the model
def predict(text, include_neutral=True):
    start_at = time.time()
    # Tokenize text
    x_test = pad_sequences(tokenizer.texts_to_sequences([text]), maxlen=SEQUENCE_LENGTH)
    # Predict
    score = model.predict([x_test])[0]
    # Decode sentiment
    label = decode_sentiment(score, include_neutral=include_neutral)

    return {"label": label, "score": float(score),
       "elapsed_time": time.time()-start_at}  
```

Zie voor meer informatie over vermelding scripts [Implementeer modellen met Azure Machine Learning-service](how-to-deploy-and-where.md).

### <a name="conda-environment"></a>Conda-omgeving

De volgende YAML beschrijft de conda-omgeving die nodig zijn voor het model en de post-script uitvoeren:

```yaml
name: inference_environment
dependencies:
- python=3.6.2
- tensorflow
- numpy
- scikit-learn
- pip:
    - azureml-defaults
    - keras
```

Zie voor meer informatie, [Implementeer modellen met Azure Machine Learning-service](how-to-deploy-and-where.md).

## <a name="define-deployment"></a>Implementatie definiëren

De [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice?view=azure-ml-py) pakket bevat de klassen gebruikt voor implementatie. De klasse die u bepaalt waar het model is geïmplementeerd. Bijvoorbeeld, als u wilt implementeren als een webservice op Azure Kubernetes Service, gebruikt u [AksWebService.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py#deploy-configuration-autoscale-enabled-none--autoscale-min-replicas-none--autoscale-max-replicas-none--autoscale-refresh-seconds-none--autoscale-target-utilization-none--collect-model-data-none--auth-enabled-none--cpu-cores-none--memory-gb-none--enable-app-insights-none--scoring-timeout-ms-none--replica-max-concurrent-requests-none--max-request-wait-time-none--num-replicas-none--primary-key-none--secondary-key-none--tags-none--properties-none--description-none--gpu-cores-none--period-seconds-none--initial-delay-seconds-none--timeout-seconds-none--success-threshold-none--failure-threshold-none--namespace-none-) om de implementatieconfiguratie te maken.

De volgende Python-code definieert een implementatieconfiguratie voor een lokale implementatie. Deze configuratie wordt het model geïmplementeerd als een webservice om de lokale computer.

> [!IMPORTANT]
> Een lokale implementatie vereist een werkende installatie van [Docker](https://www.docker.com/) op uw lokale computer:

```python
from azureml.core.webservice import LocalWebservice

deployment_config = LocalWebservice.deploy_configuration()
```

Zie voor meer informatie de [LocalWebservice.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.localwebservice?view=azure-ml-py#deploy-configuration-port-none-) verwijzing.

De CLI laadt de configuratie van de implementatie van een YAML-bestand:

```YAML
{
    "computeType": "LOCAL"
}
```

Implementeren naar een andere compute-doel, zoals Azure Kubernetes Service in de Azure-cloud is net zo gemakkelijk als het wijzigen van de implementatieconfiguratie. Zie voor meer informatie, [hoe en waar u kunt modellen implementeren](how-to-deploy-and-where.md).

## <a name="deploy-the-model"></a>Het model implementeren

Het volgende voorbeeld wordt informatie over het geregistreerde model met de naam geladen `sentiment`, en implementeert u dit als een service met de naam `sentiment`. Tijdens de implementatie van worden de Deductie configuratie en de configuratie van de implementatie gebruikt om te maken en configureren van de service-omgeving:

```python
from azureml.core.model import Model

model = Model(ws, name='sentiment')
service = Model.deploy(ws, 'myservice', [model], inference_config, deployment_config)

service.wait_for_deployment(True)
print(service.state)
print("scoring URI: " + service.scoring_uri)
```

Zie voor meer informatie de [Model.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config--deployment-config-none--deployment-target-none-) verwijzing.

Gebruik de volgende opdracht voor het implementeren van het model van de CLI. Deze opdracht wordt versie 1 van het geregistreerde model geïmplementeerd (`sentiment:1`) met behulp van de configuratie van Deductie en implementatie die zijn opgeslagen in de `inferenceConfig.json` en `deploymentConfig.json` bestanden:

```azurecli
az ml model deploy -n myservice -m sentiment:1 --ic inferenceConfig.json --dc deploymentConfig.json
```

Zie voor meer informatie de [az ml-model implementeren](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy) verwijzing.

Zie voor meer informatie over implementatie, [hoe en waar u kunt modellen implementeren](how-to-deploy-and-where.md).

## <a name="request-response-consumption"></a>Request response-verbruik

Na de implementatie, wordt de scoring-URI weergegeven. Deze URI kan worden gebruikt door clients om te verzenden aanvragen voor de service. In het volgende voorbeeld is een eenvoudige Python-client die gegevens worden ingediend bij de service en het antwoord weergegeven:

```python
import requests
import json

scoring_uri = 'scoring uri for your service'
headers = {'Content-Type':'application/json'}

test_data = json.dumps({'text': 'Today is a great day!'})

response = requests.post(scoring_uri, data=test_data, headers=headers)
print(response.status_code)
print(response.elapsed)
print(response.json())
```

Zie voor meer informatie over het gebruik van de geïmplementeerde service [maken van een client](how-to-consume-web-service.md).

## <a name="next-steps"></a>Volgende stappen

* [Uw Azure Machine Learning-modellen met Application Insights bewaken](how-to-enable-app-insights.md)
* [Verzamelen van gegevens voor modellen in productie](how-to-enable-data-collection.md)
* [Hoe en waar u kunt modellen implementeren](how-to-deploy-and-where.md)
* [Over het maken van een client voor een geïmplementeerd model](how-to-consume-web-service.md)