---
title: Modellen implementeren in Kubernetes in Azure Machine Learning-service | Microsoft Docs
description: Leer hoe u een model implementeren vanuit de service Azure Machine Learning in Azure Kubernetes Service. Het model wordt geïmplementeerd als een webservice. Azure Kubernetes Service is geschikt voor grootschalige productieworkloads.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: raymondl
author: raymondlaghaeian
manager: cgronlun
ms.reviewer: larryfr
ms.date: 09/24/2018
ms.openlocfilehash: 7df23420445e40efe4ce95c8e55993f6069a1613
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52867220"
---
# <a name="how-to-deploy-models-from-azure-machine-learning-service-to-azure-kubernetes-service"></a>Over het implementeren van modellen van Azure Machine Learning-service in Azure Kubernetes Service

Voor grote schaal productiescenario's, kunt u uw model implementeren naar de Azure Kubernetes Service (AKS). Azure Machine Learning kunt gebruiken voor een bestaand AKS-cluster of een nieuw cluster gemaakt tijdens de implementatie. Het model wordt geïmplementeerd naar vragen als een webservice.

Implementeren naar AKS biedt automatisch schalen, logboekregistratie, gegevensverzameling van model en snelle responstijden voor uw webservice.

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u nog geen abonnement hebt, maakt u een [gratis account](https://aka.ms/AMLfree) voordat u begint.

- Een Azure Machine Learning-service-werkruimte en een lokale map met uw scripts en de Azure Machine Learning-SDK voor Python geïnstalleerd. Informatie over het verkrijgen van deze vereisten met behulp van de [het configureren van een ontwikkelomgeving](how-to-configure-environment.md) document.

- Een getrainde machine learning-model. Als u niet hebt, raadpleegt u de [classificeringsmodel voor de installatiekopie van de trein](tutorial-train-models-with-aml.md) zelfstudie.

## <a name="initialize-the-workspace"></a>Initialiseren van de werkruimte

Laden voor het initialiseren van de werkruimte, de `config.json` bestand met de gegevens over uw werkruimte.

```python
from azureml.coreazureml import Workspace

# Load existing workspace from the config file info.
ws = Workspace.from_config()
```

## <a name="register-the-model"></a>Registreer het model

Geef de model-pad, beschrijving en labels voor het registreren van een bestaand model.

```python
from azureml.core.model import Model

model = Model.register(model_path = "model.pkl", # this points to a local file
                        model_name = "best_model", # this is the name the model is registered as
                        tags = {"data": "diabetes", "type": "regression"},
                        description = "Ridge regression model to predict diabetes",
                        workspace = ws)
```

## <a name="create-a-docker-image"></a>Maak een Docker-installatiekopie.

Azure Kubernetes Service maakt gebruik van Docker-installatiekopieën. Gebruik de volgende stappen uit voor het maken van de afbeelding:

1. Voor het configureren van de installatiekopie, moet u een scoring-script en de bestandsnaam van de omgeving maken. Voor een voorbeeld van het maken van het bestand script en omgeving, Zie de volgende secties van de installatiekopie classificatie. bijvoorbeeld:

    * [Een scoring-script (score.py) maken](tutorial-deploy-models-with-aml.md#create-scoring-script)

        > [!IMPORTANT]
        > Het scoring-script ontvangt van clients verzonden gegevens en wordt doorgegeven aan het model voor het scoren. Documenteer de gegevensstructuur die het script en het model verwacht. Met deze documentatie wordt dingen gemakkelijker bij het bouwen van een client de webservice gebruiken.

    * [Maak een omgevingsbestand (myenv.yml)](tutorial-deploy-models-with-aml.md#create-environment-file) 

   Deze bestanden wordt het volgende voorbeeld het configureren van de afbeelding:

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

1. Gebruik de model- en image-configuratie voor het maken van de installatiekopie van het. Met deze bewerking kan ongeveer vijf minuten duren om uit te voeren:

    ```python
    image = ContainerImage.create(name = "myimage1",
                                    # this is the model object
                                    models = [model],
                                    image_config = image_config,
                                    workspace = ws)

    # Wait for the create process to complete
    image.wait_for_creation(show_output = True)
    ```

> [!NOTE]
> Bij het maken van een installatiekopie die gebruikmaakt van GPU-versnelling, moet de GPU-basisinstallatiekopie worden gebruikt op Microsoft Azure-Services alleen.

## <a name="create-the-aks-cluster"></a>De AKS-Cluster maken

Het volgende codefragment laat zien hoe de AKS-cluster maken. Dit proces duurt ongeveer 20 minuten om te voltooien:

> [!IMPORTANT]
> Het maken van het AKS-cluster is een proces tijd voor uw werkruimte. Nadat u hebt gemaakt, kunt u dit cluster voor meerdere implementaties opnieuw gebruiken. Als u het cluster of de resourcegroep waarin het verwijdert, moet klikt u vervolgens u een nieuw cluster de volgende keer dat u wilt implementeren.


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

### <a name="attach-existing-aks-cluster-optional"></a>Koppelen van bestaande AKS-cluster (optioneel)

Als u bestaande AKS-cluster in uw Azure-abonnement hebt, kunt u het implementeren van uw installatiekopie. Het volgende codefragment laat zien hoe u een cluster aan uw werkruimte koppelt. 

> [!IMPORTANT]
> Alleen AKS versie 1.11.3 wordt ondersteund.

```python
# Get the resource id from https://porta..azure.com -> Find your resource group -> click on the Kubernetes service -> Properties
resource_id = '/subscriptions/<your subscription id>/resourcegroups/<your resource group>/providers/Microsoft.ContainerService/managedClusters/<your aks service name>'

# Set to the name of the cluster
cluster_name='my-existing-aks' 

# Attatch the cluster to your workgroup
aks_target = AksCompute.attach(workspace=ws, name=cluster_name, resource_id=resource_id)

# Wait for the operation to complete
aks_target.wait_for_completion(True)
```

## <a name="deploy-your-web-service"></a>Uw webservice implementeren

Het volgende codefragment laat zien hoe u de installatiekopie implementeert op het AKS-cluster:

```python
from azureml.core.webservice import Webservice, AksWebservice

# Set configuration and service name
aks_config = AksWebservice.deploy_configuration()
aks_service_name ='aks-service-1'
# Deploy from image
aks_service = Webservice.deploy_from_image(workspace = ws, 
                                            name = aks_service_name,
                                            image = image,
                                            deployment_config = aks_config,
                                            deployment_target = aks_target)
# Wait for the deployment to complete
aks_service.wait_for_deployment(show_output = True)
print(aks_service.state)
```

> [!TIP]
> Als er fouten tijdens de implementatie zijn, gebruikt u `aks_service.get_logs()` om de AKS-service-logboeken weer te geven. De vastgelegde gegevens mogelijk de oorzaak van de fout.

## <a name="test-the-web-service"></a>De webservice testen

Gebruik `aks_service.run()` voor het testen van de webservice. Het volgende codefragment laat zien hoe u gegevens doorgeven aan de service en de voorspelling weer te geven:

```python
import json

test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')

prediction = aks_service.run(input_data = test_sample)
print(prediction)
```

## <a name="update-the-web-service"></a>Bijwerken van de webservice

Voor het bijwerken van de webservice, gebruikt u de `update` methode. De volgende code ziet u hoe u de webservice voor het gebruik van een nieuwe installatiekopie bijwerken:

```python
from azureml.core.webservice import Webservice

service_name = 'aci-mnist-3'
# Retrieve existing service
service = Webservice(name = service_name, workspace = ws)
# Update the image used by the service
service.update(image = new-image)
print(service.state)
```

> [!NOTE]
> Wanneer u een installatiekopie van een bijwerkt, wordt de webservice niet automatisch bijgewerkt. Elke service die u wilt gebruiken van de nieuwe installatiekopie moet u handmatig bijwerken.

## <a name="cleanup"></a>Opschonen

Als de service, de installatiekopie en het model verwijderen, gebruikt u het volgende codefragment:

```python
aks_service.delete()
image.delete()
model.delete()
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [verbruiken een ML-Model is geïmplementeerd als een webservice](how-to-consume-web-service.md).