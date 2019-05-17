---
title: Model implementeren voor inferentietaken met GPU
titleSuffix: Azure Machine Learning service
description: Informatie over het implementeren van een deep learning-model als een webservice die gebruikmaakt van een GPU voor inferentietaken. In dit artikel wordt een model Tensorflow geïmplementeerd op een Azure Kubernetes Service-cluster. Het cluster maakt gebruik van een VM met GPU score inferentietaken verzoeken en host de webservice.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: vaidyas
author: csteegz
ms.reviewer: larryfr
ms.date: 05/02/2019
ms.openlocfilehash: 7796e8dc07889c9816e4227f3b38904d91a24da3
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2019
ms.locfileid: "65595656"
---
# <a name="deploy-a-deep-learning-model-for-inferencing-with-gpu"></a>Een deep learning-model voor inferentietaken met GPU implementeren

Informatie over het gebruik van GPU inferentietaken voor een machine learning-model is geïmplementeerd als een webservice. In dit artikel leert u hoe u de Azure Machine Learning-service gebruiken voor het implementeren van een voorbeeld Tensorflow deep learning-model. Het model wordt geïmplementeerd op een Azure Kubernetes Service (AKS)-cluster dat gebruik maakt van een VM met GPU voor het hosten van de service. Wanneer aanvragen worden verzonden naar de service, het model de GPU gebruikt om uit te voeren inferentietaken.

GPU's bieden de prestatievoordelen ten opzichte van CPU's op maximaal worden opgestart berekening. Training en inferentietaken deep learning-modellen (vooral voor grote batches van aanvragen) zijn uitstekende gebruiksvoorbeelden voor GPU's.  

In dit voorbeeld wordt beschreven hoe u een opgeslagen TensorFlow-model implementeren voor Azure Machine Learning door:
* Het maken van een AKS met GPU-cluster
* Implementeren van een model met Tensorflow-GPU

## <a name="prerequisites"></a>Vereisten

