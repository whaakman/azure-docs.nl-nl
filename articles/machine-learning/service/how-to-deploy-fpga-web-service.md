---
title: Modellen implementeren op Fpga's
titleSuffix: Azure Machine Learning service
description: Meer informatie over het implementeren van een webservice met een model dat wordt uitgevoerd op een FPGA met Azure Machine Learning-service voor een extreem lage latentie.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: tedway
author: tedway
ms.date: 07/25/2019
ms.custom: seodec18
ms.openlocfilehash: 5e780c1a1d18954d0b3e9413e26f478e4ea3a8b1
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68856107"
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning-service"></a>Een model als een webservice implementeren op een FPGA met Azure Machine Learning service

U kunt een model als een webservice implementeren op [veld Programmeer bare Gate arrays (fpga's)](concept-accelerate-with-fpgas.md) met Azure machine learning modellen met hardwareversnelling. Het gebruik van Fpga's biedt een dering van ultra lage latentie, zelfs met één batch grootte. Defactorion of model Score is de fase waarin het geïmplementeerde model wordt gebruikt voor de voor spelling, meestal op productie gegevens.

Deze modellen zijn momenteel beschikbaar:
  - ResNet 50
  - ResNet 152
  - DenseNet-121
  - VGG-16
  - SSD-VGG

Fpga's zijn beschikbaar in deze Azure-regio's:
  - East US
  - Azië - zuidoost
  - Europa -west
  - US - west 2

> [!IMPORTANT]
> Om de latentie en door voer te optimaliseren, moet uw client gegevens verzenden naar het FPGA-model in een van de bovenstaande regio's (het model dat u hebt geïmplementeerd.)

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement.  Als u er nog geen hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

- FPGA quota. Gebruik de Azure CLI om te controleren of u een quotum hebt:

    ```shell
    az vm list-usage --location "eastus" -o table --query "[?localName=='Standard PBS Family vCPUs']"
    ```

    > [!TIP]
    > De andere mogelijke locaties zijn ``southeastasia``, ``westeurope``, en ``westus2``.

    De opdracht retourneert tekst die er ongeveer als volgt uitziet:

    ```text
    CurrentValue    Limit    LocalName
    --------------  -------  -------------------------
    0               6        Standard PBS Family vCPUs
    ```

    Zorg ervoor dat er ten minste 6 Vcpu's zijn onder __CurrentValue__.

    Als u geen quota hebt, verzendt u een aanvraag op [https://aka.ms/accelerateAI](https://aka.ms/accelerateAI).

- Een werkruimte van Azure Machine Learning-service en de Azure Machine Learning-SDK voor Python geïnstalleerd. Zie [een werk ruimte maken](how-to-manage-workspace.md)voor meer informatie.
 
- De python-SDK voor modellen die door hardware zijn versneld:

    ```shell
    pip install --upgrade azureml-accel-models
    ```

## <a name="sample-notebooks"></a>Voorbeelden van notebooks

Voor uw gemak zijn [voorbeeld notitieblokken](https://aka.ms/aml-accel-models-notebooks) beschikbaar voor het voor beeld hieronder en andere voor beelden.

## <a name="create-and-containerize-your-model"></a>Uw model maken en container plaatsen

In dit document wordt beschreven hoe u een tensor flow-grafiek maakt om de invoer installatie kopie voor te verwerken, een featurizer te maken met ResNet 50 op een FPGA en vervolgens de functies uit te voeren via een getrainde classificatie voor de ImageNet data set.

Volg de instructies voor:

* Het tensor flow-model definiëren
* Het model converteren
* Het model implementeren
* De geïmplementeerde model gebruiken
* Geïmplementeerde services verwijderen

### <a name="load-azure-ml-workspace"></a>Azure ML-werk ruimte laden

Laad uw Azure ML-werk ruimte.

```python
import os
import tensorflow as tf

from azureml.core import Workspace

ws = Workspace.from_config()
print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep='\n')
```

### <a name="preprocess-image"></a>Afbeelding voorverwerken

De invoer voor de webservice is een JPEG-afbeelding.  De eerste stap is het decoderen van de JPEG-afbeelding en de voor verwerking ervan.  De JPEG-afbeeldingen worden behandeld als teken reeksen en het resultaat zijn tien tallen die de invoer zijn van het ResNet 50-model.

```python
# Input images as a two-dimensional tensor containing an arbitrary number of images represented a strings
import azureml.accel.models.utils as utils
tf.reset_default_graph()

in_images = tf.placeholder(tf.string)
image_tensors = utils.preprocess_array(in_images)
print(image_tensors.shape)
```

### <a name="load-featurizer"></a>Featurizer laden

Initialiseren van het model en een TensorFlow-controlepunt van de quantized versie van ResNet50 moet worden gebruikt als een featurizer downloaden.  U kunt ' QuantizedResnet50 ' in het onderstaande code fragment vervangen door andere diepe Neural-netwerken te importeren:

- QuantizedResnet152
- QuantizedVgg16
- Densenet121

```python
from azureml.accel.models import QuantizedResnet50
save_path = os.path.expanduser('~/models')
model_graph = QuantizedResnet50(save_path, is_frozen=True)
feature_tensor = model_graph.import_graph_def(image_tensors)
print(model_graph.version)
print(feature_tensor.name)
print(feature_tensor.shape)
```

### <a name="add-classifier"></a>Classificatie toevoegen

Deze classificatie is getraind op de ImageNet-gegevensset.  Voor beelden voor de overdracht van learning en training uw aangepaste gewichten zijn beschikbaar in de set voor [beeld](https://aka.ms/aml-notebooks)-notebooks.

```python
classifier_output = model_graph.get_default_classifier(feature_tensor)
print(classifier_output)
```

### <a name="save-the-model"></a>Het model opslaan

Nu de preprocessor, ResNet 50 featurizer en de classificatie zijn geladen, slaat u de grafiek en de bijbehorende variabelen op als een model.

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

### <a name="save-input-and-output-tensors"></a>Invoer-en uitvoer intensiteiten opslaan
De bewerkings-en uitvoer intensiteiten die zijn gemaakt tijdens de stappen voor voor verwerking en classificatie, zijn vereist voor model conversie en deinterferentie.

```python
input_tensors = in_images.name
output_tensors = classifier_output.name

print(input_tensors)
print(output_tensors)
```

> [!IMPORTANT]
> Sla de invoer-en uitvoer intensiteiten op, omdat u ze nodig hebt voor model conversie en aanvragen voor inschakeling.

De beschik bare modellen en de bijbehorende standaard waarden voor de classifier-uitvoer zijn lager, wat u zou gebruiken voor het afmaken van de standaard classificatie.

+ Resnet50, QuantizedResnet50
  ```python
  output_tensors = "classifier_1/resnet_v1_50/predictions/Softmax:0"
  ```
+ Resnet152, QuantizedResnet152
  ```python
  output_tensors = "classifier/resnet_v1_152/predictions/Softmax:0"
  ```
+ Densenet121, QuantizedDensenet121
  ```python
  output_tensors = "classifier/densenet121/predictions/Softmax:0"
  ```
+ Vgg16, QuantizedVgg16
  ```python
  output_tensors = "classifier/vgg_16/fc8/squeezed:0"
  ```
+ SsdVgg, QuantizedSsdVgg
  ```python
  output_tensors = ['ssd_300_vgg/block4_box/Reshape_1:0', 'ssd_300_vgg/block7_box/Reshape_1:0', 'ssd_300_vgg/block8_box/Reshape_1:0', 'ssd_300_vgg/block9_box/Reshape_1:0', 'ssd_300_vgg/block10_box/Reshape_1:0', 'ssd_300_vgg/block11_box/Reshape_1:0', 'ssd_300_vgg/block4_box/Reshape:0', 'ssd_300_vgg/block7_box/Reshape:0', 'ssd_300_vgg/block8_box/Reshape:0', 'ssd_300_vgg/block9_box/Reshape:0', 'ssd_300_vgg/block10_box/Reshape:0', 'ssd_300_vgg/block11_box/Reshape:0']
  ```

### <a name="register-model"></a>Model registreren

[Registreer](./concept-model-management-and-deployment.md) het model dat u hebt gemaakt.  Door labels en andere meta gegevens over het model toe te voegen, kunt u uw getrainde modellen volgen.

```python
from azureml.core.model import Model

registered_model = Model.register(workspace=ws,
                                  model_path=model_save_path,
                                  model_name=model_name)

print("Successfully registered: ", registered_model.name,
      registered_model.description, registered_model.version, sep='\t')
```

Als u al een model hebt geregistreerd en wilt laden, kunt u het ophalen.

```python
from azureml.core.model import Model
model_name = "resnet50"
# By default, the latest version is retrieved. You can specify the version, i.e. version=1
registered_model = Model(ws, name="resnet50")
print(registered_model.name, registered_model.description,
      registered_model.version, sep='\t')
```

### <a name="convert-model"></a>Model converteren

Converteer de tensor flow-grafiek naar de open Neural Network Exchange-indeling ([ONNX](https://onnx.ai/)).  U moet de namen van de invoer-en uitvoer-tien tallen opgeven en deze namen worden door de client gebruikt wanneer u de webservice gebruikt.

```python
from azureml.accel import AccelOnnxConverter

convert_request = AccelOnnxConverter.convert_tf_model(
    ws, registered_model, input_tensors, output_tensors)

# If it fails, you can run wait_for_completion again with show_output=True.
convert_request.wait_for_completion(show_output=False)

# If the above call succeeded, get the converted model
converted_model = convert_request.result
print("\nSuccessfully converted: ", converted_model.name, converted_model.url, converted_model.version,
      converted_model.id, converted_model.created_time, '\n')
```

### <a name="create-docker-image"></a>Docker-installatie kopie maken

Het geconverteerde model en alle afhankelijkheden worden toegevoegd aan een docker-installatie kopie.  Deze docker-installatie kopie kan vervolgens worden geïmplementeerd en geïnstantieerd.  Ondersteunde implementatie doelen zijn AKS in de Cloud of een edge-apparaat, zoals [Azure data Box Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview).  U kunt ook Tags en beschrijvingen toevoegen voor uw geregistreerde docker-installatie kopie.

```python
from azureml.core.image import Image
from azureml.accel import AccelContainerImage

image_config = AccelContainerImage.image_configuration()
# Image name must be lowercase
image_name = "{}-image".format(model_name)

image = Image.create(name=image_name,
                     models=[converted_model],
                     image_config=image_config,
                     workspace=ws)
image.wait_for_creation(show_output=False)
```

Vermeld de installatie kopieën op label en ontvang de gedetailleerde logboeken voor elke fout opsporing.

```python
for i in Image.list(workspace=ws):
    print('{}(v.{} [{}]) stored at {} with build log {}'.format(
        i.name, i.version, i.creation_state, i.image_location, i.image_build_log_uri))
```

## <a name="model-deployment"></a>Modelimplementatie

### <a name="deploy-to-the-cloud"></a>Implementeren in de Cloud

Gebruik voor het implementeren van uw modellen vervolgens als een webservice schalen-productie, Azure Kubernetes Service (AKS). U kunt een nieuw account maken met behulp van de Azure Machine Learning SDK, CLI of de Azure Portal.

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Specify the Standard_PB6s Azure VM
prov_config = AksCompute.provisioning_configuration(vm_size="Standard_PB6s",
                                                    agent_count=1)

aks_name = 'my-aks-cluster'
# Create the cluster
aks_target = ComputeTarget.create(workspace=ws,
                                  name=aks_name,
                                  provisioning_configuration=prov_config)
```

De implementatie van de AKS kan ongeveer 15 minuten duren.  Controleer of de implementatie is geslaagd.

```python
aks_target.wait_for_completion(show_output=True)
print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)
```

Implementeer de container in het AKS-cluster.
```python
from azureml.core.webservice import Webservice, AksWebservice

# For this deployment, set the web service configuration without enabling auto-scaling or authentication for testing
aks_config = AksWebservice.deploy_configuration(autoscale_enabled=False,
                                                num_replicas=1,
                                                auth_enabled=False)

aks_service_name = 'my-aks-service'

aks_service = Webservice.deploy_from_image(workspace=ws,
                                           name=aks_service_name,
                                           image=image,
                                           deployment_config=aks_config,
                                           deployment_target=aks_target)
aks_service.wait_for_deployment(show_output=True)
```

#### <a name="test-the-cloud-service"></a>De Cloud service testen
De docker-installatie kopie ondersteunt gRPC en de tensor flow voor ' voors pellen '.  Gebruik de voor beeld-client om een docker-installatie kopie aan te roepen om voor spellingen van het model te verkrijgen.  Voor beeld-client code is beschikbaar:
- [Python](https://github.com/Azure/aml-real-time-ai/blob/master/pythonlib/amlrealtimeai/client.py)
- [C#](https://github.com/Azure/aml-real-time-ai/blob/master/sample-clients/csharp)

Als u tensor flow wilt gebruiken, kunt u [een voorbeeld-client downloaden](https://www.tensorflow.org/serving/setup).

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

Omdat deze classificatie is getraind op de [ImageNet](http://www.image-net.org/) -gegevensset, wijst u de klassen toe aan mensen met lees bare labels.

```python
import requests
classes_entries = requests.get(
    "https://raw.githubusercontent.com/Lasagne/Recipes/master/examples/resnet50/imagenet_classes.txt").text.splitlines()

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

### <a name="clean-up-the-service"></a>De service opschonen
Verwijder uw webservice, installatie kopie en model (dit moet in deze volg orde worden uitgevoerd, omdat er afhankelijkheden zijn).

```python
aks_service.delete()
aks_target.delete()
image.delete()
registered_model.delete()
converted_model.delete()
```

## <a name="deploy-to-a-local-edge-server"></a>Implementeren op een lokale Edge-Server

Alle [Azure data Box Edge-](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview
) apparaten bevatten een FPGA voor het uitvoeren van het model.  Er kan slechts één model tegelijk op de FPGA worden uitgevoerd.  Als u een ander model wilt uitvoeren, implementeert u gewoon een nieuwe container. Instructies en voorbeeld code vindt u in [Dit Azure](https://github.com/Azure-Samples/aml-hardware-accelerated-models)-voor beeld.

## <a name="secure-fpga-web-services"></a>Beveiligde FPGA-webservices

Zie het document [beveiligde](how-to-secure-web-service.md) webservices voor informatie over het beveiligen van FPGA-webservices.

## <a name="pbs-family-vms"></a>Vm's uit de PBS-serie

De PBS-serie van Azure-Vm's bevat Intel Arria 10 Fpga's.  Het wordt weer gegeven als ' standaard-PBS-serie Vcpu's ' wanneer u uw Azure-quotum toewijzing controleert.  De PB6-VM heeft zes Vcpu's en één FPGA en wordt automatisch ingericht door Azure ML als onderdeel van het implementeren van een model op een FPGA.  Het wordt alleen gebruikt in combi natie met Azure ML en er kan geen wille keurige bitstreams worden uitgevoerd.  U kunt de FPGA bijvoorbeeld niet flashen met bitstreams om versleuteling, code ring, enzovoort uit te voeren. 
