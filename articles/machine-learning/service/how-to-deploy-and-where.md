---
title: Hoe en waar u kunt modellen implementeren
titleSuffix: Azure Machine Learning service
description: 'Meer informatie over hoe en waar uw Azure Machine Learning-service-modellen met inbegrip van implementeren: Azure Container Instances, Azure Kubernetes Service, Azure IoT Edge en Field-programmable gate arrays.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 07/08/2019
ms.custom: seoapril2019
ms.openlocfilehash: fb23e61142a639420d74c08e5a9a41324acab18b
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706286"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Implementeer modellen met de Azure Machine Learning-service

Informatie over het implementeren van uw machine learning-model als een webservice in de Azure-cloud, of op IoT Edge-apparaten. 

De werkstroom is vergelijkbaar, ongeacht de van [waarin u implementeren](#target) uw model:

1. Registreer het model.
1. Voorbereidingen voor het implementeren (assets, gebruik, compute-doel opgeven)
1. Het model implementeren naar de compute-doel.
1. Test de geïmplementeerd model, ook wel genoemd webservice.

Zie voor meer informatie over de concepten die betrokken zijn bij de implementatiewerkstroom [beheren, implementeren en controleren van modellen met Azure Machine Learning-Service](concept-model-management-and-deployment.md).

## <a name="prerequisites"></a>Vereisten

- Een model. Als u geen een getraind model hebt, kunt u het model en afhankelijkheidsbestanden die zijn opgegeven in [in deze zelfstudie](https://aka.ms/azml-deploy-cloud).

- De [Azure CLI-extensie voor Machine Learning-service](reference-azure-machine-learning-cli.md), [Azure Machine Learning Python SDK](https://aka.ms/aml-sdk), of de [Azure Machine Learning Visual Studio Code-extensie](how-to-vscode-tools.md).

## <a id="registermodel"></a> Registreer uw model

Een geregistreerde model logische container voor een of meer bestanden die gezamenlijk uw model. Bijvoorbeeld, als u een model dat is opgeslagen in meerdere bestanden hebt, kunt u registreren deze als één model in de werkruimte. Na de registratie, kunt u vervolgens downloaden of het geregistreerde model implementeren en ontvangen van alle bestanden die zijn geregistreerd.

Machine learning-modellen zijn geregistreerd in uw Azure Machine Learning-werkruimte. Het model kan afkomstig zijn van Azure Machine Learning of kan afkomstig zijn van een andere locatie. De volgende voorbeelden ziet u hoe u een model uit bestand te registreren:

### <a name="register-a-model-from-an-experiment-run"></a>Registreren van een model van een Experiment uitvoeren

+ **Scikit meer voorbeeld met de SDK**
  ```python
  model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
  print(model.name, model.id, model.version, sep='\t')
  ```

  > [!TIP]
  > Als u meerdere bestanden opnemen in de registratie van het model, wilt instellen `model_path` naar de map waarin de bestanden bevat.

+ **Met behulp van de CLI**

  ```azurecli-interactive
  az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment
  ```

  > [!TIP]
  > Als u meerdere bestanden opnemen in de registratie van het model, wilt instellen `--asset-path` naar de map waarin de bestanden bevat.

+ **Met behulp van Visual Studio Code**

  Registreren van modellen met behulp van een modelbestanden of mappen met de [VS Code](how-to-vscode-tools.md#deploy-and-manage-models) extensie.

### <a name="register-an-externally-created-model"></a>Een extern gemaakte model registreren

[!INCLUDE [trusted models](../../../includes/machine-learning-service-trusted-model.md)]

U kunt een extern gemaakte model registreren door op te geven een **lokaal pad** aan het model. U kunt een map of één bestand opgeven.

+ **U kunt ONNX-voorbeeld met de Python-SDK:**
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
  > Als u meerdere bestanden opnemen in de registratie van het model, wilt instellen `model_path` naar de map waarin de bestanden bevat.

+ **Met behulp van de CLI**
  ```azurecli-interactive
  az ml model register -n onnx_mnist -p mnist/model.onnx
  ```

  > [!TIP]
  > Als u meerdere bestanden opnemen in de registratie van het model, wilt instellen `-p` naar de map waarin de bestanden bevat.

**Geschatte tijd**: Ongeveer 10 seconden.

Zie voor meer informatie de documentatie bij de [Modelklasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

Zie voor meer informatie over het werken met modellen buiten Azure Machine Learning-service trainen, [over het implementeren van een bestaand model](how-to-deploy-existing-model.md).

<a name="target"></a>

## <a name="choose-a-compute-target"></a>Kies een compute-doel

De volgende compute-doelen, of compute-resources, kunnen worden gebruikt voor het hosten van uw implementatie van de webservice. 

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]

## <a name="prepare-to-deploy"></a>Implementatie voorbereiden

Als u wilt implementeren als een webservice, moet u de configuratie van een Deductie maken (`InferenceConfig`) en de configuratie van een implementatie. Deductie of het model scoren, is de fase waarin het gedistribueerde model wordt gebruikt voor voorspellingen, meestal op productiegegevens. In de configuratie van de Deductie geeft u de scripts en de afhankelijkheden die nodig zijn voor het bieden van uw model. In de implementatieconfiguratie van de geeft u informatie over het model op de compute-doel bedienen.


### <a id="script"></a> 1. De vermelding script & afhankelijkheden definiëren

Het script vermelding ontvangt gegevens verzonden naar een geïmplementeerde webservice, en wordt doorgegeven aan het model. Vervolgens wordt het antwoord geretourneerd door het model en retourneert die naar de client. **Het script is specifiek voor uw model**; deze moet informatie over de gegevens die het model wordt verwacht en retourneert.

Het script bevat twee functies die worden geladen en het model uitvoeren:

* `init()`: Deze functie worden doorgaans het model in een globale objecttoegang geladen. Deze functie wordt uitgevoerd slechts eenmaal wanneer de Docker-container voor uw webservice wordt gestart.

* `run(input_data)`: Deze functie maakt gebruik van het model om te voorspellen van een waarde op basis van de ingevoerde gegevens. Invoer en uitvoer aan het run maken doorgaans gebruik van JSON voor serialisatie en het deserialiseren. Ook kunt u werken met onbewerkte binaire gegevens. U kunt de gegevens te transformeren voordat ze worden verzonden naar het model of voordat u terugkeert naar de client.

#### <a name="what-is-getmodelpath"></a>Wat is get_model_path?

Als u een model registreert, kunt u een modelnaam die wordt gebruikt voor het beheren van het model in het register opgeven. U gebruikt deze naam door de [Model.get_model_path()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#get-model-path-model-name--version-none---workspace-none-) om op te halen van het pad van de model-bestanden op het lokale bestandssysteem. Als u zich registreert voor een map of een verzameling van bestanden, retourneert deze API het pad naar de map waarin de bestanden bevat.

Als u een model registreert, geeft u deze een naam die overeenkomt met waar het model wordt geplaatst, lokaal of tijdens de implementatie van de service.

Het onderstaande voorbeeld retourneert een pad naar een enkel bestand met de naam `sklearn_mnist_model.pkl` (die is geregistreerd met de naam van de `sklearn_mnist`):

```python
model_path = Model.get_model_path('sklearn_mnist')
``` 

#### <a name="optional-automatic-swagger-schema-generation"></a>(Optioneel) Automatisch genereren van Swagger-schema

Voor het automatisch genereren van een schema voor de webservice, staat een voorbeeld van de invoer en/of uitvoer in de constructor voor een van de objecten gedefinieerd type en het type en de voorbeeldgegevens gebruikt bij het automatisch maken het schema. Azure Machine Learning-service maakt vervolgens een [OpenAPI](https://swagger.io/docs/specification/about/) -specificatie (Swagger) voor de webservice tijdens de implementatie.

De volgende typen worden momenteel ondersteund:

* `pandas`
* `numpy`
* `pyspark`
* Standaard Python-object

Voor het gebruik van schema genereren, bevatten de `inference-schema` -pakket in uw bestand conda-omgeving. Het volgende voorbeeld wordt `[numpy-support]` omdat de post-script maakt gebruik van een type van de parameter numpy: 

#### <a name="example-dependencies-file"></a>Voorbeeld van afhankelijkheden-bestand
De volgende YAML is een voorbeeld van een Conda-afhankelijkheidsbestand voor Deductie.

```YAML
name: project_environment
dependencies:
  - python=3.6.2
  - pip:
    - azureml-defaults
    - scikit-learn==0.20.0
    - inference-schema[numpy-support]
```

Als u wilt gebruiken van uw script post-automatische schema genereren, **moet** importeren de `inference-schema` pakketten. 

Definieert u welke de invoer en uitvoer van de voorbeelden in de `input_sample` en `output_sample` variabelen, die staan voor de aanvraag- en indelingen voor de webservice. Gebruik deze voorbeelden in de invoer en uitvoer van functie decorators op de `run()` functie. De scikit-informatie over het volgende voorbeeld wordt een schema genereren.

> [!TIP]
> Nadat de service implementeert, gebruikt u de `swagger_uri` eigenschap voor het ophalen van het schema JSON-document.

#### <a name="example-entry-script"></a>Voorbeeld van invoer-script

Het volgende voorbeeld ziet u hoe u om te accepteren en JSON-gegevens geretourneerd:

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

input_sample = np.array([[10,9,8,7,6,5,4,3,2,1]])
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

#### <a name="example-script-with-dictionary-input-support-consumption-from-power-bi"></a>Voorbeeldscript met woordenlijst invoer (ondersteuning voor verbruik van Power BI)

Het volgende voorbeeld ziet u hoe u voor het definiëren van invoergegevens als < sleutel: waarde > woordenboek, met behulp van Dataframe. Deze methode wordt ondersteund voor het gebruik van de geïmplementeerde web-service van Power BI ([meer informatie over het gebruik van de webservice vanuit Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-integration)):

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
    model_path = Model.get_model_path('model_name')   # replace model_name with your actual model name, if needed
    # deserialize the model file back into a sklearn model
    model = joblib.load(model_path)

input_sample = pd.DataFrame(data=[{
              "input_name_1": 5.1,         # This is a decimal type sample. Use the data type that reflects this column in your data
              "input_name_2": "value2",    # This is a string type sample. Use the data type that reflects this column in your data
              "input_name_3": 3            # This is a integer type sample. Use the data type that reflects this column in your data
            }])

output_sample = np.array([0])              # This is a integer type sample. Use the data type that reflects the expected result

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
Zie voor meer voorbeeldscripts in de volgende voorbeelden:

* Pytorch: [https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch)
* TensorFlow: [https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow)
* Keras: [https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras)
* U KUNT ONNX: [https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/)
* Scores op basis van binaire gegevens: [Het gebruik van een webservice](how-to-consume-web-service.md)

### <a name="2-define-your-inferenceconfig"></a>2. Uw InferenceConfig definiëren

De configuratie van de Deductie wordt beschreven hoe het configureren van het model om voorspellingen te maken. Het volgende voorbeeld ziet u hoe u een configuratie Deductie maken. Deze configuratie geeft de runtime, de post-script en (optioneel) het bestand conda-omgeving:

```python
inference_config = InferenceConfig(runtime= "python",
                                   entry_script="x/y/score.py",
                                   conda_file="env/myenv.yml")
```

Zie voor meer informatie de [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) klasse verwijzing.

Zie voor meer informatie over het gebruik van een aangepaste Docker-installatiekopie met Deductie configuratie [over het implementeren van een model met behulp van een aangepaste Docker-installatiekopie](how-to-deploy-custom-docker-image.md).

### <a name="cli-example-of-inferenceconfig"></a>CLI-voorbeeld van InferenceConfig

De volgende JSON-document is een voorbeeldconfiguratie Deductie voor gebruik met de machine learning CLI:

```JSON
{
   "entryScript": "x/y/score.py",
   "runtime": "python",
   "condaFile": "env/myenv.yml",
   "sourceDirectory":"C:/abc",
}
```

De volgende entiteiten zijn geldig in dit bestand:

* __entryScript__: Pad naar een lokaal bestand met de code voor de installatiekopie van het uit te voeren.
* __Runtime__: Welke runtime moet worden gebruikt voor de installatiekopie. Huidige ondersteunde runtimes zijn 'spark-py' en 'python'.
* __condaFile__ (optioneel): Pad naar lokale bestanden met de definitie van een conda-omgeving moet worden gebruikt voor de installatiekopie.
* __extraDockerFileSteps__ (optioneel): Pad naar een lokaal bestand met aanvullende Docker-stappen om uit te voeren bij het instellen van de installatiekopie.
* __sourceDirectory__ (optioneel): Pad naar de mappen waarin alle bestanden om de installatiekopie te maken.
* __enableGpu__ (optioneel): Of om in te schakelen GPU of niet ondersteund in de afbeelding. De GPU-installatiekopie moet worden gebruikt op Microsoft Azure-Services zoals Azure Container Instances, Azure Machine Learning-Computing, Azure Virtual Machines en Azure Kubernetes Service. De standaardwaarde is False.
* __baseImage__ (optioneel): Een aangepaste installatiekopie moet worden gebruikt als basisinstallatiekopie. Als er geen basisinstallatiekopie is opgegeven, wordt de basisinstallatiekopie gebruikt op basis van de opgegeven Runtimeparameter.
* __baseImageRegistry__ (optioneel): De van het installatiekopieregister met de basisinstallatiekopie.
* __cudaVersion__ (optioneel): De versie van CUDA voor installatiekopieën die GPU-ondersteuning moeten installeren. De GPU-installatiekopie moet worden gebruikt op Microsoft Azure-Services zoals Azure Container Instances, Azure Machine Learning-Computing, Azure Virtual Machines en Azure Kubernetes Service. Ondersteunde versies zijn 9.0, 9.1 en 10.0. Als 'enable_gpu' is ingesteld, standaard '9.1'.

Deze entiteiten worden toegewezen aan de parameters voor de [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) klasse.

Thee volgende opdracht laat zien hoe u een model met behulp van de CLI implementeren:

```azurecli-interactive
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json
```

In dit voorbeeld bevat de configuratie van de volgende items:

* Een map met activa die nodig zijn voor Deductie
* Dat Python voor dit model is vereist
* De [vermelding script](#script), die wordt gebruikt voor het verwerken van webaanvragen naar de geïmplementeerde service verzonden
* Het conda-bestand met een beschrijving van de Python-pakketten die nodig zijn voor Deductie

Zie voor meer informatie over het gebruik van een aangepaste Docker-installatiekopie met Deductie configuratie [over het implementeren van een model met behulp van een aangepaste Docker-installatiekopie](how-to-deploy-custom-docker-image.md).

### <a name="3-define-your-deployment-configuration"></a>3. De configuratie van implementatie definiëren

Voordat u implementeert, moet u de configuratie van de implementatie te definiëren. De implementatieconfiguratie is specifiek voor de compute-doel die als voor de webservice host. Bijvoorbeeld bij het implementeren van lokaal moet u de poort opgeven waar de service-aanvragen accepteert.

U moet mogelijk ook de compute-resource maken. Bijvoorbeeld, als u nog niet hebt een Azure Kubernetes Service die is gekoppeld aan uw werkruimte.

De volgende tabel geeft een voorbeeld van het maken van een implementatieconfiguratie voor de compute-doel:

| COMPUTE-doel | Voorbeeld van de configuratie van implementatie |
| ----- | ----- |
| Lokaal | `deployment_config = LocalWebservice.deploy_configuration(port=8890)` |
| Azure Container Instance | `deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |
| Azure Kubernetes Service | `deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |

De volgende secties laten zien hoe u de configuratie van de implementatie te maken en vervolgens worden gebruikt om de webservice te implementeren.

### <a name="optional-profile-your-model"></a>Optioneel: Het model van uw profiel
Voordat u uw modellen vervolgens als een service implementeert, kunt u profileren om te controleren van de optimale CPU en geheugen nodig met behulp van de SDK of de CLI.  Model profilering resultaten worden verzonden als een `Run` object. De volledige details van [de schema-Model profiel kunt u vinden in de API-documentatie](https://docs.microsoft.com/python/api/azureml-core/azureml.core.profile.modelprofile?view=azure-ml-py)

Meer informatie over [hoe u uw model met behulp van de SDK-profiel](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#profile-workspace--profile-name--models--inference-config--input-data-)

## <a name="deploy-to-target"></a>Implementeren naar doel

### <a id="local"></a> Lokale implementatie

Voor het lokaal implementeren, moet u beschikken over **Docker geïnstalleerd** op uw lokale computer.

+ **Met behulp van de SDK**

  ```python
  deployment_config = LocalWebservice.deploy_configuration(port=8890)
  service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  ```

+ **Met behulp van de CLI**

    Als u wilt implementeren met behulp van de CLI, gebruikt u de volgende opdracht uit. Vervang `mymodel:1` met de naam en versie van het geregistreerde model:

  ```azurecli-interactive
  az ml model deploy -m mymodel:1 -ic inferenceconfig.json -dc deploymentconfig.json
  ```

    De vermeldingen in de `deploymentconfig.json` documentstructuur aan de parameters voor [LocalWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservicedeploymentconfiguration?view=azure-ml-py). De volgende tabel beschrijft de toewijzing tussen de entiteiten in het JSON-document en de parameters voor de methode:

    | JSON-entiteit | Parameter van de methode | Description |
    | ----- | ----- | ----- |
    | `computeType` | N.v.t. | Het rekendoel. Voor lokale, de waarde moet `local`. |
    | `port` | `port` | De lokale poort waarop HTTP-eindpunt van de service beschikbaar maken. |

    De volgende JSON wordt een voorbeeld van de implementatieconfiguratie voor gebruik met de CLI:

    ```json
    {
        "computeType": "local",
        "port": 32267
    }
    ```

### <a id="aci"></a> Azure Container Instances (DEVTEST)

Gebruik Azure Container Instances voor het implementeren van uw modellen als een webservice die als één of meer van de volgende voorwaarden is waar:
- U moet sneller te implementeren en valideren van uw model.
- U test een model dat is in ontwikkeling. 

Quota en regio de beschikbaarheid voor ACI, Zie de [quota en beschikbaarheid in regio's voor Azure Container Instances](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) artikel.

+ **Met behulp van de SDK**

  ```python
  deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
  service = Model.deploy(ws, "aciservice", [model], inference_config, deployment_config)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  ```

+ **Met behulp van de CLI**

    Als u wilt implementeren met behulp van de CLI, gebruikt u de volgende opdracht uit. Vervang `mymodel:1` met de naam en versie van het geregistreerde model. Vervang `myservice` met de naam te geven van deze service:

    ```azurecli-interactive
    az ml model deploy -m mymodel:1 -n myservice -ic inferenceconfig.json -dc deploymentconfig.json
    ```

    De vermeldingen in de `deploymentconfig.json` documentstructuur aan de parameters voor [AciWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aci.aciservicedeploymentconfiguration?view=azure-ml-py). De volgende tabel beschrijft de toewijzing tussen de entiteiten in het JSON-document en de parameters voor de methode:

    | JSON-entiteit | Parameter van de methode | Description |
    | ----- | ----- | ----- |
    | `computeType` | N.v.t. | Het rekendoel. Voor ACI, de waarde moet `ACI`. |
    | `containerResourceRequirements` | N.v.t. | Bevat configuratie-elementen voor de CPU en geheugen dat is toegewezen voor de container. |
    | &emsp;&emsp;`cpu` | `cpu_cores` | Het aantal CPU-kernen toewijzen voor deze webservice. Standaardinstellingen `0.1` |
    | &emsp;&emsp;`memoryInGB` | `memory_gb` | De hoeveelheid geheugen (in GB) om toe te wijzen voor deze webservice. Standaard `0.5` |
    | `location` | `location` | De Azure-regio deze Webservice te implementeren. Als u niets opgeeft van de werkruimte locatie wordt gebruikt. Meer informatie over de beschikbare regio's vindt u hier: [ACI-regio 's](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=container-instances) |
    | `authEnabled` | `auth_enabled` | Wel of niet auth inschakelen voor deze Webservice. Standaard ingesteld op onwaar |
    | `sslEnabled` | `ssl_enabled` | Wel of niet SSL inschakelen voor deze Webservice. De standaardwaarde is False. |
    | `appInsightsEnabled` | `enable_app_insights` | Wel of niet AppInsights inschakelen voor deze Webservice. Standaard ingesteld op onwaar |
    | `sslCertificate` | `ssl_cert_pem_file` | Het certificaat-bestand nodig als SSL is ingeschakeld |
    | `sslKey` | `ssl_key_pem_file` | Het sleutelbestand nodig als SSL is ingeschakeld |
    | `cname` | `ssl_cname` | De cname voor als SSL is ingeschakeld |
    | `dnsNameLabel` | `dns_name_label` | De dns-naamlabel voor het scoring-eindpunt. Als een unieke dns-naamlabel wordt gegenereerd voor het scoring-eindpunt niet is opgegeven. |

    De volgende JSON wordt een voorbeeld van de implementatieconfiguratie voor gebruik met de CLI:

    ```json
    {
        "computeType": "aci",
        "containerResourceRequirements":
        {
            "cpu": 0.5,
            "memoryInGB": 1.0
        },
        "authEnabled": true,
        "sslEnabled": false,
        "appInsightsEnabled": false
    }
    ```

+ **Met behulp van Visual Studio Code**

  Naar [Implementeer uw modellen met VS Code](how-to-vscode-tools.md#deploy-and-manage-models) u hoeft te maken van een ACI-container voor het testen van tevoren regelen, omdat ACI containers op elk gewenst moment worden gemaakt.

Zie voor meer informatie de documentatie bij de [AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) en [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py) klassen.

### <a id="aks"></a>Azure Kubernetes Service (DEVTEST en productie)

U kunt een bestaand AKS-cluster gebruiken of een nieuwe maken met behulp van de SDK van Azure Machine Learning, CLI of Azure portal.

<a id="deploy-aks"></a>

Als u al een AKS-cluster dat is gekoppeld, kunt u ernaar implementeren. Als u dit nog niet hebt gemaakt of die zijn gekoppeld een AKS-cluster, voert u het proces voor het <a href="#create-attach-aks">een nieuw AKS-cluster maken</a>.

+ **Met behulp van de SDK**

  ```python
  aks_target = AksCompute(ws,"myaks")
  # If deploying to a cluster configured for dev/test, ensure that it was created with enough
  # cores and memory to handle this deployment configuration. Note that memory is also used by
  # things such as dependencies and AML components.
  deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
  service = Model.deploy(ws, "aksservice", [model], inference_config, deployment_config, aks_target)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  print(service.get_logs())
  ```

+ **Met behulp van de CLI**

    Als u wilt implementeren met behulp van de CLI, gebruikt u de volgende opdracht uit. Vervang `myaks` met de naam van de AKS compute-doel. Vervang `mymodel:1` met de naam en versie van het geregistreerde model. Vervang `myservice` met de naam te geven van deze service:

  ```azurecli-interactive
  az ml model deploy -ct myaks -m mymodel:1 -n myservice -ic inferenceconfig.json -dc deploymentconfig.json
  ```

    De vermeldingen in de `deploymentconfig.json` documentstructuur aan de parameters voor [AksWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py). De volgende tabel beschrijft de toewijzing tussen de entiteiten in het JSON-document en de parameters voor de methode:

    | JSON-entiteit | Parameter van de methode | Description |
    | ----- | ----- | ----- |
    | `computeType` | N.v.t. | Het rekendoel. Voor AKS, de waarde moet `aks`. |
    | `autoScaler` | N.v.t. | Bevat configuratie-elementen voor automatisch schalen. Zie de tabel automatisch schalen. |
    | &emsp;&emsp;`autoscaleEnabled` | `autoscale_enabled` | Al dan niet om in te schakelen van automatisch schalen voor de webservice. Als `numReplicas`  =  `0`, `True`; anders `False`. |
    | &emsp;&emsp;`minReplicas` | `autoscale_min_replicas` | Het minimum aantal containers moeten worden gebruikt wanneer automatisch schalen deze webservice. Standaard `1`. |
    | &emsp;&emsp;`maxReplicas` | `autoscale_max_replicas` | Het maximum aantal containers moeten worden gebruikt wanneer automatisch schalen deze webservice. Standaard `10`. |
    | &emsp;&emsp;`refreshPeriodInSeconds` | `autoscale_refresh_seconds` | Hoe vaak probeert het automatisch schalen om deze webservice te schalen. Standaard `1`. |
    | &emsp;&emsp;`targetUtilization` | `autoscale_target_utilization` | De doelverbruik (in % van de 100) die het automatisch schalen voor deze webservice moet proberen. Standaard `70`. |
    | `dataCollection` | N.v.t. | Bevat configuratie-elementen voor het verzamelen van gegevens. |
    | &emsp;&emsp;`storageEnabled` | `collect_model_data` | Wel of niet inschakelen van gegevensverzameling van model voor de webservice. Standaard `False`. |
    | `authEnabled` | `auth_enabled` | Wel of niet-verificatie inschakelen voor de webservice. Standaard `True`. |
    | `containerResourceRequirements` | N.v.t. | Bevat configuratie-elementen voor de CPU en geheugen dat is toegewezen voor de container. |
    | &emsp;&emsp;`cpu` | `cpu_cores` | Het aantal CPU-kernen toewijzen voor deze webservice. Standaardinstellingen `0.1` |
    | &emsp;&emsp;`memoryInGB` | `memory_gb` | De hoeveelheid geheugen (in GB) om toe te wijzen voor deze webservice. Standaard `0.5` |
    | `appInsightsEnabled` | `enable_app_insights` | Wel of niet Application Insights logboekregistratie inschakelen voor de webservice. Standaard `False`. |
    | `scoringTimeoutMs` | `scoring_timeout_ms` | Een time-out om af te dwingen voor het scoren van aanroepen naar de webservice. Standaard `60000`. |
    | `maxConcurrentRequestsPerContainer` | `replica_max_concurrent_requests` | Het maximum aantal gelijktijdige aanvragen per knooppunt voor deze webservice. Standaard `1`. |
    | `maxQueueWaitMs` | `max_request_wait_time` | De maximale tijd die een aanvraag in thee wachtrij (in milliseconden) voordat u een 503 blijven fout is geretourneerd. Standaard `500`. |
    | `numReplicas` | `num_replicas` | Het aantal containers om toe te wijzen voor deze webservice. Er is geen standaardwaarde. Als deze parameter niet is ingesteld, wordt het automatisch schalen is standaard ingeschakeld. |
    | `keys` | N.v.t. | Bevat configuratie-elementen voor sleutels. |
    | &emsp;&emsp;`primaryKey` | `primary_key` | Een primaire verificatiesleutel moet worden gebruikt voor deze Webservice |
    | &emsp;&emsp;`secondaryKey` | `secondary_key` | Een secundaire verificatiesleutel moet worden gebruikt voor deze Webservice |
    | `gpuCores` | `gpu_cores` | Het aantal kerngeheugens toewijzen voor deze Webservice GPU. Standaard is 1. |
    | `livenessProbeRequirements` | N.v.t. | Bevat configuratie-elementen voor liveness test vereisten. |
    | &emsp;&emsp;`periodSeconds` | `period_seconds` | Hoe vaak (in seconden) om uit te voeren van de activiteitstest. Standaard 10 seconden. De minimumwaarde is 1. |
    | &emsp;&emsp;`initialDelaySeconds` | `initial_delay_seconds` | Het aantal seconden nadat de container is gestart voordat liveness tests worden gestart. Standaard ingesteld op 310 |
    | &emsp;&emsp;`timeoutSeconds` | `timeout_seconds` | Het aantal seconden waarna de activiteitstest een optreedt time-out. Standaard ingesteld op 2 seconden. De minimumwaarde is 1 |
    | &emsp;&emsp;`successThreshold` | `success_threshold` | Minimale opeenvolgende successen voor de activiteitstest om te worden beschouwd als geslaagd na dat is mislukt. Standaard ingesteld op 1. De minimumwaarde is 1. |
    | &emsp;&emsp;`failureThreshold` | `failure_threshold` | Wanneer een schil wordt gestart en de activiteitstest mislukt, probeert Kubernetes failureThreshold tijden voordat geeft. De standaardwaarde is 3. De minimumwaarde is 1. |
    | `namespace` | `namespace` | De Kubernetes-naamruimte die de webservice wordt geïmplementeerd in. Maximaal 63 kleine letters, alfanumerieke tekens ('a'-'z', '0'-'9') en afbreekstreepjes ('-') tekens. De eerste en laatste tekens mogen geen streepjes zijn. |

    De volgende JSON wordt een voorbeeld van de implementatieconfiguratie voor gebruik met de CLI:

    ```json
    {
        "computeType": "aks",
        "autoScaler":
        {
            "autoscaleEnabled": true,
            "minReplicas": 1,
            "maxReplicas": 3,
            "refreshPeriodInSeconds": 1,
            "targetUtilization": 70
        },
        "dataCollection":
        {
            "storageEnabled": true
        },
        "authEnabled": true,
        "containerResourceRequirements":
        {
            "cpu": 0.5,
            "memoryInGB": 1.0
        }
    }
    ```

+ **Met behulp van Visual Studio Code**

  U kunt ook [implementeren naar AKS via de VS Code-extensie](how-to-vscode-tools.md#deploy-and-manage-models), maar u moet vooraf configureren van AKS-clusters.

Meer informatie over AKS-implementatie en voor automatisch schalen in de [AksWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice) verwijzing.

#### Een nieuw AKS-cluster maken<a id="create-attach-aks"></a>
**Geschatte tijd**: Ongeveer 20 minuten.

Het maken of koppelen van een AKS-cluster een eenmalige is verwerking van voor uw werkruimte. U kunt dit cluster voor meerdere implementaties opnieuw gebruiken. Als u het cluster of de resourcegroep waarin het verwijdert, moet u een nieuw cluster maken de volgende keer dat u wilt implementeren. U kunt meerdere AKS-clusters die zijn gekoppeld aan uw werkruimte hebben.

Als u maken van een AKS-cluster voor ontwikkeling, validatie en het testen wilt, stelt u `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` bij het gebruik van [ `provisioning_configuration()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py). Een cluster gemaakt met deze instelling heeft slechts één knooppunt.

> [!IMPORTANT]
> Instellen van `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` maakt u een AKS-cluster dat is niet geschikt voor het verwerken van productieverkeer. Deductie keer mogelijk ook langer dan op een cluster dat is gemaakt voor productie. Fouttolerantie is ook geen garantie voor dev/test-clusters.
>
> U wordt aangeraden dat clusters die zijn gemaakt voor ontwikkelen/testen ten minste twee virtuele CPU's gebruiken.

Het volgende voorbeeld ziet u hoe u een nieuw Azure Kubernetes Service-cluster maken:

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (you can also provide parameters to customize this).
# For example, to create a dev/test cluster, use:
# prov_config = AksCompute.provisioning_configuration(cluster_purpose = AksComputee.ClusterPurpose.DEV_TEST)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'myaks'
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws,
                                    name = aks_name,
                                    provisioning_configuration = prov_config)

# Wait for the create process to complete
aks_target.wait_for_completion(show_output = True)
```

Zie de volgende artikelen voor meer informatie over het maken van een AKS-cluster buiten de Azure Machine Learning-SDK:
* [Een AKS-cluster maken](https://docs.microsoft.com/cli/azure/aks?toc=%2Fazure%2Faks%2FTOC.json&bc=%2Fazure%2Fbread%2Ftoc.json&view=azure-cli-latest#az-aks-create)
* [Maken van een AKS-cluster (portal)](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal?view=azure-cli-latest)

Voor meer informatie over de `cluster_purpose` parameter, Zie de [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py) verwijzing.

> [!IMPORTANT]
> Voor [ `provisioning_configuration()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py), als u aangepaste waarden voor agent_count en vm_size, kiest, moet u om ervoor te zorgen agent_count vermenigvuldigd met vm_size is groter dan of gelijk zijn aan 12 virtuele CPU's. Bijvoorbeeld, als u een vm_size van 'Standard_D3_v2', met 4 virtuele CPU's, moet vervolgens u kiezen een agent_count van 3 of hoger.
>
> De SDK van Azure Machine Learning biedt geen ondersteuning voor een AKS-cluster schalen. Als u wilt schalen van de knooppunten in het cluster worden de gebruikersinterface voor het AKS-cluster in Azure portal te gebruiken. U kunt alleen het aantal knooppunten, niet de VM-grootte van het cluster wijzigen.

#### <a name="attach-an-existing-aks-cluster"></a>Een bestaand AKS-cluster koppelen
**Geschatte tijd:** Ongeveer 5 minuten.

Als u al AKS-cluster in uw Azure-abonnement en het is versie 1.12. ##, kunt u het implementeren van uw installatiekopie.

> [!WARNING]
> Wanneer u een AKS-cluster koppelen aan een werkruimte, kunt u definiëren hoe u het cluster wordt gebruikt door in te stellen de `cluster_purpose` parameter.
>
> Als u geen instelt de `cluster_purpose` parameter of stel deze in `cluster_purpose = AksCompute.ClusterPurpose.FAST_PROD`, en vervolgens het cluster ten minste 12 virtuele CPU's beschikbaar moet.
>
> Als u `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST`, en vervolgens het cluster niet hoeft te hebben van 12 virtuele CPU's. Echter een cluster dat is geconfigureerd voor ontwikkelen/testen wordt niet altijd geschikt voor niveau productieverkeer en Deductie tijden verhogen.

De volgende code laat zien hoe u een bestaand AKS 1.12 koppelen. ## cluster aan uw werkruimte:

```python
from azureml.core.compute import AksCompute, ComputeTarget
# Set the resource group that contains the AKS cluster and the cluster name
resource_group = 'myresourcegroup'
cluster_name = 'mycluster'

# Attach the cluster to your workgroup. If the cluster has less than 12 virtual CPUs, use the following instead:
# attach_config = AksCompute.attach_configuration(resource_group = resource_group,
#                                         cluster_name = cluster_name,
#                                         cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST)
attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
aks_target = ComputeTarget.attach(ws, 'mycompute', attach_config)
```

Voor meer informatie over `attack_configuration()`, Zie de [AksCompute.attach_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-) verwijzing.

Voor meer informatie over de `cluster_purpose` parameter, Zie de [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py) verwijzing.

## <a name="consume-web-services"></a>Webservices gebruiken

Elke geïmplementeerde webservice biedt een REST-API, zodat u toepassingen in diverse programmeertalen maken kunt. Als u verificatie hebt ingeschakeld voor uw service, moet u een servicesleutel opgeven als een token in de aanvraagheader.

### <a name="request-response-consumption"></a>Request response-verbruik

Hier volgt een voorbeeld van hoe u uw service in Python aanroepen:
```python
import requests
import json

headers = {'Content-Type':'application/json'}

if service.auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_keys()[0]

print(headers)
    
test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})

response = requests.post(service.scoring_uri, data=test_sample, headers=headers)
print(response.status_code)
print(response.elapsed)
print(response.json())
```

Zie voor meer informatie, [-client maken toepassingen gebruik van webservices kunnen](how-to-consume-web-service.md).


### <a id="azuremlcompute"></a> Batch Deductie
Azure Machine Learning-Computing doelen worden gemaakt en beheerd door de Azure Machine Learning-service. Ze kunnen worden gebruikt voor batch voorspelling van Azure Machine Learning-pijplijnen.

Lees voor een overzicht van batch Deductie met Azure Machine Learning-Computing, de [hoe u Batch voorspellingen uitvoeren](how-to-run-batch-predictions.md) artikel.

### <a id="iotedge"></a> IoT Edge Deductie
Ondersteuning voor het implementeren van de rand is in preview. Zie voor meer informatie de [Azure Machine Learning implementeren als een IoT Edge-module](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning) artikel.


## <a id="update"></a> Het bijwerken van webservices

Wanneer u een nieuw model maakt, moet u elke service die u wilt gebruiken van het nieuwe model handmatig bijwerken. Voor het bijwerken van de webservice, gebruikt u de `update` methode. De volgende code ziet u hoe u de webservice voor het gebruik van een nieuw model bijwerken:

```python
from azureml.core.webservice import Webservice
from azureml.core.model import Model

# register new model
new_model = Model.register(model_path = "outputs/sklearn_mnist_model.pkl",
                       model_name = "sklearn_mnist",
                       tags = {"key": "0.1"},
                       description = "test",
                       workspace = ws)

service_name = 'myservice'
# Retrieve existing service
service = Webservice(name = service_name, workspace = ws)

# Update to new model(s)
service.update(models = [new_model])
print(service.state)
print(service.get_logs())
```

## <a name="continuous-model-deployment"></a>Continue modelimplementatie 

U kunt modellen met behulp van de Machine Learning-extensie voor continu implementeren [Azure DevOps](https://azure.microsoft.com/services/devops/). U kunt met behulp van de Machine Learning-extensie voor Azure DevOps, een implementatiepijplijn activeren wanneer een nieuwe machine learning-model is geregistreerd in de werkruimte van Azure Machine Learning-service. 

1. Zich aanmelden voor [Azure pijplijnen](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops), waardoor continue integratie en levering van uw toepassing naar een willekeurig platform/elk cloud mogelijk. Azure pijplijnen [wijkt af van ML pijplijnen](concept-ml-pipelines.md#compare). 

1. [Maak een Azure DevOps-project.](https://docs.microsoft.com/azure/devops/organizations/projects/create-project?view=azure-devops)

1. Installeer de [Machine Learning-extensie voor Azure-pijplijnen](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml&targetId=6756afbe-7032-4a36-9cb6-2771710cadc2&utm_source=vstsproduct&utm_medium=ExtHubManageList) 

1. Gebruik __verbindingen__ voor het instellen van een service-principal verbinding naar de werkruimte van uw Azure Machine Learning-service toegang tot alle uw artefacten. Ga naar instellingen voor het project, klikt u op verbindingen met de service en selecteert u Azure Resource Manager.

    ![weergave-service-verbinding](media/how-to-deploy-and-where/view-service-connection.png) 

1. Definieer AzureMLWorkspace als de __bereik niveau__ en vult u de volgende parameters.

    ![view-azure-resource-manager](media/how-to-deploy-and-where/resource-manager-connection.png)

1. Voor het continu implementeren van uw machine learning-model met behulp van de Azure-pijplijnen, onder pijplijnen Selecteer vervolgens __release__. Een nieuwe artefact toevoegen, selecteert u artefact AzureML-Model en de serviceverbinding die is gemaakt in de vorige stap. Selecteer het model en versie voor het activeren van een implementatie. 

    ![select-AzureMLmodel-artifact](media/how-to-deploy-and-where/enable-modeltrigger-artifact.png)

1. Schakel de model-trigger in uw model artefact. Door het inschakelen van de trigger elke keer de opgegeven versie (zoals) de nieuwste versie) van dit model registreren in uw werkruimte, wordt een Azure DevOps-release-pijplijn wordt geactiveerd. 

    ![enable-model-trigger](media/how-to-deploy-and-where/set-modeltrigger.png)

Bekijk voor voorbeeldprojecten en voorbeelden, [de MLOps-opslagplaats](https://github.com/Microsoft/MLOps)

## <a name="clean-up-resources"></a>Resources opschonen
Als u wilt verwijderen van een geïmplementeerde webservice, gebruikt u `service.delete()`.
Als u wilt een geregistreerde model verwijderen, gebruikt u `model.delete()`.

Zie voor meer informatie de documentatie bij [WebService.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--), en [Model.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--).

## <a name="next-steps"></a>Volgende stappen
* [Over het implementeren van een model met behulp van een aangepaste Docker-installatiekopie](how-to-deploy-custom-docker-image.md)
* [Problemen met implementatie oplossen](how-to-troubleshoot-deployment.md)
* [Azure Machine Learning-webservices met SSL beveiligde](how-to-secure-web-service.md)
* [Een ML-Model dat is geïmplementeerd als een webservice gebruiken](how-to-consume-web-service.md)
* [Uw Azure Machine Learning-modellen met Application Insights bewaken](how-to-enable-app-insights.md)
* [Verzamelen van gegevens voor modellen in productie](how-to-enable-data-collection.md)