* Azure Machine Learning-services-werkruimte
* Python
* Tensorflow SavedModel geregistreerd. Voor meer informatie over het registreren van modellen Zie [modellen implementeren](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where#registermodel)

In dit artikel is gebaseerd op Jupyter-notebook [Tensorflow-modellen implementeren naar AKS](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/production-deploy-to-aks-gpu/production-deploy-to-aks-gpu.ipynb), die gebruikmaakt van opgeslagen TensorFlow-modellen en implementeert in een AKS-cluster. Met kleine wijzigingen aan de scoring-bestand en het omgevingsbestand is het echter van toepassing op elke machine learning-frameworks die ondersteuning bieden voor GPU's.  

## <a name="provision-aks-cluster-with-gpus"></a>AKS-cluster inrichten met GPU 's
Azure heeft veel verschillende GPU-mogelijkheden, die allemaal kunnen worden gebruikt voor Inferentietaken. Zie [de lijst met N-serie](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#n-series) voor een volledig overzicht van de mogelijkheden en kosten. 

Zie voor meer informatie over het gebruik van AKS met Azure Machine Learning-service de [over het implementeren en artikel.](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where#create-a-new-cluster)

```python
# Provision AKS cluster with GPU machine
prov_config = AksCompute.provisioning_configuration(vm_size="Standard_NC6")

# Create the cluster
aks_target = ComputeTarget.create(
    workspace=ws, name=aks_name, provisioning_configuration=prov_config
)

aks_target.wait_for_deployment()
```

> [!IMPORTANT]
> Azure stuurt u een factuur, zolang het AKS-cluster is ingericht. Zorg ervoor dat u uw AKS-cluster verwijderen wanneer u klaar bent met het.


## <a name="write-entry-script"></a>Post-script schrijven

Sla uw werkmap als `score.py`. Dit bestand wordt gebruikt om afbeeldingen te beoordelen als ze worden verzonden naar uw service. Dit bestand de TensorFlow opgeslagen model wordt geladen, en vervolgens op elk bericht aanvraag geeft de afbeelding aan de TensorFlow-sessie en retourneert de resulterende scores.
Andere frameworks inferentietaken moeten verschillende scoring-bestanden.

```python
import tensorflow as tf
import numpy as np
import ujson
from azureml.core.model import Model
from azureml.contrib.services.aml_request import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse

def init():
    global session
    global input_name
    global output_name
    
    session = tf.Session()

    model_path = Model.get_model_path('resnet50')
    model = tf.saved_model.loader.load(session, ['serve'], model_path)
    if len(model.signature_def['serving_default'].inputs) > 1:
        raise ValueError("This score.py only supports one input")
    input_name = [tensor.name for tensor in model.signature_def['serving_default'].inputs.values()][0]
    output_name = [tensor.name for tensor in model.signature_def['serving_default'].outputs.values()]
    

@rawhttp
def run(request):
    if request.method == 'POST':
        reqBody = request.get_data(False)
        resp = score(reqBody)
        return AMLResponse(resp, 200)
    if request.method == 'GET':
        respBody = str.encode("GET is not supported")
        return AMLResponse(respBody, 405)
    return AMLResponse("bad request", 500)

def score(data):
    result = session.run(output_name, {input_name: [data]})
    return ujson.dumps(result[1])

if __name__ == "__main__":
    init()
    with open("lynx.jpg", 'rb') as f: #load file for testing locally
        content = f.read()
        print(score(content))

```

## <a name="define-conda-environment"></a>Conda-omgeving definiëren
Maak een conda-omgeving-bestand met de naam `myenv.yml` om op te geven van de afhankelijkheden voor uw service. Het is belangrijk om op te geven dat u gebruikmaakt van `tensorflow-gpu` versnelde prestaties bereiken.
```yaml
name: aml-accel-perf
channels:
  - defaults
dependencies:
  - tensorflow-gpu = 1.12
  - numpy
  - ujson
  - pip:
    - azureml-core
    - azureml-contrib-services
```

## <a name="define-gpu-inferenceconfig"></a>GPU-InferenceConfig definiëren

Maak een [ `InferenceConfig` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) dat aangeeft dat u GPU wilt inschakelen. Dit zorgt ervoor dat CUDA met uw installatiekopie is geïnstalleerd.

```python
from azureml.core.model import Model
from azureml.core.model import InferenceConfig

aks_service_name ='gpu-rn'
gpu_aks_config = AksWebservice.deploy_configuration(autoscale_enabled = False, 
                                                    num_replicas = 3, 
                                                    cpu_cores=2, 
                                                    memory_gb=4)
model = Model(ws,"resnet50")

inference_config = InferenceConfig(runtime= "python", 
                                   entry_script="score.py",
                                   conda_file="myenv.yml", 
                                   gpu_enabled=True)
```

Zie voor meer informatie, [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) en [AksServiceDeploymentConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py).
## <a name="deploy-the-model"></a>Het model implementeren

Het model implementeert naar uw AKS-cluster en wacht tot het maken van uw service.

```python
aks_service = Model.deploy(ws,
                           models=[model],
                           inference_config=inference_config, 
                           deployment_config=aks_config,
                           deployment_target=aks_target,
                           name=aks_service_name)

aks_service.wait_for_deployment(show_output = True)
print(aks_service.state)
```

> [!NOTE]
> Azure Machine Learning-service wordt niet geïmplementeerd voor een model met een `InferenceConfig` die GPU wordt verwacht dat aan een cluster zonder GPU.

Zie voor meer informatie, [Model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

## <a name="issue-sample-query-to-deployed-model"></a>Voorbeeldquery probleem naar geïmplementeerd model

Geven een voorbeeld-query voor het model van uw geïmplementeerde. Dit model wordt een jpeg-afbeelding die u als een post-aanvraag naar deze verzenden score. 

```python
scoring_url = aks_service.scoring_uri
api_key = aks_service.get_key()(0)
IMAGEURL = "https://upload.wikimedia.org/wikipedia/commons/thumb/6/68/Lynx_lynx_poing.jpg/220px-Lynx_lynx_poing.jpg"

headers = {'Authorization':('Bearer '+ api_key)}
img_data = read_image_from(IMAGEURL).read()
r = requests.post(scoring_url, data = img_data, headers=headers)
```

> [!IMPORTANT]
> Voor het optimaliseren van latentie en doorvoer, moet uw client zich in dezelfde Azure-regio als het eindpunt.  Op dit moment worden de API's gemaakt in de oostelijke VS Azure-regio.

## <a name="cleaning-up-the-resources"></a>De resources opschonen

Uw resources te verwijderen nadat u klaar bent met de demo.

> [!IMPORTANT]
> Azure brengt kosten in rekening op basis van hoe lang het AKS-cluster is geïmplementeerd. Zorg ervoor dat om op te schonen als u klaar bent met het.

```python
aks_service.delete()
aks_target.delete()
```

## <a name="next-steps"></a>Volgende stappen

* [Implementeren op FPGA-model](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-fpga-web-service)
* [Model met ONNX implementeren](https://docs.microsoft.com/azure/machine-learning/service/how-to-build-deploy-onnx#deploy)
* [Tensorflow DNN modellen trainen](https://docs.microsoft.com/azure/machine-learning/service/how-to-train-tensorflow)
