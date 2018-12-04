---
title: Webservices implementeren naar Azure Container Instances (ACI) - Azure Machine Learning
description: Informatie over het implementeren van een getraind model als een webservice op Azure Container Instances (ACI) met Azure Machine Learning-service. In dit artikel ziet u drie verschillende manieren om een model in ACI te implementeren. Deze verschillen in het aantal regels code en het besturingselement dat u in naamgeving delen van de implementatie hebt.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: raymondl
author: raymondlaghaeian
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 31a905e1fb16997eb80e47af3b790f431f28e49b
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52842863"
---
# <a name="deploy-web-services-to-azure-container-instances"></a>Webservices implementeren in Azure Container Instances 

U kunt het getrainde model implementeren als een webservice op [Azure Container Instances](https://azure.microsoft.com/services/container-instances/) (ACI), [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/) (AKS), IoT edge-apparaat, of [veld programmable gate arrays (FPGA's)](concept-accelerate-with-fpgas.md) 

ACI is doorgaans goedkoper dan AKS en kan worden ingesteld in 4-6-regels met code. ACI is de perfecte optie voor het testen van implementaties. Later, wanneer u klaar bent voor gebruik van uw modellen en webservices voor het productiegebruik van zeer schaalbare,, kunt u [deze implementeren naar AKS](how-to-deploy-to-aks.md).

In dit artikel ziet u drie verschillende manieren om een model in ACI te implementeren. Deze verschillen in het aantal regels code en het besturingselement dat u in naamgeving delen van de implementatie hebt. Met de methode met de minimale hoeveelheid code en beheer aan de methode met de meeste code en beheer zijn de ACI-opties:

* Implementeren van het bestand met behulp van model `Webservice.deploy()` 
* Implementeren vanaf het geregistreerde model gebruiken `Webservice.deploy_from_model()`
* Geregistreerde model uit met behulp van de installatiekopie implementeren `Webservice.deploy_from_image()`

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://aka.ms/AMLfree) aan voordat u begint.


## <a name="prerequisites"></a>Vereisten

- Een werkruimte van Azure Machine Learning-service en de Azure Machine Learning-SDK voor Python geïnstalleerd. Informatie over het verkrijgen van deze vereisten met behulp van de [aan de slag met Azure Machine Learning-snelstartgids](quickstart-get-started.md).

- Het Azure Machine Learning-werkruimte-object

    ```python
    from azureml.core import Workspace
    ws = Workspace.from_config()
    ```

- Een model om te implementeren. De voorbeelden in dit document gebruikt u het model gemaakt wanneer u volgt de "[een model te trainen](tutorial-train-models-with-aml.md)" zelfstudie. Als u dit model niet gebruikt, wijzigt u de stappen om te verwijzen naar de modelnaam van uw.  U moet ook uw eigen scoring-script om uit te voeren van uw model schrijven.


## <a name="configure-an-image"></a>Een afbeelding van configureren

Configureer de Docker-installatiekopie die wordt gebruikt voor het opslaan van alle modelbestanden.
1. Maken van een scoring-script (score.py) [met behulp van deze instructies](tutorial-deploy-models-with-aml.md#create-scoring-script).

    > [!IMPORTANT]
    > Het scoring-script ontvangt van clients verzonden gegevens en wordt doorgegeven aan het model voor het scoren. Documenteer de gegevensstructuur die het script en het model verwacht. Met deze documentatie wordt dingen gemakkelijker bij het bouwen van een client de webservice gebruiken.

1. Maak een omgevingsbestand (myenv.yml) [met behulp van deze instructies](tutorial-deploy-models-with-aml.md#create-environment-file).

1. Gebruik deze twee bestanden om te configureren van de Docker-installatiekopie in Python met behulp van de SDK, als volgt:

    ```python
    from azureml.core.image import ContainerImage

    image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                      runtime = "python",
                                                      conda_file = "myenv.yml",
                                                      description = "Image with mnist model",
                                                      tags = {"data": "mnist", "type": "classification"}
                                                     )
    ```

## <a name="configure-the-aci-container"></a>Configureer de ACI-container

Configureer de ACI-container opgeven het aantal CPU's en GB aan RAM-geheugen die nodig zijn voor uw ACI-container. De standaardwaarde van één kern en 1 GB aan RAM-geheugen is voldoende voor meerdere modellen. Als u denkt dat moet u de installatiekopie later nog meer licenties opnieuw en implementeer de service opnieuw.  

```python
from azureml.core.webservice import AciWebservice

aciconfig = AciWebservice.deploy_configuration(cpu_cores = 1, 
                                               memory_gb = 1, 
                                               tags = {"data": "mnist", "type": "classification"},
                                               description = 'Handwriting recognition')
```

## <a name="register-a-model"></a>Een model registreren

> Deze vereiste overslaan als u [implementeren vanaf een modelbestand](#deploy-from-model-file) (`Webservice.deploy()`).

Registreren van een model te gebruiken [Webservice.deploy_from_model](#deploy-from-registered-model) of [Webservice.deploy_from_image](#deploy-from-image). Of als u al een geregistreerde model, moet u deze nu ophalen.

### <a name="retrieve-a-registered-model"></a>Ophalen van een geregistreerde model
Als u uw model te trainen met Azure Machine Learning, kan het model al zijn geregistreerd in uw werkruimte.  Bijvoorbeeld, de laatste stap van de [zelfstudie: een model trainen](tutorial-train-models-with-aml.md) geregistreerd van het model.  U vervolgens het geregistreerde model implementeren opgehaald.

```python
from azureml.core.model import Model

model_name = "sklearn_mnist"
model=Model(ws, model_name)
```
  
### <a name="register-a-model-file"></a>Een modelbestand registreren

Als uw model elders is gebouwd, kunt u deze nog steeds registreren in uw werkruimte.  Een model, het modelbestand te registreren (`sklearn_mnist_model.pkl` in dit voorbeeld) moet zich in de huidige werkmap. Dit bestand vervolgens registreren als een model met de naam `sklearn_mnist` in de werkruimte met `Model.register()`.
    
```python
from azureml.core.model import Model

model_name = "sklearn_mnist"
model = Model.register(model_path = "sklearn_mnist_model.pkl",
                        model_name = model_name,
                        tags = {"data": "mnist", "type": "classification"},
                        description = "Mnist handwriting recognition",
                        workspace = ws)
```

<a name='deploy-from-model-file'/>
## <a name="option-1-deploy-from-model-file"></a>Optie 1: Implementatie van modelbestand

De optie voor het implementeren van een modelbestand is de minimale hoeveelheid code te schrijven, maar biedt controle over de naamgeving van onderdelen zo min mogelijk vereist. Deze optie wordt gestart met een modelbestand en registreert deze in de werkruimte voor u.  U kan echter naam van het model of tags of een beschrijving voor het koppelen.  

Deze optie maakt gebruik van de SDK-methode Webservice.deploy().  

**Geschatte tijd**: implementatie duurt ongeveer 6-7 minuten.

1. Zorg ervoor dat het modelbestand is in uw lokale werkmap.

1. Open het vereiste modelbestand, score.py en wijzig de `init()` gedeelte:

    ```python
    def init():
        global model
        # retreive the local path to the model using the model name
        model_path = Model.get_model_path('sklearn_mnist_model.pkl')
        model = joblib.load(model_path)
    ```

1. Implementeer uw modelbestand.

    ```python
    from azureml.core.webservice import Webservice
    
    service_name = 'aci-mnist-1'
    service = Webservice.deploy(deployment_config = aciconfig,
                                    image_config = image_config,
                                    model_paths = ['sklearn_mnist_model.pkl'],
                                    name = service_name,
                                    workspace = ws)
    
    service.wait_for_deployment(show_output = True)
    print(service.state)
    ```

1. U kunt nu [testen van de webservice](#test-web-service).

<a name='deploy-from-registered-model'/>
## <a name="option-2-deploy-from-registered-model"></a>Optie 2: Uit geregistreerde model implementeren

De optie voor het implementeren van een geregistreerde modelbestand duurt een paar regels code en kan sommige controle over de naamgeving van uitvoer. Deze optie is een handige manier voor het implementeren van een geregistreerde model dat u al hebt.  U kunt de Docker-installatiekopie echter kan niet naam.  

Deze optie maakt gebruik van de SDK-methode Webservice.deploy_from_model().

**Geschatte tijd**: implementeren met deze optie duurt ongeveer 8 minuten.

1. Voer de code voor het configureren van de Docker-container en de ACI-container en het geregistreerde model opgeven.

    ```python
    from azureml.core.webservice import Webservice

    service_name = 'aci-mnist-2'
    service = Webservice.deploy_from_model(deployment_config = aciconfig,
                                           image_config = image_config,
                                           models = [model], # this is the registered model object
                                           name = service_name,
                                           workspace = ws)
    service.wait_for_deployment(show_output = True)
    print(service.state)
    ```

1. U kunt nu [testen van de webservice](#test-web-service).

<a name='deploy-from-image'/>
## <a name="option-3-deploy-from-image"></a>Optie 3: Implementatie van installatiekopie

Implementeren van een geregistreerde model (`model`) met behulp van `Webservice.deploy_from_image()`. Deze methode kunt u de Docker-installatiekopie afzonderlijk maken en vervolgens implementeren van die installatiekopie.

1. Bouwen en te registreren in de werkruimte met behulp van de Docker-installatiekopie `ContainerImage.create()`

    Deze methode biedt u meer controle over de installatiekopie door in een aparte stap te maken.  Het geregistreerde model (`model`) is opgenomen in de afbeelding.
    
    ```python
    from azureml.core.image import ContainerImage
    
    image = ContainerImage.create(name = "myimage1",
                                  models = [model], # this is the registered model object
                                  image_config = image_config,
                                  workspace = ws)
    
    image.wait_for_creation(show_output = True)
    ```
**Geschatte tijd**: ongeveer 3 minuten.

1. Als een service met de Docker-installatiekopie implementeren `Webservice.deploy_from_image()`

    Nu de installatiekopie implementeren naar ACI.  
    
    ```python
    from azureml.core.webservice import Webservice
    
    service_name = 'aci-mnist-3'
    service = Webservice.deploy_from_image(deployment_config = aciconfig,
                                                image = image,
                                                name = service_name,
                                                workspace = ws)
    service.wait_for_deployment(show_output = True)
    print(service.state)
    ```   
 
**Geschatte tijd**: ongeveer 3 minuten.

Deze methode biedt u de meeste controle over het maken en de namen van de onderdelen in de implementatie.

U kunt de webservice nu testen.

## <a name="a-nametest-web-servicetest-the-web-service"></a><a name='test-web-service'/>De webservice testen

De webservice is hetzelfde, ongeacht welke methode is gebruikt.  Voor voorspellingen, gebruikt de `run` methode van de service.  

```python
# Load Data
import os
import urllib

os.makedirs('./data', exist_ok = True)

urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename = './data/test-images.gz')

from utils import load_data
X_test = load_data('./data/test-images.gz', False) / 255.0

from sklearn import datasets
import numpy as np
import json

# find 5 random samples from test set
n = 5
sample_indices = np.random.permutation(X_test.shape[0])[0:n]

test_samples = json.dumps({"data": X_test[sample_indices].tolist()})
test_samples = bytes(test_samples, encoding = 'utf8')

# predict using the deployed model
prediction = service.run(input_data = test_samples)
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

## <a name="clean-up-resources"></a>Resources opschonen

Als u niet verder deze webservice gebruikt, moet u het verwijderen, zodat u geen kosten.

```python
service.delete()
```

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [verbruiken een ML-Model is geïmplementeerd als een webservice](how-to-consume-web-service.md).
* Meer informatie over het [implementeren in Azure Kubernetes Service](how-to-deploy-to-aks.md) voor implementatie op grote schaal. 
