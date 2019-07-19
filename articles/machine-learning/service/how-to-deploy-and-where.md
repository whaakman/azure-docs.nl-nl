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
ms.date: 07/08/2019
ms.custom: seoapril2019
ms.openlocfilehash: 796118999041b2bef2d51657901e9e399578e97c
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68327037"
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
  az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment
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

#### <a name="what-is-getmodelpath"></a>Wat is get_model_path?

Wanneer u een model registreert, geeft u een model naam op die wordt gebruikt voor het beheren van het model in het REGI ster. U gebruikt deze naam voor het [model. Get _model_path ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#get-model-path-model-name--version-none---workspace-none-) om het pad van het model bestand (en) op het lokale bestands systeem op te halen. Als u een map of een verzameling bestanden registreert, retourneert deze API het pad naar de map waarin zich de bestanden bevinden.

Wanneer u een model registreert, geeft u het een naam die overeenkomt met waar het model lokaal of tijdens de service-implementatie wordt geplaatst.

In het onderstaande voor beeld wordt een pad geretourneerd naar één bestand `sklearn_mnist_model.pkl` (dat is geregistreerd met de naam `sklearn_mnist`):

```python
model_path = Model.get_model_path('sklearn_mnist')
``` 

#### <a name="optional-automatic-swagger-schema-generation"></a>Beschrijving Automatische Swagger-schema's genereren

Als u automatisch een schema voor uw webservice wilt genereren, geeft u een voor beeld van de invoer en/of uitvoer in de constructor voor een van de gedefinieerde type-objecten. het type en het voor beeld wordt gebruikt om automatisch het schema te maken. Azure Machine Learning-service maakt vervolgens een [OpenAPI](https://swagger.io/docs/specification/about/) -specificatie (Swagger) voor de webservice tijdens de implementatie.

De volgende typen worden momenteel ondersteund:

* `pandas`
* `numpy`
* `pyspark`
* Standard python-object

Als u schema generatie wilt gebruiken, `inference-schema` neemt u het pakket op in uw Conda-omgevings bestand. In het volgende voor `[numpy-support]` beeld wordt gebruikgemaakt van het gebruik van een numpy-parameter type: 

#### <a name="example-dependencies-file"></a>Voor beeld van afhankelijkheids bestand
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

> [!TIP]
> Nadat de service is geïmplementeerd, gebruikt `swagger_uri` u de eigenschap om het schema-JSON-document op te halen.

#### <a name="example-entry-script"></a>Voorbeeld script

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

#### <a name="example-script-with-dictionary-input-support-consumption-from-power-bi"></a>Voorbeeld script met woordenlijst invoer (ondersteunings verbruik van Power BI)

In het volgende voor beeld ziet u hoe u invoer gegevens definieert als < sleutel: waarde > woorden lijst met behulp van data frame. Deze methode wordt ondersteund voor het gebruik van de geïmplementeerde webservice van Power BI (meer informatie over het[gebruiken van de webservice van Power bi](https://docs.microsoft.com/power-bi/service-machine-learning-integration)):

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
Zie de volgende voor beelden voor meer voorbeeld scripts:

* Pytorch[https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch)
* Tensor flow[https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow)
* Keras[https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras)
* ONNX[https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/)
* Score voor binaire gegevens: [Een webservice gebruiken](how-to-consume-web-service.md)

### <a name="2-define-your-inferenceconfig"></a>2. Uw InferenceConfig definiëren

De configuratie voor afnemen beschrijft hoe u het model configureert om voor spellingen te maken. In het volgende voor beeld ziet u hoe u een configuratie voor het afwijzen van een interferentie maakt. Deze configuratie specificeert de runtime, het vermeldings script en (optioneel) het Conda-omgevings bestand:

```python
inference_config = InferenceConfig(runtime= "python",
                                   entry_script="x/y/score.py",
                                   conda_file="env/myenv.yml")
```

Zie voor meer informatie de [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) -klasse verwijzing.

Zie [een model implementeren met behulp van een aangepaste docker-installatie kopie](how-to-deploy-custom-docker-image.md)voor informatie over het gebruik van een aangepaste docker-installatie kopie met behulp van de configuratie voor inhalen.

### <a name="cli-example-of-inferenceconfig"></a>CLI-voor beeld van InferenceConfig

Het volgende JSON-document is een voor beeld van een configuratie voor het afgebruiken van een ingrijpen voor gebruik met de machine learning CLI:

```JSON
{
   "entryScript": "x/y/score.py",
   "runtime": "python",
   "condaFile": "env/myenv.yml",
   "sourceDirectory":"C:/abc",
}
```

De volgende entiteiten zijn geldig in dit bestand:

* __entryScript__: Het pad naar het lokale bestand dat de code bevat die moet worden uitgevoerd voor de installatie kopie.
* __runtime__: Welke runtime moet worden gebruikt voor de installatie kopie. De huidige ondersteunde Runtimes zijn ' Spark-py ' en ' python '.
* __condaFile__ (optioneel): Het pad naar het lokale bestand met een Conda-omgevings definitie die moet worden gebruikt voor de installatie kopie.
* __extraDockerFileSteps__ (optioneel): Pad naar lokaal bestand met extra docker-stappen die moeten worden uitgevoerd bij het instellen van de installatie kopie.
* __Source Directory__ (optioneel): Pad naar mappen met alle bestanden om de installatie kopie te maken.
* __enableGpu__ (optioneel): Hiermee wordt aangegeven of GPU-ondersteuning in de installatie kopie moet worden ingeschakeld. De GPU-installatie kopie moet worden gebruikt voor Microsoft Azure Services, zoals Azure Container Instances, Azure Machine Learning compute, Azure Virtual Machines en de Azure Kubernetes-service. De standaard waarde is False.
* __baseImage__ (optioneel): Een aangepaste installatie kopie die moet worden gebruikt als basis installatie kopie. Als er geen basis installatie kopie wordt opgegeven, wordt de basis installatie kopie gebruikt op basis van de opgegeven runtime parameter.
* __baseImageRegistry__ (optioneel): Het REGI ster van de installatie kopie met de basis installatie kopie.
* __cudaVersion__ (optioneel): De versie van CUDA die moet worden geïnstalleerd voor installatie kopieën waarvoor GPU-ondersteuning is vereist. De GPU-installatie kopie moet worden gebruikt voor Microsoft Azure Services, zoals Azure Container Instances, Azure Machine Learning compute, Azure Virtual Machines en de Azure Kubernetes-service. Ondersteunde versies zijn 9,0, 9,1 en 10,0. Als ' enable_gpu ' is ingesteld, wordt de standaard waarde van ' 9,1 ' gebruikt.

Deze entiteiten worden toegewezen aan de para meters voor de klasse [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) .

Thee volgende opdracht ziet u hoe u een model implementeert met behulp van de CLI:

```azurecli-interactive
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json
```

In dit voor beeld bevat de configuratie de volgende items:

* Een map die assets bevat die nodig zijn om te deactiveren
* Dat dit model python vereist
* Het [invoer script](#script), dat wordt gebruikt voor het afhandelen van webaanvragen die worden verzonden naar de geïmplementeerde service
* Het Conda-bestand met een beschrijving van de Python-pakketten die nodig zijn om aan te doen

Zie [een model implementeren met behulp van een aangepaste docker-installatie kopie](how-to-deploy-custom-docker-image.md)voor informatie over het gebruik van een aangepaste docker-installatie kopie met behulp van de configuratie voor inhalen.

### <a name="3-define-your-deployment-configuration"></a>3. Uw implementatie configuratie definiëren

Voordat u implementeert, moet u de implementatie configuratie definiëren. De implementatie configuratie is specifiek voor het reken doel dat als host fungeert voor de webservice. Wanneer u bijvoorbeeld lokaal implementeert, moet u de poort opgeven waar de service aanvragen accepteert.

Mogelijk moet u ook de reken resource maken. Bijvoorbeeld, als u nog geen Azure Kubernetes-service hebt die aan uw werk ruimte is gekoppeld.

De volgende tabel bevat een voor beeld van het maken van een implementatie configuratie voor elk reken doel:

| COMPUTE-doel | Implementatie configuratie-voor beeld |
| ----- | ----- |
| Lokaal | `deployment_config = LocalWebservice.deploy_configuration(port=8890)` |
| Azure Container Instance | `deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |
| Azure Kubernetes Service | `deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |

In de volgende secties wordt beschreven hoe u de implementatie configuratie maakt en hoe u deze vervolgens kunt gebruiken om de webservice te implementeren.

### <a name="optional-profile-your-model"></a>Optioneel: Uw model profiel

Voordat u het model als een service implementeert, kunt u het gebruiken om de optimale vereisten voor de CPU en het geheugen te bepalen met behulp van de SDK of CLI.  Model profilerings resultaten worden verzonden als een `Run` object. De volledige details van [het schema van het model profiel vindt u in de API-documentatie](https://docs.microsoft.com/python/api/azureml-core/azureml.core.profile.modelprofile?view=azure-ml-py)

Meer informatie over [hoe u uw model kunt profileren met behulp van de SDK](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#profile-workspace--profile-name--models--inference-config--input-data-).

Als u uw model wilt profileren met behulp van de CLI, gebruikt u [AZ ml model profile](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-profile).

## <a name="deploy-to-target"></a>Implementeren naar doel

### <a id="local"></a>Lokale implementatie

Als u lokaal wilt implementeren, moet docker op uw lokale computer zijn **geïnstalleerd** .

+ **De SDK gebruiken**

  ```python
  deployment_config = LocalWebservice.deploy_configuration(port=8890)
  service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  ```

+ **De CLI gebruiken**

    Als u wilt implementeren met behulp van de CLI, gebruikt u de volgende opdracht. Vervang `mymodel:1` door de naam en versie van het geregistreerde model:

  ```azurecli-interactive
  az ml model deploy -m mymodel:1 -ic inferenceconfig.json -dc deploymentconfig.json
  ```

    De vermeldingen in de `deploymentconfig.json` document structuur met de para meters voor [LocalWebservice. deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservicedeploymentconfiguration?view=azure-ml-py). De volgende tabel beschrijft de toewijzing tussen de entiteiten in het JSON-document en de para meters voor de-methode:

    | JSON-entiteit | Methode parameter | Description |
    | ----- | ----- | ----- |
    | `computeType` | N.v.t. | Het rekendoel. Voor Local moet de waarde zijn `local`. |
    | `port` | `port` | De lokale poort waarop het HTTP-eind punt van de service wordt weer gegeven. |

    De volgende JSON is een voorbeeld implementatie configuratie voor gebruik met de CLI:

    ```json
    {
        "computeType": "local",
        "port": 32267
    }
    ```

### <a id="aci"></a>Azure Container Instances (DEVTEST)

Gebruik Azure Container Instances voor het implementeren van uw modellen als een webservice die als één of meer van de volgende voorwaarden is waar:
- U moet sneller te implementeren en valideren van uw model.
- U test een model dat is in ontwikkeling. 

Zie het artikel [quota's en regionale Beschik baarheid voor Azure container instances](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) voor een overzicht van de beschik baarheid van quota en REGIO'S voor ACI.

+ **De SDK gebruiken**

  ```python
  deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
  service = Model.deploy(ws, "aciservice", [model], inference_config, deployment_config)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  ```

+ **De CLI gebruiken**

    Als u wilt implementeren met behulp van de CLI, gebruikt u de volgende opdracht. Vervang `mymodel:1` door de naam en versie van het geregistreerde model. Vervang `myservice` door de naam om deze service te verlenen:

    ```azurecli-interactive
    az ml model deploy -m mymodel:1 -n myservice -ic inferenceconfig.json -dc deploymentconfig.json
    ```

    De vermeldingen in de `deploymentconfig.json` document structuur met de para meters voor [AciWebservice. deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aci.aciservicedeploymentconfiguration?view=azure-ml-py). De volgende tabel beschrijft de toewijzing tussen de entiteiten in het JSON-document en de para meters voor de-methode:

    | JSON-entiteit | Methode parameter | Description |
    | ----- | ----- | ----- |
    | `computeType` | N.v.t. | Het rekendoel. Voor ACI moet de waarde zijn `ACI`. |
    | `containerResourceRequirements` | N.v.t. | Bevat configuratie-elementen voor de CPU en het geheugen dat voor de container is toegewezen. |
    | &emsp;&emsp;`cpu` | `cpu_cores` | Het aantal CPU-kernen dat moet worden toegewezen voor deze webservice. Standaard`0.1` |
    | &emsp;&emsp;`memoryInGB` | `memory_gb` | De hoeveelheid geheugen (in GB) die voor deze webservice moet worden toegewezen. Prijs`0.5` |
    | `location` | `location` | De Azure-regio voor het implementeren van deze webservice voor. Als niet wordt opgegeven, wordt de werkruimte locatie gebruikt. Meer informatie over beschik bare regio's vindt u hier: [ACI-regio's](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=container-instances) |
    | `authEnabled` | `auth_enabled` | Hiermee wordt aangegeven of verificatie voor deze webservice moet worden ingeschakeld. De standaard waarde is False |
    | `sslEnabled` | `ssl_enabled` | Hiermee wordt aangegeven of SSL moet worden ingeschakeld voor deze webservice. De standaard waarde is False. |
    | `appInsightsEnabled` | `enable_app_insights` | Hiermee wordt aangegeven of AppInsights moet worden ingeschakeld voor deze webservice. De standaard waarde is False |
    | `sslCertificate` | `ssl_cert_pem_file` | Het certificaat bestand dat nodig is als SSL is ingeschakeld |
    | `sslKey` | `ssl_key_pem_file` | Het sleutel bestand dat nodig is als SSL is ingeschakeld |
    | `cname` | `ssl_cname` | De CNAME voor als SSL is ingeschakeld |
    | `dnsNameLabel` | `dns_name_label` | Het DNS-naam label voor het Score-eind punt. Als er geen uniek DNS-naam label wordt opgegeven, wordt het Score-eind punt gegenereerd. |

    De volgende JSON is een voorbeeld implementatie configuratie voor gebruik met de CLI:

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

+ **VS code gebruiken**

  Als u [uw modellen wilt implementeren met behulp van VS code](how-to-vscode-tools.md#deploy-and-manage-models) , hoeft u geen ACI-container te maken om vooraf te testen, omdat ACI-containers op de vlucht worden gemaakt.

Zie voor meer informatie de documentatie bij de [AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) en [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py) klassen.

### <a id="aks"></a>Azure Kubernetes-service (DEVTEST & productie)

U kunt een bestaand AKS-cluster gebruiken of een nieuwe maken met behulp van de SDK van Azure Machine Learning, CLI of Azure portal.

<a id="deploy-aks"></a>

Als er al een AKS-cluster is gekoppeld, kunt u het implementeren. Als u nog geen AKS-cluster hebt gemaakt of gekoppeld, volgt u het proces voor het <a href="#create-attach-aks">maken van een nieuw AKS-cluster</a>.

+ **De SDK gebruiken**

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

+ **De CLI gebruiken**

    Als u wilt implementeren met behulp van de CLI, gebruikt u de volgende opdracht. Vervang `myaks` door de naam van het AKS Compute-doel. Vervang `mymodel:1` door de naam en versie van het geregistreerde model. Vervang `myservice` door de naam om deze service te verlenen:

  ```azurecli-interactive
  az ml model deploy -ct myaks -m mymodel:1 -n myservice -ic inferenceconfig.json -dc deploymentconfig.json
  ```

    De vermeldingen in de `deploymentconfig.json` document structuur met de para meters voor [AksWebservice. deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py). De volgende tabel beschrijft de toewijzing tussen de entiteiten in het JSON-document en de para meters voor de-methode:

    | JSON-entiteit | Methode parameter | Description |
    | ----- | ----- | ----- |
    | `computeType` | N.v.t. | Het rekendoel. Voor AKS moet de waarde zijn `aks`. |
    | `autoScaler` | N.v.t. | Bevat configuratie-elementen voor automatisch schalen. Zie de tabel met automatische schalen. |
    | &emsp;&emsp;`autoscaleEnabled` | `autoscale_enabled` | Hiermee wordt aangegeven of automatisch schalen moet worden ingeschakeld voor de webservice. If `numReplicas`  =  ,anders`False`. `0` `True` |
    | &emsp;&emsp;`minReplicas` | `autoscale_min_replicas` | Het minimum aantal containers dat moet worden gebruikt bij het automatisch schalen van deze webservice. Standaard, `1`. |
    | &emsp;&emsp;`maxReplicas` | `autoscale_max_replicas` | Het maximum aantal containers dat moet worden gebruikt bij het automatisch schalen van deze webservice. Standaard, `10`. |
    | &emsp;&emsp;`refreshPeriodInSeconds` | `autoscale_refresh_seconds` | Hoe vaak de automatische schaalr probeert deze webservice te schalen. Standaard, `1`. |
    | &emsp;&emsp;`targetUtilization` | `autoscale_target_utilization` | Het doel gebruik (in procenten 100) dat de automatische schaalr moet proberen te onderhouden voor deze webservice. Standaard, `70`. |
    | `dataCollection` | N.v.t. | Bevat configuratie-elementen voor het verzamelen van gegevens. |
    | &emsp;&emsp;`storageEnabled` | `collect_model_data` | Hiermee wordt aangegeven of het verzamelen van model gegevens moet worden ingeschakeld voor de webservice. Standaard, `False`. |
    | `authEnabled` | `auth_enabled` | Hiermee wordt aangegeven of verificatie voor de webservice moet worden ingeschakeld. Standaard, `True`. |
    | `containerResourceRequirements` | N.v.t. | Bevat configuratie-elementen voor de CPU en het geheugen dat voor de container is toegewezen. |
    | &emsp;&emsp;`cpu` | `cpu_cores` | Het aantal CPU-kernen dat moet worden toegewezen voor deze webservice. Standaard`0.1` |
    | &emsp;&emsp;`memoryInGB` | `memory_gb` | De hoeveelheid geheugen (in GB) die voor deze webservice moet worden toegewezen. Prijs`0.5` |
    | `appInsightsEnabled` | `enable_app_insights` | Hiermee wordt aangegeven of Application Insights logboek registratie moet worden ingeschakeld voor de webservice. Standaard, `False`. |
    | `scoringTimeoutMs` | `scoring_timeout_ms` | Een time-out voor het afdwingen van het bepalen van aanroepen naar de webservice. Standaard, `60000`. |
    | `maxConcurrentRequestsPerContainer` | `replica_max_concurrent_requests` | Het maximum aantal gelijktijdige aanvragen per knoop punt voor deze webservice. Standaard, `1`. |
    | `maxQueueWaitMs` | `max_request_wait_time` | De maximale tijd dat een aanvraag in de wachtrij van thee (in milliseconden) blijft voordat een 503-fout wordt geretourneerd. Standaard, `500`. |
    | `numReplicas` | `num_replicas` | Het aantal containers dat moet worden toegewezen voor deze webservice. Er is geen standaardwaarde. Als deze para meter niet is ingesteld, wordt automatisch schalen standaard ingeschakeld. |
    | `keys` | N.v.t. | Bevat configuratie-elementen voor sleutels. |
    | &emsp;&emsp;`primaryKey` | `primary_key` | Een primaire verificatie sleutel die moet worden gebruikt voor deze webservice |
    | &emsp;&emsp;`secondaryKey` | `secondary_key` | Een secundaire verificatie sleutel die moet worden gebruikt voor deze webservice |
    | `gpuCores` | `gpu_cores` | Het aantal GPU-kernen dat moet worden toegewezen voor deze webservice. De standaard waarde is 1. |
    | `livenessProbeRequirements` | N.v.t. | Bevat configuratie-elementen voor vereisten voor de test op de liveiteit. |
    | &emsp;&emsp;`periodSeconds` | `period_seconds` | Hoe vaak (in seconden) de duur van de test bewerking moet worden uitgevoerd. De standaard waarde is 10 seconden. De minimum waarde is 1. |
    | &emsp;&emsp;`initialDelaySeconds` | `initial_delay_seconds` | Aantal seconden nadat de container is gestart voordat de tests van de live-test worden geïnitieerd. De standaard waarde is 310 |
    | &emsp;&emsp;`timeoutSeconds` | `timeout_seconds` | Aantal seconden waarna een time-out optreedt voor de duur van de online test. De standaard waarde is 2 seconden. De minimum waarde is 1 |
    | &emsp;&emsp;`successThreshold` | `success_threshold` | De minimale opeenvolgende successen voor de test op de eerste keer worden als geslaagd beschouwd als ze zijn mislukt. De standaard waarde is 1. De minimum waarde is 1. |
    | &emsp;&emsp;`failureThreshold` | `failure_threshold` | Wanneer een pod wordt gestart en de test op de productie tijd mislukt, probeert Kubernetes failureThreshold tijden te proberen voordat deze wordt weer geven. De standaard waarde is 3. De minimum waarde is 1. |
    | `namespace` | `namespace` | De Kubernetes-naam ruimte waarin de webservice is geïmplementeerd. Maxi maal 63 kleine letters (' a-z ', ' 0 '-' 9 ') en afbreek streepjes ('-') tekens. De eerste en laatste tekens kunnen geen afbreek streepjes zijn. |

    De volgende JSON is een voorbeeld implementatie configuratie voor gebruik met de CLI:

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

+ **VS code gebruiken**

  U kunt ook [implementeren naar AKS via de VS code-extensie](how-to-vscode-tools.md#deploy-and-manage-models), maar u moet AKS-clusters vooraf configureren.

Meer informatie over AKS-implementatie en automatisch schalen vindt u in de naslag informatie over [AksWebservice. deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice) .

#### Een nieuw AKS-cluster maken<a id="create-attach-aks"></a>
**Geschatte tijd**: Ongeveer 20 minuten.

Het maken of koppelen van een AKS-cluster is een eenmalig proces voor uw werk ruimte. U kunt dit cluster voor meerdere implementaties opnieuw gebruiken. Als u het cluster of de resource groep verwijdert die het bevat, moet u de volgende keer dat u moet implementeren een nieuw cluster maken. Er kunnen meerdere AKS-clusters aan uw werk ruimte zijn gekoppeld.

Als u een AKS-cluster wilt maken voor ontwikkeling, validatie en testen, stelt `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` u in dat u gebruikt. [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py) Een cluster dat is gemaakt met deze instelling heeft slechts één knoop punt.

> [!IMPORTANT]
> Instelling `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` maakt een AKS-cluster dat niet geschikt is voor het verwerken van productie verkeer. De tijd voor het afleiden van interferenties kan langer zijn dan een cluster dat voor productie is gemaakt. Fout tolerantie is ook niet gegarandeerd voor dev/test-clusters.
>
> Het is raadzaam dat clusters die zijn gemaakt voor dev/test ten minste twee virtuele Cpu's gebruiken.

In het volgende voor beeld ziet u hoe u een nieuw Azure Kubernetes service-cluster maakt:

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

Raadpleeg de volgende artikelen voor meer informatie over het maken van een AKS-cluster buiten de Azure Machine Learning SDK:
* [Een AKS-cluster maken](https://docs.microsoft.com/cli/azure/aks?toc=%2Fazure%2Faks%2FTOC.json&bc=%2Fazure%2Fbread%2Ftoc.json&view=azure-cli-latest#az-aks-create)
* [Een AKS-cluster maken (Portal)](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal?view=azure-cli-latest)

Zie de naslag gids voor `cluster_purpose` [AksCompute. ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py) voor meer informatie over de para meter.

> [!IMPORTANT]
> [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py)Als u bijvoorbeeld aangepaste waarden voor agent_count en vm_size kiest, moet u ervoor zorgen dat agent_count vermenigvuldigd met vm_size groter is dan of gelijk is aan 12 virtuele cpu's. Als u bijvoorbeeld een vm_size van ' Standard_D3_v2 ' gebruikt, die 4 virtuele Cpu's heeft, moet u een agent_count van 3 of hoger kiezen.
>
> De Azure Machine Learning SDK biedt geen ondersteuning voor het schalen van een AKS-cluster. Als u de knoop punten in het cluster wilt schalen, gebruikt u de gebruikers interface voor uw AKS-cluster in de Azure Portal. U kunt alleen het aantal knoop punten wijzigen, niet de VM-grootte van het cluster.

#### <a name="attach-an-existing-aks-cluster"></a>Een bestaand AKS-cluster koppelen
**Geschatte tijd:** Ongeveer 5 minuten.

Als u al een AKS-cluster in uw Azure-abonnement hebt en dit versie 1.12. # # is, kunt u het gebruiken om uw installatie kopie te implementeren.

> [!WARNING]
> Wanneer u een AKS-cluster koppelt aan een werk ruimte, kunt u definiëren hoe u het cluster gaat gebruiken `cluster_purpose` door de para meter in te stellen.
>
> Als u de `cluster_purpose` para meter of set `cluster_purpose = AksCompute.ClusterPurpose.FAST_PROD`niet instelt, moet het cluster ten minste 12 virtuele cpu's hebben.
>
> Als u instelt `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST`, hoeft het cluster niet 12 virtuele cpu's te hebben. Een cluster dat is geconfigureerd voor dev/test, is echter niet geschikt voor verkeer op productie niveau en kan leiden tot meer tijd.

De volgende code laat zien hoe u een bestaand AKS 1.12. # # koppelt aan uw werk ruimte:

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

Zie de referentie `attack_configuration()` [AksCompute. attach_configuration ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-) voor meer informatie over.

Zie de naslag gids voor `cluster_purpose` [AksCompute. ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py) voor meer informatie over de para meter.

## <a name="consume-web-services"></a>Webservices gebruiken

Elke geïmplementeerde webservice biedt een REST API, zodat u client toepassingen kunt maken in verschillende programmeer talen. Als u verificatie voor uw service hebt ingeschakeld, moet u een service sleutel opgeven als een token in de aanvraag header.

### <a name="request-response-consumption"></a>Aanvraag-antwoord verbruik

Hier volgt een voor beeld van het aanroepen van uw service in Python:
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

Zie [client toepassingen maken om webservices te gebruiken](how-to-consume-web-service.md)voor meer informatie.


### <a id="azuremlcompute"></a>Batch-deinterferentie
Azure Machine Learning Compute-doelen worden gemaakt en beheerd door de Azure Machine Learning-service. Ze kunnen worden gebruikt voor batch voorspelling van Azure Machine Learning pijp lijnen.

Lees voor een overzicht van batch deinterferentie met Azure Machine Learning Compute het artikel [batch voorspellingen uitvoeren](how-to-run-batch-predictions.md) .

### <a id="iotedge"></a>IoT Edge afleiding
Ondersteuning voor het implementeren naar de rand is in preview. Zie voor meer informatie het artikel [Deploy Azure machine learning als IOT Edge module](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning) .


## <a id="update"></a>Webservices bijwerken

Wanneer u een nieuw model maakt, moet u elke service die u wilt gebruiken voor het nieuwe model hand matig bijwerken. Voor het bijwerken van de webservice, gebruikt u de `update` methode. De volgende code laat zien hoe u de webservice bijwerkt voor het gebruik van een nieuw model:

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

