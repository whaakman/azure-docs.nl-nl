---
title: Modellen op FPGA's implementeren
titleSuffix: Azure Machine Learning service
description: Leer hoe u een webservice implementeren met een model uitvoeren op een FPGA met Azure Machine Learning-service voor inferentietaken zeer lage latentie.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: 3148d4d63ad1464dbd45c361237ac9cd4ffd485a
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53268237"
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning-service"></a>Een model implementeren als een webservice op een FPGA met Azure Machine Learning-service

U kunt een model implementeren als een webservice op [programmable gate arrays (FPGA's) veld](concept-accelerate-with-fpgas.md).  Met behulp van FPGA's biedt zeer lage latentie inferentietaken, zelfs met een enkele batchgrootte.   

## <a name="prerequisites"></a>Vereisten

- Als u geen Azure-abonnement hebt, een gratis account maken voordat u begint. Probeer de [gratis of betaalde versie van Azure Machine Learning-service](http://aka.ms/AMLFree) vandaag nog.

- Een werkruimte van Azure Machine Learning-service en de Azure Machine Learning-SDK voor Python geïnstalleerd. Informatie over het verkrijgen van deze vereisten met behulp van de [het configureren van een ontwikkelomgeving](how-to-configure-environment.md) document.
 
  - Uw werkruimte moet zich in de *VS-Oost 2* regio.

  - Installeer de contrib extra's:

    ```shell
    pip install --upgrade azureml-sdk[contrib]
    ```  

## <a name="create-and-deploy-your-model"></a>Uw model maakt en implementeert
Een pijplijn maken voor de invoer parametriseer-installatiekopie met behulp van ResNet 50 op een FPGA voorverwerken, en voer vervolgens de functies via een classifer getraind op de ImageNet-gegevensset.

Volg de instructies voor:

* De pijplijn model definiëren
* Het model implementeren
* De geïmplementeerde model gebruiken
* Geïmplementeerde services verwijderen

> [!IMPORTANT]
> Voor het optimaliseren van latentie en doorvoer, moet uw client zich in dezelfde Azure-regio als het eindpunt.  Op dit moment worden de API's gemaakt in de oostelijke VS Azure-regio.



### <a name="preprocess-image"></a>Afbeelding voorverwerken
De eerste fase van de pijplijn is met de voorverwerking van de installatiekopieën.

```python
import os
import tensorflow as tf

# Input images as a two-dimensional tensor containing an arbitrary number of images represented a strings
import azureml.contrib.brainwave.models.utils as utils
in_images = tf.placeholder(tf.string)
image_tensors = utils.preprocess_array(in_images)
print(image_tensors.shape)
```

### <a name="add-featurizer"></a>Featurizer toevoegen
Initialiseren van het model en een TensorFlow-controlepunt van de quantized versie van ResNet50 moet worden gebruikt als een featurizer downloaden.

```python
from azureml.contrib.brainwave.models import QuantizedResnet50, Resnet50
model_path = os.path.expanduser('~/models')
model = QuantizedResnet50(model_path, is_frozen = True)
feature_tensor = model.import_graph_def(image_tensors)
print(model.version)
print(feature_tensor.name)
print(feature_tensor.shape)
```

### <a name="add-classifier"></a>Classificatie toevoegen
Deze classificatie is getraind op de ImageNet-gegevensset.

```python
classifier_input, classifier_output = Resnet50.get_default_classifier(feature_tensor, model_path)
```

### <a name="create-service-definition"></a>De servicedefinitie van de maken
Nu dat u gedefinieerd, hebt de voorverwerking van de installatiekopie, featurizer en classificatie die wordt uitgevoerd op de service, kunt u de servicedefinitie van een kunt maken. De servicedefinitie van de is een set bestanden die zijn gegenereerd op basis van het model dat is geïmplementeerd naar de FPGA-service. Definitie van de service bestaat uit een pijplijn. De pijplijn is een reeks fasen die in volgorde worden uitgevoerd.  TensorFlow-fasen, Keras-fasen en BrainWave fasen worden ondersteund.  De fasen worden uitgevoerd in volgorde van de service, met de uitvoer van de invoer van elke fase in de volgende fase.

Maken van een fase TensorFlow, een sessie met de grafiek (in dit geval standaard grafiek wordt gebruikt) en de invoer en uitvoer tensors naar deze fase.  Deze informatie wordt gebruikt om op te slaan van de grafiek, zodat deze kan worden uitgevoerd op de service.

```python
from azureml.contrib.brainwave.pipeline import ModelDefinition, TensorflowStage, BrainWaveStage

save_path = os.path.expanduser('~/models/save')
model_def_path = os.path.join(save_path, 'service_def.zip')

model_def = ModelDefinition()
with tf.Session() as sess:
    model_def.pipeline.append(TensorflowStage(sess, in_images, image_tensors))
    model_def.pipeline.append(BrainWaveStage(sess, model))
    model_def.pipeline.append(TensorflowStage(sess, classifier_input, classifier_output))
    model_def.save(model_def_path)
    print(model_def_path)
```

### <a name="deploy-model"></a>Model implementeren
Een service maken vanuit definitie van de service.  Uw werkruimte moet zich in de locatie VS-Oost 2.

```python
from azureml.core import Workspace

ws = Workspace.from_config()
print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')

from azureml.core.model import Model
model_name = "resnet-50-rtai"
registered_model = Model.register(ws, model_def_path, model_name)

from azureml.core.webservice import Webservice
from azureml.exceptions import WebserviceException
from azureml.contrib.brainwave import BrainwaveWebservice, BrainwaveImage
service_name = "imagenet-infer"
service = None
try:
    service = Webservice(ws, service_name)
except WebserviceException:
    image_config = BrainwaveImage.image_configuration()
    deployment_config = BrainwaveWebservice.deploy_configuration()
    service = Webservice.deploy_from_model(ws, service_name, [registered_model], image_config, deployment_config)
    service.wait_for_deployment(true)
```

### <a name="test-the-service"></a>Test de service
Een afbeelding verzenden naar de API en testen van het antwoord, een toewijzing van de uitvoer klasse-ID aan de naam van de klasse ImageNet toevoegen.

```python
import requests
classes_entries = requests.get("https://raw.githubusercontent.com/Lasagne/Recipes/master/examples/resnet50/imagenet_classes.txt").text.splitlines()
```

Uw service aanroepen en de naam van het bestand 'uw image.jpg' hieronder vervangen door een installatiekopie vanaf uw computer. 

```python
with open('your-image.jpg') as f:
    results = service.run(f)
# map results [class_id] => [confidence]
results = enumerate(results)
# sort results by confidence
sorted_results = sorted(results, key=lambda x: x[1], reverse=True)
# print top 5 results
for top in sorted_results[:5]:
    print(classes_entries[top[0]], 'confidence:', top[1])
``` 

### <a name="clean-up-service"></a>Opschonen van de service
De service verwijderen.

```python
service.delete()
    
registered_model.delete()
```

## <a name="secure-fpga-web-services"></a>Beveiligde FPGA-webservices

Azure Machine Learning-modellen die worden uitgevoerd op FPGA's bieden ondersteuning voor SSL en verificatie op basis van een sleutel. Hiermee kunt u toegang tot uw service en beveiligd door clients verzonden gegevens te beperken. [Meer informatie over het beveiligen van de webservice](how-to-secure-web-service.md).


## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [verbruiken een ML-Model is geïmplementeerd als een webservice](how-to-consume-web-service.md).
