---
title: Een model implementeren voor Deductie met GPU
titleSuffix: Azure Machine Learning service
description: Informatie over het implementeren van een deep learning-model als een webservice die gebruikmaakt van een GPU voor Deductie. In dit artikel wordt een model Tensorflow geïmplementeerd op een Azure Kubernetes Service-cluster. Het cluster maakt gebruik van een VM met GPU score Deductie verzoeken en host de webservice.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: vaidyas
author: csteegz
ms.reviewer: larryfr
ms.date: 05/02/2019
ms.openlocfilehash: ec71165553a1d65ff133d605bf94255100f74e6e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66388926"
---
# <a name="deploy-a-deep-learning-model-for-inference-with-gpu"></a>Een deep learning-model voor Deductie met GPU implementeren

Informatie over het gebruik van GPU Deductie voor een machine learning-model is geïmplementeerd als een webservice. Deductie of het model scoren, is de fase waarin het gedistribueerde model wordt gebruikt voor voorspellingen, meestal op productiegegevens.

In dit artikel leert u hoe u de Azure Machine Learning-service gebruiken voor het implementeren van een voorbeeld Tensorflow deep learning-model met een cluster Azure Kubernetes Service (AKS) op een GPU ingeschakeld virtuele machine (VM). Wanneer aanvragen worden verzonden naar de service, het model de GPU gebruikt om uit te voeren van de werkbelastingen Deductie.

GPU's bieden de prestatievoordelen ten opzichte van CPU's op maximaal worden opgestart berekening. Uitstekende use cases voor virtuele machines met GPU zijn deep learning model voor training en Deductie, met name voor grote batches van aanvragen.

In dit voorbeeld ziet u hoe u een TensorFlow model opgeslagen in Azure Machine Learning implementeren. U uitvoeren de volgende stappen:

* Een AKS met GPU-cluster maken
* Een Tensorflow GPU-model implementeren

## <a name="prerequisites"></a>Vereisten

* De werkruimte van een Azure Machine Learning-services
* Een Python-distributie
* Een geregistreerde Tensorflow opgeslagen model. Zie voor meer informatie over het registreren van modellen, [modellen implementeren](../service/how-to-deploy-and-where.md#registermodel).

In dit artikel is gebaseerd op de Jupyter-notebook [Tensorflow-modellen implementeren naar AKS](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/production-deploy-to-aks-gpu/production-deploy-to-aks-gpu.ipynb). De Jupyter-notebook gebruikt TensorFlow modellen opgeslagen en worden ze geïmplementeerd op een AKS-cluster. U kunt ook de notebook toepassen op elke machine learning-framework die ondersteuning biedt voor GPU's met kleine wijzigingen aanbrengen in het scoring-bestand en het omgevingsbestand.  

## <a name="provision-an-aks-cluster-with-gpus"></a>Een AKS-cluster met GPU's inrichten

Azure heeft veel verschillende GPU-opties. U kunt ze gebruiken voor inferentietaken. Zie [de lijst met virtuele machines uit de N-serie](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#n-series) voor een volledig overzicht van de mogelijkheden en kosten.

Zie voor meer informatie over het gebruik van AKS met Azure Machine Learning-service [implementeren en waar](../service/how-to-deploy-and-where.md#deploy-aks).

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

## <a name="write-the-entry-script"></a>De post-script schrijven

Sla de volgende code naar uw werkmap als `score.py`. Dit bestand beoordeelt installatiekopieën als ze worden verzonden naar uw service. Het laden van het model TensorFlow opgeslagen, wordt de afbeelding aan de sessie TensorFlow doorgegeven op elke POST-aanvraag en retourneert vervolgens de resulterende scores. Andere frameworks inferentietaken waarbij verschillende scoring-bestanden.

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

## <a name="define-the-conda-environment"></a>Het conda-omgeving definiëren

Maak een conda-omgeving-bestand met de naam `myenv.yml` om op te geven van de afhankelijkheden voor uw service. Het is belangrijk om op te geven dat u `tensorflow-gpu` versnelde prestaties bereiken.

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

## <a name="define-the-gpu-inferenceconfig-class"></a>De klasse GPU InferenceConfig definiëren

Maak een `InferenceConfig` object waarmee de GPU's en zorgt ervoor dat CUDA is geïnstalleerd met de Docker-installatiekopie.

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

Zie voor meer informatie:

- [InferenceConfig klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py)
- [AksServiceDeploymentConfiguration klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py)

## <a name="deploy-the-model"></a>Het model implementeren

Het model implementeert naar uw AKS-cluster en wacht tot het maken van uw service.

```python
aks_service = Model.deploy(ws,
                           models=[model],
                           inference_config=inference_config, 
                           deployment_config=gpu_aks_config,
                           deployment_target=aks_target,
                           name=aks_service_name)

aks_service.wait_for_deployment(show_output = True)
print(aks_service.state)
```

> [!NOTE]
> Azure Machine Learning-service wordt niet implementeren van een model met een `InferenceConfig` -object dat wordt verwacht dat de GPU naar een cluster waarop een GPU die niet zijn ingeschakeld.

Zie voor meer informatie, [Modelklasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

## <a name="issue-a-sample-query-to-your-deployed-model"></a>Een voorbeeldquery uitgeven aan uw geïmplementeerde model

Een testquery naar de geïmplementeerd model verzenden. Wanneer u een JPEG-afbeelding naar het model verzenden, beoordeelt het de installatiekopie.

```python
scoring_url = aks_service.scoring_uri
api_key = aks_service.get_key()(0)
IMAGEURL = "https://upload.wikimedia.org/wikipedia/commons/thumb/6/68/Lynx_lynx_poing.jpg/220px-Lynx_lynx_poing.jpg"

headers = {'Authorization':('Bearer '+ api_key)}
img_data = read_image_from(IMAGEURL).read()
r = requests.post(scoring_url, data = img_data, headers=headers)
```

> [!IMPORTANT]
> Latentie minimaliseren en optimaliseren van doorvoer, zorg ervoor dat uw client zich in dezelfde Azure-regio als het eindpunt. In dit voorbeeld worden de API's gemaakt in de oostelijke VS Azure-regio.

## <a name="clean-up-the-resources"></a>De resources opschonen

Uw resources te verwijderen wanneer u klaar bent met het volgende voorbeeld.

> [!IMPORTANT]
> Azure gefactureerd die op basis van hoe lang het AKS-cluster is geïmplementeerd. Zorg ervoor dat om op te schonen als u klaar bent met het.

```python
aks_service.delete()
aks_target.delete()
```

## <a name="next-steps"></a>Volgende stappen

* [Implementeren op FPGA-model](../service/how-to-deploy-fpga-web-service.md)
* [Model met ONNX implementeren](../service/concept-onnx.md#deploy-onnx-models-in-azure)
* [Tensorflow DNN modellen trainen](../service/how-to-train-tensorflow.md)
