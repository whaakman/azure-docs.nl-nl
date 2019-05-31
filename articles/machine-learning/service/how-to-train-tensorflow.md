---
title: TensorFlow-modellen trainen en registreren
titleSuffix: Azure Machine Learning service
description: Dit artikel leest u hoe te trainen en een TensorFlow-model met behulp van Azure Machine Learning-service te registreren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.date: 05/28/2019
ms.custom: seodec18
ms.openlocfilehash: f3d675d0eac1255974995fd7717192ec6a21bac1
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66400218"
---
# <a name="use-azure-machine-learning-service-to-train-and-register-tensorflow-models"></a>Azure Machine Learning-service gebruiken om te trainen en TensorFlow modellen registreren

Dit artikel leest u hoe te trainen en een TensorFlow-model met behulp van Azure Machine Learning-service te registreren. Gebruikt u de populaire [MNIST gegevensset](http://yann.lecun.com/exdb/mnist/) handgeschreven cijfers met behulp van een deep neural network is gebouwd op TensorFlow classificeren.

Met Azure Machine Learning-service zult u kunnen worden uitgebreid snel uw open-source-trainingstaken met behulp van flexibele cloud compute-resources. U zult ook mogelijk uw trainingsuitvoeringen, versie modellen bijhouden, het implementeren van modellen en nog veel meer. Of u een TensorFlow-model van de grond-up ontwikkelt of u bent u een bestaand model in de cloud laadt, is Azure Machine Learning-service er om u informatie over het bouwen van modellen gereed is voor productie.

## <a name="prerequisites"></a>Vereisten

- De Azure Machine Learning-SDK voor Python installeren
- Optioneel: Het configuratiebestand van een werkruimte maken
- Download de [script voorbeeldbestanden](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow) `mnist-tf.py` en `utils.py`

U kunt volgen de [handleiding voor het instellen van Python-SDK](setup-create-workspace.md#sdk) voor stapsgewijze instructies over het instellen van uw omgeving. De voorbeeldbestanden training kunnen u vinden op onze [GitHub-voorbeelden-pagina](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow) samen met de uitgebreide, Juypter Notebook-versie van deze handleiding.

## <a name="set-up-the-experiment"></a>Instellen van het experiment

### <a name="import-packages"></a>Pakketten importeren

Er moet eerst de benodigde Python-bibliotheken importeren.

```Python
import os
import urllib
import shutil
import azureml

from azureml.core import Experiment
from azureml.core import Workspace, Run

from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
```

### <a name="initialize-a-workspace"></a>Een werkruimte initialiseren

Het object in de werkruimte is de resource op het hoogste niveau voor de service. Het biedt u met een centrale locatie voor het werken met alle artefacten die u maakt.

Als u de optionele stap in de [sectie vereisten](#prerequisites), kunt u `Workspace.from_config()` snel maken van het object in een werkruimte van de gegevens die zijn opgeslagen in het configuratiebestand.

```Python
ws = Workspace.from_config()
```

U kunt ook kan een werkruimte maken expliciet.

```Python
ws = Workspace.create(name='<workspace-name>',
                      subscription_id='<azure-subscription-id>',
                      resource_group='<choose-a-resource-group>',
                      create_resource_group=True,
                      location='<select-location>' # For example: 'eastus2'
                      )
```

### <a name="create-an-experiment"></a>Een experiment maken

Een experiment en een map voor het opslaan van uw trainingsscripts maken. In dit voorbeeld maakt u een experiment met de naam "tf-mnist"

```Python
script_folder = './tf-mnist'
os.makedirs(script_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='tf-mnist')
```

### <a name="upload-dataset-and-scripts"></a>Gegevensset en -scripts uploaden

De [gegevensopslag](how-to-access-data.md) is een plek waar gegevens kunnen worden opgeslagen en geopend door te koppelen of kopiëren van de gegevens naar de compute-doel. Elke werkruimte biedt een standaard-gegevensopslag. Dan uploaden wij onze gegevens en trainingsscripts zodat ze eenvoudig kunnen worden geopend tijdens de training.

1. De MNIST-gegevensset lokaal downloaden

    ```Python
    os.makedirs('./data/mnist', exist_ok=True)

    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz', filename = './data/mnist/train-images.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz', filename = './data/mnist/train-labels.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename = './data/mnist/test-images.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz', filename = './data/mnist/test-labels.gz')
    ```

1. Upload de MNIST-gegevensset naar de standaard-gegevensopslag.

    ```Python
    ds = ws.get_default_datastore()
    ds.upload(src_dir='./data/mnist', target_path='mnist', overwrite=True, show_progress=True)
    ```

1. Uploaden van het script TensorFlow-training `tf_mnist.py`, en het helperbestand `utils.py`.

    ```Python
    shutil.copy('./tf_mnist.py', script_folder)
    shutil.copy('./utils.py', script_folder)
    ```

## <a name="create-a-compute-target"></a>Een compute-doel maken

Maak een compute-doel voor uw TensorFlow-taak uit te voeren op. In dit voorbeeld maken we een AmlCompute met GPU cluster. Voor een lijst van beschikbare training van compute-doelen, Zie [in dit artikel](how-to-set-up-training-targets.md#compute-targets-for-training)

```Python
cluster_name = "gpucluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6', 
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

## <a name="create-a-tensorflow-estimator"></a>Een estimator TensorFlow maken

De [TensorFlow estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) biedt een eenvoudige manier om een taak TensorFlow-training op een compute-doel starten. Dit wordt automatisch een docker-installatiekopie die geïnstalleerd TensorFlow is bieden.

U kunt extra pip of conda-pakketten in de resulterende docker-installatiekopie opnemen door door te geven hun namen door middel van de `pip_packages` en `conda_packages` argumenten.

```Python
script_params = {
    '--data-folder': ws.get_default_datastore().as_mount(),
    '--batch-size': 50,
    '--first-layer-neurons': 300,
    '--second-layer-neurons': 100,
    '--learning-rate': 0.01
}

est = TensorFlow(source_directory=script_folder,
                 script_params=script_params,
                 compute_target=compute_target,
                 entry_script='tf_mnist.py',
                 use_gpu=True)
```

## <a name="submit-a-run"></a>Een uitvoering verzenden

De [object uitvoeren](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) biedt de interface voor de uitvoeringsgeschiedenis terwijl de taak wordt uitgevoerd en wanneer deze is voltooid.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

Als de uitvoering wordt uitgevoerd, gaat u door de volgende fasen:

- **Voorbereiden van**: Een docker-installatiekopie wordt gemaakt op basis van de estimator TensorFlow. De afbeelding is geüpload naar het containerregister van de werkruimte en hoger wordt uitgevoerd in de cache opgeslagen. Logboeken ook worden gestreamd naar de uitvoeringsgeschiedenis en kunnen worden weergegeven om voortgang te controleren.

- **Schalen**: Het cluster kan worden uitgebreid als wordt geprobeerd de Batch AI-cluster vereist meer knooppunten voor het uitvoeren van de uitvoering dan er op dit moment beschikbaar zijn.

- **Uitvoeren**: Alle scripts in de scriptmap worden geüpload naar de compute-doel, data-archieven worden gekoppeld of worden gekopieerd en de entry_script wordt uitgevoerd. Uitvoer van stdout en. / map logboeken worden gestreamd naar de uitvoeringsgeschiedenis en kan worden gebruikt voor het bewaken van de uitvoering.

- **Nabewerken**: De. / map van de uitvoering is gekopieerd naar de uitvoeringsgeschiedenis levert.

## <a name="register-or-download-a-model"></a>Registreren of te downloaden van een model

Nadat u het model hebt getraind, kunt u deze kunt registreren in uw werkruimte. Registratie van het model kunt u store en versie uw modellen in de werkruimte voor het vereenvoudigen van [model voor beheer en de implementatie](concept-model-management-and-deployment.md).

```Python
model = run.register_model(model_name='tf-dnn-mnist', model_path='outputs/model')
```

U kunt ook een lokale kopie van het model downloaden met behulp van het object uitvoeren. In het trainingsscript `mnist-tf.py`, een object van de schermbeveiliging TensorFlow zich blijft voordoen het model naar een lokale map (lokaal naar de compute-doel). We kunnen de Run-object gebruiken om een kopie te downloaden.

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

for f in run.get_file_names():
    if f.startswith('outputs/model'):
        output_file_path = os.path.join('./model', f.split('/')[-1])
        print('Downloading from {} to {} ...'.format(f, output_file_path))
        run.download_file(name=f, output_file_path=output_file_path)
```

## <a name="distributed-training"></a>Gedistribueerde training

De [ `TensorFlow` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) estimator biedt ook ondersteuning voor gedistribueerde training over CPU en GPU-clusters. U kunt eenvoudig gedistribueerde TensorFlow-taken uitvoeren en Azure Machine Learning-service de indeling u wilt beheren.

Azure Machine Learning-service ondersteunt twee methoden van gedistribueerde training in TensorFlow:

* [Op basis van een MPI](https://www.open-mpi.org/) gedistribueerd training met behulp van de [Horovod](https://github.com/uber/horovod) framework
* Systeemeigen [gedistribueerde TensorFlow](https://www.tensorflow.org/deploy/distributed) met behulp van de methode van de parameter-server

### <a name="horovod"></a>Horovod

[Horovod](https://github.com/uber/horovod) is een open-source framework voor gedistribueerde cursussen ontwikkeld door Uber. Het biedt een eenvoudige manier gedistribueerde TensorFlow GPU-taken.

Geef voor het gebruik van Horovod `mpi` voor de `distributed_training` parameter in de TensorFlow estimator-constructor. Horovod wordt geïnstalleerd voor gebruik in uw trainingsscript.

```Python
from azureml.train.dnn import TensorFlow

# Tensorflow constructor
estimator= TensorFlow(source_directory=project_folder,
                      compute_target=compute_target,
                      script_params=script_params,
                      entry_script='script.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_backend='mpi',
                      use_gpu=True)
```

### <a name="parameter-server"></a>Parameterserver

U kunt ook uitvoeren [native gedistribueerde TensorFlow](https://www.tensorflow.org/deploy/distributed), waarbij het servermodel parameter worden gebruikt. Bij deze methode hoeft trainen u in een cluster van de parameter-servers en werknemers. De werknemers Bereken de verlopen tijdens de training, terwijl de parameter-servers de verlopen aggregeren.

Geef voor het gebruik van de methode van de parameter `ps` voor de `distributed_training` parameter in de TensorFlow estimator-constructor.

```Python
from azureml.train.dnn import TensorFlow

# Tensorflow constructor
estimator= TensorFlow(source_directory=project_folder,
                      compute_target=compute_target,
                      script_params=script_params,
                      entry_script='script.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_backend='ps',
                      use_gpu=True)

# submit the TensorFlow job
run = exp.submit(tf_est)
```

#### <a name="note-on-tfconfig"></a>Houd er rekening mee op `TF_CONFIG`

U moet ook de netwerkadressen en poorten van het cluster voor de [ `tf.train.ClusterSpec` ](https://www.tensorflow.org/api_docs/python/tf/train/ClusterSpec), zodat Azure Machine Learning wordt de `TF_CONFIG` omgevingsvariabele voor u.

De `TF_CONFIG` omgevingsvariabele is een JSON-tekenreeks. Hier volgt een voorbeeld van de variabele voor de parameterserver:

```JSON
TF_CONFIG='{
    "cluster": {
        "ps": ["host0:2222", "host1:2222"],
        "worker": ["host2:2222", "host3:2222", "host4:2222"],
    },
    "task": {"type": "ps", "index": 0},
    "environment": "cloud"
}'
```

Voor de hoge mate van TensorFlow [ `tf.estimator` ](https://www.tensorflow.org/api_docs/python/tf/estimator) API, TensorFlow parseren dit `TF_CONFIG` variabele en het buildnummer van het cluster specificaties voor u.

Parseren voor TensorFlow van lager niveau core API's voor training, de `TF_CONFIG` variabele en bouwen de `tf.train.ClusterSpec` in uw trainingen-code. In [in dit voorbeeld](https://aka.ms/aml-notebook-tf-ps), doet u dit in **uw trainingsscript** als volgt:

```Python
import os, json
import tensorflow as tf

tf_config = os.environ.get('TF_CONFIG')
if not tf_config or tf_config == "":
    raise ValueError("TF_CONFIG not found.")
tf_config_json = json.loads(tf_config)
cluster_spec = tf.train.ClusterSpec(cluster)

```

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt getraind en een TensorFlow-model van Azure Machine Learning-service geregistreerd. Ga verder dan het model implementeren-artikel voor meer informatie over het implementeren van een model.

> [!div class="nextstepaction"]
> [Hoe en waar u kunt modellen implementeren](how-to-deploy-and-where.md)
