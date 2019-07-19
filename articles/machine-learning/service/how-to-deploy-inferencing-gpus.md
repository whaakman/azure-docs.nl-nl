---
title: Een model implementeren voor demijnen met GPU
titleSuffix: Azure Machine Learning service
description: In dit artikel leert u hoe u de Azure Machine Learning-service kunt gebruiken voor het implementeren van een met GPU ingeschakeld tensor flow-Learning model als een webservice. aanvragen voor het instellen van de service en de score.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: vaidyas
author: csteegz
ms.reviewer: larryfr
ms.date: 06/01/2019
ms.openlocfilehash: eeb1bc35e0438a7e99ea5ed8284f0c8611108da0
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68326992"
---
# <a name="deploy-a-deep-learning-model-for-inference-with-gpu"></a>Een diep leer model implementeren voor demijnen met GPU

In dit artikel leert u hoe u de Azure Machine Learning-service kunt gebruiken om een tensor flow diepe leer model met GPU te implementeren als een webservice.

Implementeer uw model op een AKS-cluster (Azure Kubernetes service) om te voor komen dat GPU wordt ingeschakeld. Detraining of model Score is de fase waarin het geïmplementeerde model wordt gebruikt voor de voor spelling. Het gebruik van Gpu's in plaats van Cpu's biedt prestatie voordelen voor een zeer kan worden opgestart berekening.

Hoewel dit voor beeld gebruikmaakt van een tensor flow-model, kunt u de volgende stappen Toep assen op elk machine learning Framework dat Gpu's ondersteunt door kleine wijzigingen aan te brengen in het Score bestand en het omgevings bestand. 

In dit artikel voert u de volgende stappen uit:

* Een AKS-cluster met GPU-functionaliteit maken
* Een tensor flow GPU-model implementeren
* Een voorbeeld query voor uw geïmplementeerde model uitgeven

## <a name="prerequisites"></a>Vereisten

* Een Azure Machine Learning Services-werk ruimte.
* Een python-distributie.
* Een geregistreerd tensor flow-model.
    * Zie [modellen implementeren](../service/how-to-deploy-and-where.md#registermodel)voor meer informatie over het registreren van modellen.

U kunt in deel één van deze instructies-to-serie, [een tensor flow-model, trainen](how-to-train-tensorflow.md)om te voldoen aan de vereiste vereisten.

## <a name="provision-an-aks-cluster-with-gpus"></a>Een AKS-cluster inrichten met Gpu's

Azure heeft veel verschillende GPU-opties. U kunt elk daarvan gebruiken voor het afkeuren. Bekijk [de lijst met virtuele machines uit de N-serie](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#n-series) voor een volledige uitsplitsing van de mogelijkheden en kosten.

Zie [How to deploy and where](../service/how-to-deploy-and-where.md#deploy-aks)(Engelstalig) voor meer informatie over het gebruik van AKS met Azure machine learning service.

```Python
# Choose a name for your cluster
aks_name = "aks-gpu"

# Check to see if the cluster already exists
try:
    aks_target = ComputeTarget(workspace=ws, name=aks_name)
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
> In azure wordt u gefactureerd zolang het AKS-cluster is ingericht. Zorg ervoor dat u uw AKS-cluster verwijdert wanneer u klaar bent.

## <a name="write-the-entry-script"></a>Het invoer script schrijven

Sla de volgende code op in uw werkmap als `score.py`. In dit bestand worden afbeeldingen gescoord wanneer ze naar uw service worden verzonden. Hiermee wordt het opgeslagen model tensor flow geladen, wordt de invoer installatie kopie door gegeven aan de tensor flow-sessie op elke POST-aanvraag, waarna de resulterende scores worden geretourneerd. Andere frameworks in het kader van interferentie vereisen verschillende Score bestanden.

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
## <a name="define-the-conda-environment"></a>De Conda-omgeving definiëren

Maak een Conda-omgevings `myenv.yml` bestand met de naam om de afhankelijkheden voor uw service op te geven. Het is belang rijk om op te geven dat `tensorflow-gpu` u gebruikt om versnelde prestaties te behaalt.

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

## <a name="define-the-gpu-inferenceconfig-class"></a>De GPU InferenceConfig-klasse definiëren

Maak een `InferenceConfig` object dat de gpu's mogelijk maakt en zorgt ervoor dat CUDA wordt geïnstalleerd met uw docker-installatie kopie.

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

- [Klasse InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py)
- [Klasse AksServiceDeploymentConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py)

## <a name="deploy-the-model"></a>Het model implementeren

Implementeer het model op uw AKS-cluster en wacht tot het uw service heeft gemaakt.

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
> Azure machine learning-service implementeert geen model met `InferenceConfig` een object waarvan de verwachte GPU moet worden ingeschakeld voor een cluster dat geen GPU heeft.

Zie [model klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py)voor meer informatie.

## <a name="issue-a-sample-query-to-your-model"></a>Een voorbeeld query voor uw model uitgeven

Verzend een test query naar het geïmplementeerde model. Wanneer u een JPEG-afbeelding naar het model verzendt, wordt de afbeelding gescoord. Het volgende code voorbeeld maakt gebruik van een externe hulpprogramma functie voor het laden van installatie kopieën. U kunt de relevante code vinden op PIR [tensor flow-voor beeld op github](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow/utils.py). 

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
> Zorg ervoor dat uw client zich in dezelfde Azure-regio bevindt als het eind punt om de latentie te minimaliseren en de door voer te optimaliseren. In dit voor beeld worden de Api's gemaakt in de Azure-regio VS Oost.

## <a name="clean-up-the-resources"></a>De resources opschonen

Als u het AKS-cluster specifiek voor dit voor beeld hebt gemaakt, verwijdert u uw resources Nadat u klaar bent.

> [!IMPORTANT]
> Azure factureert u op basis van hoe lang het AKS-cluster wordt geïmplementeerd. Zorg ervoor dat u het opschoont nadat u klaar bent.

```python
aks_service.delete()
aks_target.delete()
```

## <a name="next-steps"></a>Volgende stappen

* [Model implementeren op FPGA](../service/how-to-deploy-fpga-web-service.md)
* [Model implementeren met ONNX](../service/concept-onnx.md#deploy-onnx-models-in-azure)
* [Tensor flow DNN-modellen trainen](../service/how-to-train-tensorflow.md)
