---
title: Een model implementeren voor Deductie met GPU
titleSuffix: Azure Machine Learning service
description: In dit artikel leert u hoe u de Azure Machine Learning-service gebruiken voor het implementeren van een deep learning-model als een web-service.service en score Deductie aanvragen met GPU Tensorflow.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: vaidyas
author: csteegz
ms.reviewer: larryfr
ms.date: 06/01/2019
ms.openlocfilehash: 8086d059913cc61bff0bca31681368bea6d76777
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543807"
---
# <a name="deploy-a-deep-learning-model-for-inference-with-gpu"></a>Een deep learning-model voor Deductie met GPU implementeren

In dit artikel leert u hoe u de Azure Machine Learning-service gebruiken voor het implementeren van een deep learning-model als een webservice met GPU Tensorflow.

Uw model implementeert naar een cluster Azure Kubernetes Service (AKS) met GPU inferentietaken doen. Inferentietaken of het model scoren, is de fase waarin het gedistribueerde model wordt gebruikt voor de voorspelling. Met behulp van GPU's in plaats van CPU's bieden prestatievoordelen op maximaal worden opgestart berekening.

Hoewel in dit voorbeeld een model TensorFlow wordt, kunt u de volgende stappen kunt toepassen op elke machine learning-framework die ondersteuning biedt voor GPU's met kleine wijzigingen aanbrengen in het scoring-bestand en het omgevingsbestand. 

In dit artikel voert u de volgende stappen uit:

* Een AKS met GPU-cluster maken
* Een Tensorflow GPU-model implementeren
* Een voorbeeldquery uitgeven aan uw geïmplementeerde model

## <a name="prerequisites"></a>Vereisten

* De werkruimte van een Azure Machine Learning-services.
* A Python distro.
* Een geregistreerde Tensorflow opgeslagen model.
    * Zie voor meer informatie over het registreren van modellen, [modellen implementeren](../service/how-to-deploy-and-where.md#registermodel).

U kunt voltooien deel één van deze serie instructies, [hoe u een TensorFlow-Model te trainen](how-to-train-tensorflow.md), om te voldoen aan de vereisten.

## <a name="provision-an-aks-cluster-with-gpus"></a>Een AKS-cluster met GPU's inrichten

Azure heeft veel verschillende GPU-opties. U kunt ze gebruiken voor inferentietaken. Zie [de lijst met virtuele machines uit de N-serie](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#n-series) voor een volledig overzicht van de mogelijkheden en kosten.

Zie voor meer informatie over het gebruik van AKS met Azure Machine Learning-service [implementeren en waar](../service/how-to-deploy-and-where.md#deploy-aks).

```Python
# Choose a name for your cluster
aks_name = "aks-gpu"

# Check to see if the cluster already exists
try:
    compute_target = ComputeTarget(workspace=ws, name=aks_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    # Provision AKS cluster with GPU machine
    prov_config = AksCompute.provisioning_configuration(vm_size="Standard_NC6")

    # Create the cluster
    aks_target = ComputeTarget.create(
        workspace=ws, name=aks_name, provisioning_configuration=prov_config
    )

    aks_target.wait_for_completion(show_output=True)
```

> [!IMPORTANT]
> Azure stuurt u een factuur, zolang het AKS-cluster is ingericht. Zorg ervoor dat u uw AKS-cluster verwijderen wanneer u klaar bent met het.

## <a name="write-the-entry-script"></a>De post-script schrijven

Sla de volgende code naar uw werkmap als `score.py`. Dit bestand beoordeelt installatiekopieën als ze worden verzonden naar uw service. Het laden van het model TensorFlow opgeslagen, wordt de afbeelding aan de sessie TensorFlow doorgegeven op elke POST-aanvraag en retourneert vervolgens de resulterende scores. Andere frameworks inferentietaken waarbij verschillende scoring-bestanden.

```python
import json
import numpy as np
import os
import tensorflow as tf

from azureml.core.model import Model

def init():
    global X, output, sess
    tf.reset_default_graph()
    model_root = Model.get_model_path('tf-dnn-mnist')
    saver = tf.train.import_meta_graph(os.path.join(model_root, 'mnist-tf.model.meta'))
    X = tf.get_default_graph().get_tensor_by_name("network/X:0")
    output = tf.get_default_graph().get_tensor_by_name("network/output/MatMul:0")
    
    sess = tf.Session()
    saver.restore(sess, os.path.join(model_root, 'mnist-tf.model'))

def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    out = output.eval(session=sess, feed_dict={X: data})
    y_hat = np.argmax(out, axis=1)
    return y_hat.tolist()

```
## <a name="define-the-conda-environment"></a>Het conda-omgeving definiëren

Maak een conda-omgeving-bestand met de naam `myenv.yml` om op te geven van de afhankelijkheden voor uw service. Het is belangrijk om op te geven dat u `tensorflow-gpu` versnelde prestaties bereiken.

```yaml
name: project_environment
dependencies:
  # The python interpreter version.
  # Currently Azure ML only supports 3.5.2 and later.
- python=3.6.2

- pip:
  - azureml-defaults==1.0.43.*
- numpy
- tensorflow-gpu=1.12
channels:
- conda-forge
```

## <a name="define-the-gpu-inferenceconfig-class"></a>De klasse GPU InferenceConfig definiëren

Maak een `InferenceConfig` object waarmee de GPU's en zorgt ervoor dat CUDA is geïnstalleerd met de Docker-installatiekopie.

```python
from azureml.core.model import Model
from azureml.core.model import InferenceConfig

aks_service_name ='aks-dnn-mnist'
gpu_aks_config = AksWebservice.deploy_configuration(autoscale_enabled = False, 
                                                    num_replicas = 3, 
                                                    cpu_cores=2, 
                                                    memory_gb=4)
model = Model(ws,"tf-dnn-mnist")

inference_config = InferenceConfig(runtime= "python", 
                                   entry_script="score.py",
                                   conda_file="myenv.yml", 
                                   enable_gpu=True)
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

## <a name="issue-a-sample-query-to-your-model"></a>Een voorbeeldquery uitgeven aan uw model

Een testquery naar de geïmplementeerd model verzenden. Wanneer u een JPEG-afbeelding naar het model verzenden, beoordeelt het de installatiekopie. Het volgende codevoorbeeld maakt gebruik van een hulpprogrammafunctie voor externe afbeeldingen laden. U kunt de relevante code vinden op pir [TensorFlow-voorbeeld op GitHub](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow/utils.py). 

```python
# Used to test your webservice
from utils import load_data 

# Load test data from model training
X_test = load_data('./data/mnist/test-images.gz', False) / 255.0
y_test = load_data('./data/mnist/test-labels.gz', True).reshape(-1)

# send a random row from the test set to score
random_index = np.random.randint(0, len(X_test)-1)
input_data = "{\"data\": [" + str(list(X_test[random_index])) + "]}"

api_key = aks_service.get_keys()[0]
headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
resp = requests.post(aks_service.scoring_uri, input_data, headers=headers)

print("POST to url", aks_service.scoring_uri)
#print("input data:", input_data)
print("label:", y_test[random_index])
print("prediction:", resp.text)
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
