---
title: Modellen op FPGA's implementeren
titleSuffix: Azure Machine Learning service
description: Leer hoe u een webservice implementeren met een model uitvoeren op een FPGA met Azure Machine Learning-service voor zeer lage latentie Deductie.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: tedway
author: tedway
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 511333342371e18f75b48b60486705571353ee13
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735052"
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning-service"></a>Een model implementeren als een webservice op een FPGA met Azure Machine Learning-service

U kunt een model implementeren als een webservice op [programmable gate arrays (FPGA's) veld](concept-accelerate-with-fpgas.md) met Azure Machine Learning Hardware versnelde-modellen. Met behulp van FPGA's biedt zeer lage latentie Deductie, zelfs met een enkele batchgrootte. Deductie of het model scoren, is de fase waarin het gedistribueerde model wordt gebruikt voor voorspellingen, meestal op productiegegevens.

Deze modellen zijn momenteel beschikbaar:
  - ResNet 50
  - ResNet 152
  - DenseNet-121
  - VGG-16
  - SSD-VGG

FPGA's zijn beschikbaar in deze Azure-regio's:
  - US - oost
  - Azië - zuidoost
  - Europa -west
  - US - west 2

> [!IMPORTANT]
> Voor het optimaliseren van latentie en doorvoer, moet uw gegevens te verzenden naar het model FPGA-client zich in een van de regio's boven (het account dat u het model hebt geïmplementeerd).

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement.  Als u een hebt, een gratis account maken voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

- FPGA quota.  De Azure CLI gebruiken om te controleren of u het quotum hebt.
    ```shell
    az vm list-usage --location "eastus" -o table
    ```

    De andere locaties zijn ``southeastasia``, ``westeurope``, en ``westus2``.

    Onder de kolom 'Name', zoek naar 'Standard PBS-serie vcpu's ' en zorg ervoor dat u hebt ten minste 6 vcpu's onder "CurrentValue."

    Als u geen quota, klikt u vervolgens een aanvraagformulier indienen [hier](https://aka.ms/accelerateAI).

- Een werkruimte van Azure Machine Learning-service en de Azure Machine Learning-SDK voor Python geïnstalleerd. Informatie over het verkrijgen van deze vereisten met behulp van de [het configureren van een ontwikkelomgeving](how-to-configure-environment.md) document.
 
- De Python-SDK voor hardwareversnelling modellen:

    ```shell
    pip install --upgrade azureml-accel-models
    ```

## <a name="sample-notebooks"></a>Voorbeelden van notebooks

Voor uw gemak [voorbeeld notitieblokken](https://aka.ms/aml-accel-models-notebooks) zijn beschikbaar voor het volgende voorbeeld en andere voorbeelden.

## <a name="create-and-containerize-your-model"></a>Maken en uw model in een container plaatsen

Dit document wordt beschreven hoe u een grafiek TensorFlow voorverwerken van de afbeelding, kunt u een featurizer met ResNet 50 op een FPGA maakt en voer de functies via een van de ImageNet-gegevensset getraind classificatie.

Volg de instructies voor:

* Het model TensorFlow definiëren
* Converteren van het model
* Het model implementeren
* De geïmplementeerde model gebruiken
* Geïmplementeerde services verwijderen

### <a name="load-azure-ml-workspace"></a>Laden van Azure ML-werkruimte

Laden van uw Azure ML-werkruimte.

```python
import os
import tensorflow as tf
 
from azureml.core import Workspace

ws = Workspace.from_config()
print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')
```

### <a name="preprocess-image"></a>Afbeelding voorverwerken

De invoer voor de webservice is een JPEG-afbeelding.  De eerste stap is het decoderen van de JPEG-afbeelding en voorverwerken deze.  De JPEG-afbeeldingen worden behandeld als tekenreeksen en het resultaat zijn tensors die de invoer voor het model ResNet 50.

```python
# Input images as a two-dimensional tensor containing an arbitrary number of images represented a strings
import azureml.accel.models.utils as utils
tf.reset_default_graph()

in_images = tf.placeholder(tf.string)
image_tensors = utils.preprocess_array(in_images)
print(image_tensors.shape)
```

### <a name="load-featurizer"></a>Featurizer laden

Initialiseren van het model en een TensorFlow-controlepunt van de quantized versie van ResNet50 moet worden gebruikt als een featurizer downloaden.  U mag 'QuantizedResnet50' in het codefragment hieronder met vervangen door het importeren van andere deep neural networks:

- QuantizedResnet152
- QuantizedVgg16
- Densenet121

```python
from azureml.accel.models import QuantizedResnet50
save_path = os.path.expanduser('~/models')
model_graph = QuantizedResnet50(save_path, is_frozen = True)
feature_tensor = model_graph.import_graph_def(image_tensors)
print(model_graph.version)
print(feature_tensor.name)
print(feature_tensor.shape)
```

### <a name="add-classifier"></a>Classificatie toevoegen

Deze classificatie is getraind op de ImageNet-gegevensset.  Voorbeelden voor overdracht leren en training van uw aangepaste gewichten zijn beschikbaar in de set [voorbeeld notitieblokken](https://aka.ms/aml-notebooks).

```python
classifier_output = model_graph.get_default_classifier(feature_tensor)
print(classifier_output)
```

### <a name="save-the-model"></a>Sla het model

Nu dat de preprocessor ResNet 50 featurizer en de classificatie zijn geladen, sla op de grafiek en de bijbehorende variabelen als een model.

```python
model_name = "resnet50"
model_save_path = os.path.join(save_path, model_name)
print("Saving model in {}".format(model_save_path))

with tf.Session() as sess:
    model_graph.restore_weights(sess)
    tf.saved_model.simple_save(sess, model_save_path,
                                   inputs={'images': in_images},
                                   outputs={'output_alias': classifier_output})
```

### <a name="save-input-and-output-tensors"></a>Invoer en uitvoer tensors opslaan
De invoer en uitvoer tensors die zijn gemaakt tijdens de voorverwerking en classificatie stappen nodig voor de conversie van het model en Deductie.

```python
input_tensors = in_images.name
output_tensors = classifier_output.name

print(input_tensors)
print(output_tensors)
```

> [!IMPORTANT]
> Sla de invoer en uitvoer tensors omdat u deze conversie en Deductie modelaanvragen moet.

De beschikbare modellen en de bijbehorende standaard classificatie uitvoer tensors zijn hieronder, dit is wat u wilt gebruiken voor Deductie als u de classificatie standaard gebruikt.

+ Resnet50, QuantizedResnet50 ``
output_tensors = "classifier_1/resnet_v1_50/predictions/Softmax:0"
``
+ Resnet152, QuantizedResnet152 ``
output_tensors = "classifier/resnet_v1_152/predictions/Softmax:0"
``
+ Densenet121, QuantizedDensenet121 ``
output_tensors = "classifier/densenet121/predictions/Softmax:0"
``
+ Vgg16, QuantizedVgg16 ``
output_tensors = "classifier/vgg_16/fc8/squeezed:0"
``
+ SsdVgg, QuantizedSsdVgg ``
output_tensors = ['ssd_300_vgg/block4_box/Reshape_1:0', 'ssd_300_vgg/block7_box/Reshape_1:0', 'ssd_300_vgg/block8_box/Reshape_1:0', 'ssd_300_vgg/block9_box/Reshape_1:0', 'ssd_300_vgg/block10_box/Reshape_1:0', 'ssd_300_vgg/block11_box/Reshape_1:0', 'ssd_300_vgg/block4_box/Reshape:0', 'ssd_300_vgg/block7_box/Reshape:0', 'ssd_300_vgg/block8_box/Reshape:0', 'ssd_300_vgg/block9_box/Reshape:0', 'ssd_300_vgg/block10_box/Reshape:0', 'ssd_300_vgg/block11_box/Reshape:0']
``

### <a name="register-model"></a>Model registreren

[Registreren](./concept-model-management-and-deployment.md) het model dat u hebt gemaakt.  -Tags en andere metagegevens over het model toe te voegen, kunt u bijhouden van uw getrainde modellen.

```python
from azureml.core.model import Model

registered_model = Model.register(workspace = ws,
                                  model_path = model_save_path,
                                  model_name = model_name)

print("Successfully registered: ", registered_model.name, registered_model.description, registered_model.version, sep = '\t')
```

Als u al een model hebt geregistreerd en u wilt laden, kunt u deze kan ophalen.

```python
from azureml.core.model import Model
model_name = "resnet50"
# By default, the latest version is retrieved. You can specify the version, i.e. version=1
registered_model = Model(ws, name="resnet50")
print(registered_model.name, registered_model.description, registered_model.version, sep = '\t')
```

### <a name="convert-model"></a>Converteren van model

De grafiek TensorFlow converteren naar de Open Neural Network-Exchange-indeling ([ONNX](https://onnx.ai/)).  U moet de namen van de invoer en uitvoer tensors opgeven en deze namen wordt gebruikt door de client wanneer u de webservice gebruiken.

```python
from azureml.accel import AccelOnnxConverter

convert_request = AccelOnnxConverter.convert_tf_model(ws, registered_model, input_tensors, output_tensors)

# If it fails, you can run wait_for_completion again with show_output=True.
convert_request.wait_for_completion(show_output = False)

# If the above call succeeded, get the converted model
converted_model = convert_request.result
print("\nSuccessfully converted: ", converted_model.name, converted_model.url, converted_model.version, 
      converted_model.id, converted_model.created_time, '\n')
```

### <a name="create-docker-image"></a>Docker-installatiekopie maken

Het geconverteerde model en alle afhankelijkheden worden toegevoegd aan een Docker-installatiekopie.  Dit Docker-installatiekopie kan vervolgens worden geïmplementeerd en geïnstantieerd.  Ondersteunde implementatiedoelen AKS opnemen in de cloud of een edge-apparaat, zoals [Azure Data Box Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview).  U kunt ook tags en beschrijvingen toevoegen voor uw geregistreerde Docker-installatiekopie.

```python
from azureml.core.image import Image
from azureml.accel import AccelContainerImage

image_config = AccelContainerImage.image_configuration()
# Image name must be lowercase
image_name = "{}-image".format(model_name)

image = Image.create(name = image_name,
                     models = [converted_model],
                     image_config = image_config, 
                     workspace = ws)
image.wait_for_creation(show_output = False)
```

Geeft u de installatiekopieën op label en de gedetailleerde logboeken voor foutopsporing ophalen.

```python
for i in Image.list(workspace = ws):
    print('{}(v.{} [{}]) stored at {} with build log {}'.format(i.name, i.version, i.creation_state, i.image_location, i.image_build_log_uri))
```

## <a name="model-deployment"></a>Modelimplementatie

### <a name="deploy-to-the-cloud"></a>Implementeren naar de cloud

Gebruik voor het implementeren van uw modellen vervolgens als een webservice schalen-productie, Azure Kubernetes Service (AKS). U kunt een nieuw subnet met de SDK van Azure Machine Learning, CLI of Azure portal maken.

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Specify the Standard_PB6s Azure VM
prov_config = AksCompute.provisioning_configuration(vm_size = "Standard_PB6s",
                                                    agent_count = 1)

aks_name = 'my-aks-cluster'
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws, 
                                  name = aks_name, 
                                  provisioning_configuration = prov_config)
```

De AKS-implementatie kan ongeveer 15 minuten duren.  Controleer of de implementatie is geslaagd.

```python
aks_target.wait_for_completion(show_output = True)
print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)
```

De container geïmplementeerd naar het AKS-cluster.
```python
from azureml.core.webservice import Webservice, AksWebservice

# For this deployment, set the web service configuration without enabling auto-scaling or authentication for testing
aks_config = AksWebservice.deploy_configuration(autoscale_enabled=False,
                                                num_replicas=1,
                                                auth_enabled = False)

aks_service_name ='my-aks-service'

aks_service = Webservice.deploy_from_image(workspace = ws,
                                           name = aks_service_name,
                                           image = image,
                                           deployment_config = aks_config,
                                           deployment_target = aks_target)
aks_service.wait_for_deployment(show_output = True)
```

#### <a name="test-the-cloud-service"></a>De cloudservice testen
De Docker-installatiekopie biedt ondersteuning voor gRPC en de TensorFlow behoeve "voorspellen" API.  De voorbeeldclient gebruiken om aan te roepen in de Docker-installatiekopie verkrijgen van voorspellingen van het model.  Client-voorbeeldcode is beschikbaar:
- [Python](https://github.com/Azure/aml-real-time-ai/blob/master/pythonlib/amlrealtimeai/client.py)
- [C#](https://github.com/Azure/aml-real-time-ai/blob/master/sample-clients/csharp)

Als u gebruiken voor de TensorFlow wilt, kunt u [een voorbeeldclient downloaden](https://www.tensorflow.org/serving/setup).

```python
# Using the grpc client in Azure ML Accelerated Models SDK package
from azureml.accel import PredictionClient

address = aks_service.scoring_uri
ssl_enabled = address.startswith("https")
address = address[address.find('/')+2:].strip('/')
port = 443 if ssl_enabled else 80

# Initialize AzureML Accelerated Models client
client = PredictionClient(address=address,
                          port=port,
                          use_ssl=ssl_enabled,
                          service_name=aks_service.name)
```

Omdat deze classificatie is getraind op de [ImageNet](http://www.image-net.org/) gegevens instellen, de klassen worden toegewezen aan mensen leesbare labels.

```python
import requests
classes_entries = requests.get("https://raw.githubusercontent.com/Lasagne/Recipes/master/examples/resnet50/imagenet_classes.txt").text.splitlines()

# Score image with input and output tensor names
results = client.score_file(path="./snowleopardgaze.jpg", 
                             input_name=input_tensors, 
                             outputs=output_tensors)

# map results [class_id] => [confidence]
results = enumerate(results)
# sort results by confidence
sorted_results = sorted(results, key=lambda x: x[1], reverse=True)
# print top 5 results
for top in sorted_results[:5]:
    print(classes_entries[top[0]], 'confidence:', top[1])
```

### <a name="clean-up-the-service"></a>Opschonen van de service
Verwijder uw web-service, de installatiekopie en het model (moet worden uitgevoerd in de aangegeven volgorde omdat er afhankelijkheden).

```python
aks_service.delete()
aks_target.delete()
image.delete()
registered_model.delete()
converted_model.delete()
```

## <a name="deploy-to-a-local-edge-server"></a>Implementeren naar een lokale edge-server

Alle [Azure Data Box-Edge-apparaten](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview
) een FPGA bevatten voor het uitvoeren van het model.  Slechts één model kan worden uitgevoerd op FPGA in één keer.  Voor het uitvoeren van een ander model, moet u alleen een nieuwe container implementeren. Instructies en voorbeelden van code kunnen worden gevonden in [in dit voorbeeld Azure](https://github.com/Azure-Samples/aml-hardware-accelerated-models).

## <a name="secure-fpga-web-services"></a>Beveiligde FPGA-webservices

Zie voor meer informatie over het beveiligen van FPGA-webservices de [Secure webservices](how-to-secure-web-service.md) document.

## <a name="pbs-family-vms"></a>PBS-serie VM 's

De PBS-serie virtuele Azure-machines bevat Intel Arria 10 FPGA's.  Wordt deze weergegeven als 'Standard PBS-serie vcpu's ' wanneer u de toewijzing van uw Azure-quotum controleren.  De VM PB6 heeft zes vcpu's, en één FPGA en deze automatisch worden ingericht met Azure ML als onderdeel van een model implementeert naar een FPGA.  Het wordt alleen gebruikt met Azure ML en willekeurige bitstreams kan niet worden uitgevoerd.  Bijvoorbeeld niet mogelijk voor flash FPGA met bitstreams hiervoor codering, versleuteling, enzovoort. 
