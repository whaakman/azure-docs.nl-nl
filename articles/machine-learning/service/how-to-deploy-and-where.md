---
title: Modellen als webservices implementeren
titleSuffix: Azure Machine Learning service
description: 'Meer informatie over hoe en waar uw Azure Machine Learning-service-modellen met inbegrip van implementeren: Azure Container Instances, Azure Kubernetes Service, Azure IoT Edge en Field-programmable gate arrays.'
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 3341dbc486ebd184979381fa6bef05ec9404aa98
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/15/2019
ms.locfileid: "54304094"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Implementeer modellen met de Azure Machine Learning-service

De Azure Machine Learning-service biedt verschillende manieren waarop die u het getrainde model met behulp van de SDK kunt implementeren. In dit document leert u hoe u uw model als een webservice in de Azure-cloud, of op IoT edge-apparaten implementeren.

> [!IMPORTANT]
> Cross-origin resource sharing (CORS) wordt momenteel niet ondersteund bij het implementeren van een model als een webservice.

U kunt modellen implementeren op de volgende compute-doelen:

| COMPUTE-doel | Implementatietype | Description |
| ----- | ----- | ----- |
| [Azure Container Instances (ACI)](#aci) | Webservice | Snelle implementatie. Goed voor ontwikkeling en testen. |
| [Azure Kubernetes Service (AKS)](#aks) | Webservice | Geschikt voor grootschalige productie-implementaties. Biedt automatisch schalen en snelle responstijden. |
| [Azure IoT Edge](#iotedge) | IoT-module | Implementeer modellen op IoT-apparaten. Inferentietaken gebeurt op het apparaat. |
| [Veld-programmable gate array (FPGA)](#fpga) | Webservice | Zeer lage latentie voor realtime inferentietaken. |

Het proces voor het implementeren van een model is vergelijkbaar voor alle compute-doelen:

1. Training en registreren van een model.
1. Configureren en registreren van een installatiekopie die gebruikmaakt van het model.
1. De installatiekopie implementeert op een compute-doel.
1. De implementatie testen

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Kwk3]


Zie voor meer informatie over de concepten die betrokken zijn bij de implementatiewerkstroom [beheren, implementeren en controleren van modellen met Azure Machine Learning-Service](concept-model-management-and-deployment.md).

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](http://aka.ms/AMLFree).

- Een werkruimte van Azure Machine Learning-service en de Azure Machine Learning-SDK voor Python geïnstalleerd. Informatie over het verkrijgen van deze vereisten met behulp van de [aan de slag met Azure Machine Learning-snelstartgids](quickstart-get-started.md).

- Een getraind model. Als u een getraind model hebt, gebruikt u de stappen in de [trainen van modellen](tutorial-train-models-with-aml.md) zelfstudie om te trainen en te registreren met de Azure Machine Learning-service.

    > [!NOTE]
    > Terwijl de service Azure Machine Learning met een algemeen model dat kan worden geladen in Python 3 werken kunt, wordt in de voorbeelden in dit document worden met behulp van een model dat is opgeslagen in pickle-indeling.
    > 
    > Zie voor meer informatie over het gebruik van ONNX-modellen, de [ONNX en Azure Machine Learning](how-to-build-deploy-onnx.md) document.

## <a id="registermodel"></a> Registreren van een getraind model

Het register model is een manier voor het opslaan en delen van uw getrainde modellen in de Azure-cloud. Modellen zijn geregistreerd in de werkruimte van uw Azure Machine Learning-service. Het model kan worden getraind met behulp van Azure Machine Learning, of een andere service. Gebruik de volgende code voor het registreren van een model uit bestand:

```python
from azureml.core.model import Model

model = Model.register(model_path = "model.pkl",
                       model_name = "Mymodel",
                       tags = {"key": "0.1"},
                       description = "test",
                       workspace = ws)
```

**Geschatte tijd**: Ongeveer 10 seconden.

Zie voor meer informatie de documentatie bij de [Modelklasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

## <a id="configureimage"></a> Maken en registreren van een installatiekopie

Geïmplementeerde modellen zijn verpakt als een afbeelding. De afbeelding bevat de afhankelijkheden die nodig zijn om uit te voeren van het model.

Voor **Azure Container Instances**, **Azure Kubernetes Service**, en **Azure IoT Edge** implementaties, de [azureml.core.image.ContainerImage](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py) klasse wordt gebruikt om de configuratie van een installatiekopie te maken. De configuratie van de installatiekopie wordt vervolgens gebruikt om te maken van een nieuwe Docker-installatiekopie. 

De volgende code ziet u hoe u de configuratie van een nieuwe installatiekopie gemaakt:

```python
from azureml.core.image import ContainerImage

# Image configuration
image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                 runtime = "python",
                                                 conda_file = "myenv.yml",
                                                 description = "Image with ridge regression model",
                                                 tags = {"data": "diabetes", "type": "regression"}
                                                 )
```

**Geschatte tijd**: Ongeveer 10 seconden.

De belangrijke parameters in dit voorbeeld wordt beschreven in de volgende tabel:

| Parameter | Description |
| ----- | ----- |
| `execution_script` | Hiermee geeft u een Python-script dat wordt gebruikt om aanvragen verzonden naar de service te ontvangen. In dit voorbeeld wordt het script is opgenomen in de `score.py` bestand. Zie voor meer informatie de [tot uitvoering van script](#script) sectie. |
| `runtime` | Geeft aan dat de installatiekopie van het maakt gebruik van Python. De andere mogelijkheid is `spark-py`, die Python gebruikt met Apache Spark. |
| `conda_file` | Gebruikt om een bestand conda-omgeving. Dit bestand definieert het conda-omgeving voor het gedistribueerde model. Zie voor meer informatie over het maken van dit bestand [maakt u een omgevingsbestand (myenv.yml)](tutorial-deploy-models-with-aml.md#create-environment-file). |

Zie voor meer informatie de documentatie bij [ContainerImage klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py)

### <a id="script"></a> Uitvoering van script

De uitvoering van script ontvangt gegevens verzonden naar een geïmplementeerde installatiekopie, en wordt doorgegeven aan het model. Vervolgens wordt het antwoord geretourneerd door het model en retourneert die naar de client. Het script is specifiek voor uw model. Deze moet de gegevens die het model wordt verwacht en retourneert begrijpen. Het script bevat doorgaans twee functies die worden geladen en het model uitvoeren:

* `init()`: Deze functie worden doorgaans het model in een globale objecttoegang geladen. Deze functie wordt slechts één keer uitgevoerd wanneer de Docker-container wordt gestart. 

* `run(input_data)`: Deze functie maakt gebruik van het model om te voorspellen van een waarde op basis van de ingevoerde gegevens. Invoer en uitvoer aan het run maken doorgaans gebruik van JSON voor serialisatie en het deserialiseren. Ook kunt u werken met onbewerkte binaire gegevens. U kunt de gegevens te transformeren voordat ze worden verzonden naar het model of voordat u terugkeert naar de client. 

#### <a name="working-with-json-data"></a>Werken met JSON-gegevens

Hier volgt een voorbeeldscript dat wordt geaccepteerd en JSON-gegevens geretourneerd. De `run` functie transformeert de gegevens uit JSON naar een indeling die wordt verwacht dat het model en vervolgens het antwoord naar JSON worden getransformeerd alvorens deze:

```python
# import things required by this script
import json
import numpy as np
import os
import pickle
from sklearn.externals import joblib
from sklearn.linear_model import LogisticRegression

from azureml.core.model import Model

# load the model
def init():
    global model
    # retrieve the path to the model file using the model name
    model_path = Model.get_model_path('sklearn_mnist')
    model = joblib.load(model_path)

# Passes data to the model and returns the prediction
def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    y_hat = model.predict(data)
    return json.dumps(y_hat.tolist())
```

#### <a name="working-with-binary-data"></a>Werken met binaire gegevens

Als uw model accepteert __binaire gegevens__, gebruikt u `AMLRequest`, `AMLResponse`, en `rawhttp`. Hier volgt een voorbeeld van een script dat binaire gegevens accepteert en retourneert de omgekeerde bytes voor POST-aanvragen. Voor GET-aanvragen retourneert deze de volledige URL in de hoofdtekst van antwoord:

```python
from azureml.contrib.services.aml_request  import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse

def init():
    print("This is init()")

# Accept and return binary data
@rawhttp
def run(request):
    print("This is run()")
    print("Request: [{0}]".format(request))
    # handle GET requests
    if request.method == 'GET':
        respBody = str.encode(request.full_path)
        return AMLResponse(respBody, 200)
    # handle POST requests
    elif request.method == 'POST':
        reqBody = request.get_data(False)
        respBody = bytearray(reqBody)
        respBody.reverse()
        respBody = bytes(respBody)
        return AMLResponse(respBody, 200)
    else:
        return AMLResponse("bad request", 500)
```

> [!IMPORTANT]
> De `azureml.contrib` naamruimte vaak verandert, zoals we werken om de service te verbeteren. Als zodanig moet in deze naamruimte worden beschouwd als een Preview-versie, en niet volledig ondersteund door Microsoft.
>
> Als u testen op uw lokale ontwikkelomgeving wilt, kunt u de onderdelen in installeren de `contrib` naamruimte met behulp van de volgende opdracht uit: 
> ```shell
> pip install azureml-contrib-services
> ```

### <a id="createimage"></a> De installatiekopie van het registreren

Als u de configuratie van de installatiekopie hebt gemaakt, kunt u deze kunt gebruiken voor het registreren van een installatiekopie. Deze installatiekopie wordt opgeslagen in de container registry voor uw werkruimte. Nadat u hebt gemaakt, kunt u dezelfde installatiekopie implementeren naar meerdere services.

```python
# Register the image from the image configuration
image = ContainerImage.create(name = "myimage", 
                              models = [model], #this is the model object
                              image_config = image_config,
                              workspace = ws
                              )
```

**Geschatte tijd**: Ongeveer 3 minuten.

Afbeeldingen zijn samengesteld automatisch wanneer u meerdere installatiekopieën met dezelfde naam registreren. Bijvoorbeeld, de eerste afbeelding geregistreerd als `myimage` is een ID toegewezen `myimage:1`. De volgende keer dat u een afbeelding als registreren `myimage`, is de ID van de nieuwe installatiekopie `myimage:2`.

Zie voor meer informatie de documentatie bij [ContainerImage klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py).

## <a id="deploy"></a> De installatiekopie implementeren

Wanneer u op de implementatie, is het proces enigszins verschillen afhankelijk van de compute-doel die u implementeert op. Gebruik de informatie in de volgende secties voor informatie over het implementeren op:

* [Azure Container Instances](#aci)
* [Azure Kubernetes-Services](#aks)
* [Project Brainwave (veld-programmable gate arrays)](#fpga)
* [Azure IoT Edge-apparaten](#iotedge)

> [!NOTE]
> Wanneer **implementeren als een webservice**, er zijn drie methoden voor het implementeren u kunt gebruiken:
>
> | Methode | Opmerkingen |
> | ----- | ----- |
> | [deploy_from_image](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-image-workspace--name--image--deployment-config-none--deployment-target-none-) | U moet het model registreren en een installatiekopie maken voordat u deze methode. |
> | [Implementeren](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-workspace--name--model-paths--image-config--deployment-config-none--deployment-target-none-) | Wanneer u deze methode gebruikt, hoeft u niet te registreren van het model of maken van de installatiekopie. Maar u geen invloed hebben op de naam van het model of de afbeelding of gekoppelde tags en beschrijvingen. |
> | [deploy_from_model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-) | Wanneer u deze methode gebruikt, hoeft u niet om een installatiekopie te maken. Maar u hebt geen controle over de naam van de installatiekopie die wordt gemaakt. |
>
> De voorbeelden in dit document gebruiken `deploy_from_image`.

### <a id="aci"></a> Implementeren in Azure Container Instances

Gebruik Azure Container Instances voor het implementeren van uw modellen als een webservice die als één of meer van de volgende voorwaarden is waar:

- U moet sneller te implementeren en valideren van uw model. ACI-implementatie is voltooid in minder dan vijf minuten.
- U test een model dat is in ontwikkeling. Quota en regio de beschikbaarheid voor ACI, Zie de [quota en beschikbaarheid in regio's voor Azure Container Instances](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) document.

Als u wilt implementeren in Azure Container Instances, gebruikt u de volgende stappen uit:

1. De implementatieconfiguratie definiëren. Het volgende voorbeeld definieert een configuratie die gebruikmaakt van één CPU-kern en 1 GB geheugen:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=configAci)]

2. Implementeren van de installatiekopie hebt gemaakt in de [maken van de installatiekopie](#createimage) sectie van dit document, gebruik de volgende code:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-deploy-to-aci/how-to-deploy-to-aci.py?name=option3Deploy)]

    **Geschatte tijd**: Ongeveer 3 minuten.

    > [!TIP]
    > Als er fouten tijdens de implementatie zijn, gebruikt u `service.get_logs()` om de AKS-service-logboeken weer te geven. De vastgelegde gegevens mogelijk de oorzaak van de fout.

Zie voor meer informatie de documentatie bij de [AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) en [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py) klassen.

### <a id="aks"></a> Implementeren in Azure Kubernetes Service

Gebruik voor het implementeren van uw modellen vervolgens als een webservice schalen-productie, Azure Kubernetes Service (AKS). U kunt een bestaand AKS-cluster gebruiken of een nieuwe maken met behulp van de SDK van Azure Machine Learning, CLI of Azure portal.

Het maken van een AKS-cluster is een proces tijd voor uw werkruimte. U kunt dit cluster voor meerdere implementaties opnieuw gebruiken. Als u het cluster verwijdert, moet klikt u vervolgens u een nieuw cluster de volgende keer dat u wilt implementeren.

Azure Kubernetes Service biedt de volgende mogelijkheden:

* Automatisch schalen
* Logboekregistratie
* Gegevensverzameling van model
* Snelle responstijden voor uw web-services

#### <a name="create-a-new-cluster"></a>Maak een nieuw cluster

Gebruik de volgende code voor het maken van een nieuw Azure Kubernetes Service-cluster:

> [!IMPORTANT]
> Het maken van het AKS-cluster is een proces tijd voor uw werkruimte. Nadat u hebt gemaakt, kunt u dit cluster voor meerdere implementaties opnieuw gebruiken. Als u het cluster of de resourcegroep waarin het verwijdert, moet klikt u vervolgens u een nieuw cluster de volgende keer dat u wilt implementeren.
> Voor [ `provisioning_configuration()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py), als u aangepaste waarden voor agent_count en vm_size, kiest, moet u om ervoor te zorgen agent_count vermenigvuldigd met vm_size is groter dan of gelijk zijn aan 12 virtuele CPU's. Bijvoorbeeld, als u een vm_size van 'Standard_D3_v2', met 4 virtuele CPU's, moet vervolgens u kiezen een agent_count van 3 of hoger.

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (you can also provide parameters to customize this)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'aml-aks-1' 
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws, 
                                    name = aks_name, 
                                    provisioning_configuration = prov_config)

# Wait for the create process to complete
aks_target.wait_for_completion(show_output = True)
print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)
```

**Geschatte tijd**: Ongeveer 20 minuten.

#### <a name="use-an-existing-cluster"></a>Gebruik een bestaand cluster

Als u al AKS-cluster in uw Azure-abonnement en het is versie 1.11. *, kunt u het implementeren van uw installatiekopie. De volgende code ziet u hoe u een bestaand cluster koppelt aan uw werkruimte:

```python
from azureml.core.compute import AksCompute, ComputeTarget
# Set the resource group that contains the AKS cluster and the cluster name
resource_group = 'myresourcegroup'
cluster_name = 'mycluster'

# Attatch the cluster to your workgroup
attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
aks_target = ComputeTarget.attach(ws, 'mycompute', attach_config)

# Wait for the operation to complete
aks_target.wait_for_completion(True)
```

**Geschatte tijd**: Ongeveer 3 minuten.

#### <a name="deploy-the-image"></a>De installatiekopie implementeren

Implementeren van de installatiekopie hebt gemaakt in de [maken van de installatiekopie](#createimage) sectie van dit document om het cluster Azure Kubernetes-Server, gebruik de volgende code:

```python
from azureml.core.webservice import Webservice, AksWebservice

# Set configuration and service name
aks_config = AksWebservice.deploy_configuration()
aks_service_name ='aks-service-1'
# Deploy from image
service = Webservice.deploy_from_image(workspace = ws, 
                                            name = aks_service_name,
                                            image = image,
                                            deployment_config = aks_config,
                                            deployment_target = aks_target)
# Wait for the deployment to complete
service.wait_for_deployment(show_output = True)
print(service.state)
```

**Geschatte tijd**: Ongeveer 3 minuten.

> [!TIP]
> Als er fouten tijdens de implementatie zijn, gebruikt u `service.get_logs()` om de AKS-service-logboeken weer te geven. De vastgelegde gegevens mogelijk de oorzaak van de fout.

Zie voor meer informatie de documentatie bij de [AksWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py) en [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice.webservice?view=azure-ml-py) klassen.

### <a id="fpga"></a> Implementeren naar het veld-programmable gate arrays (FPGA)

Project Brainwave maakt het mogelijk is om te voorzien in zeer lage latentie voor realtime inferentietaken aanvragen bereiken. Project Brainwave versnelt DNN deep neural networks () geïmplementeerd op veld-programmable gate arrays in de Azure-cloud. Gebruikte dnn's zijn beschikbaar als featurizers voor overdrachtsleren of aanpasbare met een gewicht bijvoorbeeld getraind met uw eigen gegevens.

Zie voor een overzicht van het implementeren van een model met behulp van Project Brainwave, de [implementeren in een FPGA](how-to-deploy-fpga-web-service.md) document.

### <a id="iotedge"></a> Implementeren naar Azure IoT Edge

Een Azure IoT Edge-apparaat is een op Linux of Windows-apparaat waarop de Azure IoT Edge-runtime wordt uitgevoerd. Machine learning-modellen kunnen worden geïmplementeerd op deze apparaten als IoT Edge-modules. Een model implementeert naar een IoT Edge-apparaat staat toe dat het apparaat rechtstreeks gebruik van het model, in plaats van dat voor het verzenden van gegevens naar de cloud voor verwerking. U krijgt snellere reactietijden en minder gegevens worden overgebracht.

Azure IoT Edge-modules zijn geïmplementeerd op het apparaat van een containerregister. Wanneer u een installatiekopie uit het model maakt, wordt deze opgeslagen in de containerregister voor uw werkruimte.

#### <a name="set-up-your-environment"></a>Uw omgeving instellen

* Een ontwikkelomgeving. Zie voor meer informatie de [het configureren van een ontwikkelomgeving](how-to-configure-environment.md) document.

* Een [Azure IoT Hub](../../iot-hub/iot-hub-create-through-portal.md) in uw Azure-abonnement. 

* Een getraind model. Zie voor een voorbeeld van hoe u een model te trainen, de [een model van de installatiekopie classificatie met Azure Machine Learning te trainen](tutorial-train-models-with-aml.md) document. Er is een vooraf getrainde model beschikbaar op de [AI-werkset voor Azure IoT Edge-GitHub-opslagplaats](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial).

#### <a name="prepare-the-iot-device"></a>De IoT-apparaat voorbereiden
U moet een IoT-hub maken en registreren van een apparaat of opnieuw gebruiken met [met dit script](https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/createNregister).

``` bash
ssh <yourusername>@<yourdeviceip>
sudo wget https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/createNregister
sudo chmod +x createNregister
sudo ./createNregister <The Azure subscriptionID you want to use> <Resourcegroup to use or create for the IoT hub> <Azure location to use e.g. eastus2> <the Hub ID you want to use or create> <the device ID you want to create>
```

Opslaan van de resulterende verbindingsreeks na "cs": '{deze tekenreeks kopiëren}'.

Initialiseren van uw apparaat door het downloaden van [met dit script](https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/installIoTEdge) in een UbuntuX64 IoT edge-knooppunt of DSVM om uit te voeren van de volgende opdrachten:

```bash
ssh <yourusername>@<yourdeviceip>
sudo wget https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/installIoTEdge
sudo chmod +x installIoTEdge
sudo ./installIoTEdge
```

Het IoT Edge-knooppunt is gereed voor het ontvangen van de verbindingsreeks voor uw IoT-Hub. Zoek de regel ```device_connection_string:``` en plak de verbindingsreeks van bovenstaande tussen de aanhalingstekens.

U kunt ook meer informatie over het registreren van uw apparaat en installeer de IoT-runtime stap voor stap door het [Quick Start: Uw eerste IoT Edge-module implementeren op een apparaat met Linux x64](../../iot-edge/quickstart-linux.md) document.


#### <a name="get-the-container-registry-credentials"></a>De container registerreferenties ophalen
Als u wilt een IoT Edge-module implementeert op uw apparaat, moet Azure IoT de referenties voor de container registry om docker-installatiekopieën in Azure Machine Learning-service worden opgeslagen.

U kunt gemakkelijk de vereiste container registerreferenties op twee manieren ophalen:

+ **In de Azure-Portal**:

  1. Meld u aan bij [Azure Portal](https://portal.azure.com/signin/index).

  1. Ga naar de werkruimte van uw Azure Machine Learning-service en selecteer __overzicht__. Om te gaan naar de container registry-instellingen, selecteer de __register__ koppeling.

     ![Een afbeelding van de container-register-item](./media/how-to-deploy-and-where/findregisteredcontainer.png)

  1. Selecteer één keer in het containerregister **toegangssleutels** en schakel vervolgens de gebruiker met beheerdersrechten.
 
     ![Een installatiekopie van het scherm van de sleutels toegang](./media/how-to-deploy-and-where/findaccesskey.png)

  1. Sla de waarden voor **aanmeldingsserver**, **gebruikersnaam**, en **wachtwoord**. 

+ **Met een Python-script**:

  1. Gebruik het volgende Python-script na de code die u hierboven hebt uitgevoerd om een container te maken:

     ```python
     # Getting your container details
     container_reg = ws.get_details()["containerRegistry"]
     reg_name=container_reg.split("/")[-1]
     container_url = "\"" + image.image_location + "\","
     subscription_id = ws.subscription_id
     from azure.mgmt.containerregistry import ContainerRegistryManagementClient
     from azure.mgmt import containerregistry
     client = ContainerRegistryManagementClient(ws._auth,subscription_id)
     result= client.registries.list_credentials(resource_group_name, reg_name, custom_headers=None, raw=False)
     username = result.username
     password = result.passwords[0].value
     print('ContainerURL{}'.format(image.image_location))
     print('Servername: {}'.format(reg_name))
     print('Username: {}'.format(username))
     print('Password: {}'.format(password))
     ```
  1. Sla de waarden voor de ContainerURL, servername, gebruikersnaam en wachtwoord. 

     Deze referenties zijn nodig voor de IoT Edge Apparaattoegang tot afbeeldingen in uw persoonlijke containerregister.

#### <a name="deploy-the-model-to-the-device"></a>Het model op het apparaat implementeren

U kunt eenvoudig een model implementeren door het uitvoeren van [met dit script](https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/deploymodel) en het geven van de volgende informatie uit de bovenstaande stappen: containerregister naam, gebruikersnaam, wachtwoord, afbeeldings-url voor locatie, naam van de gewenste implementatie, de naam van de IoT Hub, en de apparaat-ID u hebt gemaakt. U kunt dit doen in de virtuele machine door de volgende stappen: 

```bash 
wget https://raw.githubusercontent.com/Azure/ai-toolkit-iot-edge/master/amliotedge/deploymodel
sudo chmod +x deploymodel
sudo ./deploymodel <ContainerRegistryName> <username> <password> <imageLocationURL> <DeploymentID> <IoTHubname> <DeviceID>
```

U kunt ook u kunt de stappen in de [implementeren Azure IoT Edge-modules van de Azure-portal](../../iot-edge/how-to-deploy-modules-portal.md) document naar de installatiekopie implementeert op uw apparaat. Bij het configureren van de __registerinstellingen__ gebruiken voor het apparaat, de __aanmeldingsserver__, __gebruikersnaam__, en __wachtwoord__ voor uw werkruimte container registry.

> [!NOTE]
> Als u niet bekend met Azure IoT bent, ziet u de volgende documenten voor meer informatie over aan de slag met de service:
>
> * [Snelstart: Uw eerste IoT Edge-module implementeert op een Linux-apparaat](../../iot-edge/quickstart-linux.md)
> * [Snelstart: Uw eerste IoT Edge-module implementeert op een Windows-apparaat](../../iot-edge/quickstart.md)


## <a name="testing-web-service-deployments"></a>Testen van de webservice-implementaties

Als u wilt testen van een implementatie van de webservice, kunt u de `run` methode van de Webservice-object. In het volgende voorbeeld wordt een JSON-document is ingesteld op een webservice en het resultaat wordt weergegeven. De verzonden gegevens moet overeenkomen met wat het model wordt verwacht. In dit voorbeeld heeft de indeling van de komt overeen met de invoer verwacht door het model diabetes.

```python
import json

test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')

prediction = service.run(input_data = test_sample)
print(prediction)
```

De webservice is een REST-API, zodat u toepassingen in diverse programmeertalen maken kunt. Zie voor meer informatie, [-client maken toepassingen gebruik van webservices kunnen](how-to-consume-web-service.md).

## <a id="update"></a> Bijwerken van de webservice

Voor het bijwerken van de webservice, gebruikt u de `update` methode. De volgende code ziet u hoe u de webservice voor het gebruik van een nieuwe installatiekopie bijwerken:

```python
from azureml.core.webservice import Webservice

service_name = 'aci-mnist-3'
# Retrieve existing service
service = Webservice(name = service_name, workspace = ws)

# point to a different image
new-image = Image(workspace = ws, id="myimage2:1")

# Update the image used by the service
service.update(image = new-image)
print(service.state)
```

> [!NOTE]
> Wanneer u een installatiekopie van een bijwerkt, wordt de webservice niet automatisch bijgewerkt. Elke service die u wilt gebruiken van de nieuwe installatiekopie moet u handmatig bijwerken.

Zie voor meer informatie de documentatie bij de [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py) klasse.

## <a name="clean-up"></a>Opruimen

Als u wilt verwijderen van een geïmplementeerde webservice, gebruikt u `service.delete()`.

Als u wilt verwijderen van een installatiekopie, gebruikt u `image.delete()`.

Als u wilt een geregistreerde model verwijderen, gebruikt u `model.delete()`.

Zie voor meer informatie de documentatie bij [WebService.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--), [Image.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.image(class)?view=azure-ml-py#delete--), en [Model.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--).

## <a name="next-steps"></a>Volgende stappen

* [Azure Machine Learning-webservices met SSL beveiligde](how-to-secure-web-service.md)
* [Een ML-Model dat is geïmplementeerd als een webservice gebruiken](how-to-consume-web-service.md)
* [Het uitvoeren van voorspellingen van batch](how-to-run-batch-predictions.md)
* [Uw Azure Machine Learning-modellen met Application Insights bewaken](how-to-enable-app-insights.md)
* [Verzamelen van gegevens voor modellen in productie](how-to-enable-data-collection.md)
* [Azure Machine Learning-service SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
* [Azure Machine Learning-service gebruiken met Azure Virtual Networks](how-to-enable-virtual-network.md)
