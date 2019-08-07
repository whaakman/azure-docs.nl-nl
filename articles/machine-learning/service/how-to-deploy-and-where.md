---
title: Hoe en waar modellen moeten worden geïmplementeerd
titleSuffix: Azure Machine Learning service
description: 'Meer informatie over hoe en waar u uw Azure Machine Learning service modellen kunt implementeren, met inbegrip van: Azure Container Instances, Azure Kubernetes-service, Azure IoT Edge en veld-Programmeer bare poort matrices.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 08/06/2019
ms.custom: seoapril2019
ms.openlocfilehash: 7e88b99cf0ecede64d75b36eafdcc88798e2e4a4
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68840456"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Implementeer modellen met de Azure Machine Learning-service

Meer informatie over het implementeren van uw machine learning model als een webservice in de Azure-Cloud of voor het IoT Edge van apparaten. 

De werk stroom is vergelijkbaar, ongeacht [waar u](#target) uw model implementeert:

1. Registreer het model.
1. Voorbereiden op implementeren (assets opgeven, gebruik, reken doel)
1. Implementeer het model op het Compute-doel.
1. Test het geïmplementeerde model, ook wel webservice genoemd.

Zie [modellen beheren, implementeren en bewaken met Azure machine learning-service](concept-model-management-and-deployment.md)voor meer informatie over de concepten die bij de implementatie werk stroom zijn betrokken.

## <a name="prerequisites"></a>Vereisten

- Een model. Als u geen getraind model hebt, kunt u gebruikmaken van het model & afhankelijkheids bestanden in [deze zelf studie](https://aka.ms/azml-deploy-cloud).

- De [Azure cli-extensie voor machine learning service](reference-azure-machine-learning-cli.md), [Azure machine learning python SDK](https://aka.ms/aml-sdk)of de [Azure machine learning Visual Studio code extension](how-to-vscode-tools.md).

## <a id="registermodel"></a>Uw model registreren

Een logische container van een geregistreerd model voor een of meer bestanden waaruit het model is opgebouwd. Als u bijvoorbeeld een model hebt dat is opgeslagen in meerdere bestanden, kunt u ze registreren als één model in de werk ruimte. Na de registratie kunt u het geregistreerde model downloaden of implementeren en alle geregistreerde bestanden ontvangen.

Machine learning-modellen worden geregistreerd in uw Azure Machine Learning-werk ruimte. Het model kan afkomstig zijn van Azure Machine Learning of van ergens anders afkomstig zijn. De volgende voor beelden laten zien hoe u een model van het bestand kunt registreren:

### <a name="register-a-model-from-an-experiment-run"></a>Een model registreren vanuit een experiment

+ **Scikit-voor beeld van het gebruik van de SDK**
  ```python
  model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
  print(model.name, model.id, model.version, sep='\t')
  ```

  > [!TIP]
  > Als u meerdere bestanden wilt toevoegen aan de model registratie `model_path` , stelt u deze in op de map die de bestanden bevat.

+ **De CLI gebruiken**

  ```azurecli-interactive
  az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment --run-id myrunid
  ```

  > [!TIP]
  > Als u meerdere bestanden wilt toevoegen aan de model registratie `--asset-path` , stelt u deze in op de map die de bestanden bevat.

+ **VS code gebruiken**

  Registreer modellen met behulp van model bestanden of-mappen met de [VS code](how-to-vscode-tools.md#deploy-and-manage-models) -extensie.

### <a name="register-an-externally-created-model"></a>Een extern gemaakt model registreren

[!INCLUDE [trusted models](../../../includes/machine-learning-service-trusted-model.md)]

U kunt een extern gemaakt model registreren door een **lokaal pad** naar het model op te geven. U kunt een map of één bestand opgeven.

+ **ONNX-voor beeld met de python-SDK:**
  ```python
  onnx_model_url = "https://www.cntk.ai/OnnxModels/mnist/opset_7/mnist.tar.gz"
  urllib.request.urlretrieve(onnx_model_url, filename="mnist.tar.gz")
  !tar xvzf mnist.tar.gz
  
  model = Model.register(workspace = ws,
                         model_path ="mnist/model.onnx",
                         model_name = "onnx_mnist",
                         tags = {"onnx": "demo"},
                         description = "MNIST image classification CNN from ONNX Model Zoo",)
  ```

  > [!TIP]
  > Als u meerdere bestanden wilt toevoegen aan de model registratie `model_path` , stelt u deze in op de map die de bestanden bevat.

+ **De CLI gebruiken**
  ```azurecli-interactive
  az ml model register -n onnx_mnist -p mnist/model.onnx
  ```

  > [!TIP]
  > Als u meerdere bestanden wilt toevoegen aan de model registratie `-p` , stelt u deze in op de map die de bestanden bevat.

**Geschatte tijd**: Ongeveer 10 seconden.

Zie voor meer informatie de documentatie bij de [Modelklasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

Zie [een bestaand model implementeren](how-to-deploy-existing-model.md)voor meer informatie over het werken met modellen die zijn getraind buiten Azure machine learning service.

<a name="target"></a>

## <a name="choose-a-compute-target"></a>Een reken doel kiezen

De volgende Compute-doelen of reken bronnen kunnen worden gebruikt voor het hosten van de implementatie van de webservice. 

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]

## <a name="prepare-to-deploy"></a>Implementatie voorbereiden

Als u een webservice wilt implementeren, moet u een configuratie voor inactiviteit (`InferenceConfig`) en een implementatie configuratie maken. Defactorion of model Score is de fase waarin het geïmplementeerde model wordt gebruikt voor de voor spelling, meestal op productie gegevens. In de configuratie voor inschakeling, geeft u de scripts en afhankelijkheden op die nodig zijn om uw model te gebruiken. Geef in de implementatie configuratie details op over hoe het model op het berekenings doel moet worden gebruikt.

> [!IMPORTANT]
> De Azure Machine Learning SDK biedt geen manier voor het uitvoeren van de webservice of IoT Edge implementaties om toegang te krijgen tot uw Data Store-of gegevens sets. Als u het geïmplementeerde model nodig hebt om toegang te krijgen tot gegevens die buiten de implementatie zijn opgeslagen, zoals in een Azure Storage-account, moet u een oplossing voor aangepaste code ontwikkelen met behulp van de relevante SDK. Bijvoorbeeld de [Azure Storage SDK voor python](https://github.com/Azure/azure-storage-python).
>
> Een ander alternatief dat kan worden gebruikt voor uw scenario is [batch](how-to-run-batch-predictions.md)voorspellingen. Dit biedt ook toegang tot gegevens opslag in de score.

### <a id="script"></a> 1. & Afhankelijkheden van uw invoer script definiëren

Het item script ontvangt gegevens die zijn verzonden naar een geïmplementeerde webservice en geeft deze door aan het model. Vervolgens wordt het antwoord opgehaald dat door het model is geretourneerd en wordt dat naar de client geretourneerd. **Het script is specifiek voor uw model**. het moet inzicht hebben in de gegevens die het model verwacht en retourneert.

Het script bevat twee functies die het model laden en uitvoeren:

* `init()`: Deze functie laadt meestal het model in een globaal object. Deze functie wordt slechts één keer uitgevoerd wanneer de docker-container voor uw web service wordt gestart.

* `run(input_data)`: Deze functie maakt gebruik van het model voor het voors pellen van een waarde op basis van de invoer gegevens. Invoer en uitvoer van de run worden meestal JSON gebruikt voor serialisatie en deserialisatie. U kunt ook werken met onbewerkte binaire gegevens. U kunt de gegevens transformeren voordat u deze naar het model verzendt, of voordat u terugkeert naar de client.

#### <a name="what-is-get_model_path"></a>Wat is get_model_path?

Wanneer u een model registreert, geeft u een model naam op die wordt gebruikt voor het beheren van het model in het REGI ster. U gebruikt deze naam voor het [model. Get _model_path ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#get-model-path-model-name--version-none---workspace-none-) om het pad van het model bestand (en) op het lokale bestands systeem op te halen. Als u een map of een verzameling bestanden registreert, retourneert deze API het pad naar de map waarin zich de bestanden bevinden.

Wanneer u een model registreert, geeft u het een naam die overeenkomt met waar het model lokaal of tijdens de service-implementatie wordt geplaatst.

In het onderstaande voor beeld wordt een pad geretourneerd naar één bestand `sklearn_mnist_model.pkl` (dat is geregistreerd met de naam `sklearn_mnist`):

```python
model_path = Model.get_model_path('sklearn_mnist')
```

#### <a name="optional-automatic-schema-generation"></a>Beschrijving Automatisch schema genereren

Als u automatisch een schema voor uw webservice wilt genereren, geeft u een voor beeld van de invoer en/of uitvoer in de constructor voor een van de gedefinieerde type-objecten. het type en het voor beeld wordt gebruikt om automatisch het schema te maken. Azure Machine Learning-service maakt vervolgens een [OpenAPI](https://swagger.io/docs/specification/about/) -specificatie (Swagger) voor de webservice tijdens de implementatie.

De volgende typen worden momenteel ondersteund:

* `pandas`
* `numpy`
* `pyspark`
* Standard python-object

Als u schema generatie wilt gebruiken, `inference-schema` neemt u het pakket op in uw Conda-omgevings bestand.

##### <a name="example-dependencies-file"></a>Voor beeld van afhankelijkheids bestand

De volgende YAML is een voor beeld van een Conda-afhankelijkheids bestand voor ingrijpen.

```YAML
name: project_environment
dependencies:
  - python=3.6.2
  - pip:
    - azureml-defaults
    - scikit-learn==0.20.0
    - inference-schema[numpy-support]
```

Als u automatische schema generatie wilt gebruiken, **moet** uw invoer script de `inference-schema` pakketten importeren.

Definieer de voorbeeld indelingen voor invoer en uitvoer in `input_sample` de `output_sample` variabelen en, die de aanvraag-en antwoord indelingen voor de webservice vertegenwoordigen. Gebruik deze voor beelden in de functie voor invoer en uitvoer op de `run()` functie. Het scikit-voor beeld hieronder maakt gebruik van schema generatie.

##### <a name="example-entry-script"></a>Voorbeeld script

In het volgende voor beeld ziet u hoe u JSON-gegevens kunt accepteren en retour neren:

```python
#example: scikit-learn and Swagger
import json
import numpy as np
from sklearn.externals import joblib
from sklearn.linear_model import Ridge
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType


def init():
    global model
    # note here "sklearn_regression_model.pkl" is the name of the model registered under
    # this is a different behavior than before when the code is run locally, even though the code is the same.
    model_path = Model.get_model_path('sklearn_regression_model.pkl')
    # deserialize the model file back into a sklearn model
    model = joblib.load(model_path)


input_sample = np.array([[10, 9, 8, 7, 6, 5, 4, 3, 2, 1]])
output_sample = np.array([3726.995])


@input_schema('data', NumpyParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        result = model.predict(data)
        # you can return any datatype as long as it is JSON-serializable
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```

In het volgende voor beeld ziet u hoe u de invoer gegevens definieert `<key: value>` als een woorden lijst, met behulp van een data frame. Deze methode wordt ondersteund voor het gebruik van de geïmplementeerde webservice van Power BI (meer informatie over het[gebruiken van de webservice van Power bi](https://docs.microsoft.com/power-bi/service-machine-learning-integration)):

```python
import json
import pickle
import numpy as np
import pandas as pd
import azureml.train.automl
from sklearn.externals import joblib
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType
from inference_schema.parameter_types.pandas_parameter_type import PandasParameterType


def init():
    global model
    # replace model_name with your actual model name, if needed
    model_path = Model.get_model_path('model_name')
    # deserialize the model file back into a sklearn model
    model = joblib.load(model_path)


input_sample = pd.DataFrame(data=[{
    # This is a decimal type sample. Use the data type that reflects this column in your data
    "input_name_1": 5.1,
    # This is a string type sample. Use the data type that reflects this column in your data
    "input_name_2": "value2",
    # This is a integer type sample. Use the data type that reflects this column in your data
    "input_name_3": 3
}])

# This is a integer type sample. Use the data type that reflects the expected result
output_sample = np.array([0])


@input_schema('data', PandasParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        result = model.predict(data)
        # you can return any datatype as long as it is JSON-serializable
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```

Zie de volgende voor beelden voor meer voorbeeld scripts:

* Pytorch[https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch)
* Tensor flow[https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow)
* Keras[https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras)
* ONNX[https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/)
* Score voor binaire gegevens: [Een webservice gebruiken](how-to-consume-web-service.md)

### <a name="2-define-your-inferenceconfig"></a>2. Uw InferenceConfig definiëren

De configuratie voor afnemen beschrijft hoe u het model configureert om voor spellingen te maken. In het volgende voor beeld ziet u hoe u een configuratie voor het afwijzen van een interferentie maakt. Deze configuratie specificeert de runtime, het vermeldings script en (optioneel) het Conda-omgevings bestand:

```python
inference_config = InferenceConfig(runtime="python",
                                   entry_script="x/y/score.py",
                                   conda_file="env/myenv.yml")
```

Zie voor meer informatie de [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) -klasse verwijzing.

Zie [een model implementeren met behulp van een aangepaste docker-installatie kopie](how-to-deploy-custom-docker-image.md)voor informatie over het gebruik van een aangepaste docker-installatie kopie met behulp van de configuratie voor inhalen.

### <a name="cli-example-of-inferenceconfig"></a>CLI-voor beeld van InferenceConfig

[!INCLUDE [inference config](../../../includes/machine-learning-service-inference-config.md)]

De volgende opdracht laat zien hoe u een model implementeert met behulp van de CLI:

```azurecli-interactive
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json
```

In dit voor beeld bevat de configuratie de volgende items:

* Dat dit model python vereist
* Het [invoer script](#script), dat wordt gebruikt voor het afhandelen van webaanvragen die worden verzonden naar de geïmplementeerde service
* Het Conda-bestand met een beschrijving van de Python-pakketten die nodig zijn om aan te doen

Zie [een model implementeren met behulp van een aangepaste docker-installatie kopie](how-to-deploy-custom-docker-image.md)voor informatie over het gebruik van een aangepaste docker-installatie kopie met behulp van de configuratie voor inhalen.

### <a name="3-define-your-deployment-configuration"></a>3. Uw implementatie configuratie definiëren

Voordat u implementeert, moet u de implementatie configuratie definiëren. __De implementatie configuratie is specifiek voor het reken doel dat als host fungeert voor de webservice__. Wanneer u bijvoorbeeld lokaal implementeert, moet u de poort opgeven waar de service aanvragen accepteert.

Mogelijk moet u ook de reken resource maken. Bijvoorbeeld, als u nog geen Azure Kubernetes-service hebt die aan uw werk ruimte is gekoppeld.

De volgende tabel bevat een voor beeld van het maken van een implementatie configuratie voor elk reken doel:

| COMPUTE-doel | Implementatie configuratie-voor beeld |
| ----- | ----- |
| Lokaal | `deployment_config = LocalWebservice.deploy_configuration(port=8890)` |
| Azure Container Instance | `deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |
| Azure Kubernetes Service | `deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |

> [!TIP]
> Voordat u uw model als een service implementeert, kunt u het beste een profiel gebruiken om de optimale CPU-en geheugen vereisten te bepalen. U kunt uw model profileren met behulp van de SDK of CLI. Voor meer informatie raadpleegt u de verwijzing [Profiel ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#profile-workspace--profile-name--models--inference-config--input-data-) en [AZ ml model profile](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-profile) .
>
> Model profilerings resultaten worden verzonden als een `Run` object. Zie voor meer informatie de [ModelProfile](https://docs.microsoft.com/python/api/azureml-core/azureml.core.profile.modelprofile?view=azure-ml-py) -klasse verwijzing.

## <a name="deploy-to-target"></a>Implementeren naar doel

De implementatie maakt gebruik van de configuratie-implementatie configuratie voor het afnemen van een model (en). Het implementatie proces is vergelijkbaar, ongeacht het berekenings doel. Implementeren naar AKS is iets anders, omdat u een verwijzing naar het AKS-cluster moet opgeven.

### <a id="local"></a>Lokale implementatie

Als u lokaal wilt implementeren, moet docker op uw lokale computer zijn **geïnstalleerd** .

#### <a name="using-the-sdk"></a>De SDK gebruiken

```python
deployment_config = LocalWebservice.deploy_configuration(port=8890)
service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
print(service.state)
```

Zie de referentie documentatie voor [LocalWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservice?view=azure-ml-py), [model. Deploy ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config--deployment-config-none--deployment-target-none-)en [webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py)voor meer informatie.

#### <a name="using-the-cli"></a>De CLI gebruiken

Als u wilt implementeren met behulp van de CLI, gebruikt u de volgende opdracht. Vervang `mymodel:1` door de naam en versie van het geregistreerde model:

```azurecli-interactive
az ml model deploy -m mymodel:1 -ic inferenceconfig.json -dc deploymentconfig.json
```

[!INCLUDE [aml-local-deploy-config](../../../includes/machine-learning-service-local-deploy-config.md)]

Zie voor meer informatie de referentie [AZ ml model Deploy](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy) .

### <a id="aci"></a>Azure Container Instances (DEVTEST)

Zie [implementeren naar Azure container instances](how-to-deploy-azure-container-instance.md).

### <a id="aks"></a>Azure Kubernetes-service (DEVTEST & productie)

Zie [implementeren naar Azure Kubernetes service](how-to-deploy-azure-kubernetes-service.md).

## <a name="consume-web-services"></a>Webservices gebruiken

Elke geïmplementeerde webservice biedt een REST API, zodat u client toepassingen kunt maken in verschillende programmeer talen. Als u sleutel verificatie voor uw service hebt ingeschakeld, moet u een service sleutel opgeven als een token in de aanvraag header.
Als u token verificatie voor uw service hebt ingeschakeld, moet u een Azure Machine Learning JWT-token opgeven als Bearer-token in de aanvraag header.

> [!TIP]
> Nadat de service is geïmplementeerd, kunt u het JSON-document van het schema ophalen. Gebruik de [eigenschap swagger_uri](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservice?view=azure-ml-py#swagger-uri) van de geïmplementeerde webservice, zoals `service.swagger_uri`, om de URI naar het Swagger-bestand van de lokale webservice op te halen.

### <a name="request-response-consumption"></a>Aanvraag-antwoord verbruik

Hier volgt een voor beeld van het aanroepen van uw service in Python:
```python
import requests
import json

headers = {'Content-Type': 'application/json'}

if service.auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_keys()[0]
elif service.token_auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_token()[0]

print(headers)

test_sample = json.dumps({'data': [
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
]})

response = requests.post(
    service.scoring_uri, data=test_sample, headers=headers)
print(response.status_code)
print(response.elapsed)
print(response.json())
```

Zie [client toepassingen maken om webservices te gebruiken](how-to-consume-web-service.md)voor meer informatie.

### <a name="web-service-schema-openapi-specification"></a>Web Service-schema (OpenAPI-specificatie)

Als u het automatisch genereren van schema's met de implementatie hebt gebruikt, kunt u het adres van de OpenAPI-specificatie voor de service ophalen met behulp van de [eigenschap swagger_uri](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservice?view=azure-ml-py#swagger-uri). Bijvoorbeeld `print(service.swagger_uri)`. Gebruik een GET-aanvraag (of open de URI in een browser) om de specificatie op te halen.

Het volgende JSON-document is een voor beeld van een schema (OpenAPI-specificatie) dat is gegenereerd voor een implementatie:

```json
{
    "swagger": "2.0",
    "info": {
        "title": "myservice",
        "description": "API specification for the Azure Machine Learning service myservice",
        "version": "1.0"
    },
    "schemes": [
        "https"
    ],
    "consumes": [
        "application/json"
    ],
    "produces": [
        "application/json"
    ],
    "securityDefinitions": {
        "Bearer": {
            "type": "apiKey",
            "name": "Authorization",
            "in": "header",
            "description": "For example: Bearer abc123"
        }
    },
    "paths": {
        "/": {
            "get": {
                "operationId": "ServiceHealthCheck",
                "description": "Simple health check endpoint to ensure the service is up at any given point.",
                "responses": {
                    "200": {
                        "description": "If service is up and running, this response will be returned with the content 'Healthy'",
                        "schema": {
                            "type": "string"
                        },
                        "examples": {
                            "application/json": "Healthy"
                        }
                    },
                    "default": {
                        "description": "The service failed to execute due to an error.",
                        "schema": {
                            "$ref": "#/definitions/ErrorResponse"
                        }
                    }
                }
            }
        },
        "/score": {
            "post": {
                "operationId": "RunMLService",
                "description": "Run web service's model and get the prediction output",
                "security": [
                    {
                        "Bearer": []
                    }
                ],
                "parameters": [
                    {
                        "name": "serviceInputPayload",
                        "in": "body",
                        "description": "The input payload for executing the real-time machine learning service.",
                        "schema": {
                            "$ref": "#/definitions/ServiceInput"
                        }
                    }
                ],
                "responses": {
                    "200": {
                        "description": "The service processed the input correctly and provided a result prediction, if applicable.",
                        "schema": {
                            "$ref": "#/definitions/ServiceOutput"
                        }
                    },
                    "default": {
                        "description": "The service failed to execute due to an error.",
                        "schema": {
                            "$ref": "#/definitions/ErrorResponse"
                        }
                    }
                }
            }
        }
    },
    "definitions": {
        "ServiceInput": {
            "type": "object",
            "properties": {
                "data": {
                    "type": "array",
                    "items": {
                        "type": "array",
                        "items": {
                            "type": "integer",
                            "format": "int64"
                        }
                    }
                }
            },
            "example": {
                "data": [
                    [ 10, 9, 8, 7, 6, 5, 4, 3, 2, 1 ]
                ]
            }
        },
        "ServiceOutput": {
            "type": "array",
            "items": {
                "type": "number",
                "format": "double"
            },
            "example": [
                3726.995
            ]
        },
        "ErrorResponse": {
            "type": "object",
            "properties": {
                "status_code": {
                    "type": "integer",
                    "format": "int32"
                },
                "message": {
                    "type": "string"
                }
            }
        }
    }
}
```

Zie voor meer informatie over de specificatie de [open API-specificatie](https://swagger.io/specification/).

Zie [Swagger-CodeGen](https://github.com/swagger-api/swagger-codegen)voor een hulp programma waarmee client bibliotheken kunnen worden gemaakt op basis van de specificatie.

### <a id="azuremlcompute"></a>Batch-deinterferentie
Azure Machine Learning Compute-doelen worden gemaakt en beheerd door de Azure Machine Learning-service. Ze kunnen worden gebruikt voor batch voorspelling van Azure Machine Learning pijp lijnen.

Lees voor een overzicht van batch deinterferentie met Azure Machine Learning Compute het artikel [batch voorspellingen uitvoeren](how-to-run-batch-predictions.md) .

### <a id="iotedge"></a>IoT Edge afleiding
Ondersteuning voor het implementeren naar de rand is in preview. Zie voor meer informatie het artikel [Deploy Azure machine learning als IOT Edge module](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning) .


## <a id="update"></a>Webservices bijwerken

[!INCLUDE [aml-update-web-service](../../../includes/machine-learning-update-web-service.md)]

## <a name="continuous-model-deployment"></a>Continue model implementatie 

U kunt met behulp van de Machine Learning-extensie voor [Azure DevOps](https://azure.microsoft.com/services/devops/)continu modellen implementeren. Door de Machine Learning-extensie voor Azure DevOps te gebruiken, kunt u een implementatie pijplijn activeren wanneer een nieuw machine learning model wordt geregistreerd in Azure Machine Learning service-werk ruimte. 

1. Meld u aan voor [Azure-pijp lijnen](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops), wat een continue integratie en levering van uw toepassing naar elk platform/elke Cloud mogelijk maakt. Azure-pijp lijnen [wijken af van ml-pijp lijnen](concept-ml-pipelines.md#compare). 

1. [Een Azure DevOps-project maken.](https://docs.microsoft.com/azure/devops/organizations/projects/create-project?view=azure-devops)

1. De [uitbrei ding voor de machine learning voor Azure-pijp lijnen](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml&targetId=6756afbe-7032-4a36-9cb6-2771710cadc2&utm_source=vstsproduct&utm_medium=ExtHubManageList) installeren 

1. Gebruik __service verbindingen__ om een Service-Principal-verbinding met uw Azure machine learning service-werk ruimte in te stellen voor toegang tot al uw artefacten. Ga naar project instellingen, klik op service verbindingen en selecteer Azure Resource Manager.

    ![weer gave-service-verbinding](media/how-to-deploy-and-where/view-service-connection.png) 

1. Definieer AzureMLWorkspace als __Scope niveau__ en vul de volgende para meters in.

    ![view-azure-resource-manager](media/how-to-deploy-and-where/resource-manager-connection.png)

1. Ga vervolgens als u uw machine learning model continu wilt implementeren met behulp van de Azure-pijp lijnen, onder pijp lijnen de optie __release__. Voeg een nieuw artefact toe, selecteer een model artefact voor AzureML en de service verbinding die in de vorige stap is gemaakt. Selecteer het model en de versie om een implementatie te activeren. 

    ![select-AzureMLmodel-artifact](media/how-to-deploy-and-where/enable-modeltrigger-artifact.png)

1. Schakel de model trigger in voor uw model artefact. Door de trigger in te scha kelen, elke keer dat de opgegeven versie (bijvoorbeeld de nieuwste versie) van het model is geregistreerd in uw werk ruimte, een Azure DevOps release-pijp lijn wordt geactiveerd. 

    ![Enable-model-trigger](media/how-to-deploy-and-where/set-modeltrigger.png)

Voor voorbeeld projecten en voor beelden raadpleegt u [de MLOps-opslag plaats](https://github.com/Microsoft/MLOps)

## <a name="clean-up-resources"></a>Resources opschonen
Als u wilt verwijderen van een geïmplementeerde webservice, gebruikt u `service.delete()`.
Als u wilt een geregistreerde model verwijderen, gebruikt u `model.delete()`.

Zie de referentie documentatie voor [webservice. Delete ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--)en [model. Delete ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
* [Een model implementeren met behulp van een aangepaste docker-installatie kopie](how-to-deploy-custom-docker-image.md)
* [Problemen met implementatie oplossen](how-to-troubleshoot-deployment.md)
* [Azure Machine Learning-webservices met SSL beveiligde](how-to-secure-web-service.md)
* [Een ML-Model dat is geïmplementeerd als een webservice gebruiken](how-to-consume-web-service.md)
* [Uw Azure Machine Learning modellen bewaken met Application Insights](how-to-enable-app-insights.md)
* [Gegevens verzamelen voor modellen in productie](how-to-enable-data-collection.md)

